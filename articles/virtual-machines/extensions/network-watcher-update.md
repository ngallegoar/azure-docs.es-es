---
title: Actualización de la extensión Network Watcher a la versión más reciente
description: Obtenga información sobre cómo actualizar la extensión Azure Network Watcher a la versión más reciente.
services: virtual-machines-windows
documentationcenter: ''
author: damendo
manager: balar
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure-services
ms.date: 09/23/2020
ms.author: damendo
ms.openlocfilehash: 23520a0249e22b3f81c7f7c598ef10d8c3acb550
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92900187"
---
# <a name="update-the-network-watcher-extension-to-the-latest-version"></a>Actualización de la extensión Network Watcher a la versión más reciente

## <a name="overview"></a>Información general

[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md) es un servicio de supervisión, diagnóstico y análisis del rendimiento de red que supervisa las redes de Azure. La extensión de máquina virtual (VM) del agente de Network Watcher es un requisito para capturar el tráfico de red a petición y usar otras funcionalidades avanzadas en máquinas virtuales de Azure. La extensión Network Watcher se usa en características como Connection Monitor, Connection Monitor (versión preliminar), la solución de problemas de conexión y la captura de paquetes.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se supone que tiene la extensión Network Watcher instalada en la máquina virtual.

## <a name="latest-version"></a>La versión más reciente

La versión más reciente de la extensión Network Watcher es actualmente `1.4.1654.1`.

## <a name="update-your-extension"></a>Actualización de la extensión

Para actualizar la extensión, debe conocer su versión.

### <a name="check-your-extension-version"></a>Comprobación de la versión de la extensión

Puede comprobar la versión de la extensión mediante Azure Portal, la CLI de Azure o PowerShell.

#### <a name="usetheazureportal"></a>Uso de Azure Portal

1. Vaya al panel **Extensiones** de la máquina virtual en Azure Portal.
1. Seleccione la extensión **AzureNetworkWatcher** para ver el panel de detalles.
1. Busque el número de versión en el campo **Versión** .  

#### <a name="use-the-azure-cli"></a>Uso de la CLI de Azure

Ejecute el siguiente comando en un símbolo del sistema de la CLI de Azure:

```azurecli
az vm get-instance-view --resource-group  "SampleRG" --name "Sample-VM"
```
Busque **"AzureNetworkWatcherExtension"** en la salida e identifique el número de versión en el campo *"TypeHandlerVersion"* en la salida.  Tenga en cuenta lo siguiente: La información sobre la extensión aparece varias veces en la salida JSON. Mire en el bloque "extensions", debería ver el número de versión completo de la extensión. 

Debería ver algo parecido a lo siguiente: ![Captura de pantalla de la CLI de Azure](./media/network-watcher/azure-cli-screenshot.png)

#### <a name="usepowershell"></a>Uso de PowerShell

Ejecute los comandos siguientes en un símbolo del sistema de PowerShell:

```powershell
Get-AzVM -ResourceGroupName "SampleRG" -Name "Sample-VM" -Status
```
Busque la extensión Azure Network Watcher en la salida e identifique el número de versión en el campo *"TypeHandlerVersion"* de la salida.   

Debería ver algo parecido a lo siguiente: ![Captura de pantalla de PowerShell](./media/network-watcher/powershell-screenshot.png)

### <a name="update-your-extension"></a>Actualización de la extensión

Si la versión es anterior a `1.4.1654.1`, que es la versión actual más reciente, actualice la extensión con cualquiera de las siguientes opciones.

#### <a name="option-1-use-powershell"></a>Opción 1: Uso de PowerShell

Ejecute los comandos siguientes:

```powershell
#Linux command
Set-AzVMExtension -ResourceGroupName "myResourceGroup1" -Location "WestUS" -VMName "myVM1" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux"

#Windows command
Set-AzVMExtension -ResourceGroupName "myResourceGroup1" -Location "WestUS" -VMName "myVM1" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows"
```

Si esto no funciona. Quite e instale la extensión de nuevo siguiendo los pasos que se indican a continuación. Esto agregará automáticamente la versión más reciente.

Eliminación de la extensión 

```powershell
#Same command for Linux and Windows
Remove-AzVMExtension -ResourceGroupName "SampleRG" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension"
``` 

Nueva instalación de la extensión

```powershell
#Linux command
Set-AzVMExtension -ResourceGroupName "SampleRG" -Location "centralus" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux" -typeHandlerVersion "1.4"

#Windows command
Set-AzVMExtension -ResourceGroupName "SampleRG" -Location "centralus" -VMName "Sample-VM" -Name "AzureNetworkWatcherExtension" -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows" -typeHandlerVersion "1.4"
```

#### <a name="option-2-use-the-azure-cli"></a>Opción 2: Uso de la CLI de Azure

Aplique una actualización.

```azurecli
#Linux command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher" --force-update

#Windows command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" --force-update
```

Si eso no funciona, quite e instale de nuevo la extensión, y siga estos pasos para agregar automáticamente la versión más reciente.

Quite la extensión.

```azurecli
#Same for Linux and Windows
az vm extension delete --resource-group "myResourceGroup1" --vm-name "myVM1" -n "AzureNetworkWatcherExtension"

```

Instale de nuevo la extensión.

```azurecli
#Linux command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher"

#Windows command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher"

```

#### <a name="option-3-reboot-your-vms"></a>Opción 3: Reinicio de las máquinas virtuales

Si tiene la actualización automática establecida en verdadero para la extensión Network Watcher, reinicie la instalación de la máquina virtual con la extensión más reciente.

## <a name="support"></a>Soporte técnico

Si necesita más ayuda en cualquier punto de este artículo, consulte la documentación de la extensión Network Watcher para [Linux](./network-watcher-linux.md) o [Windows](./network-watcher-windows.md). También puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure y Stack Overflow](https://azure.microsoft.com/support/forums/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte técnico** . Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).
