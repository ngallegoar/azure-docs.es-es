---
title: Traslado de una red virtual de Azure a otra región de Azure mediante Azure PowerShell
description: Traslade una red virtual de Azure de una región de Azure a otra mediante una plantilla de Resource Manager y Azure PowerShell.
author: asudbring
ms.service: virtual-network
ms.topic: how-to
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: e13164c3ec6049a8ae3954528a02d20e313dd883
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84711466"
---
# <a name="move-an-azure-virtual-network-to-another-region-by-using-azure-powershell"></a>Traslado de una red virtual de Azure a otra región mediante Azure PowerShell

Hay varios escenarios para mover una red virtual de Azure existente de una región a otra. Por ejemplo, puede que desee crear una red virtual con la misma configuración que la red virtual existente para realizar pruebas y conseguir disponibilidad. También es posible que quiera mover una red virtual de producción a otra región como parte del planeamiento de la recuperación ante desastres.

Puede usar una plantilla de Azure Resource Manager para completar el traslado de la red virtual a otra región. Para ello, exporte la red virtual a una plantilla, modifique los parámetros para que coincidan con la región de destino y, a continuación, implemente la plantilla en la región nueva. Para más información sobre las plantillas de Resource Manager, consulte [Exportación de grupos de recursos a plantillas](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates).


## <a name="prerequisites"></a>Requisitos previos

- Asegúrese de que la red virtual se encuentra en la región de Azure desde la que desea moverla.

- Para exportar una red virtual e implementar una plantilla para crear una red virtual en otra región, necesitará tener el rol Colaborador de red u otro superior.

- Los emparejamientos de redes virtuales no se volverán a crear y se producirá un error si todavía están presentes en la plantilla. Antes de exportar la plantilla, debe quitar todos los emparejamientos de red virtual. Podrá restablecerlos después de mover la red virtual.
    
- Identifique el diseño de red de origen y todos los recursos que está usando actualmente. Este diseño incluye, pero no se limita a los equilibradores de carga, grupos de seguridad de red (NSG), e IP públicas.

- Compruebe que la suscripción de Azure le permite crear redes virtuales en la región de destino. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

- Asegúrese de que la suscripción tiene suficientes recursos para admitir la incorporación de redes virtuales para este proceso. Para más información, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-for-the-move"></a>Preparación para el traslado
En esta sección va a preparar la red virtual para el traslado mediante el uso de una plantilla de Resource Manager. Después moverá la red virtual a la región de destino mediante comandos de Azure PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para exportar la red virtual e implementar la red virtual de destino mediante PowerShell, haga lo siguiente:

1. Inicie sesión en la suscripción de Azure con el comando [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) y siga las instrucciones de la pantalla:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

1. Obtenga el identificador de recurso de la red virtual que quiere mover a la región de destino y colóquelo en una variable mediante [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourceVNETID = (Get-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>).Id
    ```

1. Exporte la red virtual de origen a un archivo .json en el directorio donde ejecute el comando [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0):
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

1. El archivo descargado tendrá el mismo nombre que el grupo de recursos desde el que se exportó el recurso. Busque el archivo *\<resource-group-name>.json*, que exportó con el comando y, a continuación, ábralo en el editor:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

1. Para editar el parámetro del nombre de la red virtual, cambie la propiedad **defaultValue** del nombre de la red virtual de origen por el nombre de la red virtual de destino. Asegúrese de incluir el nombre entre comillas.
    
    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentmyResourceGroupVNET.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_myVNET1_name": {
        "defaultValue": "<target-virtual-network-name>",
        "type": "String"
        }
    ```

1. Para editar la región de destino a la que se va a trasladar la red virtual, cambie la propiedad **location** en resources:

    ```json
    "resources": [
                {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region>",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                        "addressSpace": {
                            "addressPrefixes": [
                                "10.0.0.0/16"
                            ]
                        },

    ```
  
1. Para obtener los códigos de ubicación de la región, puede usar el cmdlet Azure PowerShell [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) al ejecutar el siguiente comando:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    ```

1. (Opcional) También puede cambiar otros parámetros del archivo *\<resource-group-name>.json*, según sus requisitos:

    * **Espacio de direcciones**: antes de guardar el archivo, puede modificar el espacio de direcciones de la red virtual; para ello, modifique la sección **resources** > **addressSpace** y cambie la propiedad **addressPrefixes**:

        ```json
                "resources": [
                    {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                    "addressSpace": {
                        "addressPrefixes": [
                        "10.0.0.0/16"
                        ]
                    },
        ```

    * **Subred**: puede cambiar o agregar el nombre de subred y el espacio de direcciones de subred si cambia la sección **subnets**. Para cambiar el nombre de la subred, cambie la propiedad **name**. Para cambiar el espacio de direcciones de subred, cambie la propiedad **addressPrefix**:

        ```json
                "subnets": [
                    {
                    "name": "subnet-1",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.0.0/24",
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    },
                    {
                    "name": "GatewaySubnet",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.1.0/29",
                    "serviceEndpoints": [],
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    }

                ]
        ```

        Para cambiar el prefijo de dirección, edite el archivo en dos lugares: en el código de la sección anterior y en la sección **type** del siguiente código. Cambie la propiedad **addressPrefix** del siguiente código para que coincida con la propiedad **addressPrefix** del código de la sección anterior.

        ```json
         "type": "Microsoft.Network/virtualNetworks/subnets",
           "apiVersion": "2019-06-01",
           "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/GatewaySubnet')]",
              "dependsOn": [
                 "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                   ],
              "properties": {
                 "provisioningState": "Succeeded",
                 "addressPrefix": "10.0.1.0/29",
                 "serviceEndpoints": [],
                 "delegations": [],
                 "privateEndpointNetworkPolicies": "Enabled",
                 "privateLinkServiceNetworkPolicies": "Enabled"
                  }
                 },
                  {
                  "type": "Microsoft.Network/virtualNetworks/subnets",
                  "apiVersion": "2019-06-01",
                  "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/subnet-1')]",
                     "dependsOn": [
                        "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                          ],
                     "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "10.0.0.0/24",
                        "delegations": [],
                        "privateEndpointNetworkPolicies": "Enabled",
                        "privateLinkServiceNetworkPolicies": "Enabled"
                         }
                  }
         ]
        ```

1. Guarde el archivo *\<resource-group-name>.json*.

1. Cree un grupo de recursos en la región de destino para la red virtual de destino que se va a implementar, mediante [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0):
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
    
1. Implemente el archivo *\<resource-group-name>.json* editado en el grupo de recursos que creó en el paso anterior mediante el recurso [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0):

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    ```

1. Para comprobar que los recursos se crearon en la región de destino, use [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) y [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>
    ```

    ```azurepowershell-interactive

    Get-AzVirtualNetwork -Name <target-virtual-network-name> -ResourceGroupName <target-resource-group-name>
    ```

## <a name="delete-the-virtual-network-or-resource-group"></a>Eliminación de la red virtual o el grupo de recursos 

Después de la implementación de la red virtual, si quiere empezar de nuevo o descartar la red virtual en la región de destino, elimine el grupo de recursos que creó en la región de destino; esto eliminará la red virtual trasladada. 

Para quitar el grupo de recursos, use [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>
```

## <a name="clean-up"></a>Limpieza

Para confirmar los cambios y completar el traslado de la red virtual, realice una de las acciones siguientes:

* Elimine el grupo de recursos con [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0):

    ```azurepowershell-interactive

    Remove-AzResourceGroup -Name <source-resource-group-name>
    ```

* Elimine la red virtual de origen con [Remove-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork?view=azps-2.6.0):  
    ``` azurepowershell-interactive

    Remove-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha movido una red virtual de una región a otra mediante PowerShell y, a continuación, ha limpiado los recursos de origen innecesarios. Para más información sobre el traslado de recursos entre regiones y la recuperación ante desastres en Azure, consulte:

- [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Traslado de máquinas virtuales de Azure a otra región](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
