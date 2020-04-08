---
title: Incorporación de máquinas para su administración mediante Azure Automation State Configuration
description: Configuración de máquinas para su administración mediante Azure Automation State Configuration
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 12/10/2019
manager: carmonm
ms.openlocfilehash: 554a4c64700bb189b4b9f085bd7c259312a36b4b
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410930"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Incorporación de máquinas para su administración mediante Azure Automation State Configuration

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>¿Por qué administrar máquinas con Azure Automation State Configuration?

Azure Automation State Configuration es un servicio de administración de configuración para los nodos de Desired State Configuration (DSC) de cualquier centro de datos local o en la nube. Para acceder a este servicio desde Azure Portal, seleccione **State Configuration (DSC)** en **Administración de configuración**. 

Este servicio permite la escalabilidad en miles de máquinas de forma rápida y sencilla desde una ubicación central y segura. Puede incorporar fácilmente máquinas, asignarles configuraciones declarativas y ver informes que muestran el cumplimiento por parte de cada máquina del estado deseado que se especifique.

Azure Automation State Configuration es a DSC lo que los runbooks de Azure Automation son al scripting de PowerShell. En otras palabras, Azure Automation le ayuda a administrar las configuraciones de DSC, de la misma manera que lo hace con los scripts de PowerShell. Para más información acerca de las ventajas de usar Azure Automation State Configuration, consulte [Introducción a Azure Automation State Configuration](automation-dsc-overview.md).

Azure Automation State Configuration puede usarse para administrar diversas máquinas:

- Azure Virtual Machines
- Azure Virtual Machines (clásico)
- Máquinas físicas y virtuales con Windows locales o en una nube que no sea Azure (incluidas las instancias de AWS EC2)
- Máquinas físicas y virtuales con Linux locales, en Azure o en una nube que no sea Azure

Si no está preparado para administrar la configuración de las máquinas desde la nube, State Configuration de Azure Automation también se puede usar como punto de conexión meramente informativo. Esta característica permite establecer (insertar) configuraciones a través de DSC y ver detalles de los informes en Azure Automation.

> [!NOTE]
> La administración de máquinas virtuales de Azure con State Configuration de Azure Automation se incluye sin cargo extra alguno si la versión de la extensión Desired State Configuration de máquina virtual de Azure instalada es superior a 2.70. Consulte la [**página de precios de Automation**](https://azure.microsoft.com/pricing/details/automation/) para más información.

En las secciones siguientes de este artículo se describe cómo incorporar las máquinas a State Configuration de Azure Automation.

## <a name="onboarding-azure-vms"></a>Incorporación de máquinas virtuales de Azure

Con State Configuration de Azure Automation, se pueden incorporar fácilmente máquinas virtuales de Azure para la administración de configuraciones usando Azure Portal, plantillas de Azure Resource Manager o PowerShell. Internamente, y sin que un administrador tenga que acceder de forma remota a la máquina virtual, la extensión Desired State Configuration de máquina virtual de Azure registra la máquina virtual con State Configuration de Azure Automation. Como esta extensión de Azure se ejecuta de forma asincrónica, en la sección [Solución de problemas de incorporación de máquinas virtuales de Azure](#troubleshooting-azure-virtual-machine-onboarding) de este artículo se proporcionan los pasos para realizar un seguimiento de su progreso o solucionar problemas.

> [!NOTE]
>En la implementación de DSC en un nodo de Linux se usa la carpeta **/tmp**. Ciertos módulos, como `nxautomation`, se descargan temporalmente para su comprobación antes de instalarlos en las ubicaciones adecuadas correspondientes. Para garantizar que los módulos se instalan correctamente, el agente de Log Analytics para Linux necesita permiso de lectura y escritura para la carpeta **/tmp**.<br><br>
>El agente de Log Analytics para Linux se ejecuta como el usuario `omsagent`. Para conceder permiso de escritura al usuario `omsagent`, ejecute el comando `setfacl -m u:omsagent:rwx /tmp`.

### <a name="onboard-a-vm-using-azure-portal"></a>Incorporación de una máquina virtual con Azure Portal

Para incorporar una máquina virtual de Azure a State Configuration de Azure Automation a través de [Azure Portal](https://portal.azure.com/):

1. Vaya a la cuenta de Azure Automation a la que vaya a incorporar las máquinas virtuales. 

2. En la página de State Configuration, seleccione la pestaña **Nodos** y haga clic en **Agregar**.

3. Elija la máquina virtual que quiera incorporar.

4. Si la máquina no tiene instalada la extensión de estado de PowerShell deseada y el estado de energía es "en ejecución", haga clic en **Conectar**.

5. En **Registro**, escriba los [valores del Administrador de configuración local de DSC de PowerShell](/powershell/scripting/dsc/managing-nodes/metaConfig) necesarios para su caso de uso. Opcionalmente, se puede especificar una configuración de nodo para asignarla a la máquina virtual.

![incorporación](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="onboard-a-vm-using-azure-resource-manager-templates"></a>Incorporación de una máquina virtual con plantillas de Azure Resource Manager

Una máquina virtual se puede implementar e incorporar a State Configuration de Azure Automation con plantillas de Azure Resource Manager. Vea [Server managed by Desired State Configuration service](https://azure.microsoft.com/resources/templates/101-automation-configuration/) (Servidor administrado por el servicio Desired State Configuration) para obtener una plantilla de ejemplo que incorpora una máquina virtual existente a Azure Automation State Configuration. Si administra un conjunto de escalado de máquinas virtuales, vea la plantilla de ejemplo [Configuración de conjunto de escalado de máquinas virtuales administrada por Azure Automation](https://azure.microsoft.com/resources/templates/201-vmss-automation-dsc/).

### <a name="onboard-machines-using-powershell"></a>Incorporación de máquinas con PowerShell

Puede usar el cmdlet [Register-AzAutomationDscNode](/powershell/module/az.automation/register-azautomationdscnode) de PowerShell para incorporar máquinas virtuales a State Configuration de Azure Automation. 

> [!NOTE]
>Actualmente, el cmdlet `Register-AzAutomationDscNode` se implementa solo para equipos que ejecutan Windows, ya que únicamente desencadena la extensión de Windows.

### <a name="register-vms-across-azure-subscriptions"></a>Registro de máquinas virtuales en suscripciones de Azure

La mejor manera de registrar máquinas virtuales desde otras suscripciones de Azure consiste en usar la extensión DSC en una plantilla de implementación de Azure Resource Manager. Los ejemplos se proporcionan en [Extensión Desired State Configuration con plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template).

Para buscar la clave y la dirección URL de registro que se van a usar como parámetros en esta plantilla, vea la sección [Incorporación segura con un protocolo de registro](#onboarding-securely-using-registration) de este artículo.

## <a name="onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances"></a>Incorporación de máquinas físicas y virtuales Windows locales o en una nube distinta de Azure (incluidas instancias de AWS EC2)

Los servidores de Windows que se ejecutan de forma local o en otros entornos de nube se pueden incorporar a State Configuration de Azure Automation. Los servidores deben tener [acceso de salida a Azure](automation-dsc-overview.md#network-planning).

1. Asegúrese de que tiene instalada la versión más reciente de [WMF 5](https://aka.ms/wmf5latest) en las máquinas que vaya a incorporar a State Configuration de Azure Automation. Además, WMF 5 debe estar instalado en el equipo que esté usando para realizar la operación de incorporación.
1. Siga las indicaciones de la sección [Generación de metaconfiguraciones de DSC](#generating-dsc-metaconfigurations) para crear una carpeta que contenga las metaconfiguraciones de DSC necesarias. 
1. Use el siguiente cmdlet para aplicar las metaconfiguraciones de DSC de PowerShell de forma remota a las máquinas que quiera incorporar. 

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. Si no puede aplicar las metaconfiguraciones de DSC de PowerShell de forma remota, copie la carpeta de **metaconfiguraciones** del paso 2 en las máquinas que vaya a incorporar. Después, agregue código para llamar a `Set-DscLocalConfigurationManager` localmente en las máquinas.
1. Usando Azure Portal o cmdlets, compruebe que las máquinas que se van a incorporar aparecen ahora como nodos de State Configuration registrados en su cuenta de Azure Automation.

## <a name="onboarding-physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure"></a>Incorporación de máquinas físicas y virtuales Linux locales o en una nube distinta de Azure

Los servidores de Linux que se ejecutan de forma local o en otros entornos de nube se pueden incorporar a State Configuration de Azure Automation. Los servidores deben tener [acceso de salida a Azure](automation-dsc-overview.md#network-planning).

1. Asegúrese de que tiene instalada la versión más reciente d [PowerShell Desired State Configuration para Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) en las máquinas que quiera incorporar a State Configuration de Azure Automation.
2. Si los [valores predeterminados del Administrador de configuración local de DSC de PowerShell](/powershell/scripting/dsc/managing-nodes/metaConfig4) coinciden con su caso de uso, y quiere incorporar máquinas de modo que extraigan información de State Configuration de Azure Automation y también envíen informes allí:

   - En cada máquina Linux que vaya a incorporar a State Configuration de Azure Automation, use `Register.py` para incorporarla con los valores predeterminados del Administrador de configuración local de DSC de PowerShell.

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Para buscar la clave y la dirección URL de registro de su cuenta de Automation, vea la sección [Incorporación segura con un protocolo de registro](#onboarding-securely-using-registration) de este artículo.

3. Si los valores predeterminados del Administrador de configuración local de DSC de PowerShell no coinciden con el caso de uso, o si quiere incorporar máquinas para que solo notifiquen a State Configuration de Azure Automation, siga los pasos 4-7. De lo contrario, vaya directamente al paso 7.

4. Siga las indicaciones de la sección [Generación de metaconfiguraciones de DSC](#generating-dsc-metaconfigurations) para crear una carpeta que contenga las metaconfiguraciones de DSC necesarias.

5. Asegúrese de que tiene instalada la versión más reciente de [WMF 5](https://aka.ms/wmf5latest) en el equipo que esté usando para realizar la operación de incorporación.

6. Agregue el siguiente código para aplicar las metaconfiguraciones de DSC de PowerShell de forma remota a las máquinas que quiera incorporar.

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

7. Si no puede aplicar las metaconfiguraciones de DSC de PowerShell de forma remota, copie la metaconfiguración correspondiente a las máquinas remotas desde la carpeta en el paso 4 en las máquinas Linux.

8. Agregue código para llamar a `Set-DscLocalConfigurationManager.py` localmente en cada máquina Linux que vaya a incorporar a State Configuration de Azure Automation.

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

9. Usando Azure Portal o cmdlets, compruebe que las máquinas que va a incorporar ahora aparecen como nodos de DSC registrados en su cuenta de Azure Automation.

## <a name="generating-dsc-metaconfigurations"></a>Generación de metaconfiguraciones de DSC

Para incorporar cualquier máquina a State Configuration de Azure Automation, puede generar una [metaconfiguración de DSC](/powershell/scripting/dsc/managing-nodes/metaConfig). Esta configuración indica al agente de DSC que extraiga y/o informe a State Configuration de Azure Automation. Una metaconfiguración de State Configuration de Azure Automation se puede generar usando una configuración de DSC de PowerShell o con cmdlets de PowerShell de Azure Automation.

> [!NOTE]
> Las metaconfiguraciones de DSC contienen los secretos necesarios para incorporar una máquina a una cuenta de Automation para su administración. Asegúrese de proteger adecuadamente cualquier metaconfiguración de DSC que cree o elimínelos tras su uso.

La compatibilidad de proxy de las metaconfiguraciones se controla a través de LCM, que es el motor de DSC de Windows PowerShell. LCM se ejecuta en todos los nodos de destino, y es el responsable de llamar a los recursos de configuración que se incluyen en un script de metaconfiguración de DSC. Para incluir compatibilidad de proxy en una metaconfiguración, incluya definiciones de la dirección URL del proxy y la credencial del proxy según sea necesario en los bloques `ConfigurationRepositoryWeb`, `ResourceRepositoryWeb` y `ReportServerWeb`. Vea [Configuración del administrador de configuración local](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig?view=powershell-7).

### <a name="generate-dsc-metaconfigurations-using-a-dsc-configuration"></a>Generación de metaconfiguraciones de DSC con una configuración de DSC

1. Abra VSCode (o el editor que prefiera) como administrador en una máquina del entorno local. La máquina debe tener instalada la versión más reciente de [WMF 5](https://aka.ms/wmf5latest) .
1. Copie el siguiente script localmente. Este script contiene una configuración de DSC de PowerShell para crear metaconfiguraciones y un comando para iniciar la creación de la metaconfiguración.

    > [!NOTE]
    > Los nombres de configuración del nodo de State Configuration distinguen mayúsculas de minúsculas en Azure Portal. Si estas no coinciden, el nodo no se mostrará en la pestaña **Nodos**.

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl       = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. Rellene la clave del Registro y la dirección URL para su cuenta de Automation, así como los nombres de las máquinas que quiere incorporar. Todos los demás parámetros son opcionales. Para buscar la clave y la dirección URL de registro de su cuenta de Automation, vea la sección [Incorporación segura con un protocolo de registro](#onboarding-securely-using-registration).

1. Si quiere que las máquinas notifiquen la información de estado de DSC a State Configuration de Azure Automation, pero que no extraigan la configuración o módulos de PowerShell, establezca el parámetro `ReportOnly` en verdadero.

1. Si el parámetro `ReportOnly` no se establece, las máquinas notificarán la información de estado de DSC a State Configuration de Azure Automation y extraerán la configuración o módulos de PowerShell. Establezca los parámetros según corresponda en los bloques `ConfigurationRepositoryWeb`, `ResourceRepositoryWeb` y `ReportServerWeb`.

1. Ejecute el script. Ahora, debería tener una carpeta directorio llamada **DscMetaConfigs** en funcionamiento en el directorio de trabajo que contenga las metaconfiguraciones de DSC de PowerShell de las máquinas que quiera incorporar (como administrador).

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="generate-dsc-metaconfigurations-using-azure-automation-cmdlets"></a>Generación de metaconfiguraciones de DSC con cmdlets de Azure Automation

Si los valores predeterminados de LCM de DSC de PowerShell coinciden con su caso de uso, y quiere incorporar máquinas de modo que extraigan de State Configuration Azure Automation y también notifiquen allí, puede usar cmdlets de Azure Automation para generar las metaconfiguraciones de DSC necesarias de forma más sencilla.

1. Abra la consola de PowerShell o VSCode como administrador en una máquina de su entorno local.
2. Conéctese a Azure Resource Manager con `Connect-AzAccount`
3. Descargue las metaconfiguraciones de DSC de PowerShell de las máquinas que quiera incorporar desde la cuenta de Automation en la que esté configurando los nodos.

   ```powershell
   # Define the parameters for Get-AzAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation Account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzAutomationDscOnboardingMetaconfig @Params
   ```

1. Ahora, debería tener una carpeta llamada **DscMetaConfigs** que contenga las metaconfiguraciones de DSC de PowerShell de las máquinas que quiera incorporar (como administrador).

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="onboarding-securely-using-registration"></a>Incorporación segura con un protocolo de registro

Se pueden incorporar máquinas a una cuenta de Azure Automation de forma segura a través del protocolo de registro de DSC WMF 5. Este protocolo permite que un nodo de DSC se autentique en un servidor de informes o de extracción de DSC de PowerShell, incluido State Configuration de Azure Automation. El nodo se registra en el servidor en una dirección URL de registro y se autentica usando una clave de registro. Durante el registro, el nodo de DSC y el servidor de informes/extracción de DSC negocian un certificado único que este nodo usará para autenticarse en el servidor después del registro. Este proceso impide que los nodos incorporados se suplanten entre sí, por ejemplo, en el caso de que un nodo esté afectado y se comporte de forma malintencionada. Después del registro, la clave de registro no se vuelve a usar en operaciones de autenticación y se elimina del nodo.

Puede obtener la información necesaria para el protocolo de registro de State Configuration en **Claves** dentro de **Configuración de la cuenta** en Azure Portal. 

![Claves y dirección URL de Azure Automation](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- La dirección URL de registro es el campo de dirección URL en la página Claves.
- La clave de registro es el valor del campo **Clave de acceso primaria** o del campo **Clave de acceso secundaria** de la página Claves. Se puede usar cualquiera de las dos.

Para una mayor seguridad, las claves de acceso primaria y secundaria de una cuenta de Automation se pueden volver a generar en cualquier momento en la página Claves. Volver a generar las claves evita que se produzcan futuros registros de nodo en los que se usen las claves anteriores.

## <a name="re-registering-a-node"></a>Nuevo registro de un nodo

Después de registrar un equipo como un nodo de DSC en State Configuration de Azure Automation, existen diversos motivos por los que puede que sea necesario volver a registrar ese nodo en el futuro.

- **Renovación de certificados.** En las versiones de Windows Server anteriores a Windows Server 2019, cada nodo negocia automáticamente un certificado único para la autenticación que expira después de un año. Si un certificado expira sin renovarse, el nodo no puede comunicarse con Azure Automation y se marca como `Unresponsive`. En la actualidad, el protocolo de registro de DSC de PowerShell no puede renovar automáticamente los certificados que vayan a expirar, por lo que tendrá que volver a registrar los nodos transcurrido un año. Antes de volver a registrarlos, asegúrese de que cada nodo ejecuta WMF 5 RTM. 

    Si dicha actualización se realiza en un plazo de 90 días o menos desde la fecha de expiración del certificado o en cualquier momento después de dicha fecha, se generará y usará un certificado nuevo. En Windows Server 2019 y versiones posteriores se incluye una solución a este problema.

- **Cambios en los valores de LCM de DSC.** Puede que haya que cambiar los [valores de LCM de DSC de PowerShell](/powershell/scripting/dsc/managing-nodes/metaConfig4) establecidos durante el registro inicial del nodo, por ejemplo, `ConfigurationMode`. Actualmente, estos valores de agente DSC solo se pueden cambiar realizando un nuevo registro. La única excepción es el valor Configuración del nodo que el nodo tiene asignado. Esto se puede cambiar directamente en DSC de Azure Automation.

Un nodo se puede volver a registrar del mismo modo en que se registró inicialmente, con cualquiera de los métodos incorporados descritos en este documento. No es necesario anular el registro de un nodo desde Azure Automation State Configuration antes de volver a registrarlo.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Solución de problemas de incorporación de máquinas virtuales de Azure

Azure Automation State Configuration permite incorporar fácilmente máquinas virtuales de Azure con Windows para la administración de configuraciones. Internamente, la extensión Configuración de estado deseado de la máquina virtual de Azure se usa para registrar la máquina virtual con Azure Automation State Configuration. Puesto que la extensión Desired State Configuration de máquina virtual de Azure se ejecuta de forma asincrónica, puede ser importante realizar el seguimiento de su progreso y solucionar los problemas de ejecución.

> [!NOTE]
> Cualquier método para incorporar una máquina virtual de Azure con Windows a State Configuration de Azure Automation que use la extensión Desired State Configuration de máquina virtual de Azure podría tardar hasta una hora en mostrar el nodo como registrado en Azure Automation. Este retraso se debe a que la extensión Desired State Configuration de máquina virtual de Azure debe instalar WMF 5 en la máquina virtual como parte necesaria del proceso para incorporar la máquina virtual a State Configuration de Azure Automation.

Para solucionar problemas o ver el estado de la extensión Desired State Configuration de máquina virtual de Azure:

1. En Azure Portal, vaya a la máquina virtual que quiera a incorporar.
2. Haga clic en **Extensiones** en **Configuración**. 
3. Ahora, seleccione **DSC** o **DSCForLinux**, según el sistema operativo. 
4. Para obtener más detalles, puede hacer clic en **Ver estado detallado**.

Para más información sobre la solución de problemas, consulte [Solución de problemas con Desired State Configuration (DSC) de Azure Automation](./troubleshoot/desired-state-configuration.md).

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a trabajar, consulte [Introducción a State Configuration de Azure Automation](automation-dsc-getting-started.md).
- Para obtener información acerca de la compilación de configuraciones de DSC para poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones en State Configuration de Azure Automation](automation-dsc-compile.md).
- Para la referencia de cmdlets de PowerShell, consulte [Cmdlets de State Configuration de Azure Automation](/powershell/module/az.automation#automation).
- Para obtener información de precios, consulte [Precios de State Configuration de Azure Automation](https://azure.microsoft.com/pricing/details/automation/).
- Para obtener un ejemplo del uso de State Configuration de Azure Automation en una canalización de implementación continua, vea [Ejemplo de uso: implementación continua en máquinas virtuales con Azure Automation State Configuration y Chocolatey](automation-dsc-cd-chocolatey.md).
