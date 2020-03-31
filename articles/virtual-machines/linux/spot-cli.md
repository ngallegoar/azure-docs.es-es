---
title: Uso de la CLI para implementar máquinas virtuales de Azure Spot (versión preliminar)
description: Aprenda a usar la CLI para implementar máquinas virtuales de Azure Spot para ahorrar costos.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2020
ms.author: cynthn
ms.openlocfilehash: 110e935671ab1d640b2ff3dc26c203b262e999fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77163099"
---
# <a name="preview-deploy-spot-vms-using-the-azure-cli"></a>Vista previa: Implementación de máquinas virtuales de Spot con la CLI de Azure

El uso de [máquinas virtuales de Azure Spot](spot-vms.md) permite aprovechar las ventajas de nuestra capacidad no utilizada con un importante ahorro en los costos. Siempre que Azure necesite recuperar la capacidad, su infraestructura expulsará las máquinas virtuales de Spot. Por lo tanto, estas son excelentes para cargas de trabajo que soportan interrupciones, como los trabajos de procesamiento por lotes, los entornos de desarrollo y pruebas, las grandes cargas de trabajo de proceso, etc.

Los precios de las máquinas virtuales de Spot varían en función de la región y la SKU. Para más información, consulte precios de las máquinas virtuales para [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) y [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 

Puede establecer el precio máximo por hora que esté dispuesto por la máquina virtual. El precio máximo de una máquina virtual de Spot se puede establecer en dólares estadounidenses (USD), con un máximo de 5 decimales. Por ejemplo, el valor `0.98765` correspondería a un precio máximo de 0,98765 USD por hora. Si establece el precio máximo en `-1`, la máquina virtual no se expulsará por precio. El precio de la máquina virtual será el actual de Spot o el de una máquina virtual estándar, el menor de los dos, siempre que haya capacidad y cuota disponibles. Para más información sobre la configuración del precio máximo, consulte [Máquinas virtuales de Spot - Precios](spot-vms.md#pricing).

El proceso de creación de una máquina virtual con la CLI de Azure es el mismo que el que se detalla en el [artículo de inicio rápido](/azure/virtual-machines/linux/quick-create-cli). Solo tiene que agregar el parámetro "--priority Spot" y proporcionar un precio máximo o el valor `-1`.

> [!IMPORTANT]
> Las instancias de Spot se encuentran actualmente en versión preliminar pública,
> la cual no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>



## <a name="install-azure-cli"></a>Instalación de la CLI de Azure

Para crear máquinas virtuales de Spot, debe ejecutar la versión 2.0.74 o posterior de la CLI de Azure. Para saber qué versión tiene, ejecute el comando **az --version**. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

Inicie sesión en Azure mediante [az login](/cli/azure/reference-index#az-login).

```azurecli
az login
```

## <a name="create-a-spot-vm"></a>Creación de una máquina virtual de Spot

En este ejemplo se muestra cómo implementar una máquina virtual de Spot en Linux que no se expulse por precio. 

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1
```

Una vez creada la máquina virtual, puede realizar una consulta para ver el precio máximo de facturación de todas las máquinas virtuales del grupo de recursos.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

**Pasos siguientes**

También puede crear una máquina virtual de Spot con [Azure PowerShell](../windows/spot-powershell.md) o una [plantilla](spot-template.md).

Si se produce un error, consulte [Códigos de error](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
