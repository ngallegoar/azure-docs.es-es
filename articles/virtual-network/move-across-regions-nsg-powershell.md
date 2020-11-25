---
title: Traslado del grupo de seguridad de red (NSG) de Azure a otra región de Azure mediante Azure PowerShell
description: Use la plantilla de Azure Resource Manager para trasladar el grupo de seguridad de red de Azure de una región de Azure a otra mediante Azure PowerShell.
author: asudbring
ms.service: virtual-network
ms.topic: how-to
ms.date: 08/31/2019
ms.author: allensu
ms.openlocfilehash: 04abc051cec8a6fb38ce6aa8f5347ae06cb8bd1d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96019759"
---
# <a name="move-azure-network-security-group-nsg-to-another-region-using-azure-powershell"></a>Traslado del grupo de seguridad de red (NSG) de Azure a otra región mediante Azure PowerShell

Hay varios escenarios en los que puede que deba mover los NSG existentes de una región a otra. Por ejemplo, quiere crear un NSG con las mismas reglas de configuración y seguridad para pruebas. También quiere mover un NSG a otra región como parte del planeamiento de la recuperación ante desastres.

Los grupos de seguridad de Azure no se pueden mover de una región a otra. Sin embargo, puede usar una plantilla de Azure Resource Manager para exportar las reglas de seguridad y configuración existentes de un NSG.  Después, puede preparar el recurso en otra región exportando el NSG a una plantilla y modificando los parámetros para que coincidan con la región de destino, y luego implementar la plantilla en la nueva región.  Para más información sobre Resource Manager y sus plantillas, consulte [Exportación de grupos de recursos a plantillas](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates).


## <a name="prerequisites"></a>Requisitos previos

- Asegúrese de que el grupo de seguridad de red de Azure se encuentra en la región de Azure desde la que quiere moverlo.

- Los grupos de seguridad de red de Azure no se pueden mover entre regiones.  Tendrá que asociar el nuevo NSG a los recursos de la región de destino.

- Para exportar una configuración de NSG e implementar una plantilla para crear un NSG en otra región, necesitará el rol de colaborador de red u otro superior.
   
- Identifique el diseño de red de origen y todos los recursos que está usando actualmente. Este diseño incluye, entre otros, los equilibradores de carga, las direcciones IP públicas y las redes virtuales.

- Compruebe que su suscripción de Azure permite crear grupos NSG en la región de destino. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

- Asegúrese de que la suscripción tiene suficientes recursos para admitir la adición de grupos NSG para este proceso.  Vea [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Preparación y traslado
En los pasos siguientes se muestra cómo preparar el grupo de seguridad de red para mover las reglas de configuración y seguridad mediante una plantilla de Resource Manager y cómo mover dichas reglas a la región de destino mediante Azure PowerShell.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-template-and-deploy-from-a-script"></a>Exportación de la plantilla e implementación desde un script

1. Inicie sesión en la suscripción a Azure con el comando [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) y siga las instrucciones de la pantalla:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Obtenga el identificador de recurso del NSG que quiere trasladar a la región de destino y colóquelo en una variable mediante [Get-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/get-aznetworksecuritygroup?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourceNSGID = (Get-AzNetworkSecurityGroup -Name <source-nsg-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Exporte el NSG de origen a un archivo .json en el directorio donde se ejecuta el comando [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0):
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceNSGID -IncludeParameterDefaultValue
   ```

4. El nombre del archivo descargado se asignará en función del grupo de recursos desde el que se exportó el recurso.  Busque el archivo que se exportó con el comando denominado **\<resource-group-name>.json** y ábralo en el editor que prefiera:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

5. Para editar el parámetro del nombre del NSG, cambie el valor **defaultValue** de la propiedad del nombre del NSG de origen por el nombre del NSG de destino. Asegúrese de que el nombre se encuentra entre comillas:
    
    ```json
            {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "networkSecurityGroups_myVM1_nsg_name": {
            "defaultValue": "<target-nsg-name>",
            "type": "String"
            }
        }

    ```


6. Para editar la región de destino donde se van a mover las reglas de configuración y seguridad del NSG, cambie la propiedad **location** en **resources**:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2019-06-01",
            "name": "[parameters('networkSecurityGroups_myVM1_nsg_name')]",
            "location": "<target-region>",
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "2c846acf-58c8-416d-be97-ccd00a4ccd78", 
             }
            }
    ```
  
7. Para obtener los códigos de ubicación de la región, puede usar el cmdlet Azure PowerShell [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) al ejecutar el siguiente comando:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8. Si quiere, también puede cambiar otros parámetros del archivo **\<resource-group-name>.json**. Estos son opcionales según sus necesidades:

    * **Reglas de seguridad**: puede editar las reglas que se implementan en el NSG de destino agregando o quitando reglas en la sección **securityRules** del archivo **\<resource-group-name>.json**:

        ```json
           "resources": [
                  {
                  "type": "Microsoft.Network/networkSecurityGroups",
                  "apiVersion": "2019-06-01",
                  "name": "[parameters('networkSecurityGroups_myVM1_nsg_name')]",
                  "location": "TARGET REGION",
                  "properties": {
                       "provisioningState": "Succeeded",
                       "resourceGuid": "2c846acf-58c8-416d-be97-ccd00a4ccd78",
                  "securityRules": [
                    {
                        "name": "RDP",
                        "etag": "W/\"c630c458-6b52-4202-8fd7-172b7ab49cf5\"",
                        "properties": {
                             "provisioningState": "Succeeded",
                             "protocol": "TCP",
                             "sourcePortRange": "*",
                             "destinationPortRange": "3389",
                             "sourceAddressPrefix": "*",
                             "destinationAddressPrefix": "*",
                             "access": "Allow",
                             "priority": 300,
                             "direction": "Inbound",
                             "sourcePortRanges": [],
                             "destinationPortRanges": [],
                             "sourceAddressPrefixes": [],
                             "destinationAddressPrefixes": []
                            }
                        ]
            }  
            
        ```

        Para completar la adición o eliminación de las reglas en el grupo de seguridad de red de destino, también debe editar los tipos de reglas personalizadas al final del archivo **\<resource-group-name>.json** en el formato del ejemplo siguiente:

        ```json
           {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('networkSecurityGroups_myVM1_nsg_name'), '/Port_80')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_myVM1_nsg_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "protocol": "*",
                "sourcePortRange": "*",
                "destinationPortRange": "80",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 310,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        ```

9. Guarde el archivo **\<resource-group-name>.json**.

10. Cree un grupo de recursos en la región de destino para el NSG de destino que se va a implementar mediante [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0):
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
    
11. Implemente el archivo **\<resource-group-name>.json** editado en el grupo de recursos que creó en el paso anterior mediante [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0):

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Para comprobar que los recursos se crearon en la región de destino, use los comandos [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) y [Get-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/get-aznetworksecuritygroup?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzNetworkSecurityGroup -Name <target-nsg-name> -ResourceGroupName <target-resource-group-name>

    ```

## <a name="discard"></a>Discard (Descartar) 

Después de la implementación, si quiere empezar de nuevo o descartar el NSG en el destino, elimine el grupo de recursos que se creó en el destino y se eliminará el NSG trasladado.  Para quitar el grupo de recursos, use [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>

```

## <a name="clean-up"></a>Limpieza

Para confirmar los cambios y completar el traslado del NSG, elimine el NSG o el grupo de recursos de origen mediante [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) o [Remove-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/remove-aznetworksecuritygroup?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <source-resource-group-name>

```

``` azurepowershell-interactive

Remove-AzNetworkSecurityGroup -Name <source-nsg-name> -ResourceGroupName <source-resource-group-name>

```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha movido un grupo de seguridad de red de Azure de una región a otra y ha limpiado los recursos de origen.  Para obtener más información sobre cómo trasladar recursos entre regiones y la recuperación ante desastres en Azure, consulte:


- [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Traslado de máquinas virtuales de Azure a otra región](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
