---
title: Uso de datos de referencia de SQL Database en un trabajo de Azure Stream Analytics
description: En este artículo se describe cómo utilizar una instancia de SQL Database como entrada de datos de referencia para un trabajo de Azure Stream Analytics en Azure Portal y en Visual Studio.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: b9a855a89a37cde0be3c30b2428c32db361aa2e8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021694"
---
# <a name="use-reference-data-from-a-sql-database-for-an-azure-stream-analytics-job"></a>Uso de datos de referencia de una instancia de SQL Database para un trabajo de Azure Stream Analytics

Azure Stream Analytics admite Azure SQL Database como origen de la entrada de datos de referencia. Puede usar SQL Database como datos de referencia para su trabajo de Stream Analytics en Azure Portal y en Visual Studio con herramientas de Stream Analytics. En este artículo se muestra cómo realizar ambos métodos.

## <a name="azure-portal"></a>Azure portal

Use los pasos siguientes para agregar Azure SQL Database como un origen de entrada de referencia mediante Azure Portal:

### <a name="portal-prerequisites"></a>Requisitos previos de Azure Portal

1. Cree un trabajo de Stream Analytics.

2. Cree una cuenta de almacenamiento para que la use Stream Analytics.

3. Cree una base de datos de Azure SQL con un conjunto de datos para que el trabajo de Stream Analytics la pueda usar como datos de referencia.

### <a name="define-sql-database-reference-data-input"></a>Definición de la entrada de datos de referencia de SQL Database

1. En su trabajo de Stream Analytics, seleccione **Entradas** en **Topología de trabajo**. Haga clic en **Agregar entrada de referencia** y elija **SQL Database**.

   ![Entrada del trabajo de Stream Analytics](./media/sql-reference-data/stream-analytics-inputs.png)

2. Rellene la configuración de entrada de Stream Analytics. Elija el nombre de la base de datos, el nombre del servidor, el nombre de usuario y la contraseña. Si desea que la entrada de datos de referencia se actualice periódicamente, elija "Activar" para especificar la frecuencia de actualización con el formato DD:HH:MM. Si tiene conjuntos de datos grandes con una frecuencia de actualización breve, puede usar una [consulta delta](sql-reference-data.md#delta-query).

   ![Configuración de la referencia de SQL Database](./media/sql-reference-data/sql-input-config.png)

3. Pruebe la consulta de instantánea en el editor de consultas SQL. Para más información, vea [Uso del Editor de consultas SQL de Azure Portal para conectarse a datos y consultarlos](../azure-sql/database/connect-query-portal.md).

### <a name="specify-storage-account-in-job-config"></a>Especificación de la cuenta de almacenamiento en la configuración de trabajos

Vaya a **Configuración de cuenta de almacenamiento** en **Configurar** y seleccione **Agregar cuenta de almacenamiento** .

   ![Configuración de la cuenta de almacenamiento de Stream Analytics](./media/sql-reference-data/storage-account-settings.png)

### <a name="start-the-job"></a>Inicio del trabajo

Una vez haya configurado otras entradas, salidas y consultas, puede iniciar el trabajo de Stream Analytics.

## <a name="tools-for-visual-studio"></a>Herramientas para Visual Studio

Use los pasos siguientes para agregar Azure SQL Database como un origen de entrada de referencia mediante Visual Studio:

### <a name="visual-studio-prerequisites"></a>Requisitos previos de Visual Studio

1. [Instale las herramientas de Stream Analytics para Visual Studio](stream-analytics-tools-for-visual-studio-install.md). Las siguientes versiones de Visual Studio son compatibles:

   * Visual Studio 2015
   * Visual Studio 2019

2. Familiarícese con la guía de inicio rápido de las [herramientas de Stream Analytics para Visual Studio](stream-analytics-quick-create-vs.md).

3. Cree una cuenta de almacenamiento.

### <a name="create-a-sql-database-table"></a>Creación de una tabla de SQL Database

Use SQL Server Management Studio para crear una tabla para almacenar los datos de referencia. Vea [Diseño de la primera base de datos de Azure SQL mediante SSMS](../azure-sql/database/design-first-database-tutorial.md) para obtener información detallada.

La tabla de ejemplo utilizada en el ejemplo siguiente se creó a partir de la siguiente instrucción:

```SQL
create table chemicals(Id Bigint,Name Nvarchar(max),FullName Nvarchar(max));
```

### <a name="choose-your-subscription"></a>Elija una suscripción

1. En Visual Studio, en el menú **Ver**, seleccione **Explorador de servidores**.

2. Haga clic con el botón derecho en **Azure**, seleccione **Connect to Microsoft Azure Subscription** (Conectarse a la suscripción de Microsoft Azure) e inicie sesión con su cuenta de Azure.

### <a name="create-a-stream-analytics-project"></a>Creación de un trabajo de Stream Analytics

1. Seleccione **Archivo > Nuevo proyecto**. 

2. En la lista de plantillas de la izquierda, seleccione **Stream Analytics** y **Aplicación de Azure Stream Analytics**. 

3. Escriba el **nombre** del proyecto, la **ubicación** y el **nombre de la solución** y seleccione **Aceptar**.

   ![Nuevo proyecto de Stream Analytics en Visual Studio](./media/sql-reference-data/stream-analytics-vs-new-project.png)

### <a name="define-sql-database-reference-data-input"></a>Definición de la entrada de datos de referencia de SQL Database

1. Cree una entrada.

   ![Nueva entrada de Stream Analytics en Visual Studio](./media/sql-reference-data/stream-analytics-vs-input.png)

2. Haga doble clic en **Input.json** en el **Explorador de soluciones**.

3. Rellene la **configuración de entrada de Stream Analytics**. Elija el nombre de la base de datos, el nombre del servidor, el tipo de actualización y la frecuencia de actualización. Especifique la frecuencia de actualización con el formato `DD:HH:MM`.

   ![Configuración de entrada de Stream Analytics en Visual Studio](./media/sql-reference-data/stream-analytics-vs-input-config.png)

   Si elige "Ejecutar solo una vez" o "Ejecutar periódicamente", un archivo de código subyacente de SQL denominado **[Alias de entrada].snapshot.sql** se genera en el proyecto en el nodo del archivo **Input.json**.

   ![Código de entrada subyacente en Visual Studio](./media/sql-reference-data/once-or-periodically-codebehind.png)

   Si elige "Actualizar periódicamente con Delta", se generarán dos archivos de código subyacentes de SQL: **[Input Alias].snapshot.sql** y **[Input Alias].delta.sql**.

   ![Código subyacente en el Explorador de soluciones](./media/sql-reference-data/periodically-delta-codebehind.png)

4. Abra el archivo SQL en el editor y escriba la consulta SQL.

5. Si usa Visual Studio 2019 y tiene herramientas de datos de SQL Server instaladas, puede probar la consulta al hacer clic en **Ejecutar**. Se abrirá una ventana del asistente para ayudarle a conectarse a la instancia de SQL Database, y el resultado de la consulta aparecerá en la ventana de la parte inferior.

### <a name="specify-storage-account"></a>Definición de la cuenta de almacenamiento

Abra **JobConfig.json** para especificar la cuenta de almacenamiento para almacenar las instantáneas de referencia SQL.

   ![Configuración de trabajos de Stream Analytics en Visual Studio](./media/sql-reference-data/stream-analytics-job-config.png)

### <a name="test-locally-and-deploy-to-azure"></a>Prueba local e implementación en Azure

Antes de implementar el trabajo en Azure, puede probar la lógica de consulta localmente con los datos de entrada activos. Para más información sobre esta característica, vea [Prueba local de datos activos mediante herramientas de Azure Stream Analytics para Visual Studio (versión preliminar)](stream-analytics-live-data-local-testing.md). Cuando haya terminado las pruebas, haga clic en **Enviar a Azure**. Consulte la guía de inicio rápido [Creación de un trabajo de Stream Analytics mediante las herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-quick-create-vs.md) para obtener información sobre cómo iniciar el trabajo.

## <a name="delta-query"></a>Consulta delta

Cuando se usa la consulta delta, se recomiendan las [tablas temporales de Azure SQL Database](../azure-sql/temporal-tables.md).

1. Cree una tabla temporal en Azure SQL Database.
   
   ```SQL 
      CREATE TABLE DeviceTemporal 
      (  
         [DeviceId] int NOT NULL PRIMARY KEY CLUSTERED 
         , [GroupDeviceId] nvarchar(100) NOT NULL
         , [Description] nvarchar(100) NOT NULL 
         , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
         , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
         , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
      )  
      WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DeviceHistory));  -- DeviceHistory table will be used in Delta query
   ```
2. Cree la consulta de instantánea. 

   Use el parámetro **\@snapshotTime** para indicar a Stream Analytics en tiempo de ejecución que obtenga el conjunto de datos de referencia de una tabla temporal válida de SQL Database a la hora del sistema. Si no proporciona este parámetro, corre el riesgo de obtener un conjunto de datos de referencia base impreciso debido al desfase del reloj. A continuación, se muestra un ejemplo de consulta de instantánea completa:
   ```SQL
      SELECT DeviceId, GroupDeviceId, [Description]
      FROM dbo.DeviceTemporal
      FOR SYSTEM_TIME AS OF @snapshotTime
   ```
 
2. Cree la consulta delta. 
   
   Esta consulta recupera todas las filas de la instancia de SQL Database insertadas o eliminadas dentro de una hora de inicio, **\@deltaStartTime**, y de una hora de finalización **\@deltaEndTime**. La consulta delta debe devolver las mismas columnas que la consulta de instantánea, además de la columna **_operation_**. Esta columna define si la fila se inserta o elimina entre **\@deltaStartTime** y **\@deltaEndTime**. Las filas resultantes se marcan como **1** si se insertaron los registros, o como **2** si estos se eliminaron. La consulta también debe agregar un **límite** del lado de SQL Server para asegurarse de que todas las actualizaciones del período delta se capturan adecuadamente. El uso de consultas delta sin **límite** puede devolver un conjunto de resultados de referencia incorrecto.  

   En el caso de los registros actualizados, las tablas temporales realiza la contabilidad mediante la captura de una operación de inserción y eliminación. Después, el tiempo de ejecución de Stream Analytics aplicará los resultados de la consulta delta a la instantánea anterior para mantener actualizados los datos de referencia. A continuación, se muestra un ejemplo de consulta delta:

   ```SQL
      SELECT DeviceId, GroupDeviceId, Description, ValidFrom as _watermark_, 1 as _operation_
      FROM dbo.DeviceTemporal
      WHERE ValidFrom BETWEEN @deltaStartTime AND @deltaEndTime   -- records inserted
      UNION
      SELECT DeviceId, GroupDeviceId, Description, ValidTo as _watermark_, 2 as _operation_
      FROM dbo.DeviceHistory   -- table we created in step 1
      WHERE ValidTo BETWEEN @deltaStartTime AND @deltaEndTime     -- record deleted
   ```
 
   Tenga en cuenta que el tiempo de ejecución de Stream Analytics puede ejecutar periódicamente la consulta de instantánea, además de la consulta delta para almacenar los puntos de control.

## <a name="test-your-query"></a>Prueba de la consulta
   Es importante comprobar que la consulta devuelve el conjunto de datos esperado que el trabajo de Stream Analytics usará como datos de referencia. Para probar la consulta, vaya a Entrada en la sección Topología de trabajo en el portal. Luego, puede seleccionar los datos de ejemplo en la entrada Referencia de base de datos SQL. Una vez que el ejemplo está disponible, puede descargar el archivo y comprobar si los datos se devuelven según lo esperado. Si quiere optimizar las iteraciones de prueba y desarrollo, le recomendamos usar las [Herramientas de Stream Analytics para Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install). También puede usar cualquier otra herramienta que prefiera para asegurarse primero de que la consulta devuelve los resultados adecuados desde la base de datos de Azure SQL y, luego, usarla en el trabajo de Stream Analytics. 

## <a name="faqs"></a>Preguntas más frecuentes

**¿Puedo incurrir en costos adicionales al usar la entrada de datos de referencia SQL en Azure Stream Analytics?**

No se aplica ningún [costo por unidad de streaming](https://azure.microsoft.com/pricing/details/stream-analytics/) adicional en el trabajo de Stream Analytics. Sin embargo, el trabajo de Stream Analytics debe tener una cuenta de Azure Storage asociada. El trabajo de Stream Analytics consulta la base de datos SQL (durante el inicio de trabajo y el intervalo de actualización) para recuperar el conjunto de datos de referencia y almacena dicha instantánea en la cuenta de almacenamiento. El almacenamiento de estas instantáneas incurrirá en los cargos adicionales detallados en la [página de precios](https://azure.microsoft.com/pricing/details/storage/) de la cuenta de Azure Storage.

**¿Cómo puedo saber si se realiza una consulta de la instantánea de los datos de referencia desde la base de datos SQL y si se usa en el trabajo de Azure Stream Analytics?**

Hay dos métricas filtradas por nombre lógico (en las métricas de Azure Portal) que puede usar para supervisar el estado de la entrada de datos de referencia de la base de datos SQL.

   * InputEvents: esta métrica mide el número de registros que se cargan desde el conjunto de datos de referencia de la base de datos SQL.
   * InputEventBytes: esta métrica mide el tamaño de la instantánea de datos de referencia cargada en memoria del trabajo de Stream Analytics. 

La combinación de ambas métricas se puede utilizar para deducir si el trabajo consulta la base de datos SQL para obtener el conjunto de datos de referencia y si después lo carga en memoria.

**¿Necesito un tipo especial de base de datos de Azure SQL?**

Azure Stream Analytics funcionará con cualquier tipo de base de datos de Azure SQL. Sin embargo, es importante comprender que el conjunto de frecuencia de actualización de la entrada de datos de referencia podría afectar a la carga de la consulta. Para usar la opción de la consulta delta, se recomienda usar las tablas temporales de Azure SQL Database.

**¿Por qué Azure Stream Analytics almacena las instantáneas en la cuenta de Azure Storage?**

Stream Analytics garantiza un procesamiento de eventos y al menos una entrega de eventos. En los casos en los que los problemas transitorios afectan a su trabajo, se necesita una pequeña cantidad de reproducción para restaurar el estado. Para habilitar la reproducción, es necesario que estas instantáneas estén almacenadas en una cuenta de Azure Storage. Para más información sobre la reproducción del punto de comprobación, vea [Conceptos de punto de control y reproducción en trabajos de Azure Stream Analytics](stream-analytics-concepts-checkpoint-replay.md).

## <a name="next-steps"></a>Pasos siguientes

* [Uso de datos de referencia para las búsquedas en Stream Analytics](stream-analytics-use-reference-data.md)
* [Inicio rápido: Creación de un trabajo de Stream Analytics mediante las herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-quick-create-vs.md)
* [Prueba local de datos activos mediante herramientas de Azure Stream Analytics para Visual Studio (versión preliminar)](stream-analytics-live-data-local-testing.md)
