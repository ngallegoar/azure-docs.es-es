---
title: Configuración de un entorno de ejecución de integración autohospedado como proxy para SSIS
description: Aprenda a configurar un entorno de ejecución de integración autohospedado como proxy para Azure-SSIS Integration Runtime.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 04/15/2020
ms.openlocfilehash: 4cb5b84f3889dcf4e0f28d525afb42cfeac5b54c
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605491"
---
# <a name="configure-a-self-hosted-ir-as-a-proxy-for-an-azure-ssis-ir-in-azure-data-factory"></a>Configuración de IR autohospedado como proxy para Azure-SSIS IR en Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe cómo ejecutar paquetes de SQL Server Integration Services (SSIS) en Azure-SSIS Integration Runtime (IR) en Azure Data Factory (ADF) con un entorno de ejecución de integración autohospedado (IR autohospedado) configurado como proxy. 

Con esta característica, puede acceder a los datos locales sin tener que [unir la instancia de Azure-SSIS IR a una red virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). La característica resulta útil cuando la configuración de la red corporativa es demasiado compleja o una directiva es demasiado restrictiva para que pueda insertar la instancia de Azure-SSIS IR en ella.

Esta característica divide cualquier tarea de flujo de datos de SSIS que tenga un origen de datos local en dos tareas de almacenamiento provisional: 
* La primera tarea, que se ejecuta en IR autohospedado, primero mueve los datos desde el origen de datos local a un área de almacenamiento provisional de Azure Blob Storage.
* La segunda tarea, que se ejecuta en la instancia de Azure-SSIS IR, mueve los datos del área de almacenamiento provisional al destino de los datos previsto.

Otras ventajas y funcionalidades de esta característica permiten, por ejemplo, configurar IR autohospedado en regiones que Azure-SSIS IR todavía no admite y permitir la dirección IP estática pública de la instancia del entorno de ejecución de integración autohospedado en el firewall de los orígenes de datos.

## <a name="prepare-the-self-hosted-ir"></a>Preparación de IR autohospedado

Para usar esta característica, primero debe crear una factoría de datos y configurar una instancia de Azure-SSIS IR en ella. Si todavía no lo ha hecho, siga las instrucciones de [Configuración de un entorno de Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure).

A continuación, configure la instancia de IR autohospedado en la misma factoría de datos en la que está configurada la instancia de Azure-SSIS IR. Para ello, consulte [Creación de un entorno de ejecución de integración autohospedado](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime).

Por último, descargue e instale la versión más reciente de IR autohospedado, así como los controladores y el entorno de ejecución adicionales, en la máquina virtual de Azure o en el equipo local, tal como se indica a continuación:
- Descargue e instale la versión más reciente de [IR autohospedado](https://www.microsoft.com/download/details.aspx?id=39717).
- Si usa conectores de vinculación e incrustación de objetos (OLEDB) en los paquetes, descargue e instale los controladores OLEDB apropiados en el mismo equipo donde está instalada la instancia de IR autohospedado, si aún no lo ha hecho.  

  Si usa la versión anterior del controlador OLEDB para SQL Server (SQL Server Native Client [SQLNCLI]), [descargue la versión de 64 bits](https://www.microsoft.com/download/details.aspx?id=50402).  

  Si usa la versión más reciente del controlador OLEDB para SQL Server (MSOLEDBSQL), [descargue la versión de 64 bits](https://www.microsoft.com/download/details.aspx?id=56730).  
  
  Si usa controladores OLEDB para otros sistemas de base de datos, como PostgreSQL, MySQL, Oracle, etc., puede descargar las versiones de 64 bits de sus sitios web.
- Si aún no lo ha hecho, [descargue e instale la versión de 64 bits del entorno de ejecución de Visual C++ (VC)](https://www.microsoft.com/download/details.aspx?id=40784) en la misma máquina donde está instalada la instancia de IR autohospedado.

## <a name="prepare-the-azure-blob-storage-linked-service-for-staging"></a>Preparación del servicio vinculado de Azure Blob Storage para almacenamiento provisional

Si aún no lo ha hecho, cree un servicio vinculado de Azure Blob Storage en la misma factoría de datos en la que está configurada la instancia de Azure-SSIS IR. Para ello, consulte [Creación de un servicio vinculado de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service). Asegúrese de hacer lo siguiente:
- En **Almacén de datos**, seleccione **Azure Blob Storage**.  
- En **Connect via integration runtime** (Conectar mediante el entorno de ejecución de integración), seleccione **AutoResolveIntegrationRuntime** (no su Azure-SSIS IR ni su IR autohospedado), ya que usamos la instancia predeterminada de Azure IR para capturar las credenciales de acceso de su instancia de Azure Blob Storage.
- En **Método de autenticación**, seleccione **Clave de cuenta**, **SAS URI** (URI de SAS) o **Entidad de servicio**.  

    >[!TIP]
    >Si selecciona el método **Entidad de servicio**, conceda a la entidad de servicio al menos un rol  *Colaborador de datos de Storage Blob* . Para obtener más información, consulte [Conector de Azure Blob Storage](connector-azure-blob-storage.md#linked-service-properties).

![Preparación del servicio vinculado de Azure Blob Storage para almacenamiento provisional](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-an-azure-ssis-ir-with-your-self-hosted-ir-as-a-proxy"></a>Configuración de una instancia de Azure-SSIS IR con IR autohospedado como proxy

Cuando haya preparado el servicio vinculado de Azure Blob Storage e IR autohospedado para el almacenamiento provisional, puede configurar la instancia nueva o existente de Azure-SSIS IR con el entorno de ejecución de integración autohospedado como proxy en el portal o la aplicación de factoría. Sin embargo, antes de hacerlo, si la instancia de Azure-SSIS IR existente ya está en ejecución, deténgala y reiníciela después.

1. En el panel **Integration runtime setup** (Configuración del entorno de ejecución de integración), omita las secciones **Configuración general** y **Configuración de SQL** seleccionando **Siguiente**. 

1. En la sección **Configuración avanzada**, haga lo siguiente:

   1. Active la casilla **Set up Self-Hosted Integration Runtime as a proxy for your Azure-SSIS Integration Runtime** (Configurar el entorno de ejecución de integración autohospedado como proxy para su instancia de Azure-SSIS Integration Runtime). 

   1. En la lista desplegable **Self-Hosted Integration Runtime** (Integration Runtime autohospedado), seleccione la instancia existente de IR autohospedado como proxy para Azure-SSIS IR.

   1. En la lista desplegable **Staging Storage Linked Service** (Servicio vinculado de almacenamiento provisional), seleccione el servicio vinculado de Azure Blob Storage existente o cree uno para el almacenamiento provisional.

   1. En **Ruta de acceso provisional**, especifique un contenedor de blobs de la cuenta seleccionada de Azure Blob Storage o deje este campo vacío para usar uno predeterminado como almacenamiento provisional.

   1. Seleccione **Continuar**.

   ![Configuración avanzada con IR autohospedado](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

También puede configurar la instancia nueva o existente de Azure-SSIS IR con IR autohospedado como proxy mediante PowerShell.

```powershell
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$AzureSSISName = "[your Azure-SSIS IR name]"
# Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 

# Add self-hosted integration runtime parameters if you configure a proxy for on-premises data accesss
if(![string]::IsNullOrEmpty($DataProxyIntegrationRuntimeName) -and ![string]::IsNullOrEmpty($DataProxyStagingLinkedServiceName))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -DataProxyIntegrationRuntimeName $DataProxyIntegrationRuntimeName `
        -DataProxyStagingLinkedServiceName $DataProxyStagingLinkedServiceName

    if(![string]::IsNullOrEmpty($DataProxyStagingPath))
    {
        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -DataProxyStagingPath $DataProxyStagingPath
    }
}
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force
```

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>Habilitación de paquetes SSIS para conectarse por proxy

Mediante el uso de la extensión más reciente de SSDT con proyectos SSIS para Visual Studio o un instalador independiente, puede encontrar la nueva propiedad `ConnectByProxy` que se ha agregado en los administradores de conexiones de archivos planos u OLEDB.
* [Descargar la extensión de SSDT con proyectos SSIS para Visual Studio](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Descargar el instalador independiente](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)   

Al diseñar nuevos paquetes que contienen tareas Flujo de datos con orígenes de OLEDB o de archivo plano, que permiten tener acceso a bases de datos o archivos locales, puede habilitar esta propiedad mediante su establecimiento en *True* en el panel **Propiedades** de los administradores de conexiones correspondientes.

![Habilitación de la propiedad ConnectByProxy](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

También puede habilitar esta propiedad al ejecutar paquetes existentes sin tener que cambiarlos manualmente uno a uno.  Hay dos opciones:
- **Opción A**: Abrir, recompilar y volver a implementar el proyecto que contiene los paquetes con la extensión SSDT más reciente para ejecutarla en Azure-SSIS IR. A continuación, puede habilitar la propiedad estableciéndola en *True* para los administradores de conexiones pertinentes. Cuando ejecutan paquetes de SSMS, los administradores de conexiones aparecen en la pestaña **Administrador de conexiones** de la ventana emergente **Ejecutar paquete**.

  ![Habilitación de la propiedad ConnectByProxy 2](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  También puede habilitar la propiedad estableciéndola en *True* para los administradores de conexiones correspondientes que aparecen en la pestaña **Administradores de conexiones** de la actividad [Ejecutar paquete de SSIS](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) al ejecutar paquetes en las canalizaciones de Data Factory.
  
  ![Habilitación de la propiedad ConnectByProxy 3](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- **Opción B:** Volver a implementar el proyecto que contiene los paquetes que se van a ejecutar en el entorno de ejecución de integración de SSIS. Puede habilitar la propiedad después, proporcionando su ruta de acceso de propiedad, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`, y estableciéndola en *True* como una invalidación de propiedad en la pestaña **Avanzadas** de la ventana emergente **Ejecutar paquete** al ejecutar paquetes de SSMS.

  ![Habilitación de la propiedad ConnectByProxy 4](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  También puede habilitar la propiedad proporcionando la ruta de acceso de propiedad, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`, y estableciéndola en *True* como una invalidación de propiedad en la pestaña **Invalidaciones de propiedad** de la actividad [Ejecutar paquete de SSIS](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) al ejecutar paquetes en las canalizaciones de Data Factory.
  
  ![Habilitación de la propiedad ConnectByProxy 5](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-first-and-second-staging-tasks"></a>Depuración de la primera y segunda tareas de almacenamiento provisional

En la instancia de IR autohospedado, puede encontrar los registros del entorno de ejecución en la carpeta *C:\ProgramData\SSISTelemetry* y los registros de ejecución de las primeras tareas de almacenamiento provisional en la carpeta *C:\ProgramData\SSISTelemetry\ExecutionLog*.  Puede encontrar los registros de ejecución de las segundas tareas de almacenamiento provisional en la instancia de SSISDB o en las rutas de acceso de registro especificadas, en función de si se almacenan los paquetes en SSISDB o en el sistema de archivos, recursos compartidos de archivos o Azure Files. También puede encontrar los identificadores únicos de las primeras tareas de almacenamiento provisional en los registros de ejecución de las segundas tareas de almacenamiento provisional. 

![Identificador único de la primera tarea de almacenamiento provisional](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

## <a name="use-windows-authentication-in-staging-tasks"></a>Uso de la autenticación de Windows en tareas de almacenamiento provisional

Si las tareas de almacenamiento provisional de IR autohospedado requieren la autenticación de Windows, [configure los paquetes SSIS para que usen la misma autenticación de Windows](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth?view=sql-server-ver15). 

Las tareas de almacenamiento provisional se invocarán con la cuenta de servicio de IR autohospedado (*NT SERVICE\DIAHostService* de forma predeterminada) y se tendrá acceso a los almacenes de datos con la cuenta de autenticación de Windows. Ambas cuentas requieren que se les asignen determinadas directivas de seguridad. En el equipo de IR autohospedado, vaya a **Directiva de seguridad local** > **Directivas locales** > **Asignación de derechos de usuario** y haga lo siguiente:

1. Asigne las directivas *Ajustar las cuotas de la memoria para un proceso* y *Reemplazar un símbolo (token) de nivel de proceso* a la cuenta del servicio de IR autohospedado. Esto debe producirse automáticamente al instalar el IR autohospedado con la cuenta de servicio predeterminada. Si no es así, asigne esas directivas de forma manual. Si utiliza una cuenta de servicio diferente, asígnele las mismas directivas.

1. Asigne la directiva *Iniciar sesión como servicio* a la cuenta de autenticación de Windows.

## <a name="billing-for-the-first-and-second-staging-tasks"></a>Facturación de la primera y segunda tareas de almacenamiento provisional

Las primeras tareas de almacenamiento provisional que se ejecutan en la instancia de IR autohospedado se facturan por separado, de la misma forma que se facturan las actividades de movimiento de datos que se ejecutan en una instancia de IR autohospedado. Esto se especifica en el artículo [Azure Data Factory: Precios de las canalizaciones de datos](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/).

Las segundas tareas de almacenamiento provisional que se ejecutan en la instancia de Azure-SSIS IR no se facturan por separado, pero la instancia de Azure-SSIS IR en ejecución se factura tal como se especifica en el artículo sobre [Precios de Azure-SSIS IR](https://azure.microsoft.com/pricing/details/data-factory/ssis/).

## <a name="enabling-tls-12"></a>Habilitación de TLS 1.2

Si necesita usar un protocolo de red de alta seguridad y cifrado seguro (TLS 1.2) y deshabilitar las versiones anteriores de SSL/TLS en el entorno de ejecución de integración autohospedado, puede descargar y ejecutar el script *main.cmd* que se encuentra en la carpeta *CustomSetupScript/UserScenarios/TLS 1.2* del contenedor en versión preliminar pública.  Con el [Explorador de Azure Storage](https://storageexplorer.com/), puede conectarse al contenedor en versión preliminar pública; para ello, escriba el siguiente URI de SAS:

`https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

## <a name="current-limitations"></a>Limitaciones actuales

- Actualmente, solo se admiten las tareas de flujo de datos con orígenes de conectividad abierta de bases de datos (ODBC), OLEDB o archivo plano, o destino OLEDB. 
- Solo se admiten actualmente los servicios vinculados de Azure Blob Storage configurados con la autenticación de *clave de cuenta*, *URI de firma de acceso compartido (SAS)* o *entidad de servicio*.
- Todavía no se admite *ParameterMapping* en el origen de OLEDB. Como solución alternativa, use *Comando SQL de variable* como *AccessMode* y *Expresión* para insertar las variables o los parámetros en un comando SQL. Como ejemplo, vea el paquete *ParameterMappingSample.dtsx* que se puede encontrar en la carpeta *SelfHostedIRProxy/Limitations* del contenedor en versión preliminar pública. Con el Explorador de Azure Storage, puede conectarse al contenedor en versión preliminar pública especificando el URI de SAS anterior.

## <a name="next-steps"></a>Pasos siguientes

Después de configurar la instancia de IR autohospedado como proxy para Azure-SSIS IR, puede implementar y ejecutar los paquetes para acceder a los datos locales como actividades de ejecución de paquetes SSIS en las canalizaciones de Data Factory. Para obtener información de cómo hacerlo, consulte [Ejecución de un paquete de SSIS mediante la actividad Ejecutar paquete SSIS de Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).
