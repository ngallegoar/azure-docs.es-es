---
title: Creación de un equilibrador de carga interno mediante una plantilla de Azure Resource Manager (plantilla de ARM)
description: Obtenga información sobre cómo crear un equilibrador de carga interno de Azure mediante una plantilla de Azure Resource Manager (plantilla de ARM).
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: allensu
ms.date: 09/14/2020
ms.openlocfilehash: 287afc51aa15ed4cadba7e2d6cd389e4869d7d61
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90532849"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-by-using-an-arm-template"></a>Inicio rápido: Creación de un equilibrador de carga interno para equilibrar la carga de las máquinas virtuales mediante una plantilla de ARM

En esta guía de inicio rápido se describe cómo usar una plantilla de Azure Resource Manager (plantilla de ARM) para crear un equilibrador de carga interno de Azure.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Prerrequisitos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla utilizada en esta guía de inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/201-2-vms-internal-load-balancer).

:::code language="json" source="~/quickstart-templates/201-2-vms-internal-load-balancer/azuredeploy.json":::

En la plantilla se han definido varios recursos de Azure:

- [**Microsoft.Storage/storageAccounts**](/azure/templates/microsoft.storage/storageaccounts): cuentas de almacenamiento de máquina virtual para el diagnóstico de arranque.
- [**Microsoft.Compute/availabilitySets**](/azure/templates/microsoft.compute/availabilitySets): conjunto de disponibilidad para máquinas virtuales.
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualNetworks): red virtual para el equilibrador de carga y las máquinas virtuales.
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkInterfaces): interfaces de red para las máquinas virtuales.
- [**Microsoft.Network/loadBalancers**](/azure/templates/microsoft.network/loadBalancers): Equilibrador de carga interno.
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualMachines): Máquinas virtuales.

Para encontrar más plantillas relacionadas con Azure Load Balancer, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Implementación de la plantilla

**CLI de Azure**

```azurecli-interactive
read -p "Enter the location (i.e. westcentralus): " location
resourceGroupName="myResourceGroupLB"
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json" 

az group create \
--name $resourceGroupName \
--location $location

az deployment group create \
--resource-group $resourceGroupName \
--template-uri  $templateUri
```

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Seleccione **Grupos de recursos** en el panel izquierdo.

3. Seleccione el grupo de recursos que creó en la sección anterior. El nombre predeterminado del grupo de recursos es **myResourceGroupLB**.

4. Compruebe que los recursos siguientes se han creado en el grupo de recursos:

:::image type="content" source="media/quickstart-load-balancer-standard-internal-template/verify-creation.png" alt-text="Uso de Azure Portal para comprobar la creación de los recursos." border="true":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, puede usar el comando [az group delete](/cli/azure/group#az-group-delete) para quitar el grupo de recursos y todos los recursos que contiene.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupLB
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener un tutorial paso a paso que le guíe en el proceso de creación de una plantilla, consulte:

> [!div class="nextstepaction"]
> Creación e implementación de su primera plantilla de Resource Manager[