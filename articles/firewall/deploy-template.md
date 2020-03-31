---
title: Implementación de Azure Firewall mediante una plantilla
description: Implementación de Azure Firewall mediante una plantilla. La red creada tiene una red virtual con tres subredes. Se implementan dos máquinas virtuales con Windows Server de dos núcleos.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: b9e82998283bf44eede88148a12d88aae521b1ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74169194"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Implementación de Azure Firewall mediante una plantilla

La [plantilla para crear la configuración de un espacio aislado de Azure Firewall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-with-zones-sandbox) crea un entorno de red de prueba con un firewall. La red tiene una red virtual (VNet) con tres subredes: *AzureFirewallSubnet*, *ServersSubnet* y *JumpboxSubnet*. Cada una de las subredes *ServersSubnet* y *JumpboxSubnet* tiene una única máquina virtual Windows Server de doble núcleo.

El firewall está en la subred *AzureFirewallSubnet* y tiene una colección de reglas de aplicación con una única regla que permite el acceso a `www.microsoft.com`.

Una ruta definida por el usuario señala el tráfico de red desde la subred *ServersSubnet* a través del firewall, en donde se aplican las reglas de firewall.

Para más información sobre Azure Firewall, vea [Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-the-template-to-deploy-azure-firewall"></a>Uso de la plantilla para implementar Azure Firewall

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

**Para instalar e implementar Azure Firewall con la plantilla**:

1. Acceda a la plantilla en [https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-with-zones-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-with-zones-sandbox).
   
1. Lea la introducción y, cuando esté listo para implementar, seleccione **Implementar en Azure**.
   
1. Inicie sesión en Azure Portal si es necesario. 

1. En el portal, en la página **Create a sandbox setup of AzureFirewall** (Crear una configuración de espacio aislado de Azure Firewall), escriba o seleccione los valores siguientes:
   
   - **Grupo de recursos**: seleccione **Crear nuevo**, escriba un nombre para el grupo de recursos y seleccione **Aceptar**. 
   - **Nombre de la red virtual**: escriba un nombre para la red virtual nueva. 
   - **Nombre de usuario administrador**: escriba un nombre de usuario para la cuenta de usuario de administrador.
   - **Contraseña de administrador**: escriba una contraseña de administrador. 
   
1. Consulte los términos y condiciones y seleccione **Acepto los términos y condiciones indicados anteriormente**.
   
1. Seleccione **Comprar**.
   
   La creación de los recursos tardará unos minutos. 
   
1. Explore los recursos que se crearon con el firewall. 

Para información sobre la sintaxis y las propiedades de JSON de un firewall en una plantilla, consulte [Microsoft.Network/azureFirewalls](/azure/templates/microsoft.network/azurefirewalls).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede quitar el grupo de recursos, el firewall y todos los recursos relacionados mediante la ejecución del comando de PowerShell [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup). Para quitar un grupo de recursos llamado *MyResourceGroup*, ejecute: 

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyResourceGroup
```
Si pretende continuar con el tutorial de supervisión de firewall, no quite aún el firewall ni el grupo de recursos. 

## <a name="next-steps"></a>Pasos siguientes

A continuación, puede supervisar los registros de Azure Firewall:

> [!div class="nextstepaction"]
> [Tutorial: Supervisión de los registros de Azure Firewall](./tutorial-diagnostics.md)
