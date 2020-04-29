---
title: Ejecución de paquetes SSIS desde SSDT
description: Aprenda a ejecutar paquetes SSIS en Azure desde SSDT.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: sawinark
author: swinarko
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 07/31/2019
ms.openlocfilehash: 11e76fea87c60ae2b56cc15d5827be6e1b2b5a01
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81399440"
---
# <a name="execute-ssis-packages-in-azure-from-ssdt"></a>Ejecución de paquetes SSIS en Azure desde SSDT

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se describe la característica de los proyectos de SQL Server Integration Services (SSIS) habilitados para Azure en SQL Server Data Tools (SSDT), que permite ejecutar paquetes de Azure-SSIS Integration Runtime (IR) en Azure Data Factory (ADF).  Esta característica se puede usar para probar los paquetes SSIS existentes antes de migrarlos mediante lift-and-shift a Azure o desarrollar nuevos paquetes SSIS para ejecutarlos en Azure.

Con esta característica, puede crear un nuevo Azure-SSIS Integration Runtime o adjuntar uno existente a los proyectos de SSIS y, después, ejecutar los paquetes en él.  Admitimos la ejecución de paquetes que se van a implementar en el catálogo de SSIS (SSISDB) en el modelo de implementación de proyectos y los que se van a implementar en sistemas de archivos, recursos compartidos de archivos o Azure Files en el modelo de implementación de paquetes. 

## <a name="prerequisites"></a>Prerrequisitos
Para usar esta característica, descargue e instale la extensión SSDT con proyectos de SSIS para Visual Studio más reciente desde [aquí](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) o como un programa de instalación independiente desde [aquí](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer).

## <a name="azure-enable-ssis-projects"></a>Proyectos de SSIS habilitados para Azure
En SSDT, puede crear proyectos de SSIS habilitados para Azure mediante la plantilla **Proyecto de Integration Services (habilitado para Azure)** .

![Nuevo proyecto de SSIS habilitado para Azure](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-new-project.png)

Como alternativa, puede habilitar para Azure los proyectos de SSIS existentes; para ello, haga clic con el botón derecho en el nodo del proyecto en el panel de SSDT del Explorador de soluciones para que aparezca un menú y, después, seleccione el elemento de menú **Azure-Enabled** (Habilitado para Azure).

![Proyecto de SSIS existente habilitado para Azure](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-existing-project.png)

La habilitación para Azure de los proyectos de SSIS existentes requiere que establezca que la versión del servidor de destino sea la más reciente compatible con Azure-SSIS IR, que actualmente es **SQL Server 2017**, por lo que si aún no lo ha hecho, aparecerá una ventana de cuadro de diálogo para que lo haga.

## <a name="connect-azure-enabled-projects-to-ssis-in-azure-data-factory"></a>Conectar proyectos habilitados para Azure a SSIS en Azure Data Factory
Mediante la conexión de proyectos habilitados para Azure a SSIS en ADF, puede cargar sus paquetes en Azure Files y ejecutarlos en Azure-SSIS IR.  Para ello, siga estos pasos:

1. Haga clic con el botón derecho en el proyecto o en el nodo **Recursos de Azure vinculados**  del panel del Explorador de soluciones de SSDT y aparecerá un menú, donde puede seleccionar el elemento **Connect To SSIS in Azure Data Factory** (Conectar a SSIS en Azure Data Factory)  **para iniciar el Asistente para conexión de SSIS en ADF**.

   ![Conectar a SSIS en ADF](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-existing-project2.png)

2. En la página **SSIS en la introducción de ADF**, lea la introducción y haga clic en el botón **Siguiente** para continuar.

   ![SSIS en la introducción de ADF](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard.png)

3. En la página **Selección de SSIS IR en ADF**, seleccione los ADF y Azure-SSIS IR existentes para ejecutar paquetes o cree unos nuevos si no tiene ninguno.
   - Para seleccionar el Azure-SSIS IR existente, es preciso seleccionar antes el archivo de definición de aplicación y la suscripción a Azure relevante.
   - Si selecciona un archivo de definición de aplicación existente que no tenga ningún Azure-SSIS IR, haga clic en el botón**Crear SSIS IR** para crearlo en el portal o la aplicación de ADF.
   - Si selecciona una suscripción de Azure existente que no tenga ningún archivo de definición de aplicación, haga clic en el botón **Crear SSIS IR** para iniciar el **Asistente para la creación de Integration Runtime**, donde puede especificar la ubicación y prefijo para que se cree automáticamente un grupo de recursos de Azure, Data Factory y SSIS IR en su nombre, y para asignar el nombre se usa el siguiente patrón: **SuPrefijo-RG/DF/IR-SuHoraDeCreación**.
   
   ![Selección de SSIS IR en ADF](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard2.png)

4. En la página **Seleccionar Azure Storage** , seleccione su cuenta de Azure Storage existente para cargar paquetes en Azure Files, o si no tiene una cuenta, créela.
   - Para seleccionar la cuenta de Azure Storage existente, seleccione antes la suscripción a Azure relevante.
   - Si selecciona la misma suscripción de Azure que Azure-SSIS Integration Runtime que no tiene ninguna cuenta de Azure Storage, haga clic en el botón **Create Azure Storage** (Crear instancia de Azure Storage) para que se cree automáticamente una en su nombre en la misma ubicación que su Azure-SSIS IR, y para asignarle un nombre se combina un prefijo del nombre de Azure-SSIS Integration Runtime y su fecha de creación.
   - Si selecciona una suscripción de Azure diferente que no tiene ninguna cuenta de Azure Storage, haga clic en e,l botón **Create Azure Storage** (Crear instancia de Azure Storage) para crear una nueva en Azure Portal.
   
   ![Selección de Azure Storage](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard3.png)

5. Haga clic en el botón **Conectar** para completar la conexión.  En el panel del Explorador de soluciones de SSDT, se mostrará el Azure-SSIS IR y la cuenta de Azure Storage seleccionados en el nodo **Recursos de Azure vinculados** .  También se actualizará el estado de Azure-SSIS Integration Runtime, que puede administrar haciendo clic con el botón derecho en su nodo para que aparezca un menú y, después, seleccionando el elemento de menú **Start\Stop\Manage** (Iniciar\detener\administrar), que le lleva al portal o a la aplicación de ADF para hacerlo.

## <a name="execute-ssis-packages-in-azure"></a>Ejecución de paquetes SSIS en Azure
### <a name="starting-package-executions"></a>Inicio de las ejecuciones del paquete
Después de conectar los proyectos a SSIS en ADF, puede ejecutar paquetes en Azure-SSIS IR.  Tiene dos opciones para iniciar las ejecuciones de paquetes:
-  Haga clic en el botón **Iniciar** de la barra de herramientas de SSDT para desplegar un menú y seleccione el elemento de menú **Execute in Azure** (Ejecutar en Azure) 

   ![Ejecutar en Azure](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package.png)

-  Haga clic con el botón derecho en el nodo del paquete en el panel del Explorador de soluciones de SSDT para que aparezca un menú y seleccione el elemento de menú **Execute Package in Azure** (Ejecutar paquete en Azure).

   ![Ejecutar paquete en Azure](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package2.png)

> [!NOTE]
> La ejecución de paquetes en Azure requiere que Azure-SSIS Integration Runtime esté en ejecución, por lo que si Azure-SSIS IR se detiene, aparecerá una ventana de diálogo para iniciarlo.  Sin incluir el tiempo de configuración personalizada, este proceso debería completarse en 5 minutos, pero puede que tarde aproximadamente de 20 a 30 minutos en unir Azure-SSIS IR a una red virtual.  Después de ejecutar los paquetes en Azure, puede detener Azure-SSIS Integration Runtime para administrar su costo de ejecución haciendo clic con el botón derecho en su nodo en el panel del Explorador de soluciones de SSDT para que aparezca un menú y, después, seleccionando el elemento de menú **Start\Stop\Manage** (Iniciar\detener\administrar), que le lleva al portal o a la aplicación de ADF para hacerlo.

### <a name="checking-package-execution-logs"></a>Comprobación de los registros de ejecución de paquetes
Al iniciar la ejecución del paquete, formatearemos y mostraremos su registro en la ventana de progreso de SSDT.  En el caso de un paquete de ejecución prolongada, actualizaremos periódicamente su registro.  Para detener la ejecución del paquete, haga clic en el botón **Detener** de la barra de herramientas de SSDT, que la cancelará de inmediato.  También puede encontrar temporalmente los datos sin procesar de registro en su ruta de acceso UNC (convención de nomenclatura universal): `\\<YourConnectedAzureStorage>.file.core.windows.net\ssdtexecution\<YourProjectName-FirstConnectTime>\<YourPackageName-tmp-ExecutionTime>\logs`, pero se limpiarán a las 24 horas.

### <a name="switching-package-protection-level"></a>Cambio del nivel de protección de paquetes
La ejecución de paquetes SSIS en Azure no admite los niveles de protección **EncryptSensitiveWithUserKey**/**EncryptAllWithUserKey**.  Por tanto, si los paquetes están configurados con ellos, los cambiaremos temporalmente a **EncryptSensitiveWithPassword**/**EncryptAllWithPassword**, respectivamente, y se generarán contraseñas de forma aleatoria cuando se cargan los paquetes en Azure Files para su ejecución en Azure-SSIS Integration Runtime.

> [!NOTE]
> Si los paquetes contienen tareas Ejecutar paquete que hacen referencia a otros paquetes configurados con los niveles de protección **EncryptSensitiveWithUserKey**/**EncryptAllWithUserKey**, debe volver a configurar manualmente los otros paquetes para que usen **EncryptSensitiveWithPassword**/**EncryptAllWithPassword**, respectivamente, antes de ejecutar los paquetes.

Si los paquetes ya están configurados con los niveles de protección **EncryptSensitiveWithPassword**/**EncryptAllWithPassword**, se mantendrán inalterados, pero se seguirán usando contraseñas generadas de forma aleatoria al cargar los paquetes en Azure Files para su ejecución en Azure-SSIS Integration Runtime.

### <a name="using-package-configuration-file"></a>Uso del archivo de configuración de paquetes
Si usa archivos de configuración de paquetes en el modelo de implementación de paquetes para cambiar los valores de las variables en tiempo de ejecución, esos archivos se cargarán automáticamente con los paquetes en Azure Files para su ejecución en Azure-SSIS Integration Runtime.

### <a name="using-execute-package-task"></a>Usar la tarea Ejecutar paquete
Si los paquetes contienen tareas Ejecutar paquete que hacen referencia a otros paquetes almacenados en sistemas de archivos locales, es preciso realizar las siguientes configuraciones adicionales:

1. Cargue los restantes paquetes en Azure Files, en la misma cuenta de Azure Storage que está conectada a los proyectos y obtenga su nueva ruta de acceso UNC, por ejemplo, `\\test.file.core.windows.net\ssdtexecution\Package1.dtsx`

2. Reemplace la ruta de acceso de archivo de los restantes paquetes en el administrador de conexiones de archivos de las tareas Ejecutar paquete por su nueva ruta de acceso UNC
   - Si el equipo que ejecuta SSDT no puede acceder a la nueva ruta de acceso UNC, puede cambiar la ruta de acceso de archivo en el panel Propiedades del administrador de conexiones de archivos
   - Como alternativa, puede usar una variable para la ruta de acceso de archivo para asignar el valor correcto en tiempo de ejecución

Si los paquetes contienen tareas Ejecutar paquete que hacen referencia a otros paquetes del mismo proyecto, no es necesario realizar ninguna configuración adicional.

## <a name="next-steps"></a>Pasos siguientes
Una vez que esté satisfecho con la ejecución de los paquetes en Azure desde SSDT, puede implementarlos y ejecutarlos como actividades de Ejecutar paquetes SSIS en las canalizaciones de ADF, consulte [Ejecución de paquetes de SSIS como actividades de Ejecutar paquete SSIS en canalizaciones de ADF](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).
