---
title: Ejecución de paquetes SSIS mediante el agente de Instancia administrada de Azure°SQL
description: Aprenda a ejecutar paquetes SSIS mediante el agente de Instancia administrada de Azure°SQL.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: lle
author: lle
ms.date: 04/14/2020
ms.openlocfilehash: b3b7a25149a9d075c81b30307ade2beb71907637
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394062"
---
# <a name="execute-ssis-packages-by-azure-sql-managed-instance-agent"></a>Ejecución de paquetes SSIS mediante el agente de Instancia administrada de Azure°SQL
En este artículo se describe cómo ejecutar un paquete de SQL Server Integration Services (SSIS) mediante el agente de Instancia administrada de Azure SQL. Esta característica proporciona comportamientos similares a cuando se programan paquetes SSIS mediante el Agente SQL Server en el entorno local.

Con esta característica, puede ejecutar paquetes SSIS que se almacenan en SSISDB de Instancia administrada de Azure SQL o un sistema de archivos como Azure Files.

## <a name="prerequisites"></a>Prerrequisitos
Para usar esta característica, descargue e instale la versión más reciente de SSMS, que es la versión 18.5 o posterior. Descárguelo de [este sitio web](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

Además, debe aprovisionar una instancia de Azure-SSIS Integration Runtime en Azure Data Factory, que usa la Instancia administrada de Azure SQL como servidor de punto de conexión. Si todavía no lo aprovisiona, siga estas instrucciones que aparecen en el [tutorial](tutorial-create-azure-ssis-runtime-portal.md) para aprovisionarlo. 

## <a name="run-ssis-packages-in-ssisdb-by-azure-sql-managed-instance-agent"></a>Ejecución de paquetes SSIS en SSISDB con el agente de Instancia administrada de Azure SQL
En este paso, usará el agente de Instancia administrada de Azure SQL para invocar paquetes SSIS que se almacenan en SSISDB en la Instancia administrada de Azure SQL.
1. En la versión más reciente de SSMS, conéctese a la Instancia administrada de Azure°SQL.
2. Cree un trabajo del Agente nuevo y un paso de trabajo nuevo.

![Nuevo trabajo del Agente](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

3. En la página **Nuevo paso de trabajo**, elija el tipo **Paquete de SQL Server Integration Services**.

![Nuevo Paso de trabajo de SSIS](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

4. En la pestaña **Paquete**, elija **Catálogo de SSIS** como el tipo de origen del paquete.
5. Como SSISDB está en la misma Instancia administrada de Azure SQL, no necesita especificar la autenticación.
6. Especifique un paquete SSIS de su SSISDB.

![Tipo de origen del paquete: catálogo de SSIS](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb.png)

7. En la pestaña **Configuraciones**, puede especificar los valores de **parámetro**, invalidar los valores de **Administradores de conexión**, sustituir **Propiedad** y elegir **Nivel de registro**.

![Tipo de origen del paquete: configuración del catálogo de SSIS](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb-configuration.png)

8. Cuando complete toda la configuración anterior, haga clic en **Aceptar** para guardar la configuración del trabajo del Agente.
9. Inicie el trabajo del Agente para ejecutar el paquete SSIS.


## <a name="run-ssis-packages-in-file-system-by-azure-sql-managed-instance-agent"></a>Ejecución de paquetes SSIS en el sistema de archivos con el agente de Instancia administrada de Azure SQL
En este paso, usará el agente de Instancia administrada de Azure SQL para invocar paquetes SSIS que se almacenan sistemas de archivos para su ejecución.
1. En la versión más reciente de SSMS, conéctese a la Instancia administrada de Azure°SQL.
2. Cree un trabajo del Agente nuevo y un paso de trabajo nuevo.

   ![Nuevo trabajo del Agente](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

3. En la página **Nuevo paso de trabajo**, elija el tipo **Paquete de SQL Server Integration Services**.

   ![Nuevo Paso de trabajo de SSIS](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

4. En la pestaña **Paquete**, elija **Sistema de archivos** como el tipo de origen del paquete.

   ![Tipo de origen del paquete: sistema de archivos](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-file-system.png)

   1. Si el paquete se carga a Azure Files, elija **Recurso compartido de archivos de Azure** como el tipo de origen del archivo.
      - La ruta de acceso al paquete es **\\<storage account name>.file.core.windows.net\<nombre del recurso compartido de archivos>\<nombre del paquete>.dtsx**
      - Escriba el nombre de la cuenta del archivo de Azure y la clave de cuenta en la **credencial de acceso al archivo del paquete** para acceder al archivo de Azure. El dominio se establece como **Azure**.
   2. Si el paquete se carga a un recurso compartido de red, elija **Recurso compartido de red** como el tipo de origen del archivo.
      - La ruta de acceso al paquete es la **ruta de acceso UNC** del archivo del paquete con su extensión dtsx.
      - Escriba el **dominio**, el **nombre de usuario** y la **contraseña** correspondientes para acceder al archivo de paquete del recurso compartido de red.
   3. Si el archivo de paquete está cifrado con una contraseña, seleccione **Contraseña de cifrado** y escriba la contraseña.

 5. En la pestaña **Configuraciones**, escriba la **ruta de acceso al archivo de configuración** si necesita un archivo de configuración para ejecutar el paquete SSIS.
 6. En la pestaña **Opciones de ejecución**, puede elegir si usar la **autenticación de Windows** o **tiempo de ejecución de 32 bits** para ejecutar el paquete SSIS.
 7. En la pestaña **Registro**, puede elegir la **ruta de acceso al registro** y la credencial de acceso al registro correspondiente para almacenar los archivos de registro. De forma predeterminada, la ruta de acceso al registro será la misma que la ruta de acceso a la carpeta del paquete y la credencial de acceso al registro será igual que la credencial de acceso al paquete.
 8. En la pestaña **Establecer valores**, puede escribir la **Ruta de acceso de la propiedad** y el **Valor** para invalidar las propiedades del paquete.
 Por ejemplo, para invalidar el valor de la variable de usuario, escriba su ruta de acceso con el formato siguiente: **\Package.Variables[Usuario::<variable name>].Value**.
 9. Cuando complete toda la configuración anterior, haga clic en **Aceptar** para guardar la configuración del trabajo del Agente.
 10. Inicie el trabajo del Agente para ejecutar el paquete SSIS.


 ## <a name="cancel-ssis-package-execution"></a>Cancelación de la ejecución de paquetes SSIS
 Para cancelar la ejecución de un paquete desde un trabajo del Agente de Instancia administrada de Azure SQL, debe seguir estos pasos en lugar de detener directamente el trabajo del Agente.
 1. Busque el valor de **jobId** del Agente de SQL en **msdb.dbo.sysjobs**.
 2. Busque el valor de **executionId** de SSIS correspondiente en función del identificador del trabajo según la consulta que se muestra a continuación:
    ```sql
    select * from ssisdb.internal.execution_parameter_values_noncatalog where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
    ```
 3. Seleccione **Operaciones activas** en el catálogo de SSIS.

    ![Tipo de origen del paquete: sistema de archivos](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

 4. Detenga la operación correspondiente según **executionId**.

## <a name="next-steps"></a>Pasos siguientes
 También puede programar los paquetes SSIS con Azure Data Factory. Para instrucciones paso a paso, consulte [Desencadenador de eventos de Azure Data Factory](how-to-create-event-trigger.md). 