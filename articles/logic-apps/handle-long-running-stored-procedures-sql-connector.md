---
title: Gestión de procedimientos almacenados de ejecución prolongada en el conector de SQL
description: Cómo gestionar los procedimientos almacenados que agotan el tiempo de espera cuando se usa el conector de SQL en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: camerost, logicappspm
ms.topic: conceptual
ms.date: 10/27/2020
ms.openlocfilehash: f5b04c563dc81497f591788dc4890d379c0f898f
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93102739"
---
# <a name="handle-stored-procedure-timeouts-in-the-sql-connector-for-azure-logic-apps"></a>Gestión de los tiempos de espera de los procedimientos almacenados en el conector de SQL para Azure Logic Apps

Si la aplicación lógica funciona con conjuntos de resultados tan grandes que el [conector de SQL](../connectors/connectors-create-api-sqlazure.md) no devuelve todos los resultados al mismo tiempo, o si desea tener más control sobre el tamaño y la estructura de los conjuntos de resultados, puede crear un [procedimiento almacenado](/sql/relational-databases/stored-procedures/stored-procedures-database-engine) que organice los resultados de la manera que desee. El conector de SQL proporciona muchas características de back-end a las que se puede acceder mediante [Azure Logic Apps](../logic-apps/logic-apps-overview.md), lo que permite automatizar más fácilmente las tareas empresariales que funcionan con tablas de base de datos SQL.

Por ejemplo, al obtener o insertar varias filas, la aplicación lógica puede iterar a través de estas filas mediante un [bucle **Until**](../logic-apps/logic-apps-control-flow-loops.md#until-loop) dentro de estos [límites](../logic-apps/logic-apps-limits-and-config.md). No obstante, si su aplicación lógica tiene que trabajar con miles o millones de filas, querrá minimizar los costos de las llamadas a la base de datos. Para obtener más información, consulte [Control de datos masivos mediante el conector de SQL](../connectors/connectors-create-api-sqlazure.md#handle-bulk-data).

<a name="timeout-limit"></a>

## <a name="timeout-limit-on-stored-procedure-execution"></a>Límite de tiempo de espera de la ejecución de procedimiento almacenado

El conector de SQL tiene un límite de tiempo de espera de procedimiento almacenado que es [inferior a 2 minutos](/connectors/sql/#known-issues-and-limitations). Algunos procedimientos almacenados pueden tardar más que este límite en completarse, lo que provocaría un error `504 Timeout`. A veces, estos procesos de ejecución prolongada se codifican como procedimientos almacenados explícitamente para este fin. Debido al límite de tiempo de espera, la llamada a estos procedimientos desde Azure Logic Apps puede causar problemas. Aunque el conector de SQL no admite de forma nativa el modo asincrónico, puede resolver este problema y simular este modo mediante un desencadenador de finalización de SQL, una consulta de paso a través de SQL nativa, una tabla de estado y trabajos del lado servidor. Para esta tarea, puede usar el [agente de trabajos elásticos de Azure](../azure-sql/database/elastic-jobs-overview.md) para [Azure SQL Database](../azure-sql/database/sql-database-paas-overview.md). Para [SQL Server local](/sql/sql-server/sql-server-technical-documentation) y [Azure SQL Managed Instance](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md), puede usar el [Agente SQL Server](/sql/ssms/agent/sql-server-agent).

Por ejemplo, supongamos que tiene el siguiente procedimiento almacenado de ejecución prolongada, que supera el límite de tiempo de espera al finalizar la ejecución. Si ejecuta este procedimiento almacenado desde una aplicación lógica mediante el conector de SQL, obtendrá un error `HTTP 504 Gateway Timeout` como resultado.

```sql
CREATE PROCEDURE [dbo].[WaitForIt]
   @delay char(8) = '00:03:00'
AS
BEGIN
   SET NOCOUNT ON;
   WAITFOR DELAY @delay
END
```

En lugar de llamar directamente al procedimiento almacenado, puede ejecutar el procedimiento de forma asincrónica en segundo plano mediante un *agente de trabajo*. Puede almacenar las entradas y salidas en una tabla de estados, con la que luego pueda interactuar a través de la aplicación lógica. Si no necesita las entradas y salidas, o si ya está escribiendo los resultados en una tabla en el procedimiento almacenado, puede simplificar este enfoque.

> [!IMPORTANT]
> Asegúrese de que el procedimiento almacenado y todos los trabajos son *idempotentes* , lo que significa que se pueden ejecutar varias veces sin que ello afecte a los resultados. Si se produce un error en el procesamiento asincrónico o se agota el tiempo de espera, el agente de trabajo podría reintentar el paso y, por tanto, el procedimiento almacenado, varias veces. Para evitar la duplicación de la salida, antes de crear ningún objeto, revise estos [procedimientos recomendados y enfoques](../azure-sql/database/elastic-jobs-overview.md#idempotent-scripts).

En la sección siguiente se describe cómo puede usar el agente de trabajos elásticos de Azure para Azure SQL Database. Para SQL Server y Azure SQL Managed Instance, puede usar el Agente SQL Server. Algunos detalles de administración variarán, pero los pasos fundamentales seguirán siendo los mismos que para la configuración de un agente de trabajo para Azure SQL Database.

<a name="azure-sql-database"></a>

## <a name="job-agent-for-azure-sql-database"></a>Agente de trabajo para Azure SQL Database

Para crear un trabajo que pueda ejecutar el procedimiento almacenado para [Azure SQL Database](../azure-sql/database/sql-database-paas-overview.md), use el [agente de trabajos elásticos de Azure](../azure-sql/database/elastic-jobs-overview.md). Cree el agente de trabajo en Azure Portal. Este enfoque agregará varios procedimientos almacenados a la base de datos que utiliza el agente, también conocida como *base de datos del agente*. Después, puede crear un trabajo que ejecute el procedimiento almacenado en la base de datos de destino y capture la salida al terminar.

Antes de poder crear el trabajo, debe configurar los permisos, los grupos y los destinos como se describe en la [documentación completa del agente de trabajos elásticos de Azure](../azure-sql/database/elastic-jobs-overview.md). También debe crear una tabla auxiliar en la base de datos de destino, tal y como se describe en las secciones siguientes.

<a name="create-state-table"></a>

### <a name="create-state-table-for-registering-parameters-and-storing-inputs"></a>Creación de una tabla de estados para registrar parámetros y almacenar entradas

Los trabajos del Agente SQL no aceptan parámetros de entrada. En su lugar, en la base de datos de destino, cree una tabla de estados donde registrar los parámetros y almacenar las entradas que se van a usar para llamar a los procedimientos almacenados. Todos los pasos del trabajo del agente se ejecutan en la base de datos de destino, pero los procedimientos almacenados del trabajo se ejecutan en la base de datos del agente. 

Para crear la tabla de estados, utilice este esquema:

```sql
CREATE TABLE [dbo].[LongRunningState](
   [jobid] [uniqueidentifier] NOT NULL,
   [rowversion] [timestamp] NULL,
   [parameters] [nvarchar](max) NULL,
   [start] [datetimeoffset](7) NULL,
   [complete] [datetimeoffset](7) NULL,
   [code] [int] NULL,
   [result] [nvarchar](max) NULL,
   CONSTRAINT [PK_LongRunningState] PRIMARY KEY CLUSTERED
      (   [jobid] ASC
      )WITH (STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF) ON [PRIMARY]
      ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
```

Este es el aspecto de la tabla resultante en [SQL Server Management Studio (SMSS)](/sql/ssms/download-sql-server-management-studio-ssms):

![Captura de pantalla que muestra la tabla de estados creada que almacena las entradas del procedimiento almacenado.](media/handle-long-running-stored-procedures-sql-connector/state-table-input-parameters.png)

Para garantizar un buen rendimiento y asegurarse de que el trabajo del agente pueda encontrar el registro asociado, la tabla usa el identificador de ejecución del trabajo (`jobid`) como clave principal. Si lo desea, también puede agregar columnas individuales para los parámetros de entrada. El esquema descrito anteriormente puede controlar varios parámetros de manera más general, pero se limita al tamaño calculado por `NVARCHAR(MAX)`.

<a name="create-top-level-job"></a>

### <a name="create-a-top-level-job-to-run-the-stored-procedure"></a>Creación de un trabajo de nivel superior para ejecutar el procedimiento almacenado

Para ejecutar el procedimiento almacenado de ejecución prolongada, cree este agente de trabajo de nivel superior en la base de datos del agente:

```sql
EXEC jobs.sp_add_job 
   @job_name='LongRunningJob',
   @description='Execute Long-Running Stored Proc',
   @enabled = 1
```

Ahora, agregue pasos al trabajo para parametrizar, ejecutar y completar el procedimiento almacenado. De forma predeterminada, el tiempo de espera de un paso de trabajo se agota al cabo de 12 horas. Si el procedimiento almacenado necesita más tiempo, o si desea que el procedimiento agote el tiempo de espera antes, puede cambiar el parámetro `step_timeout_seconds` a otro valor especificado en segundos. De forma predeterminada, un paso tiene 10 reintentos integrados con un tiempo de espera de retroceso entre cada reintento, que puede usar según le convenga.

Estos son los pasos que debe agregar:

1. Espere a que los parámetros aparezcan en la tabla `LongRunningState`.

   Este primer paso espera a que los parámetros se agreguen en la tabla `LongRunningState`, lo que sucede poco después del inicio del trabajo. Si el identificador de ejecución del trabajo (`jobid`) no se agrega a la tabla `LongRunningState`, el paso simplemente no se realizará correctamente y el tiempo de espera de reintento o de retroceso predeterminado se encargará de la espera:

   ```sql
   EXEC jobs.sp_add_jobstep
      @job_name='LongRunningJob',
      @step_name= 'Parameterize WaitForIt',
      @step_timeout_seconds = 30,
      @command= N'
         IF NOT EXISTS(SELECT [jobid] FROM [dbo].[LongRunningState]
            WHERE jobid = $(job_execution_id))
            THROW 50400, ''Failed to locate call parameters (Step1)'', 1',
      @credential_name='JobRun',
      @target_group_name='DatabaseGroupLongRunning'
   ```

1. Consulte los parámetros de la tabla de estados y páselos al procedimiento almacenado. Este paso también ejecuta el procedimiento en segundo plano. 

   Si el procedimiento almacenado no necesita parámetros, basta con llamar directamente al procedimiento almacenado. De lo contrario, para pasar el parámetro `@timespan`, use `@callparams`, que también puede ampliar para pasar parámetros adicionales.

   ```sql
   EXEC jobs.sp_add_jobstep
      @job_name='LongRunningJob',
      @step_name='Execute WaitForIt',
      @command=N'
         DECLARE @timespan char(8)
         DECLARE @callparams NVARCHAR(MAX)
         SELECT @callparams = [parameters] FROM [dbo].[LongRunningState]
            WHERE jobid = $(job_execution_id))
         SET @timespan = @callparams
         EXECUTE [dbo].[WaitForIt] @delay = @timespan', 
      @credential_name='JobRun',
      @target_group_name='DatabaseGroupLongRunning'
   ```

1. Complete el trabajo y registre los resultados.

   ```sql
   EXEC jobs.sp_add_jobstep
      @job_name='LongRunningJob',
      @step_name='Complete WaitForIt',
      @command=N'
         UPDATE [dbo].[LongRunningState]
            SET [complete] = GETUTCDATE(),
               [code] = 200,
               [result] = ''Success''
            WHERE jobid = $(job_execution_id)',
      @credential_name='JobRun',
      @target_group_name='DatabaseGroupLongRunning'
   ```

<a name="start-job-pass-parameters"></a>

### <a name="start-the-job-and-pass-the-parameters"></a>Inicio del trabajo y paso de los parámetros

Para iniciar el trabajo, use una consulta nativa de paso a través con la [acción **Ejecutar una consulta SQL**](/connectors/sql/#execute-a-sql-query-(v2)) e inserte inmediatamente los parámetros del trabajo en la tabla de estados. Para proporcionar la entrada al atributo `jobid` en la tabla de destino, Logic Apps agrega un bucle **For each** que itera en la salida de la tabla de la acción anterior. Para cada identificador de ejecución de trabajo, ejecute una acción **Insertar fila** que use la salida de datos dinámicos, `ResultSets JobExecutionId`, para agregar los parámetros del trabajo que se va a desempaquetar y pasarlos al procedimiento almacenado de destino.

![Captura de pantalla que muestra las acciones que se deben usar para iniciar el trabajo y pasar los parámetros al procedimiento almacenado.](media/handle-long-running-stored-procedures-sql-connector/start-job-actions.png)

Cuando el trabajo se completa, el trabajo actualiza la tabla `LongRunningState` para que pueda desencadenar fácilmente el resultado mediante el [desencadenador **Cuando se modifica un elemento**](/connectors/sql/#when-an-item-is-modified-(v2)). Si no necesita la salida o si ya tiene un desencadenador que supervisa una tabla de salida, puede omitir esta parte.

![Captura de pantalla que muestra el desencadenador SQL usado para modificar un elemento.](media/handle-long-running-stored-procedures-sql-connector/trigger-on-results-after-job-completes.png)

<a name="sql-on-premises-or-managed-instance"></a>

## <a name="job-agent-for-sql-server-or-azure-sql-managed-instance"></a>Agente de trabajo para SQL Server o Azure SQL Managed Instance

En el mismo escenario, puede usar el [Agente SQL Server](/sql/ssms/agent/sql-server-agent) para [SQL Server local](/sql/sql-server/sql-server-technical-documentation) y [Azure SQL Managed Instance](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md). Aunque algunos detalles de administración varíen, los pasos fundamentales siguen siendo los mismos que para la configuración de un agente de trabajo para Azure SQL Database.

## <a name="next-steps"></a>Pasos siguientes

[Conexión a SQL Server, Azure SQL Database o Azure SQL Managed Instance](../connectors/connectors-create-api-sqlazure.md)

