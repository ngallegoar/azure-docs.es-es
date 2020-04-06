---
title: Personalización de la instalación en una instancia de Azure-SSIS Integration Runtime
description: En este artículo se describe cómo usar la interfaz de instalación personalizada en una instancia de Azure-SSIS Integration Runtime para instalar componentes adicionales o cambiar los valores de configuración.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 03/27/2020
ms.openlocfilehash: d6252b7a0ecce553bc3a1519055375fd4cd034f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336209"
---
# <a name="customize-the-setup-for-an-azure-ssis-integration-runtime"></a>Personalización de la instalación en una instancia de Azure-SSIS Integration Runtime

La instalación personalizada en una instancia del entorno de ejecución de integración de Azure SQL Server Integration Services (Azure-SSIS-R) ofrece una interfaz para que agregue sus propios pasos durante la instalación o la reconfiguración de dicha instancia. 

Mediante la instalación personalizada, puede modificar la configuración o el entorno de funcionamiento predeterminados; por ejemplo, iniciar servicios de Windows adicionales o conservar las credenciales de acceso para los recursos compartidos de archivos o usar el protocolo de red de alta seguridad y cifrado seguro (TLS 1.2). También, puede instalar componentes adicionales, como ensamblados, controladores o extensiones, en cada nodo de Azure-SSIS IR.

Puede realizar instalaciones personalizadas en la instancia de Azure-SSIS IR de dos maneras: 
* **Instalación personalizada rápida sin un script**: ejecute algunas configuraciones del sistema y comandos de Windows comunes o instale algunos componentes conocidos o recomendados sin usar ningún script.
* **Instalación personalizada estándar con un script**: prepare un script y sus archivos asociados y cárguelos juntos en un contenedor de blobs en la cuenta de Azure Storage. Luego, debe proporcionar un identificador uniforme de recursos (URI) de firma de acceso compartido (SAS) para el contenedor al instalar o volver a configurar su instancia de Azure-SSIS IR. Cada nodo de la instancia de Azure-SSIS-IR descarga entonces el script y sus archivos asociados del contenedor y ejecuta la instalación personalizada con privilegios elevados. Una vez finalizada la instalación personalizada, cada nodo carga la salida estándar de la ejecución y otros registros en el contenedor.

Puede instalar componentes gratuitos sin licencia y componentes de pago con licencia con instalaciones personalizadas rápidas y estándar. Si es fabricante de software independiente (ISV), consulte [Desarrollo de componentes con licencia o de pago para Azure-SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md).

> [!IMPORTANT]
> Como los nodos de la serie v2 de una instancia de Azure-SSIS IR no son adecuados para la instalación personalizada, use en su lugar nodos de la serie v3. Si ya usa nodos de la serie v2, pase a los de la serie v3 tan pronto como sea posible.

## <a name="current-limitations"></a>Limitaciones actuales

Las siguientes limitaciones solo se aplican a instalaciones personalizadas estándar:

- Si quiere usar *gacutil.exe* en el script para instalar ensamblados en la caché global de ensamblados (GAC), deberá proporcionar *gacutil.exe* como parte de su instalación personalizada. También, puede usar la copia proporcionada en nuestro contenedor *Versión preliminar pública*, descrito anteriormente en la sección "Instrucciones".

- Si quiere hacer referencia a una subcarpeta del script, *msiexec.exe* no admite la notación `.\` para hacer referencia a la carpeta raíz. Use un comando como `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` en lugar de `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

- Los recursos compartidos administrativos o los recursos compartidos de red ocultos creados automáticamente por Windows no se admiten actualmente en Azure-SSIS IR.

- El controlador ODBC de IBM iSeries Access no se admite en Azure-SSIS IR. Puede que vea errores de instalación durante la instalación personalizada. Si este es el caso, póngase en contacto con el servicio de soporte técnico de IBM para obtener ayuda.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para personalizar Azure-SSIS IR, necesita los siguientes elementos:

- [Una suscripción de Azure](https://azure.microsoft.com/)

- [Aprovisionamiento del entorno de ejecución de integración de Azure-SSIS](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

- Una [cuenta de almacenamiento de Azure](https://azure.microsoft.com/services/storage/). No se necesita para las instalaciones rápidas personalizadas. Para instalaciones estándar personalizadas, cargue y almacene el script de la instalación personalizada y sus archivos asociados en un contenedor de blobs. El proceso de instalación personalizada también carga sus registros de ejecución en el mismo contenedor de blobs.

## <a name="instructions"></a>Instructions

1. Si quiere instalar o volver a configurar su instancia de Azure-SSIS IR con PowerShell, descargue e instale [Azure PowerShell](/powershell/azure/install-az-ps). En el caso de instalaciones rápidas personalizadas, vaya al paso 4.

1. Prepare el script de instalación personalizada y sus archivos asociados (por ejemplo, archivos .bat, .cmd, .exe, .dll, .msi o. ps1).

   * Debe tener un archivo de script denominado *main.cmd*, que es el punto de entrada de la instalación personalizada.  
   * Para asegurarse de que el script se puede ejecutar en modo silencioso, se recomienda probarlo primero en la máquina local.  
   * Si quiere que se carguen en el contenedor registros adicionales generados mediante otras herramientas (por ejemplo, *msiexec.exe*), especifique la variable de entorno predefinida, `CUSTOM_SETUP_SCRIPT_LOG_DIR`, como la carpeta de registros de los scripts (por ejemplo, *msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log*).

1. Descargue, instale y abra el [Explorador de Azure Storage](https://storageexplorer.com/). Para ello:

   a. En **(Local and Attached)** (Local y asociada), haga clic con el botón derecho en **Cuentas de almacenamiento** y seleccione **Conectar a Azure Storage**.

      ![Conectar a Azure Storage](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   b. Seleccione **Usar un nombre y clave de la cuenta de almacenamiento** y, a continuación, seleccione **Siguiente**.

      ![Usar un nombre y clave de la cuenta de almacenamiento](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   c. Escriba el nombre y la clave de la cuenta de Azure Storage, seleccione **Siguiente** y, luego, seleccione **Conectar**.

      ![Proporcionar el nombre y la clave de la cuenta de almacenamiento](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   d. En la cuenta conectada de Azure Storage, haga clic con el botón derecho en **Contenedores de blobs**, seleccione **Crear contenedor de blobs** y ponga un nombre al nuevo contenedor.

      ![Creación de un contenedor de blobs](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   e. Seleccione el nuevo contenedor y cargue el script de instalación personalizada y sus archivos asociados. Asegúrese de cargar *main.cmd* en el nivel superior del contenedor, no en una carpeta. Asegúrese también de que el contenedor incluye solo los archivos necesarios de la instalación personalizada, para que la posterior descarga en Azure-SSIS IR no tarde demasiado. La duración máxima de una instalación personalizada se establece actualmente en 45 minutos antes de que se agote el tiempo de espera. Esto incluye el tiempo para descargar todos los archivos del contenedor e instalarlos en Azure-SSIS IR. Si la instalación requiere más tiempo, genere una incidencia de soporte técnico.

      ![Carga de archivos en el contenedor de blobs](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   f. Haga clic con el botón derecho en el contenedor y seleccione **Obtener firma de acceso compartido**.

      ![Obtención de una firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   g. Cree el URI de SAS para el contenedor con un tempo de expiración suficientemente largo y con permisos de lectura, escritura y enumeración. Lo necesitará para descargar y ejecutar el script de instalación personalizada y sus archivos asociados siempre que restablezca la imagen o reinicie cualquier nodo de su instancia de Azure-SSIS IR. Se necesita permiso de escritura para cargar los registros de ejecución del programa de instalación.

      > [!IMPORTANT]
      > Asegúrese de que el URI de SAS no expira y que los recursos de instalación personalizada están siempre disponibles durante todo el ciclo de vida de Azure-SSIS IR, desde la creación a la eliminación, especialmente si lo detiene e inicia periódicamente durante este tiempo.

      ![Generación de la firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   h. Copie y guarde el URI de SAS del contenedor.

      ![Copia y guardado de la firma de acceso compartido](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

1. Al instalar o volver a configurar su instancia de Azure-SSIS IR con una interfaz de usuario de factoría de datos, puede agregar o quitar instalaciones personalizadas mediante la selección de la casilla **Customize your Azure-SSIS Integration Runtime with additional system configurations/component installations** (Personalizar Azure-SSIS Integration Runtime con configuraciones de sistema/instalación de componentes adicionales) de la sección **Configuración avanzada** del panel **Integration runtime setup** (Configuración de Integration Runtime). 

   Si quiere agregar instalaciones personalizadas estándar, escriba el URI de SAS del contenedor en el cuadro **Custom setup container SAS URI** (URI de SAS del contenedor de configuración personalizada). 
   
   Si quiere agregar instalaciones personalizadas rápidas, seleccione **Nueva** para abrir el panel **Add express custom setup** (Agregar instalación personalizada rápida) y luego seleccione un tipo en la lista desplegable **Express custom setup type** (Tipo de instalación personalizada rápida):

   * Si selecciona el tipo **Run cmdkey command** (Ejecutar comando cmdkey), puede conservar las credenciales de acceso de los recursos compartidos de archivos o de los recursos compartidos de Azure Files en Azure-SSIS IR mediante la especificación del nombre de equipo o dominio de destino, el nombre de cuenta o el nombre de usuario y la clave de cuenta o la contraseña en los campos **/Add** (/Agregar), **/User** (/Usuario) y **/Pass** (/Pasar), respectivamente. Esta acción es similar a ejecutar el comando [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) de Windows en la máquina local.
   
   * Si selecciona el tipo **Add environment variable** (Agregar variable de entorno), puede agregar las variables de entorno de Windows que se van a usar en los paquetes que se ejecutan en Azure-SSIS IR mediante la especificación del nombre y el valor de la variable de entorno en los campos **Nombre de la variable** y **Valor de la variable**, respectivamente. Esta acción es similar a ejecutar el comando [set](https://docs.microsoft.com/windows-server/administration/windows-commands/set_1) de Windows en la máquina local.

   * Si selecciona el tipo **Install licensed component** (Instalar componente con licencia), puede seleccionar los componentes integrados de nuestros asociados de ISV en la lista desplegable **Nombre del componente**:

     * Si selecciona el componente **SentryOne's Task Factory** (Generador de tareas de SentryOne), puede instalar el conjunto de componentes de [Task Factory](https://www.sentryone.com/products/task-factory/high-performance-ssis-components) (Generador de tareas) desde SentryOne en su instancia de Azure-SSIS IR mediante la especificación de la clave de licencia del producto que compró en el campo **Clave de licencia**. La versión integrada actual es la **2019.4.3**.

     * Si selecciona el componente **oh22's HEDDA.IO** (HEDDA.IO de oh22), puede instalar el componente de calidad y limpieza de datos [HEDDA.IO](https://hedda.io/ssis-component/) de oh22 en su Azure-SSIS IR después de adquirir su servicio. La versión integrada actual es la **1.0.13**.

     * Si selecciona el componente **oh22's SQLPhonetics.NET** (SQLPhonetics.NET de oh22), puede instalar el componente de calidad y coincidencia de datos [SQLPhonetics.NET](https://sqlphonetics.oh22.is/sqlphonetics-net-for-microsoft-ssis/) desde oh22 en su instancia de Azure-SSIS IR mediante la especificación de la clave de licencia del producto que compró en el campo **Clave de licencia**. La versión integrada actual es la **1.0.43**.

     * Si selecciona el componente**SSIS Integration Toolkit de KingswaySoft**, puede instalar el conjunto de conectores de [SSIS Integration Toolkit](https://www.kingswaysoft.com/products/ssis-integration-toolkit-for-microsoft-dynamics-365) para aplicaciones de CRM/ERP/marketing/colaboración, como Microsoft Dynamics/SharePoint/Project Server, Oracle/Salesforce Marketing Cloud, etc., desde KingswaySoft en su instancia de Azure-SSIS IR, escribiendo la clave de licencia del producto que compró en el cuadro **Clave de licencia**. La versión integrada actual es la **2019.2**.

     * Si selecciona el componente **SSIS Productivity Pack de KingswaySoft**, puede instalar el conjunto de componentes de [SSIS Productivity Pack ](https://www.kingswaysoft.com/products/ssis-productivity-pack) desde KingswaySoft en su instancia de Azure-SSIS IR, escribiendo la clave de licencia del producto que compró en el campo **Clave de licencia**. La versión integrada actual es la **10.0**.

     * Si selecciona el componente **Xtract IS de Theobald Software**, puede instalar el conjunto [Xtract IS](https://theobald-software.com/en/xtract-is/) de conectores para el sistema SAP (ERP, S/4HANA, BW) desde Theobald Software en Azure-SSIS IR; para ello, arrastre y suelte o cargue el archivo de licencia del producto que compró en el cuadro del **archivo de licencia**. La versión integrada actual es la **6.1.1.3**.

   Las instalaciones personalizadas rápidas agregadas aparecerán en la sección **Configuración avanzada**. Para quitarlas, active sus casillas y, luego, seleccione **Eliminar**.

   ![Configuración avanzada con instalaciones personalizadas](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)

1. Al instalar o volver a configurar su instancia de Azure-SSIS IR con PowerShell, puede agregar o quitar las instalaciones personalizadas mediante la ejecución del cmdlet `Set-AzDataFactoryV2IntegrationRuntime` antes de iniciar Azure-SSIS IR.
   
   ```powershell
   $ResourceGroupName = "[your Azure resource group name]"
   $DataFactoryName = "[your data factory name]"
   $AzureSSISName = "[your Azure-SSIS IR name]"
   # Custom setup info: Standard/express custom setups
   $SetupScriptContainerSasUri = "" # OPTIONAL to provide a SAS URI of blob container for standard custom setup where your script and its associated files are stored
   $ExpressCustomSetup = "[RunCmdkey|SetEnvironmentVariable|SentryOne.TaskFactory|oh22is.SQLPhonetics.NET|oh22is.HEDDA.IO|KingswaySoft.IntegrationToolkit|KingswaySoft.ProductivityPack|Theobald.XtractIS or leave it empty]" # OPTIONAL to configure an express custom setup without script

   # Add custom setup parameters if you use standard/express custom setups
   if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
   {
       Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
           -DataFactoryName $DataFactoryName `
           -Name $AzureSSISName `
           -SetupScriptContainerSasUri $SetupScriptContainerSasUri
   }
   if(![string]::IsNullOrEmpty($ExpressCustomSetup))
   {
       if($ExpressCustomSetup -eq "RunCmdkey")
       {
           $addCmdkeyArgument = "YourFileShareServerName or YourAzureStorageAccountName.file.core.windows.net"
           $userCmdkeyArgument = "YourDomainName\YourUsername or azure\YourAzureStorageAccountName"
           $passCmdkeyArgument = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourPassword or YourAccessKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.CmdkeySetup($addCmdkeyArgument, $userCmdkeyArgument, $passCmdkeyArgument)
       }
       if($ExpressCustomSetup -eq "SetEnvironmentVariable")
       {
           $variableName = "YourVariableName"
           $variableValue = "YourVariableValue"
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.EnvironmentVariableSetup($variableName, $variableValue)
       }
       if($ExpressCustomSetup -eq "SentryOne.TaskFactory")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       if($ExpressCustomSetup -eq "oh22is.SQLPhonetics.NET")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       if($ExpressCustomSetup -eq "oh22is.HEDDA.IO")
       {
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup)
       }
       if($ExpressCustomSetup -eq "KingswaySoft.IntegrationToolkit")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       if($ExpressCustomSetup -eq "KingswaySoft.ProductivityPack")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }    
       if($ExpressCustomSetup -eq "Theobald.XtractIS")
       {
           $jsonData = Get-Content -Raw -Path YourLicenseFile.json
           $jsonData = $jsonData -replace '\s',''
           $jsonData = $jsonData.replace('"','\"')
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString($jsonData)
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       # Create an array of one or more express custom setups
       $setups = New-Object System.Collections.ArrayList
       $setups.Add($setup)

       Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
           -DataFactoryName $DataFactoryName `
           -Name $AzureSSISName `
           -ExpressCustomSetup $setups
   }
   Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
       -DataFactoryName $DataFactoryName `
       -Name $AzureSSISName `
       -Force
   ```
   
   Después de que finalice la instalación personalizada estándar y se inicie Azure-SSIS IR, podrá encontrar la salida estándar de *main.cmd* y otros registros de ejecución en la carpeta *main.cmd.log* del contenedor de almacenamiento.

1. Para ver algunos ejemplos de instalaciones personalizadas estándar, conéctese a nuestro contenedor Versión preliminar pública con el Explorador de Azure Storage.

   a. En **(Local and Attached)** (Local y asociada), haga clic con el botón derecho en **Cuentas de almacenamiento**, seleccione **Conectar a Azure Storage**, luego **Use a connection string or a shared access signature URI** (Use una cadena de conexión o un URI de firma de acceso compartido) y finalmente **Siguiente**.

      ![Conexión a Azure Storage con la firma de acceso compartido](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   b. Seleccione **Use a SAS URI** (Usar un URI de SAS) y, luego, en el cuadro **URI**, escriba el siguiente:

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

      ![Provisión de la firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   c. Seleccione **Siguiente** y después **Conectar**.

   d. En el panel izquierdo, seleccione el contenedor **publicpreview** conectado y, luego, haga doble clic en la carpeta *CustomSetupScript*. En esta carpeta se encuentran los siguientes elementos:

      * Una carpeta *Sample*, que contiene una instalación personalizada para instalar una tarea básica en cada nodo de la instancia de Azure-SSIS IR. La tarea no hace nada, pero se queda en suspensión durante unos segundos. La carpeta también contiene una carpeta *gacutil*, cuyo contenido completo (*gacutil.exe*, *gacutil.exe.config* y *1033\gacutlrc.dll*) se puede copiar tal cual en el contenedor.

      * Una carpeta *UserScenarios*, que contiene varios ejemplos de instalaciones personalizadas de escenarios de usuario reales.

        ![Contenido del contenedor de versión preliminar pública](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   e. Haga doble clic en la carpeta *UserScenarios* para buscar los elementos siguientes:

      * Una carpeta *.NET FRAMEWORK 3.5*, que contiene un script de instalación personalizada (*main.cmd*) para instalar una versión anterior de .NET Framework que puede ser necesaria para los componentes personalizados de cada nodo de la instancia de Azure-SSIS IR.

      * Una carpeta *BCP*, que contiene un script de instalación personalizada (*main.cmd*) para instalar utilidades de línea de comandos de SQL Server (*MsSqlCmdLnUtils.msi*), incluido el programa de copia masiva (*bcp*), en cada nodo de Azure-SSIS IR.

      * Una carpeta *EXCEL*, que contiene un script de instalación personalizada (*main.cmd*) para instalar ensamblados o bibliotecas de C# que puede usar en tareas de script para realizar operaciones de lectura o escritura dinámicamente en archivos de Excel en cada nodo de su instancia de Azure-SSIS IR. 
      
        Primero, descargue [*ExcelDataReader.dll*](https://www.nuget.org/packages/ExcelDataReader/) y [*DocumentFormat.OpenXml.dll*](https://www.nuget.org/packages/DocumentFormat.OpenXml/) y, luego, cárguelos todos junto con *main.cmd* en el contenedor. Como alternativa, si solo quiere usar el Administrador de conexiones, el origen y el destino de Excel estándar, el acceso redistribuible necesario ya está preinstalado en su instancia de Azure-SSIS IR, por lo que no necesita ninguna instalación personalizada.
      
      * Una carpeta *MYSQL ODBC*, que contiene un script de instalación personalizada (*main.cmd*) para instalar los controladores ODBC de MySQL en cada nodo de Azure-SSIS IR. Esta instalación permite usar el Administrador de conexiones, el origen y el destino de ODBC para conectarse al servidor de MySQL. 
     
        Primero, [descargue las versiones más recientes de 64 y 32 bits de los instaladores de controladores ODBC de MySQL](https://dev.mysql.com/downloads/connector/odbc/) (por ejemplo, *mysql-connector-odbc-8.0.13-winx64.msi* y *mysql-connector-odbc-8.0.13-win32.msi*) y, luego, cárguelas todas junto con *main.cmd* en el contenedor.

      * Una carpeta *ORACLE ENTERPRISE*, que contiene un script de instalación personalizada (*main.cmd*) y el archivo de configuración de instalación silenciosa (*client.rsp*) para instalar los conectores de Oracle y el controlador de OCI en cada nodo de Enterprise Edition de Azure-SSIS IR. Esta instalación permite usar el Administrador de conexiones, el origen y el destino de Oracle para conectarse al servidor de Oracle. 
      
        Primero, descargue Microsoft Connectors v5.0 para Oracle (*AttunitySSISOraAdaptersSetup.msi* y *AttunitySSISOraAdaptersSetup64.msi*) del [Centro de descarga de Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=55179) y el cliente más reciente de Oracle (por ejemplo, *winx64_12102_client.zip*) de [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) y, luego, cárguelos todos junto con *main.cmd* y *client.rsp* en el contenedor. Si usa TNS para conectarse a Oracle, también debe descargar *tnsnames.ora*, editarlo y cargarlo en el contenedor, para que se pueda copiar en la carpeta de instalación de Oracle durante el proceso de instalación.

      * Una carpeta *ORACLE STANDARD ADO.NET*, que contiene un script de instalación personalizada (*main.cmd*) para instalar el controlador ODP.NET de Oracle en cada nodo de Azure-SSIS-IR. Esta instalación permite usar el Administrador de conexiones, el origen y el destino de ADO.NET para conectarse al servidor de Oracle. 
      
        Primero, [descargue el controlador ODP.NET más reciente de Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html) (por ejemplo, *ODP.NET_Managed_ODAC122cR1.zip*) y, luego, cárguelo junto con *main.cmd* en el contenedor.
       
      * Una carpeta *ORACLE STANDARD ODBC*, que contiene un script de instalación personalizada (*main.cmd*) para instalar el controlador ODBC de Oracle y configurar el nombre del origen de datos (DSN) en cada nodo de Azure-SSIS IR. Esta instalación le permite usar el Administrador de conexiones, el origen y el destino de ODBC, o el Administrador de conexiones y el origen de Power Query con el tipo de origen de datos ODBC para conectarse al servidor de Oracle. 
      
        Primero, descargue el cliente Oracle Instant más reciente (Basic Package o Basic Lite Package) y ODBC Package y cárguelos todos junto con *main.cmd* en el contenedor:
        * [Descargar paquetes de 64 bits](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (Basic Package: *instantclient-basic-windows.x64-18.3.0.0.0dbru.zip*; Basic Lite Package: *instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip*; ODBC Package: *instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip*) 
        * [Descargar paquetes de 32 bits](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (Basic Package: *instantclient-basic-nt-18.3.0.0.0dbru.zip*; Basic Lite Package: *instantclient-basiclite-nt-18.3.0.0.0dbru.zip*; ODBC Package: *instantclient-odbc-nt-18.3.0.0.0dbru.zip*)

      * Una carpeta *ORACLE STANDARD OLEDB*, que contiene un script de instalación personalizada (*main.cmd*) para instalar el controlador OLEDB de Oracle en cada nodo de la instancia de Azure-SSIS IR. Esta instalación permite usar el Administrador de conexiones, el origen y el destino de OLEDB para conectarse al servidor de Oracle. 
     
        Primero, [descargue el controlador OLEDB de Oracle más reciente](https://www.oracle.com/partners/campaign/index-090165.html) (por ejemplo, *ODAC122010Xcopy_x64.zip*) y, luego, cárguelo junto con *main.cmd* en el contenedor.

      * Una carpeta *POSTGRESQL ODBC*, que contiene un script de instalación personalizada (*main.cmd*) para instalar los controladores ODBC de PostgreSQL en cada nodo de Azure-SSIS IR. Esta instalación permite usar el Administrador de conexiones, el origen y el destino de ODBC para conectarse al servidor de PostgreSQL. 
     
        Primero, [descargue las versiones más recientes de 64 y 32 bits de los instaladores de controladores ODBC de PostgreSQL](https://www.postgresql.org/ftp/odbc/versions/msi/) (por ejemplo, *psqlodbc_x64.msi* y *psqlodbc_x86.msi*) y, luego, cárguelas todas junto con *main.cmd* en el contenedor.

      * Una carpeta *SAP BW*, que contiene un script de instalación personalizada (*main.cmd*) para instalar el ensamblado de conector .NET de SAP (*librfc32.dll*) en cada nodo de Enterprise Edition de Azure-SSIS IR. Esta instalación permite usar el Administrador de conexiones, el origen y el destino de SAP Business Warehouse (BW) para conectarse al servidor de SAP BW. 
      
        Primero, cargue la versión de 64 o 32 bits de *librfc32.dll* desde la carpeta de instalación de SAP junto con *main.cmd* en el contenedor. El script copia entonces el ensamblado de SAP en la carpeta *%windir%\SysWow64* o *%windir%\System32* durante la instalación.

      * Una carpeta *STORAGE*, que contiene un script de instalación personalizada (*main.cmd*) para instalar Azure PowerShell en cada nodo de Azure-SSIS IR. Esta instalación le permite implementar y ejecutar paquetes de SSIS que ejecutan [scripts de PowerShell para manipular la cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). 
      
        Copie *main.cmd*, un ejemplo de *AzurePowerShell.msi* (o use la versión más reciente) y *storage.ps1* en el contenedor. Use *PowerShell.dtsx* como plantilla para los paquetes. La plantilla de paquetes combina una [tarea de descarga de blobs de Azure](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), que descarga *storage.ps1* como un script de PowerShell modificable, y una [tarea de ejecución de proceso](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) que ejecuta el script en cada nodo.

      * Una carpeta *TERADATA*, que contiene un script de instalación personalizada (*main.cmd*), su archivo asociado (*install.cmd*) y los paquetes del instalador ( *.msi*). Estos archivos instalan los conectores de Teradata, la API Teradata Parallel Transporter (TPT) y el controlador ODBC en cada nodo de Enterprise Edition de Azure-SSIS Integration Runtime. Esta instalación permite usar el Administrador de conexiones, el origen y el destino de Teradata para conectarse al servidor de Teradata. 
      
        Primero, [descargue el archivo comprimido de herramientas y utilidades de Teradata 15.x](http://partnerintelligence.teradata.com) (por ejemplo, *TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip*) y, luego, cárguelo junto con los archivos *.cmd* y *.msi* mencionados anteriormente en el contenedor.

      * Una carpeta *TLS 1.2*, que contiene un script de instalación personalizada (*main.cmd*) para usar un protocolo de red de alta seguridad y cifrado seguro (TLS 1.2), y deshabilitar versiones anteriores de SSL/TLS en cada nodo de su instancia de Azure-SSIS IR.

      * Una carpeta *ZULU OPENJDK*, que contiene un script de instalación personalizada (*main.cmd*) y un archivo de PowerShell (*install_openjdk.ps1*) para instalar Zulu OpenJDK en cada nodo de Azure-SSIS IR. Esta instalación permite usar conectores de Azure Data Lake Store o Flexible File para procesar archivos ORC o Parquet. Para más información, consulte [Azure Feature Pack para Integration Services](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-ver15#dependency-on-java). 
      
        Primero, [descargue el paquete Zulu OpenJDK más reciente](https://www.azul.com/downloads/zulu/zulu-windows/) (por ejemplo, *zulu8.33.0.1-jdk8.0.192-win_x64.zip*) y, luego, cárguelo junto con *main.cmd* y *install_openjdk.ps1* en el contenedor.

        ![Carpetas en la carpeta de escenarios de usuario](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   f. Para probar estos ejemplos de instalación personalizada, copie el contenido de la carpeta seleccionada en el contenedor.
   
      Al instalar o volver a configurar Azure-SSIS IR con la interfaz de usuario de Data Factory, active la casilla **Customize your Azure-SSIS Integration Runtime with additional system configurations/component installations** (Personalizar su Azure-SSIS IR con configuraciones del sistema e instalaciones de componentes adicionales) en la sección **Configuración avanzada** y escriba el URI de SAS del contenedor en el campo **Custom setup container SAS URI** (URI de SAS del contenedor de instalación personalizada).
   
      Al instalar o volver a configurar Azure-SSIS-IR con PowerShell, ejecute el cmdlet `Set-AzDataFactoryV2IntegrationRuntime` con el URI de SAS del contenedor como valor del parámetro `SetupScriptContainerSasUri`.

## <a name="next-steps"></a>Pasos siguientes

- [Instalación de Enterprise Edition de Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)
- [Desarrollo de componentes personalizados de pago o con licencia para Azure-SSIS Integration Runtime](how-to-develop-azure-ssis-ir-licensed-components.md)