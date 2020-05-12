---
title: 'Inicio rápido: Creación de una instancia de Azure Firewall con Availability Zones: plantilla de Resource Manager'
description: Implementación de Azure Firewall mediante una plantilla. La red virtual tiene una red virtual con tres subredes. Se implementan dos máquinas virtuales con Windows Server; un jumpbox y un servidor.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.date: 04/30/2020
ms.author: victorh
ms.openlocfilehash: 17ab693033b61c25ba2f5b5bd588ef52caf8c046
ms.sourcegitcommit: acc558d79d665c8d6a5f9e1689211da623ded90a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82597712"
---
# <a name="quickstart-deploy-azure-firewall-with-availability-zones---resource-manager-template"></a>Inicio rápido: Implementación de Azure Firewall con Availability Zones: plantilla de Resource Manager

En este inicio rápido, se usa una plantilla de Resource Manager para implementar una instancia de Azure Firewall en tres instancias de Availability Zones. 

Esta plantilla crea un entorno de red de prueba con un firewall. La red tiene una red virtual (VNet) con tres subredes: *AzureFirewallSubnet*, *ServersSubnet* y *JumpboxSubnet*. Cada una de las subredes *ServersSubnet* y *JumpboxSubnet* tiene una única máquina virtual Windows Server de doble núcleo.

El firewall está en la subred *AzureFirewallSubnet* y tiene una colección de reglas de aplicación con una única regla que permite el acceso a `www.microsoft.com`.

Una ruta definida por el usuario señala el tráfico de red desde la subred *ServersSubnet* a través del firewall, en donde se aplican las reglas de firewall.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Para más información sobre Azure Firewall, vea [Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md).

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-azure-firewall-with-availability-zones"></a>Creación de una instancia de Azure Firewall con Availability Zones

Esta plantilla crea una instancia de Azure Firewall con Availability Zones, junto con los recursos necesarios para admitir Azure Firewall.

### <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-azurefirewall-with-zones-sandbox/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-azurefirewall-with-zones-sandbox/azuredeploy.json" range="001-444" highlight="369-442":::

En la plantilla se definen varios recursos de Azure:

- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines):
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft.Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft.Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft.Network/routeTables**](/azure/templates/microsoft.network/routeTables)

### <a name="deploy-the-template"></a>Implementación de la plantilla

Implemente la plantilla de Resource Manager en Azure:

1. Seleccione **Implementar en Azure** para iniciar sesión en Azure y abrir la plantilla. La plantilla crea una instancia de Azure Firewall, la infraestructura de red y dos máquinas virtuales.

   [![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurefirewall-with-zones-sandbox%2Fazuredeploy.json)

2. En el portal, en la página **Create a sandbox setup of Azure Firewall** (Crear una configuración de espacio aislado de Azure Firewall), escriba o seleccione los valores siguientes:
   - **Grupo de recursos**: seleccione **Crear nuevo**, escriba un nombre para el grupo de recursos y seleccione **Aceptar**. 
   - **Nombre de la red virtual**: escriba un nombre para la red virtual nueva. 
   - **Nombre de usuario administrador**: escriba un nombre de usuario para la cuenta de usuario de administrador.
   - **Contraseña de administrador**: escriba una contraseña de administrador. 

3. Lea los términos y condiciones, y seleccione **Acepto los términos y condiciones indicados anteriormente** y, después, seleccione **Comprar**. La implementación puede tardar 10 minutos o más en completarse.

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

Explore los recursos que se crearon con el firewall.

Para información sobre la sintaxis y las propiedades de JSON de un firewall en una plantilla, consulte [Microsoft.Network/azureFirewalls](/azure/templates/microsoft.network/azurefirewalls).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede quitar el grupo de recursos, el firewall y todos los recursos relacionados mediante la ejecución del comando de PowerShell `Remove-AzResourceGroup`. Para quitar un grupo de recursos llamado *MyResourceGroup*, ejecute: 

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyResourceGroup
```
Si planea continuar con el tutorial de supervisión de firewall, no quite el firewall ni el grupo de recursos. 

## <a name="next-steps"></a>Pasos siguientes

A continuación, puede supervisar los registros de Azure Firewall:

[Tutorial: Supervisión de los registros de Azure Firewall](./tutorial-diagnostics.md)
