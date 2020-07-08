---
title: 'Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer: CLI de Azure'
description: Aprenda a configurar un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: how-to
ms.date: 03/25/2020
ms.openlocfilehash: 2d734e5242ff2a250d332de78cfa3b7f017a3fff
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84809460"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-cli"></a>Configuración de un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer mediante la CLI de Azure

En este artículo, aprenderá a configurar un conjunto de escalado de máquinas virtuales con una instancia existente de Azure Load Balancer. 

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción a Azure.
- Un equilibrador de carga de SKU estándar existente en la suscripción donde se implementará el conjunto de escalado de máquinas virtuales.
- Una instancia de Azure Virtual Network para el conjunto de escalado de máquinas virtuales.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Si decide usar la CLI localmente, para este artículo es preciso que tenga instalada la versión 2.0.28 o posterior de la CLI de Azure. Para encontrar la versión, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure-cli"></a>Inicio de sesión en la CLI de Azure

Inicie sesión en Azure.

```azurecli-interactive
az login
```

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
                                