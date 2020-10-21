---
title: Actualización de la extensión Network Watcher a la versión más reciente
description: Aprenda a actualizar la extensión Network Watcher a la versión más reciente.
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
ms.openlocfilehash: fd3fff2d438bbf804e35f04db0cfae15eea5e782
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91973346"
---
# <a name="how-to-update-the-network-watcher-extension-to-the-latest-the-version"></a>Actualización de la extensión Network Watcher a la versión más reciente. 

## <a name="overview"></a>Información general

[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md) es un servicio de supervisión, diagnóstico y análisis del rendimiento de red que permite supervisar redes de Azure. La extensión de máquina virtual del agente de Network Watcher es un requisito para capturar el tráfico de red a petición y otra funcionalidad avanzada en máquinas virtuales de Azure. La extensión Network Watcher se usa en características como Connection Monitor, Connection Monitor (versión preliminar), la solución de problemas de conexión y la captura de paquetes.   

## <a name="prerequisites"></a>Requisitos previos
En este documento se supone que tiene la extensión Network Watcher instalada en la máquina virtual y se proporcionan instrucciones para actualizarla a la versión más reciente. 

## <a name="latest-version"></a>La versión más reciente
La versión más reciente de la extensión Network Watcher es actualmente `1.4.1654.1`.

## <a name="updating-your-extension"></a>Actualización de la extensión 

### <a name="check-your-extension-version"></a>Comprobación de la versión de la extensión  

**Con Azure Portal**

1. Vaya a la hoja "Extensiones" de la máquina virtual en Azure Portal.   
2. Haga clic en la extensión "AzureNetworkWatcher" para ver el panel de detalles.  
3. Busque el número de versión en el campo "Versión".  

**Con la CLI de Azure** Ejecute el siguiente comando desde un símbolo del sistema de la CLI de Azure.   

```azurecli
az vm extension list --resource-group  <ResourceGroupName> --vm-name <VMName>
```

Busque la extensión AzureNetworkWatcher en la salida e identifique el número de versión en el campo "TypeHandlerVersion" en la salida.  


**Con PowerShell** Ejecute los comandos siguientes en un símbolo del sistema de PowerShell:   

```powershell
Get-AzVMExtension -ResourceGroupName <ResourceGroupName> -VMName <VMName>  
```

Busque la extensión AzureNetworkWatcher en la salida e identifique el número de versión en el campo "TypeHandlerVersion" en la salida.   


### <a name="update-your-extension"></a>Actualización de la extensión

En caso de que la versión sea inferior a `1.4.1654.1` (la última versión actual), actualice la extensión con cualquiera de las siguientes opciones. 

**Opción 1: Uso de PowerShell**

```powershell
#Linux command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentLinux"   

#Windows command
Set-AzVMExtension `  -ResourceGroupName "myResourceGroup1" `  -Location "WestUS" `  -VMName "myVM1" `  -Name "AzureNetworkWatcherExtension" `  -Publisher "Microsoft.Azure.NetworkWatcher" -Type "NetworkWatcherAgentWindows"   
```


**Opción 2: Uso de la CLI de Azure**  

Forzado de la actualización 

```azurecli
#Linux command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher" --force-update

#Windows command
az vm extension set --resource-group "myResourceGroup1" --vm-name "myVM1" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" --force-update
```

Si esto no funciona. Quite e instale la extensión de nuevo siguiendo los pasos que se indican a continuación. Esto agregará automáticamente la versión más reciente. 

Eliminación de la extensión 

```azurecli
#Same for Linux and Windows
az vm extension delete --resource-group "myResourceGroup1" --vm-name "myVM1" -n "AzureNetworkWatcherExtension"

```

Nueva instalación de la extensión

```azurecli
#Linux command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentLinux" --publisher "Microsoft.Azure.NetworkWatcher"  

#Windows command
az vm extension set --resource-group "DALANDEMO" --vm-name "Linux-01" --name "NetworkWatcherAgentWindows" --publisher "Microsoft.Azure.NetworkWatcher" 

```

**Opción 3: Reinicio de las máquinas virtuales**

Si tiene la actualización automática establecida en true para la extensión NetworkWatcher. Al reiniciar la máquina virtual, instale la extensión más reciente.


## <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede consultar la documentación de la extensión Network Watcher ([Linux](./network-watcher-linux.md), [Windows](./network-watcher-windows.md)) o ponerse en contacto con los expertos de Azure en los foros de [MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).