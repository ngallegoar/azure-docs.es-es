---
title: Uso de la extensión Estado de la aplicación con conjuntos de escalado de máquinas virtuales de Azure
description: Obtenga información sobre cómo usar la extensión Estado de la aplicación para supervisar el estado de las aplicaciones implementadas en conjuntos de escalado de máquinas virtuales.
author: mimckitt
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: mimckitt
ms.openlocfilehash: cb5f1d48bb1a95db004d9da553e19a35071c73b0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81273739"
---
# <a name="using-application-health-extension-with-virtual-machine-scale-sets"></a>Uso de la extensión Estado de la aplicación con conjuntos de escalado de máquinas virtuales
Supervisar el estado de la aplicación es una señal importante para administrar y actualizar la implementación. Los conjuntos de escalado de máquinas virtuales de Azure proporcionan compatibilidad para [actualizaciones graduales](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) incluyendo [las actualizaciones automáticas de imagen de sistema operativo](virtual-machine-scale-sets-automatic-upgrade.md), que se basan en la supervisión del estado de las instancias individuales para actualizar la implementación.

En este artículo se describe cómo usar la extensión Estado de la aplicación para supervisar el estado de las aplicaciones implementadas en conjuntos de escalado de máquinas virtuales.

## <a name="prerequisites"></a>Prerequisites
En este artículo se asume que está familiarizado con:
-   [Extensiones](../virtual-machines/extensions/overview.md) de máquina virtual de Azure
-   [Modificación](virtual-machine-scale-sets-upgrade-scale-set.md) de conjuntos de escalado de máquinas virtuales

## <a name="when-to-use-the-application-health-extension"></a>Cuándo se debe usar la extensión Estado de la aplicación
La extensión Estado de la aplicación se implementa dentro de una instancia de conjunto de escalado de máquinas virtuales e informa sobre el estado de la máquina virtual desde dentro de la instancia de conjunto de escalado. Puede configurar la extensión para hacer un sondeo en un punto de conexión de la aplicación y actualizar el estado de la aplicación en esa instancia. Azure comprueba el estado de esta instancia para determinar si una instancia es apta para las operaciones de actualización.

Dado que la extensión informa sobre el estado desde dentro de una máquina virtual, se puede usar en situaciones donde se pueden aprovechar los sondeos externos, como los de Estado de la aplicación (que usan [sondeos](../load-balancer/load-balancer-custom-probe-overview.md) de Azure Load Balancer personalizados).

## <a name="extension-schema"></a>Esquema de extensión

En el siguiente JSON, se muestra el esquema para la extensión de Estado de la aplicación. La extensión requiere como mínimo una solicitud "tcp" o "http" con un puerto asociado o solicitar la ruta de acceso, respectivamente.

```json
{
  "type": "extensions",
  "name": "HealthExtension",
  "apiVersion": "2018-10-01",
  "location": "<location>",  
  "properties": {
    "publisher": "Microsoft.ManagedServices",
    "type": "< ApplicationHealthLinux or ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}  
```

### <a name="property-values"></a>Valores de propiedad

| Nombre | Valor / ejemplo | Tipo de datos
| ---- | ---- | ---- 
| apiVersion | `2018-10-01` | date |
| publisher | `Microsoft.ManagedServices` | string |
| type | `ApplicationHealthLinux`Linux, `ApplicationHealthWindows` (Windows) | string |
| typeHandlerVersion | `1.0` | int |

### <a name="settings"></a>Configuración

| Nombre | Valor / ejemplo | Tipo de datos
| ---- | ---- | ----
| protocol | `http` o `tcp` | string |
| port | Opcional cuando el protocolo es `http`, obligatorio cuando el protocolo es `tcp` | int |
| requestPath | Obligatorio cuando el protocolo es `http`, no se permite cuando el protocolo es `tcp` | string |

## <a name="deploy-the-application-health-extension"></a>Implementación de la extensión Estado de la aplicación
Hay varias maneras de implementar la extensión Estado de la aplicación tal como se detalla en los siguientes ejemplos.

### <a name="rest-api"></a>API DE REST

En el siguiente ejemplo se agrega la extensión Estado de la aplicación (con el nombre myHealthExtension) a extensionProfile en el un modelo de conjunto de escalado de un conjunto de escalado basado en Windows.

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/myHealthExtension?api-version=2018-10-01`
```

```json
{
  "name": "myHealthExtension",
  "properties": {
    "publisher": " Microsoft.ManagedServices",
    "type": "< ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}
```
Use `PATCH` para editar una extensión ya implementada.

### <a name="azure-powershell"></a>Azure PowerShell

Use el cmdlet [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) para agregar la extensión Estado de la aplicación a la definición del modelo del conjunto de escalado.

En el siguiente ejemplo se agrega la extensión Estado de la aplicación al modelo de conjunto de escalado `extensionProfile` de un conjunto de escalado basado en Windows. El ejemplo utiliza el nuevo módulo de PowerShell Az.

```azurepowershell-interactive
# Define the scale set variables
$vmScaleSetName = "myVMScaleSet"
$vmScaleSetResourceGroup = "myVMScaleSetResourceGroup"

# Define the Application Health extension properties
$publicConfig = @{"protocol" = "http"; "port" = 80; "requestPath" = "/healthEndpoint"};
$extensionName = "myHealthExtension"
$extensionType = "ApplicationHealthWindows"
$publisher = "Microsoft.ManagedServices"

# Get the scale set object
$vmScaleSet = Get-AzVmss `
  -ResourceGroupName $vmScaleSetResourceGroup `
  -VMScaleSetName $vmScaleSetName

# Add the Application Health extension to the scale set model
Add-AzVmssExtension -VirtualMachineScaleSet $vmScaleSet `
  -Name $extensionName `
  -Publisher $publisher `
  -Setting $publicConfig `
  -Type $extensionType `
  -TypeHandlerVersion "1.0" `
  -AutoUpgradeMinorVersion $True

# Update the scale set
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup `
  -Name $vmScaleSetName `
  -VirtualMachineScaleSet $vmScaleSet
```


### <a name="azure-cli-20"></a>CLI de Azure 2.0

Use el [conjunto de extensiones az vmss](/cli/azure/vmss/extension#az-vmss-extension-set) para agregar la extensión Estado de la aplicación a la definición del modelo del conjunto de escalado.

En el siguiente ejemplo se agrega la extensión Estado de la aplicación al modelo del conjunto de escalado basado en Linux.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthLinux \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --settings ./extension.json
```
El contenido del archivo extension.json.

```json
{
  "protocol": "<protocol>",
  "port": "<port>",
  "requestPath": "</requestPath>"
}
```


## <a name="troubleshoot"></a>Solución de problemas
El resultado de la ejecución de las extensiones se registra en los archivos que se encuentran en los siguientes directorios:

```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.ManagedServices.ApplicationHealthWindows\<version>\
```

```Linux
/var/lib/waagent/apphealth
```

Los registros también capturan periódicamente el estado de mantenimiento de la aplicación.

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo [implementar la aplicación](virtual-machine-scale-sets-deploy-app.md) en conjuntos de escalado de máquinas virtuales.
