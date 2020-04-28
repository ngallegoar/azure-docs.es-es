---
title: 'Inicio rápido: Creación de una instancia de Azure Firewall y grupos de IP: plantilla de Resource Manager'
description: Aprenda a usar una plantilla de Resource Manager para crear una instancia de Azure Firewall y grupos de IP.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.date: 04/06/2020
ms.author: victorh
ms.openlocfilehash: 53e5bf4f770ce986af2f3572bd6c1ef4cd9e3c2b
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605248"
---
# <a name="quickstart-create-an-azure-firewall-and-ip-groups---resource-manager-template"></a>Inicio rápido: Creación de una instancia de Azure Firewall y grupos de IP: plantilla de Resource Manager

En este inicio rápido, se va a usar una plantilla de Resource Manager para implementar una instancia de Azure Firewall con grupos de IP de ejemplo usados en una regla de red y una regla de aplicación.

Un grupo de IP es un recurso de nivel superior que le permite definir y agrupar direcciones IP, intervalos y subredes en un único objeto. Resulta útil para administrar direcciones IP en reglas de Azure Firewall. Puede especificar manualmente las direcciones IP o importarlas desde un archivo.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-azure-firewall-and-ip-groups"></a>Creación de una instancia de Azure Firewall y grupos de IP

Esta plantilla crea una instancia de Azure Firewall y grupos de IP, junto con los recursos necesarios para admitir Azure Firewall.

### <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox/azuredeploy.json" range="001-512" highlight="118-141":::

En la plantilla se definen varios recursos de Azure:

- [**Microsoft.Network/ipGroups**](/azure/templates/microsoft.network/ipGroups)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines):
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft.Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft.Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft.Network/routeTables**](/azure/templates/microsoft.network/routeTables)


### <a name="deploy-the-template"></a>Implementación de la plantilla

Implementación de la plantilla de Resource Manager en Azure:

1. Seleccione **Implementar en Azure** para iniciar sesión en Azure y abrir la plantilla. La plantilla crea una instancia de Azure Firewall, la infraestructura de red y dos máquinas virtuales.

   [![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurefirewall-create-with-ipgroups-and-linux-jumpbox%2Fazuredeploy.json)

2. En el portal, en la página **Create an Azure Firewall with IpGroups** (Crear una instancia de Azure Firewall con grupos de IP), escriba o seleccione los valores siguientes:
   - Suscripción: seleccione una de las suscripciones existentes. 
   - Grupo de recursos:  seleccione uno de los grupos de recursos existentes o elija **Crear nuevo** y, luego, **Aceptar**.
   - Ubicación: Seleccionar una ubicación
   - Nombre de la red virtual: escriba un nombre para la nueva red virtual. 
   - Nombre del grupo de IP 1: escriba el nombre del grupo de IP 1. 
   - Nombre del grupo de IP 2: escriba el nombre del grupo de IP 2. 
   - Nombre de usuario de administrador: escriba un nombre de usuario para la cuenta de usuario de administrador. 
   - Autenticación: seleccione sshPublicKey o contraseña. 
   - Contraseña de administrador: escriba una contraseña o clave de administrador.

3. Seleccione **Acepto los términos y condiciones indicadas anteriormente** y, después, seleccione **Comprar**. La implementación puede tardar 10 minutos o más en completarse.

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

En Azure Portal, revise los recursos implementados, especialmente las reglas de firewall que usan grupos de IP.

:::image type="content" source="media/quick-create-ipgroup-template/ipgroups.png" alt-text="Grupos de IP.":::

:::image type="content" source="media/quick-create-ipgroup-template/network-rule.png" alt-text="Reglas de red."::::

Para información sobre la sintaxis y las propiedades de JSON de un firewall en una plantilla, consulte la [referencia de plantilla de Microsoft.Network/azureFirewalls](https://docs.microsoft.com/azure/templates/Microsoft.Network/2019-11-01/azureFirewalls).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos que ha creado con el firewall, elimine el grupo de recursos. Esta acción quita el firewall y todos los recursos relacionados.

Para eliminar el grupo de recursos, llame al cmdlet `Remove-AzResourceGroup`:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Implementación y configuración de Azure Firewall en una red híbrida con Azure Portal](tutorial-hybrid-portal.md)