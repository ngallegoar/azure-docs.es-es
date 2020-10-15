---
title: Traslado de una red virtual de Azure a otra región de Azure mediante Azure Portal
description: Traslade una red virtual de Azure de una región de Azure a otra mediante una plantilla de Resource Manager y Azure Portal.
author: asudbring
ms.service: virtual-network
ms.topic: how-to
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: ed3da649ba65484a79b42ba5bb45431839e123d4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84711449"
---
# <a name="move-an-azure-virtual-network-to-another-region-by-using-the-azure-portal"></a>Traslado de una red virtual de Azure a otra región mediante Azure Portal

Hay varios escenarios para mover una red virtual de Azure existente de una región a otra. Por ejemplo, puede que desee crear una red virtual con la misma configuración que la red virtual existente para realizar pruebas y conseguir disponibilidad. También es posible que quiera mover una red virtual de producción a otra región como parte del planeamiento de la recuperación ante desastres.

Puede usar una plantilla de Azure Resource Manager para completar el traslado de la red virtual a otra región. Para ello, exporte la red virtual a una plantilla, modifique los parámetros para que coincidan con la región de destino y, a continuación, implemente la plantilla en la región nueva. Para más información sobre las plantillas de Resource Manager, consulte [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).


## <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que la red virtual se encuentra en la región de Azure desde la que desea moverla.

- Para exportar una red virtual e implementar una plantilla para crear una red virtual en otra región, necesitará tener el rol Colaborador de red u otro superior.

- Los emparejamientos de redes virtuales no se volverán a crear y se producirá un error si todavía están presentes en la plantilla. Antes de exportar la plantilla, debe quitar todos los emparejamientos de red virtual. Podrá restablecerlos después de mover la red virtual.

- Identifique el diseño de red de origen y todos los recursos que está usando actualmente. Este diseño incluye, pero no se limita a los equilibradores de carga, grupos de seguridad de red (NSG), e IP públicas.

- Compruebe que la suscripción de Azure le permite crear redes virtuales en la región de destino. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

- Asegúrese de que la suscripción tiene suficientes recursos para admitir la incorporación de redes virtuales para este proceso. Para más información, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-for-the-move"></a>Preparación para el traslado
En esta sección va a preparar la red virtual para el traslado mediante el uso de una plantilla de Resource Manager. Después moverá la red virtual a la región de destino mediante Azure Portal.

Para exportar la red virtual e implementar la red virtual de destino mediante Azure Portal, haga lo siguiente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después seleccione **Grupos de recursos**.
1. Busque el grupo de recursos que contiene la red virtual de origen y selecciónelo.
1. Seleccione **Configuración** > **Exportar plantilla**.
1. En el panel **Exportar plantilla**, seleccione **Implementar**.
1. Para abrir el archivo *parameters.json* en el editor en línea, seleccione **Plantilla** > **Editar parámetros**.
1. Para editar el parámetro correspondiente al nombre de la red virtual, cambie la propiedad **value** de **parameters**:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "virtualNetworks_myVNET1_name": {
                "value": "<target-virtual-network-name>"
            }
        }
    }
    ```

1. En el editor, cambie el valor de nombre de la red virtual de origen en el editor por el nombre que desee para la red virtual de destino. Asegúrese de incluir el nombre entre comillas.

1. Seleccione **Guardar** en el editor.

1. Para abrir el archivo *template.json* en el editor en línea, seleccione **Plantilla** > **Editar plantilla**.

1. En el editor en línea, para editar la región de destino a la que se va a trasladar la red virtual, cambie la propiedad **location** en **resources**:

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

1. Para obtener los códigos de ubicación de la región, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/). El código de una región es el nombre en inglés de la región sin espacios (por ejemplo, Centro de EE. UU. es **Central US** = **centralus**).

1. (Opcional) También puede cambiar otros parámetros de la plantilla, según sus requisitos:

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

    * **Subred**: puede cambiar o agregar el nombre de subred y el espacio de direcciones de subred si cambia la sección **subnets** de la plantilla. Para cambiar el nombre de la subred, cambie la propiedad **name**. Para cambiar el espacio de direcciones de subred, cambie la propiedad **addressPrefix**:

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

        Para cambiar el prefijo de dirección en el archivo *template.json*, debe editarlo en dos lugares: en el código de la sección anterior y en la sección **type** del siguiente código. Cambie la propiedad **addressPrefix** del siguiente código para que coincida con la propiedad **addressPrefix** del código de la sección anterior.

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

1. En el editor en línea, seleccione **Guardar**.

1. Para elegir la suscripción donde se implementará la red virtual de destino, seleccione **Datos básicos** > **Suscripción**.

1. Para elegir el grupo de recursos donde se implementará la red virtual de destino, seleccione **Datos básicos** > **Grupo de recursos**. 

    Si necesita crear un nuevo grupo de recursos para la red virtual de destino, seleccione **Crear nuevo**. Asegúrese de que el nombre no es el mismo que el nombre del grupo de recursos de origen en la red virtual existente.

1. Compruebe que **Datos básicos** > **Ubicación** está establecido en la ubicación de destino en la que quiere implementar la red virtual.

1. En **Configuración**, compruebe que el nombre coincide con el nombre que especificó anteriormente en el editor de parámetros.

1. Active la casilla **Términos y condiciones**.

1. Para implementar la red virtual de destino, seleccione **Adquirir**.

## <a name="delete-the-target-virtual-network"></a>Eliminación de la red virtual de destino

Si quiere descartar la red virtual de destino, elimine el grupo de recursos que la contiene. Para ello:
1. En el panel de Azure Portal, seleccione el grupo de recursos.
1. En la parte superior del panel **Información general**, seleccione **Eliminar**.

## <a name="clean-up"></a>Limpieza

Para confirmar los cambios y completar el traslado de la red virtual, elimine la red virtual o el grupo de recursos de origen. Para ello:
1. En el panel de Azure Portal, seleccione la red virtual o el grupo de recursos.
1. En la parte superior de cada panel, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha movido una red virtual de Azure de una región a otra mediante Azure Portal y, a continuación, ha limpiado los recursos de origen innecesarios. Para más información sobre el traslado de recursos entre regiones y la recuperación ante desastres en Azure, consulte:


- [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Traslado de máquinas virtuales de Azure a otra región](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
