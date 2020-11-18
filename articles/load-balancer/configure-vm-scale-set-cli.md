---
title: 'Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer: CLI de Azure'
description: Aprenda a configurar un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer mediante la CLI de Azure.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: how-to
ms.date: 03/25/2020
ms.openlocfilehash: a60a6889217ce6ca8dccd5ebf5ee74b8f67a7757
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94518216"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-cli"></a>Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer mediante la CLI de Azure

En este artículo, aprenderá a configurar un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerrequisitos 

- Necesitará un equilibrador de carga de SKU Estándar existente en la suscripción donde se implementará el conjunto de escalado de máquinas virtuales.

- Necesitará una instancia de Azure Virtual Network para el conjunto de escalado de máquinas virtuales.
 
[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- En este artículo se necesita la versión 2.0.28 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="deploy-a-virtual-machine-scale-set-with-existing-load-balancer"></a>Implementación de un conjunto de escalado de máquinas virtuales con un equilibrador de carga existente

Reemplace los valores entre corchetes por los nombres de los recursos en la configuración.

```azurecli-interactive
az vmss create \
    --resource-group <resource-group> \
    --name <vmss-name>\
    --image <your-image> \
    --admin-username <admin-username> \
    --generate-ssh-keys  \
    --upgrade-policy-mode Automatic \
    --instance-count 3 \
    --vnet-name <virtual-network-name> \
    --subnet <subnet-name> \
    --lb <load-balancer-name> \
    --backend-pool-name <backend-pool-name>
```

En el ejemplo siguiente se implementa un conjunto de escalado de máquinas virtuales con:

- Un conjunto de escalado de máquinas virtuales denominado **myVMSS**
- Una instancia de Azure Load Balancer denominada **myLoadBalancer**
- Un grupo de back-end de equilibradores de carga denominado **myBackendPool**
- Una instancia de Azure Virtual Network denominada **myVnet**
- Una subred denominada **mySubnet**
- Un grupo de recursos denominado **myResourceGroup**
- Imagen del servidor Ubuntu para el conjunto de escalado de máquinas virtuales

```azurecli-interactive
az vmss create \
    --resource-group myResourceGroup \
    --name myVMSS \
    --image Canonical:UbuntuServer:18.04-LTS:latest \
    --admin-username adminuser \
    --generate-ssh-keys \
    --upgrade-policy-mode Automatic \
    --instance-count 3 \
    --vnet-name myVnet\
    --subnet mySubnet \
    --lb myLoadBalancer \
    --backend-pool-name myBackendPool
```
> [!NOTE]
> Después de crear el conjunto de escalado, no se puede modificar el puerto de back-end para una regla de equilibrio de carga que se usa en un sondeo de estado del equilibrador de carga. Para cambiar el puerto, puede quitar el sondeo de estado mediante la actualización del conjunto de escalado de máquinas virtuales de Azure, actualizar el puerto y, a continuación, volver a configurar el sondeo de estado.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, implementó un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer.  Para obtener más información sobre los conjuntos de escalado de máquinas virtuales y el equilibrador de carga, consulte:

- [¿Qué es Azure Load Balancer?](load-balancer-overview.md)
- [¿Qué son los conjuntos de escalado de máquinas virtuales?](../virtual-machine-scale-sets/overview.md)
                                