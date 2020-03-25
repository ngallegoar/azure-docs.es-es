---
title: 'Ejemplo de script de la CLI de Azure: creación de una máquina virtual con un disco duro virtual'
description: 'Ejemplo de script de la CLI de Azure: crear una máquina virtual mediante un disco duro virtual.'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 16f4e3e153cbc02f8626199d168d069add48e4b6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74037671"
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Crear una máquina virtual con un disco duro virtual

Este ejemplo crea una máquina virtual con un disco duro virtual (VHD).
Crea un grupo de recursos, una cuenta de almacenamiento y un contenedor, y después carga el VHD en el contenedor para crear una máquina virtual.
Reemplaza la clave pública SSH con su clave pública para que tenga acceso a la máquina virtual.

Necesitará un VHD de arranque. El script busca `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una máquina virtual, un conjunto de disponibilidad, un equilibrador de carga y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account) | Enumera las cuentas de almacenamiento |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account) | Comprueba que un nombre de cuenta de almacenamiento es válido y que no existe ya |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys) | Enumera las claves de las cuentas de almacenamiento |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob) | Comprueba si existe el blob |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container) | Crea un contenedor en una cuenta de almacenamiento. |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob) | Crea un blob en el contenedor cargando el VHD. |
| [az vm list](https://docs.microsoft.com/cli/azure/vm) | Se usa con `--query` para comprobar si el nombre de la máquina virtual está en uso. | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | Crea las máquinas virtuales. |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az-vm-list-ip-addresses) | Obtiene la dirección IP de la máquina virtual que se creó. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de la CLI de máquina virtual en la [documentación sobre máquinas virtuales Linux de Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
