---
title: Instalación y configuración de la extensión de Azure Diagnostics (WAD) para Windows
description: Obtenga información sobre cómo recopilar datos de diagnóstico de Azure en una cuenta de Azure Storage para que pueda verlos con una de las diversas herramientas disponibles.
services: azure-monitor
author: bwren
ms.subservice: diagnostic-extension
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: bwren
ms.openlocfilehash: 929ab4109eb8d0e90b6c561a2135c0b7dd4205bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672266"
---
# <a name="install-and-configure-windows-azure-diagnostics-extension-wad"></a>Instalación y configuración de la extensión de Azure Diagnostics (WAD) para Windows
La extensión de Azure Diagnostics es un agente de Azure Monitor que recopila datos de supervisión del sistema operativo invitado y cargas de trabajo de las máquinas virtuales de Azure y otros recursos de proceso. En este artículo se proporcionan detalles sobre cómo instalar y configurar la extensión de diagnósticos para Windows y una descripción de cómo se almacenan los datos en la cuenta de Azure Storage.

La extensión de diagnósticos se implementa como una [extensión de máquina virtual](../../virtual-machines/extensions/overview.md) en Azure, por lo que admite las mismas opciones de instalación mediante las plantillas de Resource Manager, PowerShell y la CLI. Consulte [Características y extensiones de las máquinas virtuales para Windows](../../virtual-machines/extensions/features-windows.md) para más información sobre la instalación y el mantenimiento de extensiones de máquina virtual.

## <a name="install-with-azure-portal"></a>Instalación con Azure Portal
Puede instalar y configurar la extensión de diagnósticos en una máquina virtual de Azure Portal que proporcione una interfaz en lugar de trabajar directamente con la configuración. Al habilitar la extensión de diagnósticos, se usará automáticamente una configuración predeterminada con los eventos y contadores de rendimiento más comunes. Esta configuración predeterminada se puede modificar de acuerdo con sus requisitos específicos.

> [!NOTE]
> Hay configuraciones de la extensión de diagnósticos que no se pueden realizar con Azure Portal, como el envío de datos a Azure Event Hubs. Para ellas, debe usar uno de los otros métodos de configuración.

1. Abra el menú de una máquina virtual en Azure Portal.
2. Haga clic en **Configuración de diagnóstico** en la sección **Supervisión** del menú de la máquina virtual.
3. Haga clic en **Habilitar supervisión a nivel de invitado** si aún no se ha habilitado la extensión de diagnósticos.
4. Se crea una cuenta de Azure Storage para la máquina virtual, donde el nombre se basa en el del grupo de recursos de la máquina virtual. Para asociar la máquina virtual a otra cuenta de almacenamiento, seleccione la pestaña **Agente**.

![Configuración de diagnóstico](media/diagnostics-extension-windows-install/diagnostic-settings.png)


Puede modificar la configuración predeterminada una vez que se haya habilitado la extensión de diagnósticos. En la tabla siguiente se describen las opciones que puede modificar en las distintas pestañas. Algunas opciones tienen un comando **Personalizado** que permite especificar una configuración más detallada; para información sobre las distintas configuraciones, consulte [Esquema de extensiones de diagnóstico de Windows](diagnostics-extension-schema-windows.md).

| Pestaña | Descripción |
|:---|:---|
| Información general | Muestra la configuración actual con vínculos a las demás pestañas. |
| Contadores de rendimiento | Seleccione los contadores de rendimiento que se van a recopilar y la frecuencia de muestreo de cada uno.  |
| Registros | Seleccione los datos de registro que se van a recopilar. Aquí se incluyen los registros de eventos de Windows, los registros de IIS, los registros de aplicación .NET y los eventos ETW.  |
| Volcados de memoria | Habilite el volcado de memoria para los distintos procesos. |
| Receptores | Habilite los receptores de datos para enviar datos a otros destinos, además de Azure Storage.<br>Azure Monitor: envía datos de rendimiento a métricas de Azure Monitor.<br>Application Insights: envía datos a una aplicación de Application Insights. |
| Agente | Modifique la siguiente configuración del agente:<br>- Cambie la cuenta de almacenamiento.<br>- Especifique el disco local máximo usado para el agente.<br>- Configure registros para el propio estado del agente.|


> [!NOTE]
> Aunque se puede dar formato a la configuración de la extensión de diagnósticos en JSON o XML, cualquier configuración realizada en Azure Portal se almacenará siempre como JSON. Si usa XML con otro método de configuración y, luego, cambia la configuración con Azure Portal, esta cambiará a JSON.

## <a name="resource-manager-template"></a>Plantilla de Resource Manager
Consulte [Uso de la supervisión y el diagnóstico con una máquina virtual Windows y plantillas de Azure Resource Manager](../../virtual-machines/extensions/diagnostics-template.md) para información sobre la implementación de la extensión de diagnósticos con plantillas de Azure Resource Manager. 

## <a name="azure-cli-deployment"></a>Implementación de la CLI de Azure
La CLI de Azure se puede usar para implementar la extensión de Azure Diagnostics en una máquina virtual existente mediante [az vm extension set](https://docs.microsoft.com/cli/azure/vm/extension?view=azure-cli-latest#az-vm-extension-set), como en el ejemplo siguiente. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --protected-settings protected-settings.json \
  --settings public-settings.json 
```

La configuración protegida se define en el [elemento PrivateConfig](diagnostics-extension-schema-windows.md#privateconfig-element) del esquema de configuración. A continuación se indica un ejemplo mínimo de un archivo de configuración protegida que define la cuenta de almacenamiento. Consulte [Configuración de ejemplo](diagnostics-extension-schema-windows.md#privateconfig-element) para ver detalles completos de la configuración privada.

```JSON
{
    "storageAccountName": "mystorageaccount",
    "storageAccountKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "storageAccountEndPoint": "https://mystorageaccount.blob.core.windows.net"
}
```
La configuración pública se define en el [elemento Public](diagnostics-extension-schema-windows.md#publicconfig-element) del esquema de configuración. A continuación se ofrece un ejemplo mínimo de un archivo de configuración pública que permite la recopilación de registros de infraestructura de diagnóstico, un único contador de rendimiento y un único registro de eventos. Consulte [Configuración de ejemplo](diagnostics-extension-schema-windows.md#publicconfig-element) para información detallada sobre la configuración pública.

```JSON
{
  "StorageAccount": "mystorageaccount",
  "WadCfg": {
    "DiagnosticMonitorConfiguration": {
      "overallQuotaInMB": 5120,
      "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": [
          {
            "counterSpecifier": "\\Processor Information(_Total)\\% Processor Time",
            "unit": "Percent",
            "sampleRate": "PT60S"
          }
        ]
      },
      "WindowsEventLog": {
        "scheduledTransferPeriod": "PT1M",
        "DataSource": [
          {
            "name": "Application!*[System[(Level=1 or Level=2 or Level=3)]]"
          }
        ]
      }
    }
  }
}
```



## <a name="powershell-deployment"></a>Implementación de PowerShell
PowerShell se puede usar para implementar la extensión de Azure Diagnostics en una máquina virtual existente mediante [Set-AzVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension), como en el ejemplo siguiente. 

```powershell
Set-AzVMDiagnosticsExtension -ResourceGroupName "myvmresourcegroup" `
  -VMName "myvm" `
  -DiagnosticsConfigurationPath "DiagnosticsConfiguration.json"
```

La configuración privada se define en el [elemento PrivateConfig](diagnostics-extension-schema-windows.md#privateconfig-element), mientras que la configuración pública se define en el [elemento Public](diagnostics-extension-schema-windows.md#publicconfig-element) del esquema de configuración. También puede optar por especificar los detalles de la cuenta de almacenamiento como parámetros del cmdlet Set-AzVMDiagnosticsExtension en lugar de incluirlos en la configuración privada.

A continuación se ofrece un ejemplo mínimo de un archivo de configuración que permite la recopilación de registros de infraestructura de diagnóstico, un único contador de rendimiento y un único registro de eventos. Consulte [Configuración de ejemplo](diagnostics-extension-schema-windows.md#publicconfig-element) para información detallada sobre la configuración pública y privada. 

```JSON
{
    "PublicConfig": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
                "overallQuotaInMB": 10000
            },
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT3M",
                        "unit": "percent"
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT1M",
                    "DataSource": [
                    {
                        "name": "Application!*[System[(Level=1 or Level=2 or Level=3)]]"
                    }
                ]
            }
        },
        "StorageAccount": "mystorageaccount",
        "StorageType": "TableAndBlob"
    },
    "PrivateConfig": {
        "storageAccountName": "mystorageaccount",
        "storageAccountKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "storageAccountEndPoint": "https://mystorageaccount.blob.core.windows.net"
    }
}
```

Consulte también [Uso de PowerShell para habilitar Diagnósticos de Azure en una máquina virtual con Windows](../../virtual-machines/extensions/diagnostics-windows.md).

## <a name="data-storage"></a>Almacenamiento de datos
En la tabla siguiente se enumeran los distintos tipos de datos recopilados por la extensión de diagnósticos y se indica si se almacenan como una tabla o un blob. Los datos almacenados en tablas también se pueden almacenar en blobs en función del [valor de StorageType](diagnostics-extension-schema-windows.md#publicconfig-element) de la configuración pública.


| data | Tipo de almacenamiento | Descripción |
|:---|:---|:---|
| WADDiagnosticInfrastructureLogsTable | Tabla | Monitor de diagnóstico y cambios de configuración. |
| WADDirectoriesTable | Tabla | Directorios que supervisa el monitor de diagnóstico.  Esto incluye los registros de IIS, los registros de solicitudes con error de IIS y los directorios personalizados.  La ubicación del archivo de registro de blob se especifica en el campo de contenedor y el nombre del blob está en el campo RelativePath.  El campo AbsolutePath indica la ubicación y el nombre del archivo tal como existía en la máquina virtual de Azure. |
| WadLogsTable | Tabla | Registros escritos en código mediante la escucha de seguimiento. |
| WADPerformanceCountersTable | Tabla | Contadores de rendimiento. |
| WADWindowsEventLogsTable | Tabla | Registros de eventos de Windows. |
| wad-iis-failedreqlogfiles | Blob | Contiene información de los registros de solicitudes con error de IIS. |
| wad-iis-logfiles | Blob | Contiene información sobre los registros de IIS. |
| "custom" | Blob | Un contenedor personalizado basado en la configuración de directorios supervisados por el monitor de diagnóstico.  El nombre de este contenedor de blob se especificará en WADDirectoriesTable. |

## <a name="tools-to-view-diagnostic-data"></a>Herramientas para ver los datos de diagnóstico
Existen varias herramientas para ver los datos una vez que se transfieren al almacenamiento. Por ejemplo:

* El Explorador de servidores en Visual Studio: si instaló Azure Tools para Microsoft Visual Studio, puede usar el nodo de Azure Storage en el Explorador de servidores para ver los datos de tabla y blob de solo lectura de las cuentas de Azure Storage. Puede mostrar datos de la cuenta del emulador de almacenamiento local y también desde cuentas de almacenamiento que haya creado para Azure. Para obtener más información, consulte [Exploración y administración de recursos de almacenamiento con el Explorador de servidores](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage).
* [Explorador de Microsoft Azure Storage](../../vs-azure-tools-storage-manage-with-storage-explorer.md) es una aplicación independiente que permite trabajar fácilmente con los datos de Azure Storage en Windows, OSX y Linux.
* [Azure Management Studio](https://www.cerebrata.com/products/azure-management-studio/introduction) incluye Azure Diagnostics Manager, que permite ver, descargar y administrar los datos de diagnóstico recopilados por las aplicaciones que se ejecutan en Azure.

## <a name="next-steps"></a>Pasos siguientes
- Consulte [Envío de datos desde la extensión Azure Diagnostics para Windows a Event Hubs](diagnostics-extension-stream-event-hubs.md) para más información sobre el reenvío de datos de supervisión a Azure Event Hubs.
