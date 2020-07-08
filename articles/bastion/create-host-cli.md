---
title: Creación de un host de Bastion con CLI de Azure | Azure Bastion
description: En este artículo aprenderá a crear y eliminar un host de Bastion
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: how-to
ms.date: 03/26/2020
ms.author: mialdrid
ms.openlocfilehash: e7f80bb7f9be2e01aa24090d7305b1a5d882da04
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85255522"
---
# <a name="create-an-azure-bastion-host-using-azure-cli"></a>Creación de un host de Azure Bastion con CLI de Azure

En este artículo se muestra cómo crear un host de Azure Bastion con CLI de Azure. Una vez que haya aprovisionado el servicio Azure Bastion en la red virtual, ya puede disponer de la completa experiencia de RDP/SSH en todas las máquinas virtuales de la misma red virtual. La implementación de Azure Bastion se realiza por red virtual, no por suscripción o cuenta, ni por máquina virtual.

Opcionalmente, puede crear un host de Azure Bastion con [Azure Portal](bastion-create-host-portal.md) o con [Azure PowerShell](bastion-create-host-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que tiene una suscripción a Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) o registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [cloudshell cli](../../includes/vpn-gateway-cloud-shell-cli.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Creación de un host de Bastion

Esta sección le ayuda a crear un recurso de Azure Bastion con CLI de Azure.

1. Cree una red virtual y una subred Azure Bastion. Debe crear la subred Azure Bastion con el valor de nombre **AzureBastionSubnet**. Este valor permite a Azure saber en qué subred se deben implementar los recursos de Bastion. Esto no es lo mismo que una subred de puerta de enlace. Debe usar una subred de al menos /27 o más grande (/27, /26, etc.). Cree **AzureBastionSubnet** sin ninguna delegación ni tabla de rutas. Cuando use grupos de seguridad de red en **AzureBastionSubnet**, consulte el artículo [Trabajo con grupos de seguridad de red](bastion-nsg.md).

   ```azurecli-interactive
   az network vnet create -g MyResourceGroup -n MyVnet --address-prefix 10.0.0.0/16 --subnet-name AzureBastionSubnet  --subnet-prefix 10.0.0.0/24
   ```

2. Cree una dirección IP pública para Azure Bastion. La IP pública es la dirección IP pública del recurso de Bastion en la que se accederá a RDP/SSH (a través del puerto 443). La dirección debe estar en la misma región que el recurso de Bastion que está creando.

   ```azurecli-interactive
   az network public-ip create -g MyResourceGroup -n MyIp --sku Standard
   ```

3. Cree un nuevo recurso Azure Bastion en AzureBastionSubnet de su red virtual. El recurso de Bastion tarda aproximadamente cinco minutos en crearse e implementarse.

   ```azurecli-interactive
   az network bastion create --name $name--public-ip-address $publicip--resource-group $RgName --vnet-name $VNetName --location $location
                           
   ```

## <a name="next-steps"></a>Pasos siguientes

* Para más información, lea las [P+F sobre Bastion](bastion-faq.md).

* Para usar grupos de seguridad de red con la subred de Azure Bastion, consulte [Trabajo con grupos de seguridad de red](bastion-nsg.md).
