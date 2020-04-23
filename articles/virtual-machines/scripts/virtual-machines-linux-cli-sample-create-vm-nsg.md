---
title: 'CLI de ejemplo: Creación de dos máquinas virtuales con un grupo de seguridad de red interno y externo'
description: Cree dos máquinas virtuales con grupos de seguridad de red internos y externos para proteger el tráfico de red mediante la CLI de Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b4339ab2c512a96614158f673cb07b60184f5f71
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459957"
---
# <a name="secure-network-traffic-between-virtual-machines-using-an-nsg"></a>Protección del tráfico de red entre máquinas virtuales mediante un grupo de seguridad de red

Este script crea dos máquinas virtuales y protege el tráfico entrante en ambas. Una máquina virtual es accesible en Internet y tiene un grupo de seguridad de red (NSG) configurado para permitir el tráfico en el puerto 80 y el puerto 22. La segunda máquina virtual no es accesible en Internet, y tiene un NSG configurado para permitir el tráfico únicamente a la primera máquina virtual.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una máquina virtual y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Crea una red virtual y una subred de Azure. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet) | Crea una subred. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Crea la máquina virtual y la conecta con la tarjeta de red, la red virtual, la subred y el NSG. Este comando también especifica la imagen de máquina virtual que se usará, y las credenciales administrativas.  |
| [az network nsg rule list](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Devuelve información acerca de una regla de grupo de seguridad de red. En este ejemplo, el nombre de la regla se almacena en una variable para usarlo más adelante en el script. |
| [az network nsg rule update](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Actualiza una regla de NSG. En este ejemplo, la regla de back-end se actualiza para que solo pase el tráfico de la subred de front-end. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de la CLI de máquina virtual en la [documentación sobre máquinas virtuales Linux de Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
