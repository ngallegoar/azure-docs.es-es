---
title: 'Ejemplos de Azure PowerShell: conjunto de escalado de zona única'
description: Este script crea un conjunto de escalado de máquinas virtuales que ejecuta Windows Server 2016 en una sola zona de disponibilidad.
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: sample
ms.date: 04/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: a9b3547844d0b2dcca58a95d04c9cb81d32fa52c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76276426"
---
# <a name="create-a-single-zone-virtual-machine-scale-set-with-powershell"></a>Creación de un conjunto de escalado de máquinas virtuales de zona única con Azure PowerShell
Este script crea un conjunto de escalado de máquinas virtuales que ejecuta Windows Server 2016 en una sola zona de disponibilidad. Después de ejecutar el script, puede acceder a la máquina virtual a través de RDP.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.ps1 "Create single-zone scale set")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Ejecute el siguiente comando para quitar el grupo de recursos, el conjunto de escalado y todos los recursos relacionados.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script
Este script usa los siguientes comandos para crear la implementación. Cada elemento de la tabla incluye vínculos a la documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzVmss](/powershell/module/az.compute/new-azvmss) | Crea el conjunto de escalado de máquinas virtuales y todos los recursos auxiliares, como la red virtual, el equilibrador de carga y las reglas NAT. |
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Obtiene información sobre un conjunto de escalado de máquinas virtuales. |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) | Agrega una extensión de máquina virtual para script personalizado para instalar una aplicación web básica. |
| [Update-AzVmss](/powershell/module/az.compute/update-azvmss) | Actualiza el modelo de conjunto de escalado de máquinas virtuales para aplicar la extensión de máquina virtual. |
| [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) | Obtiene información sobre la dirección IP pública asignada que usa el equilibrador de carga. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Quita un grupo de recursos y todos los recursos incluidos en él. |

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Se pueden encontrar otros ejemplos de scripts de PowerShell de conjunto de escalado de máquinas virtuales en la [documentación del conjunto de escalado de máquinas virtuales de Azure](../powershell-samples.md).
