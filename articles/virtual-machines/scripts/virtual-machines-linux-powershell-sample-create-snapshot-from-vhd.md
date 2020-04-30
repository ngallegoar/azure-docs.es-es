---
title: 'Creación de una instantánea desde un VHD para crear varios discos administrados idénticos: Ejemplo de PowerShell'
description: 'Ejemplo de script de Azure PowerShell: creación de una instantánea desde un VHD para crear varios discos administrados idénticos en poco tiempo'
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: b163b5a1aad343382b9b2d22a91c1b07078c14df
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81459719"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Creación de una instantánea desde un VHD para crear varios discos administrados idénticos en poco tiempo con PowerShell

Este script crea una instantánea desde un archivo VHD en una cuenta de almacenamiento en una misma suscripción o en una suscripción distinta. Use este script para importar un VHD especializado (no generalizado ni preparado con SysPrep) a una instantánea y luego usar esta instantánea para crear varios discos administrados idénticos en poco tiempo. Además, úsela para importar un VHD de datos a una instantánea y luego use esta instantánea para crear varios discos administrados en poco tiempo.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]

## <a name="script-explanation"></a>Explicación del script

Este script utiliza los comandos siguientes para crear un disco administrado desde un archivo VHD en una suscripción distinta. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzDiskConfig) | Crea la configuración de disco que se usa para la creación del disco. Incluye tipo de almacenamiento, ubicación, identificador de recurso de la cuenta de almacenamiento donde se almacena el VHD principal y el URI de VHD del VHD principal. |
| [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Crea un disco mediante la configuración de disco, el nombre del disco y el nombre del grupo de recursos que se pasan como parámetros. |

## <a name="next-steps"></a>Pasos siguientes

[Creación de un disco administrado a partir de una instantánea](virtual-machines-linux-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Encontrará más ejemplos de scripts de PowerShell de máquina virtual en la [documentación sobre máquinas virtuales Linux de Azure](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
