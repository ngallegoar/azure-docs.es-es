---
title: Conexión de equipos Windows a Azure Monitor | Microsoft Docs
description: En este artículo se describe la manera de conectar equipos Windows hospedados en otras nubes o de forma local en Azure Monitor mediante el agente de Log Analytics de Windows.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/07/2019
ms.openlocfilehash: 70fa66a96291e0c2a638bf69bdce7da531d32bb7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80637465"
---
# <a name="connect-windows-computers-to-azure-monitor"></a>Conexión de equipos Windows a Azure Monitor

Con el fin de supervisar y administrar máquinas virtuales o equipos físicos en el centro de datos local o en otro entorno en la nube mediante Azure Monitor, debe implementar el agente de Log Analytics (también conocido como Microsoft Monitoring Agent (MMA)) y configurarlo para que informe a una o varias áreas de trabajo de Log Analytics. Asimismo, el agente admite el rol de Hybrid Runbook Worker de Azure Automation.  

En un equipo Windows supervisado, el agente aparece como el servicio Microsoft Monitoring Agent. El servicio Microsoft Monitoring Agent se encarga de recopilar eventos de archivos de registro y del registro de eventos de Windows, datos de rendimiento y otros datos de telemetría. Incluso cuando el agente no puede comunicarse con Azure Monitor al que debe informar, este continúa ejecutándose y pone en cola los datos recopilados en el disco del equipo supervisado. Cuando se restaura la conexión, el servicio Microsoft Monitoring Agent envía los datos recopilados al servicio.

El agente puede instalarse mediante uno de los métodos siguientes. La mayoría de las instalaciones usan una combinación de estos métodos para instalar los distintos conjuntos de equipos, según corresponda.  Más adelante en el artículo se proporcionan detalles sobre el uso de cada método.

* Instalación manual. El programa de instalación se ejecuta de forma manual en el equipo mediante el Asistente para la instalación, desde la línea de comandos, o bien se implementa mediante una herramienta de distribución de software existente.
* Desired State Configuration (DSC) de Azure Automation. Use DSC de Azure Automation con un script para equipos Windows que ya esté implementado en su entorno.  
* Script de PowerShell.
* Plantilla de Resource Manager para máquinas virtuales que ejecutan Windows de forma local en Azure Stack. 

>[!NOTE]
>Azure Security Center (ASC) depende de Microsoft Monitoring Agent (también denominado el agente de Windows de Log Analytics) y lo instalará y configurará para que informe a un área de trabajo de Log Analytics como parte de su implementación. ASC incluye una opción de aprovisionamiento automático que permite la instalación automática del agente de Windows de Log Analytics en todas las máquinas virtuales de su suscripción y lo configura para que informe a un área de trabajo específica. Para obtener más información acerca de esta opción, vea [Habilitación del aprovisionamiento automático del agente de Log Analytics](../../security-center/security-center-enable-data-collection.md#auto-provision-mma).
>

Si tiene que configurar el agente para que informe a varias áreas de trabajo, esto no puede realizarse durante la instalación inicial, solo después actualizando la configuración del Panel de control o PowerShell, tal como se describe en [Adición o eliminación de un área de trabajo](agent-manage.md#adding-or-removing-a-workspace).  

Para comprender la configuración compatible, revise los [sistemas operativos Windows admitidos](log-analytics-agent.md#supported-windows-operating-systems) y la [configuración del firewall de red](log-analytics-agent.md#network-requirements).

## <a name="obtain-workspace-id-and-key"></a>Obtención de la clave y el identificador de área de trabajo
Antes de instalar el agente Log Analytics para Windows, necesita la clave y el identificador de área de trabajo para el área de trabajo de Log Analytics.  Esta información es necesaria durante la configuración de cada método de instalación para configurar de forma correcta el agente y garantizar que puede comunicarse correctamente con Azure Monitor en la nube US Gov y comercial de Azure. 

1. En Azure Portal, busque y seleccione **Áreas de trabajo de Log Analytics**.
2. En la lista de áreas de trabajo de Log Analytics, seleccione el área de trabajo que quiera configurar para informar al agente.
3. Seleccione **Configuración avanzada**.<br><br> ![Configuración avanzada de Log Analytics](media/agent-windows/log-analytics-advanced-settings-01.png)<br><br>  
4. Seleccione **Orígenes conectados** y **Servidores Windows**.   
5. En su editor favorito, copie y pegue **Id. del área de trabajo** y **Clave principal**.    
   
## <a name="configure-agent-to-use-tls-12"></a>Configuración del agente para usar TLS 1.2
Para configurar el uso del protocolo [TLS 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12) para la comunicación entre el agente de Windows y el servicio Log Analytics, puede seguir los pasos siguientes para habilitarlo antes de que el agente se instale en la máquina virtual o después.

>[!NOTE]
>Si va a configurar una máquina virtual que ejecuta Windows Server 2008 SP2 x64 para usar TLS 1.2, debe instalar la siguiente [actualización de compatibilidad con la firma de código SHA-2](https://support.microsoft.com/help/4474419/sha-2-code-signing-support-update) antes de realizar los pasos siguientes. 
>

1. Busque la siguiente subclave del Registro: **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols**.
2. Cree una subclave en **Protocolos** para TLS 1.2 **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2**
3. Cree una subclave **Cliente** debajo de la subclave de la versión del protocolo TLS 1.2 que ha creado anteriormente. Por ejemplo, **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**.
4. Cree los siguientes valores DWORD en **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**:

    * **Enabled** [Value = 1]
    * **DisabledByDefault** [Value = 0]  

Configure .NET Framework 4.6 o posterior para que sea compatible con la criptografía segura, ya que, de forma predeterminada, está desactivada. La [criptografía fuerte](https://docs.microsoft.com/dotnet/framework/network-programming/tls#schusestrongcrypto) utiliza protocolos de red más seguros como TLS 1.2, y bloquea los protocolos que no son seguros. 

1. Busque la siguiente subclave del Registro:**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319**.  
2. Cree el valor DWORD **SchUseStrongCrypto** bajo esta subclave con un valor de **1**.  
3. Busque la siguiente subclave del Registro: **HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\\.NETFramework\v4.0.30319**.  
4. Cree el valor DWORD **SchUseStrongCrypto** bajo esta subclave con un valor de **1**. 
5. Reinicie el sistema para que la configuración surta efecto. 

## <a name="install-the-agent-using-setup-wizard"></a>Instalación del agente con el asistente para instalación
Los siguientes pasos permiten la instalación y configuración del agente de Log Analytics en Azure y en la nube de Azure Government mediante el asistente para la configuración del agente en su equipo. Si desea aprender a configurar el agente para que también informe a un grupo de administración de System Center Operations Manager, consulte [Para implementar el agente de Operations Manager con el Asistente para instalación del agente](https://docs.microsoft.com/system-center/scom/manage-deploy-windows-agent-manually#to-deploy-the-operations-manager-agent-with-the-agent-setup-wizard).

1. En el área de trabajo de Log Analytics, en la página **Servidores Windows** a la que navegó anteriormente, seleccione la versión **Descargar el agente de Windows** que desee descargar según la arquitectura del procesador del sistema operativo Windows.   
2. Ejecute el programa de instalación para instalar al agente en el equipo.
2. En la página **principal**, haga clic en **Siguiente**.
3. En la página **Términos de licencia**, lea la licencia y haga clic en **Acepto**.
4. En la página e **Carpeta de destino**, cambie o mantenga la carpeta de instalación predeterminada y haga clic en **Siguiente**.
5. En la página **Opciones de instalación del agente**, elija la opción para conectar el agente a Azure Log Analytics y luego haga clic en **Siguiente**.   
6. En la página **Azure Log Analytics**, realice lo siguiente:
   1. Pegue el **Id. del área de trabajo** y la **clave del área de trabajo (clave principal)** que copió anteriormente.  Si el equipo tiene que notificar a un área de trabajo de Log Analytics en Azure Government Cloud, seleccione **Azure US Gov** desde la lista desplegable **Azure Cloud**.  
   2. Si el equipo necesita comunicarse a través de un servidor proxy con el servicio de Log Analytics, haga clic en **Avanzado** y proporcione la dirección URL y el número de puerto del servidor proxy.  Si el servidor proxy requiere autenticación, escriba el nombre de usuario y la contraseña para autenticar con el servidor proxy y, luego, haga clic en **Siguiente**.  
7. Haga clic en **Siguiente** cuando haya terminado de proporcionar las opciones de configuración necesarias.<br><br> ![pegar identificador del área de trabajo y clave principal](media/agent-windows/log-analytics-mma-setup-laworkspace.png)<br><br>
8. En la página **Preparado para instalar**, revise las opciones seleccionadas y haga clic en **Instalar**.
9. En la página **La configuración finalizó correctamente**, haga clic en **Finalizar**.

Una vez completado el proceso, el **Agente de administración de Microsoft** aparece en el **Panel de control**. Para confirmar que está informando a Log Analytics, revise [Comprobar la conectividad del agente con Log Analytics](#verify-agent-connectivity-to-log-analytics). 

## <a name="install-the-agent-using-the-command-line"></a>Instalación del agente a través de la línea de comandos
El archivo descargado del agente es un paquete de instalación independiente.  El programa de instalación del agente y los archivos auxiliares se encuentran en el paquete y deben extraerse para realizar la instalación correctamente mediante la línea de comandos que se muestra en los ejemplos siguientes.    

>[!NOTE]
>Si desea actualizar a un agente, debe utilizar la API de scripting de Log Analytics. Si quiere obtener más información, consulte [Administrar y mantener el agente de Log Analytics para Windows y Linux](agent-manage.md).

En la tabla siguiente se resaltan los parámetros específicos que admite la configuración del agente, aunque la implementación se haya realizado mediante DSC de Automatización.

|Opciones específicas de MMA                   |Notas         |
|---------------------------------------|--------------|
| NOAPM=1                               | Parámetro opcional. Instala al agente sin la supervisión de rendimiento de aplicaciones .NET.|   
|ADD_OPINSIGHTS_WORKSPACE               | 1 = Configurar el agente para informar a un área de trabajo                |
|OPINSIGHTS_WORKSPACE_ID                | Identificador de área de trabajo (guid) para el área de trabajo que se agregará                    |
|OPINSIGHTS_WORKSPACE_KEY               | Clave del área de trabajo que se usa para autenticar inicialmente con el área de trabajo |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Especificar el entorno en la nube donde se encuentra el área de trabajo <br> 0 = nube comercial de Azure (valor predeterminado) <br> 1= Azure Government |
|OPINSIGHTS_PROXY_URL               | Identificador URI del proxy que se va a usar |
|OPINSIGHTS_PROXY_USERNAME               | Nombre de usuario para acceder a un proxy autenticado |
|OPINSIGHTS_PROXY_PASSWORD               | Contraseña para acceder a un proxy autenticado |

1. Para extraer los archivos de instalación del agente, desde un símbolo del sistema con privilegios elevados ejecute `MMASetup-<platform>.exe /c` y se le solicitará la ruta en la que extraer los archivos.  Como alternativa, puede especificar la ruta de acceso pasando los argumentos `MMASetup-<platform>.exe /c /t:<Full Path>`.  
2. Para instalar el agente de forma silenciosa y configurarlo para que informe a un área de trabajo en la nube comercial de Azure, vaya a la carpeta en la que extrajo los archivos de instalación y escriba: 
   
     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID="<your workspace ID>" OPINSIGHTS_WORKSPACE_KEY="<your workspace key>" AcceptEndUserLicenseAgreement=1
    ```

   o bien, para configurar el agente para que informe a la nube de la administración pública de EE. UU., escriba: 

     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID="<your workspace ID>" OPINSIGHTS_WORKSPACE_KEY="<your workspace key>" AcceptEndUserLicenseAgreement=1
    ```
    >[!NOTE]
    >Los valores de cadena para los parámetros *OPINSIGHTS_WORKSPACE_ID* y *OPINSIGHTS_WORKSPACE_KEY* se deben encapsular en comillas dobles para indicar a Windows Installer que los interprete como opciones válidas para el paquete. 

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Instalación del agente utilizando DSC en Azure Automation

Puede usar el siguiente script de ejemplo para instalar el agente mediante DSC de Azure Automation.   Si no tiene una cuenta de Automation, consulte [Introducción a Azure Automation](/azure/automation/) para comprobar los requisitos y los pasos necesarios para crear una cuenta de Automation antes de poder usar DSC de Automatización.  Si no está familiarizado con DSC de Automatización, consulte [Introducción a DSC de Automatización](../../automation/automation-dsc-getting-started.md).

En el ejemplo siguiente se instala el agente de 64 bits, identificado mediante el valor `URI`. También puede utilizar la versión de 32 bits si se reemplaza el valor del identificador URI. Estos son los identificadores URI para ambas versiones:

- Agente de Windows de 64 bits: https://go.microsoft.com/fwlink/?LinkId=828603
- Agente de Windows de 32 bits: https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>Este procedimiento y el script de ejemplo no admiten la actualización del agente que ya está implementado en un equipo Windows.

Las versiones de 32 y 64 bits del paquete de agente tienen códigos de producto diferentes y las nuevas versiones publicadas también tienen un valor único.  El código de producto es un GUID, que es la identificación principal de una aplicación o un producto, y se representa mediante la propiedad **ProductCode** de Windows Installer.  El valor `ProductId` del script **MMAgent.ps1** tiene que coincidir con el código de producto del paquete del instalador del agente de 32 o 64 bits.

Para recuperar el código de producto del paquete para instalar el agente directamente, puede usar Orca.exe desde la página [Windows SDK Components for Windows Installer Developers](https://msdn.microsoft.com/library/windows/desktop/aa370834%28v=vs.85%29.aspx) (Componentes del SDK de Windows para desarrolladores de Windows Installer), donde encontrará un componente del Kit de desarrollo de software de Windows, o mediante PowerShell y un [script de ejemplo](https://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) que haya escrito un MVP (profesional más valioso) de Microsoft.  Para cualquier enfoque, primero debe extraer el archivo **MOMagent.msi** del paquete de instalación MMASetup.  Esto se explicó anteriormente en el primero paso de la sección [Instalación del agente a través de la línea de comandos](#install-the-agent-using-the-command-line).  

1. Importe el módulo DSC xPSDesiredStateConfiguration de [https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) en Azure Automation.  
2.    En Azure Automation, cree los recursos de variable *OPSINSIGHTS_WS_ID* y *OPSINSIGHTS_WS_KEY*. Establezca *OPSINSIGHTS_WS_ID* en el identificador del área de trabajo de Log Analytics y *OPSINSIGHTS_WS_KEY* en la clave principal del área de trabajo.
3.    Copie el script y guárdelo como MMAgent.ps1.

```powershell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\Deploy\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"

    Import-DscResource -ModuleName xPSDesiredStateConfiguration
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}

```

4. Actualice el valor `ProductId` en el script con el código de producto extraído de la versión más reciente del paquete de instalación del agente mediante los métodos recomendados anteriormente. 
5. [Importe el script de configuración MMAgent.ps1](../../automation/automation-dsc-getting-started.md#importing-a-configuration-into-azure-automation) en su cuenta de Automation. 
5. [Asigne un equipo Windows o un nodo](../../automation/automation-dsc-getting-started.md#onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration) a la configuración. Durante los 15 minutos siguientes, el nodo comprueba la configuración y el agente se inserta en el nodo.

## <a name="verify-agent-connectivity-to-log-analytics"></a>Comprobación de la conectividad del agente a Log Analytics

Una vez completada la instalación del agente, compruebe que se ha conectado correctamente y que la creación de informes puede realizarse desde ambos lados.  

En el **Panel de control** del equipo, busque el elemento **Microsoft Monitoring Agent**.  Selecciónelo y, en la pestaña **Azure Log Analytics**, el agente mostrará un mensaje similar al siguiente: **Microsoft Monitoring Agent se conectó correctamente al servicio Microsoft Operations Management Suite.**<br><br> ![Estado de la conexión de MMA en Log Analytics](media/agent-windows/log-analytics-mma-laworkspace-status.png)

También puede realizar una consulta de registros sencilla en Azure Portal.  

1. En Azure Portal, busque y seleccione **Monitor**.
1. Seleccione **Registros** en el menú.
1. En el panel de**Registros**, en el campo tipo de consulta:  

    ```
    Heartbeat 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

En los resultados de la búsqueda devueltos, verá los registros de latido del equipo que indican que está conectado y que está creando informes para el servicio.   

## <a name="next-steps"></a>Pasos siguientes

- Revise [Administrar y mantener el agente de Log Analytics para Windows y Linux](agent-manage.md) para obtener información sobre cómo volver a configurar, actualizar o quitar el agente de la máquina virtual.

- Consulte [Solución de problemas del agente de Windows](agent-windows-troubleshoot.md) si encuentra incidencias durante la instalación o la administración del agente.
