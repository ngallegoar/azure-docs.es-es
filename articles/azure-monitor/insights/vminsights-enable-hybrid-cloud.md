---
title: Habilitación de Azure Monitor para un entorno híbrido | Microsoft Docs
description: En este artículo se describe cómo habilitar Azure Monitor para VM para un entorno de nube híbrida que contenga una o más máquinas virtuales.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/15/2019
ms.openlocfilehash: 734f61c2e96002516e9e15af88d2c6b0fce00e98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480749"
---
# <a name="enable-azure-monitor-for-vms-for-a-hybrid-environment"></a>Habilitación de Azure Monitor para VM para un entorno híbrido

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

En este artículo se explica cómo habilitar Azure Monitor para VM para máquinas virtuales o equipos físicos hospedados en el centro de datos o en otro entorno en la nube. Al final de este proceso, habrá empezado a supervisar todas las máquinas virtuales de su entorno y a aprender si están experimentando problemas de rendimiento o disponibilidad.

Antes de comenzar, asegúrese de revisar los [requisitos previos](vminsights-enable-overview.md) y compruebe que su suscripción y sus recursos los cumplen. Revise los requisitos y los métodos de implementación para el [Agente Linux y Windows de Log Analytics](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

>[!NOTE]
>La extensión Dependency Agent para Service Map de Azure Monitor para VM no transmite ningún dato y no requiere ningún cambio en firewalls o puertos. Los datos de Mapa siempre son transmitidos por el agente de Log Analytics al servicio de Azure Monitor, ya sea directamente o mediante la [Puerta de enlace de Operations Management Suite](../../azure-monitor/platform/gateway.md), si las directivas de seguridad de TI no permiten que los equipos de la red se conecten a Internet.

Los pasos necesarios para completar esta tarea se resumen como sigue:

1. Instalar el agente de Log Analytics para Windows o Linux. Antes de instalar el agente, revise el artículo [Información general del agente de Log Analytics](../platform/log-analytics-agent.md) para comprender los requisitos previos del sistema y los métodos de implementación.

2. Descargar e instalar Dependency Agent para Service Map de Azure Monitor para VM en [Windows](https://aka.ms/dependencyagentwindows) o [Linux](https://aka.ms/dependencyagentlinux).

3. Habilitar la recopilación de contadores de rendimiento.

4. Implementar Azure Monitor para VM.

>[!NOTE]
>La información que se describe en este artículo para implementar el agente de dependencia también se aplica a la [solución Service Map](service-map.md).  

## <a name="install-the-dependency-agent-on-windows"></a>Instalar Dependency Agent en Windows

Para instalar Dependency Agent manualmente en equipos Windows, puede ejecutar `InstallDependencyAgent-Windows.exe`. Si ejecuta este archivo ejecutable sin opciones, se inicia un asistente para instalación que puede seguir para realizar la instalación del agente de forma interactiva.

>[!NOTE]
>Se requieren privilegios de *administrador* para instalar o desinstalar al agente.

En la tabla siguiente se destacan los parámetros que admite el programa de instalación para el agente desde la línea de comandos.

| Parámetro | Descripción |
|:--|:--|
| /? | Devuelve una lista de las opciones de la línea de comandos. |
| /S | Realiza una instalación silenciosa sin interacción del usuario. |

Por ejemplo, para ejecutar el programa de instalación con el parámetro `/?`, escriba **InstallDependencyAgent-Windows.exe /?** .

Los archivos de Dependency Agent para Windows se instalan en *C:\Archivos de programa\Microsoft Dependency Agent* de manera predeterminada. Si Dependency Agent no se inicia una vez completada la instalación, compruebe los registros para obtener información detallada del error. El directorio de registro es *%Programfiles%\Microsoft Dependency Agent\logs*.

## <a name="install-the-dependency-agent-on-linux"></a>Instalación de Dependency Agent en Linux

Dependency Agent se instala en los servidores Linux desde *InstallDependencyAgent-Linux64.bin*, un script de shell con un archivo binario autoextraíble. Puede ejecutar el archivo mediante `sh` o agregar permisos de ejecución al propio archivo.

>[!NOTE]
> Es necesario el acceso raíz para instalar o configurar el agente.
>

| Parámetro | Descripción |
|:--|:--|
| -help | Obtenga una lista de las opciones de la línea de comandos. |
| -S | Realice una instalación silenciosa sin preguntas. |
| --check | Compruebe los permisos y el sistema operativo, pero no instale el agente. |

Por ejemplo, para ejecutar el programa de instalación con el parámetro `-help`, escriba **InstallDependencyAgent-Linux64.bin -help**.

Para instalar Dependency en Linux como raíz, ejecute el comando `sh InstallDependencyAgent-Linux64.bin`.

Si Dependency Agent no se inicia, compruebe los registros para obtener información detallada del error. En los agentes de Linux, el directorio de registro es */var/opt/microsoft/dependency-agent/log*.

Los archivos de Dependency Agent se colocan en los directorios siguientes:

| Archivos | Location |
|:--|:--|
| Archivos principales | /opt/microsoft/dependency-agent |
| Archivos de registro | /var/opt/microsoft/dependency-agent/log |
| Archivos de configuración | /etc/opt/microsoft/dependency-agent/config |
| Archivos ejecutables de servicio | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Archivos binarios de almacenamiento | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Ejemplos de script de instalación

Para implementar fácilmente Dependency Agent en muchos servidores a la vez, se proporciona el siguiente script de ejemplo para descargar e instalar Dependency Agent en Windows o Linux.

### <a name="powershell-script-for-windows"></a>Script de PowerShell para Windows

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Script de shell para Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Configuración de estado deseado

Para implementar Dependency Agent mediante Desired State Configuration (DSC), puede usar el módulo xPSDesiredStateConfiguration con el siguiente código de ejemplo:

```powershell
configuration VMInsights {

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    $DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

    Node localhost
    {
        # Download and install the Dependency agent
        xRemoteFile DAPackage
        {
            Uri = "https://aka.ms/dependencyagentwindows"
            DestinationPath = $DAPackageLocalPath
        }

        xPackage DA
        {
            Ensure="Present"
            Name = "Dependency Agent"
            Path = $DAPackageLocalPath
            Arguments = '/S'
            ProductId = ""
            InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
            InstalledCheckRegValueName = "DisplayName"
            InstalledCheckRegValueData = "Dependency Agent"
            DependsOn = "[xRemoteFile]DAPackage"
        }
    }
}
```

## <a name="enable-performance-counters"></a>Activar los contadores de rendimiento

Si el área de trabajo de Log Analytics a la que la solución hace referencia no está todavía configurada para recopilar los contadores de rendimiento que la solución requiere, debe habilitarlos. Puede hacerlo de dos maneras:
* Manualmente, según se describe en [Orígenes de datos de rendimiento de Windows y Linux en Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Descargando y ejecutando un script de PowerShell que está disponible en la [Galería de Azure PowerShell](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

## <a name="deploy-azure-monitor-for-vms"></a>Implementar Azure Monitor para VM

Este método incluye una plantilla JSON que especifica la configuración para habilitar los componentes de la solución en el área de trabajo de Log Analytics.

Si no sabe cómo implementar los recursos mediante una plantilla, consulte:
* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md)
* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../../azure-resource-manager/templates/deploy-cli.md)

Para usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecuta la versión 2.0.27 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Para instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="create-and-execute-a-template"></a>Creación y ejecución de una plantilla

1. Copie y pegue la siguiente sintaxis JSON en el archivo:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('VMInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('VMInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'VMInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Guarde este archivo como *installsolutionsforvminsights.json* en una carpeta local.

1. Capture los valores de *WorkspaceName*, *ResourceGroupName* y *WorkspaceLocation*. El valor de *WorkspaceName* es el nombre del área de trabajo de Log Analytics. El valor de *WorkspaceLocation* es la región en la que el área de trabajo está definida.

1. Ya puede implementar una plantilla mediante el siguiente comando de PowerShell:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    El cambio de configuración puede tardar unos minutos en finalizar. Cuando finalice, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

    ```powershell
    provisioningState       : Succeeded
    ```
   Después de habilitar la supervisión, pueden pasar unos 10 minutos hasta que pueda ver la métrica y el estado del equipo híbrido.

## <a name="troubleshooting"></a>Solución de problemas

### <a name="vm-doesnt-appear-on-the-map"></a>La VM no aparece en la asignación

Si la instalación del agente de dependencia se completó correctamente, pero no ve el equipo en el mapa, siga estos pasos para diagnosticar el problema.

1. ¿Se ha instalado Dependency Agent correctamente? Para validarlo, compruebe si el servicio está instalado y se ejecuta.

    **Windows**: Busque el servicio llamado "Microsoft Dependency Agent".

    **Linux**: Busque el proceso en ejecución "microsoft-dependency-agent".

2. ¿Está en el [plan de tarifa gratuito de Log Analytics ](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)? El plan gratuito permite hasta cinco equipos únicos. Ningún equipo posterior aparecerá en la asignación, incluso si los cinco anteriores ya no envían datos.

3. ¿El equipo envía datos de registro y de rendimiento a los registros de Azure Monitor? Realice la consulta siguiente para el equipo:

    ```Kusto
    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

    ¿Devolvió uno o más resultados? ¿Son los datos recientes? Si es así, el agente de Log Analytics funciona correctamente y se comunica con el servicio. Si no es así, compruebe el agente en el servidor: [Log Analytics agent for Windows troubleshooting](../platform/agent-windows-troubleshoot.md) (Cómo solucionar problemas relacionados con el agente de Windows de Log Analytics) o [Cómo solucionar problemas relacionados con el agente de Linux de Log Analytics](../platform/agent-linux-troubleshoot.md).

#### <a name="computer-appears-on-the-map-but-has-no-processes"></a>El equipo aparece en la asignación, pero no tiene ningún proceso

Si ve el servidor en el mapa, pero no tienen datos de procesos ni de conexión, eso indica que Dependency Agent está instalado y se está ejecutando, pero el controlador del kernel no se cargó.

Revise el archivo C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log (Windows) o el archivo /var/opt/microsoft/dependency-agent/log/service.log (Linux). Las últimas líneas del archivo deben indicar por qué no se cargó el kernel. Por ejemplo, es posible que el kernel no sea compatible con Linux si actualizó el kernel.


## <a name="next-steps"></a>Pasos siguientes

Ahora que la supervisión está habilitada para las máquinas virtuales, esta información está disponible para analizarse con Azure Monitor para VM.

- Para ver las dependencias de las aplicaciones detectadas, consulte [Uso de la asignación de Azure Monitor para VM (versión preliminar) para conocer los componentes de una aplicación](vminsights-maps.md).

- Para identificar los cuellos de botella y el uso general con el rendimiento de la máquina virtual, vea [Cómo representar el rendimiento en gráficos con Azure Monitor para VM (versión preliminar)](vminsights-performance.md).
