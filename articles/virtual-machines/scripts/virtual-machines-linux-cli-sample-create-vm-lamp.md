---
title: 'Ejemplo de script de la CLI de Azure: implementación de la pila de LAMP en un conjunto de escalado de máquinas virtuales con carga equilibrada'
description: Use una extensión de script personalizado para implementar la pila de LAMP en un conjunto de escalado de máquinas virtuales de carga equilibrada en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
ms.author: cynthn
tags: azure-service-management
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.custom: mvc
ms.date: 04/05/2017
ms.openlocfilehash: 00aa744f09b7bcbbbc258b92a8fd5ab750e447c7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74039111"
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Implementación de la pila de LAMP en un conjunto de escalado de máquinas virtuales de carga equilibrada

En este ejemplo se crea un conjunto de escalado de máquinas virtuales y se aplica una extensión que ejecuta un script personalizado para implementar la pila de LAMP en cada máquina virtual del conjunto de escalado.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Conectar

Use este código para ver cómo conectarse a las máquinas virtuales y al conjunto de escalado.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, el conjunto de escalado y las máquinas virtuales, y todos los recursos relacionados.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una máquina virtual, un conjunto de disponibilidad, un equilibrador de carga y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss) | Crea un conjunto de escalado de máquinas virtuales. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) | Agrega un punto de conexión de carga equilibrada. |
| [az vmss extension set](https://docs.microsoft.com/cli/azure/vmss/extension) | Crea la extensión que ejecuta el script personalizado en la implementación de una máquina virtual. |
| [az vmss update-instances](https://docs.microsoft.com/cli/azure/vmss) | Ejecuta el script personalizado en las instancias de máquinas virtuales que se implementaron antes de aplicar la extensión al conjunto de escalado. |
| [az vmss scale](https://docs.microsoft.com/cli/azure/vmss) | Permite escalar verticalmente el conjunto de escalado agregando más instancias de máquinas virtuales. El script personalizado se ejecuta en esas instancias cuando se implementa. |
| [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip) | Permite obtener las direcciones IP de las máquinas virtuales creadas por el ejemplo. |
| [az network lb show](https://docs.microsoft.com/cli/azure/network/lb) | Permite obtener los puertos de front-end y back-end utilizados por el equilibrador de carga. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de la CLI de máquina virtual en la [documentación sobre máquinas virtuales Linux de Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
