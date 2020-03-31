---
title: Movimiento del grupo de seguridad de red (NSG) de Azure a otra región de Azure mediante Azure Portal
description: Use la plantilla de Azure Resource Manager para mover el grupo de seguridad de red de Azure de una región de Azure a otra mediante Azure Portal.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/31/2019
ms.author: allensu
ms.openlocfilehash: dce267178c3caf813ccdcac4bba86ccfde3f3421
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75647193"
---
# <a name="move-azure-network-security-group-nsg-to-another-region-using-the-azure-portal"></a>Movimiento del grupo de seguridad de red (NSG) de Azure a otra región mediante Azure Portal

Hay varios escenarios en los que puede que deba mover los NSG existentes de una región a otra. Por ejemplo, quiere crear un NSG con las mismas reglas de configuración y seguridad para pruebas. También quiere mover un NSG a otra región como parte del planeamiento de la recuperación ante desastres.

Los grupos de seguridad de Azure no se pueden mover de una región a otra. Sin embargo, puede usar una plantilla de Azure Resource Manager para exportar las reglas de seguridad y configuración existentes de un NSG.  Después, puede preparar el recurso en otra región exportando el NSG a una plantilla y modificando los parámetros para que coincidan con la región de destino, y luego implementar la plantilla en la nueva región.  Para más información sobre Resource Manager y las plantillas, consulte [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).


## <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que el grupo de seguridad de red de Azure se encuentra en la región de Azure desde la que quiere moverlo.

- Los grupos de seguridad de red de Azure no se pueden mover entre regiones.  Tendrá que asociar el nuevo NSG a los recursos de la región de destino.

- Para exportar una configuración de NSG e implementar una plantilla para crear un NSG en otra región, necesitará el rol de colaborador de red u otro superior.

- Identifique el diseño de red de origen y todos los recursos que está usando actualmente. Este diseño incluye, entre otros, los equilibradores de carga, las direcciones IP públicas y las redes virtuales.

- Compruebe que su suscripción de Azure permite crear grupos NSG en la región de destino. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

- Asegúrese de que la suscripción tiene suficientes recursos para admitir la adición de grupos NSG para este proceso.  Vea [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Preparación y traslado
En los pasos siguientes se muestra cómo preparar el grupo de seguridad de red para mover las reglas de configuración y seguridad mediante una plantilla de Resource Manager y cómo mover dichas reglas a la región de destino mediante el portal.


### <a name="export-the-template-and-deploy-from-the-portal"></a>Exportación de la plantilla e implementación desde el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Grupos de recursos**.
2. Busque el grupo de recursos que contiene el NSG de origen y haga clic en él.
3. Seleccione > **Configuración** > **Exportar plantilla**.
4. En la hoja **Exportar plantilla**, elija **Implementar**.
5. Haga clic en **PLANTILLA** > **Editar parámetros** para abrir el archivo **parameters.json** en el editor en línea.
6. Para editar el parámetro del nombre de NSG, cambie la propiedad **value** en **parameters**:

    ```json
            {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
            "networkSecurityGroups_myVM1_nsg_name": {
               "value": "<target-nsg-name>"
                }
               }
            }
    ```

7. Cambie el valor del NSG de origen en el editor por un nombre de su elección para el NSG de destino. Asegúrese de escribir el nombre entre comillas.

8.  Haga clic en **Guardar** en el editor.

9.  Haga clic en **Plantilla** > **Editar plantilla** para abrir el archivo **template.json** en el editor en línea.

10. Para editar la región de destino donde se van a mover las reglas de configuración y seguridad de NSG, vaya al editor en línea y cambie la propiedad **location** en **resources**:

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
           ]

    ```

11. Para obtener los códigos de ubicación de la región, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).  El código de una región es el nombre de la región sin espacios, **Centro de EE. UU.**  = **centralus**.

12. También puede cambiar otros parámetros de la plantilla si así lo desea; son opcionales según sus requisitos:

    * **Reglas de seguridad**: puede editar las reglas que se implementan en el NSG de destino agregando o quitando reglas en la sección **securityRules** del archivo **template.json**:

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
                    },
                ]
            }
        ```

      Para completar la adición o la eliminación de las reglas en el NSG de destino, también debe editar los tipos de reglas personalizadas al final del archivo **template.json** en el formato del ejemplo siguiente:

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

13. Haga clic en **Guardar** en el editor en línea.

14. Haga clic en **Aspectos básicos** > **Suscripción** para elegir la suscripción en la que se implementará el NSG de destino.

15. Haga clic en **Aspectos básicos** > **Grupo de recursos** para elegir el grupo de recursos donde se implementará el NSG de destino.  Puede hacer clic en **Crear nuevo** para crear un grupo de recursos para el NSG de destino.  Asegúrese de que el nombre no sea el mismo que el del grupo de recursos de origen del NSG existente.

16. Compruebe que **Aspectos básicos** > **Ubicación** está establecido en la ubicación de destino en la que quiere que se implemente el NSG.

17. En **CONFIGURACIÓN**, compruebe que el nombre coincide con el nombre que especificó en el editor de parámetros anterior.

18. Marque la casilla en **TÉRMINOS Y CONDICIONES**.

19. Haga clic en el botón **Comprar** para implementar el grupo de seguridad de red de destino.

## <a name="discard"></a>Discard (Descartar)

Si quiere descartar el NSG de destino, elimine el grupo de recursos que lo contiene.  Para ello, en el portal, seleccione el grupo de recursos en el panel y, luego, **Eliminar** en la parte superior de la página de información general.

## <a name="clean-up"></a>Limpieza

Para confirmar los cambios y completar el movimiento del NSG, elimine el NSG de origen o el grupo de recursos. Para ello, en el portal, seleccione el grupo de seguridad de red o el grupo de recursos en el panel y, luego, **Eliminar** en la parte superior de cada página.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha movido un grupo de seguridad de red de Azure de una región a otra y ha limpiado los recursos de origen.  Para obtener más información sobre cómo trasladar recursos entre regiones y la recuperación ante desastres en Azure, consulte:


- [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Traslado de máquinas virtuales de Azure a otra región](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
