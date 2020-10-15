---
title: Traslado de equilibradores de carga internos de Azure a otra región de Azure mediante Azure Portal
description: Use una plantilla de Azure Resource Manager para trasladar un equilibrador de carga interno de Azure de una región de Azure a otra mediante Azure Portal
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 09/18/2019
ms.author: allensu
ms.openlocfilehash: eb3605249578b15d67bdd9764490d61812b21c18
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84808439"
---
# <a name="move-azure-internal-load-balancer-to-another-region-using-the-azure-portal"></a>Traslado de equilibradores de carga internos de Azure a otra región mediante Azure Portal

Hay varios escenarios en los que quizá quiera trasladar su equilibrador de carga interno actual de una región a otra. Por ejemplo, puede que quiera crear un equilibrador de carga interno con la misma configuración para realizar pruebas. También puede que quiera trasladar un equilibrador de carga interno a otra región como parte del planeamiento para la recuperación ante desastres.

Los equilibradores de carga internos de Azure no se pueden trasladar de una región a otra. Sin embargo, puede usar una plantilla de Azure Resource Manager para exportar la configuración y la red virtual actuales de un equilibrador de carga interno.  Después, puede preparar el recurso para otra región al exportar el equilibrador de carga y la red virtual a una plantilla, modificar los parámetros para que coincidan con la región de destino y, a continuación, implementar la plantilla en la nueva región.  Para más información sobre Resource Manager y las plantillas, consulte [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).


## <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que el equilibrador de carga interno de Azure se encuentra en la región de Azure desde la que va a realizar el traslado.

- Los equilibradores de carga internos de Azure no se pueden trasladar entre regiones.  Tendrá que asociar el nuevo equilibrador de carga a los recursos de la región de destino.

- Necesitará contar con el rol de colaborador de red u otro superior para exportar la configuración de un equilibrador de carga interno e implementar una plantilla para crear un equilibrador de carga interno en otra región.

- Identifique el diseño de red de origen y todos los recursos que está usando actualmente. Este diseño incluye, pero no se limita a los equilibradores de carga, los grupos de seguridad de red, las máquinas virtuales y las redes virtuales.

- Compruebe que su suscripción a Azure permite crear equilibradores de carga internos en la región de destino que se usa. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

- Asegúrese de que la suscripción tiene suficientes recursos para admitir la adición de equilibradores de carga para este proceso.  Vea [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Preparación y traslado
En los pasos siguientes se muestra cómo preparar el equilibrador de carga interno para el traslado mediante una plantilla de Resource Manager y cómo trasladar la configuración del equilibrador de carga interno a la región de destino mediante Azure Portal.  Como parte de este proceso, la configuración de la red virtual del equilibrador de carga interno debe incluirse y debe crearse antes de trasladar el equilibrador de carga interno.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-virtual-network-template-and-deploy-from-the-azure-portal"></a>Exportación de la plantilla de red virtual e implementación desde Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Grupos de recursos**.
2. Seleccione el grupo de recursos que contiene la red virtual de origen y haga clic en él.
3. Seleccione > **Configuración** > **Exportar plantilla**.
4. En la hoja **Exportar plantilla**, elija **Implementar**.
5. Haga clic en **PLANTILLA** > **Editar parámetros** para abrir el archivo **parameters.json** en el editor en línea.
6. Para editar el parámetro correspondiente al nombre de la red virtual, cambie la propiedad **value** de **parameters**:

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
7. Cambie el valor de nombre de la red virtual de origen en el editor por un nombre de su elección para la red virtual de destino. Asegúrese de escribir el nombre entre comillas.

8. Haga clic en **Guardar** en el editor.

9. Haga clic en **Plantilla** > **Editar plantilla** para abrir el archivo **template.json** en el editor en línea.

10. Para editar la región de destino a la que se va a trasladar la red virtual, cambie la propiedad **location** en resources:

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

11. Para obtener los códigos de ubicación de la región, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).  El código de una región es el nombre de la región sin espacios, **Centro de EE. UU.**  = **centralus**.

12. Si quiere, también puede cambiar otros parámetros del archivo **template.json**. Estos son opcionales según sus necesidades:

    * **Espacio de direcciones**: el espacio de direcciones de la red virtual se puede modificar antes de guardar al editar la sección **resources** > **addressSpace** y cambiar la propiedad **addressPrefixes** en el archivo **template.json**:

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

    * **Subred**: es posible modificar o realizar adiciones al nombre de subred y al espacio de direcciones de subred si edita la sección **subnets** del archivo **template.json**. El nombre de la subred se puede cambiar si modifica la propiedad **name**. El espacio de direcciones de subred se puede cambiar si modifica la propiedad **addressPrefix** en el archivo **template.json**:

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

         Para cambiar el prefijo de dirección, debe editar el archivo **template.json** en dos lugares; primero en la sección mencionada anteriormente y después en la sección **type** a continuación.  Cambie la propiedad **addressPrefix** para que coincida con la anterior:

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

13. Haga clic en **Guardar** en el editor en línea.

14. Haga clic en **BÁSICO** > **Suscripción** para elegir la suscripción en la que se implementará la red virtual de destino.

15. Haga clic en **BÁSICO** > **Grupo de recursos** para elegir el grupo de recursos en el que se implementará la red virtual de destino.  Puede hacer clic en **Crear nuevo** para crear un nuevo grupo de recursos para la red virtual de destino.  Asegúrese de que el nombre no es el mismo que el del grupo de recursos de origen de la red virtual existente.

16. Compruebe que **BÁSICO** > **Ubicación** está establecido en la ubicación de destino en la que quiere implementar la red virtual.

17. En **CONFIGURACIÓN**, compruebe que el nombre coincide con el nombre que especificó en el editor de parámetros anterior.

18. Marque la casilla en **TÉRMINOS Y CONDICIONES**.

19. Haga clic en el botón **Comprar** para implementar la red virtual de destino.

### <a name="export-the-internal-load-balancer-template-and-deploy-from-azure-powershell"></a>Exportación de la plantilla del equilibrador de carga interno e implementación desde Azure PowerShell

1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Grupos de recursos**.
2. Seleccione el grupo de recursos que contiene el equilibrador de carga interno de origen y haga clic en él.
3. Seleccione > **Configuración** > **Exportar plantilla**.
4. En la hoja **Exportar plantilla**, elija **Implementar**.
5. Haga clic en **PLANTILLA** > **Editar parámetros** para abrir el archivo **parameters.json** en el editor en línea.

6. Para editar el parámetro del nombre del equilibrador de carga interno, cambie la propiedad **defaultValue** del nombre del equilibrador de carga interno de origen por el nombre del equilibrador de carga interno de destino. Asegúrese de que el nombre se encuentra entre comillas:

    ```json
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "loadBalancers_myLoadBalancer_name": {
            "defaultValue": "<target-internal-lb-name>",
            "type": "String"
             },
            "virtualNetworks_myVNET2_internalid": {
             "defaultValue": "<target-vnet-resource-ID>",
             "type": "String"
             }
    ```

6. Para editar el valor de la red virtual de destino que se traslado anteriormente, primero debe obtener el Id. de recurso y después copiarlo y pegarlo en el archivo **parameters.json**. Para obtener el identificador realice lo siguiente:

    1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Grupos de recursos** en otra pestaña o ventana del explorador.
    2. Busque el grupo de recursos de destino que contiene la red virtual trasladada en los pasos anteriores y haga clic en él.
    3. Seleccione **Configuración** > **Propiedades**.
    4. En la hoja de la derecha, resalte **Id. de recurso** y cópielo en el Portapapeles.  También puede hacer clic en el botón **Copiar en el Portapapeles** a la derecha de la ruta de acceso del **Id. de recurso**.
    5. Pegue el Id. de recurso en la propiedad **defaultValue** del editor **Editar parámetros** que abrió en otra ventana o pestaña del explorador:

        ```json
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "loadBalancers_myLoadBalancer_name": {
            "defaultValue": "<target-internal-lb-name>",
            "type": "String"
             },
            "virtualNetworks_myVNET2_internalid": {
             "defaultValue": "<target-vnet-resource-ID>",
             "type": "String"
             }
        ```
    6. Haga clic en **Guardar** en el editor en línea.

7. Haga clic en **Plantilla** > **Editar plantilla** para abrir el archivo **template.json** en el editor en línea.
8. Para editar la región de destino a la que se va a trasladar la configuración del equilibrador de carga interno, cambie la propiedad **location** en **resources** del archivo **template.json**:

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-internal-lb-region>",
                "sku": {
                    "name": "Standard",
                    "tier": "Regional"
                },
    ```

9.  Para obtener los códigos de ubicación de la región, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).  El código de una región es el nombre de la región sin espacios, **Centro de EE. UU.**  = **centralus**.

10. También puede cambiar otros parámetros de la plantilla si así lo desea; son opcionales según sus requisitos:

    * **SKU**: puede cambiar la SKU del equilibrador de carga interno en la configuración del nivel estándar al básico o viceversa si modifica la propiedad **sku** > **name** en el archivo **template.json**:

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-internal-lb-region>",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
        ```
      Para más información sobre las diferencias entre los equilibradores de carga de la SKU básica y estándar, consulte [Introducción a Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

    * **Reglas de equilibrio de carga**: puede agregar o quitar reglas de equilibrio de carga en la configuración agregando o quitando entradas en la sección **loadBalancingRules** del archivo **template.json**:

        ```json
        "loadBalancingRules": [
                    {
                        "name": "myInboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false,
                            "loadDistribution": "Default",
                            "disableOutboundSnat": true,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolInbound')]"
                            },
                            "probe": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/probes/myHTTPProbe')]"
                            }
                        }
                    }
                ]
        ```
       Para más información sobre las reglas de equilibrio de carga, consulte [¿Qué es Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Sondeos**: puede agregar o quitar sondeos para el equilibrador de carga en la configuración agregando o quitando entradas en la sección **probes** del archivo **template.json**:

        ```json
        "probes": [
                    {
                        "name": "myHTTPProbe",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "Http",
                            "port": 80,
                            "requestPath": "/",
                            "intervalInSeconds": 15,
                            "numberOfProbes": 2
                        }
                    }
                ],
        ```
       Para más información sobre los sondeos de estado de Azure Load Balancer, consulte [Sondeos de estado de Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

    * **Reglas NAT de entrada**: puede agregar o quitar reglas NAT de entrada para el equilibrador de carga agregando o quitando entradas en la sección **inboundNatRules** del archivo **template.json**:

        ```json
        "inboundNatRules": [
                    {
                        "name": "myInboundNATRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 4422,
                            "backendPort": 3389,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false
                        }
                    }
                ]
        ```
        Para completar la adición o eliminación de una regla NAT de entrada, esta debe agregarse o quitarse como una propiedad **type** al final del archivo **template.json**:

        ```json
        {
            "type": "Microsoft.Network/loadBalancers/inboundNatRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('loadBalancers_myLoadBalancer_name'), '/myInboundNATRule')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "frontendIPConfiguration": {
                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                },
                "frontendPort": 4422,
                "backendPort": 3389,
                "enableFloatingIP": false,
                "idleTimeoutInMinutes": 4,
                "protocol": "Tcp",
                "enableTcpReset": false
            }
        }
        ```
        Para más información sobre las reglas NAT de entrada, consulte [¿Qué es Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

12. Haga clic en **Guardar** en el editor en línea.

13. Haga clic en **BÁSICO** > **Suscripción** para elegir la suscripción en la que se implementará el equilibrador de carga interno de destino.

15. Haga clic en **BÁSICO** > **Grupo de recursos** para elegir el grupo de recursos en el que se implementará el equilibrador de carga de destino.  Puede hacer clic en **Crear nuevo** para crear un nuevo grupo de recursos para el equilibrador de carga interno de destino o puede elegir el grupo de recursos existente que se creó anteriormente para la red virtual.  Asegúrese de que el nombre no es el mismo que el del grupo de recursos de origen del equilibrador de carga interno de origen existente.

16. Compruebe que **BÁSICO** > **Ubicación** está establecido en la ubicación de destino en la que quiere implementar el equilibrador de carga interno.

17. En **CONFIGURACIÓN**, compruebe que el nombre coincide con el nombre que especificó en el editor de parámetros anterior.  Compruebe que los Id. de recursos estén especificados para todas las redes virtuales en la configuración.

18. Marque la casilla en **TÉRMINOS Y CONDICIONES**.

19. Haga clic en el botón **Comprar** para implementar la red virtual de destino.

## <a name="discard"></a>Discard (Descartar)

Si quiere descartar la red virtual de destino y el equilibrador de carga interno, elimine el grupo de recursos que contiene la red virtual de destino y el equilibrador de carga interno.  Para ello, en el portal, seleccione el grupo de recursos en el panel y, luego, **Eliminar** en la parte superior de la página de información general.

## <a name="clean-up"></a>Limpieza

Para confirmar los cambios y completar el traslado de la red virtual y el equilibrador de carga interno, elimine la red virtual de origen y el equilibrador de carga interno o el grupo de recursos. Para ello, seleccione la red virtual y el equilibrador de carga interno o grupo de recursos desde el panel en el portal y seleccione **Eliminar** en la parte superior de cada página.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, migró un equilibrador de carga interno de Azure de una región a otra y limpió los recursos de origen.  Para obtener más información sobre cómo trasladar recursos entre regiones y la recuperación ante desastres en Azure, consulte:


- [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Traslado de máquinas virtuales de Azure a otra región](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
