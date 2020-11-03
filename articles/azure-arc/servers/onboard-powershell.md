---
title: Conexión de máquinas híbridas a Azure mediante PowerShell
description: En este artículo, obtendrá información sobre cómo instalar el agente y conectar una máquina a Azure mediante servidores habilitados para Azure Arc con PowerShell.
ms.date: 10/28/2020
ms.topic: conceptual
ms.openlocfilehash: 0755846ef02377edade98b69e478908a111ab247
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92901540"
---
# <a name="connect-hybrid-machines-to-azure-using-powershell"></a>Conexión de máquinas híbridas a Azure mediante PowerShell

Puede habilitar servidores habilitados para Azure Arc para una o varias máquinas Windows o Linux en el entorno si realiza una serie de pasos manuales. O bien, puede usar el cmdlet de PowerShell [Connect-AzConnectedMachine](/powershell/module/az.connectedmachine/remove-azconnectedmachine) para descargar el Agente de Connected Machine, instalarlo y registrar la máquina con Azure Arc. El cmdlet descarga el paquete de Windows Installer del agente de Windows del Centro de descarga de Microsoft y el paquete del agente de Linux desde el repositorio de paquetes de Microsoft.

Este método requiere que tenga permisos de administrador en la máquina para instalar y configurar el agente. En Linux, mediante la cuenta raíz, y en Windows, usted es miembro del grupo local de administradores. Puede completar este proceso de forma interactiva o remota en un servidor de Windows mediante la [comunicación remota de PowerShell](/powershell/scripting/learn/ps101/08-powershell-remoting).

Antes de comenzar, asegúrese de revisar los [requisitos previos](agent-overview.md#prerequisites) y compruebe que su suscripción y sus recursos los cumplen. Para obtener información sobre las regiones admitidas y otras consideraciones relacionadas, vea [Regiones admitidas de Azure](overview.md#supported-regions).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

- Una máquina con Azure PowerShell. Para obtener más información, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/).

Antes de usar Azure PowerShell para administrar las extensiones de máquina virtual en el servidor híbrido administrado por los servidores habilitados para Arc, tendrá que instalar el módulo `Az.ConnectedMachine`. Ejecute el comando siguiente en el servidor habilitado para Arc:

```powershell
Install-Module -Name Az.ConnectedMachine
```

Una vez que haya finalizado la instalación, se devuelve el mensaje siguiente:

`The installed extension ``Az.ConnectedMachine`` is experimental and not covered by customer support. Please use with discretion.`

## <a name="install-the-agent-and-connect-to-azure"></a>Instalación del agente y conexión a Azure

1. Abra una consola de PowerShell con privilegios elevados.

2. Ejecute el comando `Connect-AzAccount` para iniciar sesión en Azure.

3. Para instalar el Agente de Connected Machine, use `Connect-AzConnectedMachine` con los parámetros `-Name`, `-ResourceGroupName` y `-Location`. Use el parámetro `-SubscriptionId` para invalidar la suscripción predeterminada como resultado del contexto de Azure creado después del inicio de sesión. Ejecute uno de los siguientes comandos:

    * Para instalar el Agente de Connected Machine en el equipo de destino que se puede comunicar directamente con Azure, ejecute:

        ```azurepowershell
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e
        ```
    
    * Para instalar el Agente de Connected Machine en el equipo de destino que se comunica a través de un servidor proxy, ejecute:
        
        ```azurepowershell
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e -proxy http://<proxyURL>:<proxyport>
        ```

Si el agente no se inicia una vez completada la instalación, compruebe los registros para obtener información detallada del error. En Windows en *%ProgramData%\AzureConnectedMachineAgent\Log\himds.log* y en Linux en */var/opt/azcmagent/log/himds.log*.

## <a name="install-and-connect-using-powershell-remoting"></a>Instalación y conexión mediante la comunicación remota de PowerShell

Realice los pasos siguientes para configurar uno o más servidores de Windows con servidores habilitados para Azure Arc. La comunicación remota de PowerShell debe estar habilitada en la máquina remota. Use el cmdlet `Enable-PSRemoting` para habilitar la comunicación remota de PowerShell.

1. Abra una consola de PowerShell como administrador.

2. Ejecute el comando `Connect-AzAccount` para iniciar sesión en Azure.

3. Para instalar el Agente de Connected Machine, use `Connect-AzConnectedMachine` con los parámetros `-Name`, `-ResourceGroupName` y `-Location`. Use el parámetro `-SubscriptionId` para invalidar la suscripción predeterminada como resultado del contexto de Azure creado después del inicio de sesión.

    * Para instalar el Agente de Connected Machine en la máquina de destino que se puede comunicar directamente con Azure, ejecute el comando siguiente:
    
        ```azurepowershell
        $session = Connect-PSSession -ComputerName myMachineName
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -PSSession $session
        ```
    
    * Para instalar el Agente de Connected Machine en varias máquinas remotas al mismo tiempo, agregue una lista de nombres de máquinas remotas separados por una coma.

        ```azurepowershell
        $session = Connect-PSSession -ComputerName myMachineName1, myMachineName2, myMachineName3
        Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -PSSession $session
        ```

    El ejemplo siguiente es el resultado del comando que tiene como destino una sola máquina:
    
    ```azurepowershell
    time="2020-08-07T13:13:25-07:00" level=info msg="Onboarding Machine. It usually takes a few minutes to complete. Sometimes it may take longer depending on network and server load status."
    time="2020-08-07T13:13:25-07:00" level=info msg="Check network connectivity to all endpoints..."
    time="2020-08-07T13:13:29-07:00" level=info msg="All endpoints are available... continue onboarding"
    time="2020-08-07T13:13:50-07:00" level=info msg="Successfully Onboarded Resource to Azure" VM Id=f65bffc7-4734-483e-b3ca-3164bfa42941
    
    Name           Location OSName   Status     ProvisioningState
    ----           -------- ------   ------     -----------------
    myMachineName  eastus   windows  Connected  Succeeded
    ```

## <a name="verify-the-connection-with-azure-arc"></a>Comprobación de la conexión con Azure Arc

Después de instalar el agente y configurarlo para que se conecte a los servidores habilitados para Azure Arc, vaya a Azure Portal para comprobar que el servidor se ha conectado correctamente. Vea las máquinas en [Azure Portal](https://portal.azure.com).

![Una conexión de servidor correcta](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Pasos siguientes

* Puede encontrar información sobre la solución de problemas en la guía [Solución de problemas de conexión del agente de Connected Machine](troubleshoot-agent-onboard.md).

* Obtenga información sobre cómo administrar la máquina con [Azure Policy](../../governance/policy/overview.md) para, por ejemplo, la [configuración de invitado](../../governance/policy/concepts/guest-configuration.md) de VM, la comprobación de que la máquina informa al área de trabajo de Log Analytics esperada, la habilitación de la supervisión con [Azure Monitor con máquinas virtuales](../../azure-monitor/insights/vminsights-enable-policy.md) y mucho más.

* Más información sobre el [agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md). El agente de Log Analytics para Windows y Linux es necesario si quiere recopilar datos de supervisión del sistema operativo y de las cargas de trabajo y administrarlos con runbooks de Automation o con características como Update Management, o bien mediante otros servicios de Azure, como [Azure Security Center](../../security-center/security-center-introduction.md).