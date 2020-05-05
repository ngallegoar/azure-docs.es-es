---
title: Configuración de reglas de firewall de IP para Azure Service Bus
description: Uso de las reglas de firewall para permitir las conexiones desde direcciones IP específicas a Azure Service Bus.
services: service-bus
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 12/20/2019
ms.author: aschhab
ms.openlocfilehash: 9601689bbce9566b52664058911e9c45647152d6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82116825"
---
# <a name="configure-ip-firewall-rules-for-azure-service-bus"></a>Configuración de reglas de firewall de IP para Azure Service Bus
De forma predeterminada, los espacios de nombres de Service Bus son accesibles desde Internet, siempre que la solicitud venga con una autenticación y una autorización válidas. Con el firewall de IP, puede restringirlo aún más a solo un conjunto de direcciones o intervalos de direcciones IPv4 en notación [CIDR (Enrutamiento de interdominios sin clases)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

Esta característica es útil en escenarios en los que Azure Service Bus debe ser accesible únicamente desde ciertos sitios conocidos. Las reglas de firewall permiten configurar reglas para aceptar el tráfico procedente de direcciones IPv4 concretas. Por ejemplo, si usa Service Bus con [Azure ExpressRoute][express-route], puede crear una **regla de firewall** para permitir el tráfico procedente únicamente de las direcciones IP de la infraestructura local o de las direcciones de una puerta de enlace de NAT corporativa. 

> [!IMPORTANT]
> Solo se admiten firewalls y redes virtuales en el nivel **premium** de Service Bus. Si no considera la posibilidad de actualizar al nivel **Premier**, se recomienda que proteja el token de firma de acceso compartido (SAS) y que lo comparta solo con usuarios autorizados. Para más información sobre la autenticación de SAS, consulte [Autenticación y autorización](service-bus-authentication-and-authorization.md#shared-access-signature).

## <a name="ip-firewall-rules"></a>Reglas de firewall de IP
Las reglas de firewall de IP se aplican en el nivel de espacio de nombres de Service Bus. Por lo tanto, las reglas se aplican a todas las conexiones de clientes que usan cualquier protocolo admitido. Cualquier intento de conexión desde una dirección IP que no coincida con una regla IP admitida en el espacio de nombres de Service Bus se rechaza como no autorizado. La respuesta no menciona la regla IP. Las reglas de filtro IP se aplican en orden y la primera regla que coincida con la dirección IP determina la acción de aceptar o rechazar.

>[!WARNING]
> La implementación de reglas de firewall puede evitar que otros servicios de Azure interactúen con Service Bus.
>
> Los servicios de confianza de Microsoft no se admiten cuando se implementa el filtro de IP (reglas de firewall), pero estarán disponibles muy pronto.
>
> Estos son los escenarios comunes de Azure que no funcionan con el filtro de IP (tenga en cuenta que la lista **NO** está completa).
> - Integración con Azure Event Grid
> - Enrutamientos de Azure IoT Hub
> - Azure IoT Device Explorer
>
> Los siguientes servicios de Microsoft deben estar en una red virtual
> - Azure App Service
> - Azure Functions

## <a name="use-azure-portal"></a>Usar Azure Portal
En esta sección se muestra cómo usar Azure Portal para crear reglas de firewall de IP para un espacio de nombres de Service Bus. 

1. Vaya al **espacio de nombres de Service Bus** en [Azure Portal](https://portal.azure.com).
2. En el menú de la izquierda, seleccione la opción **Redes**. De forma predeterminada, está seleccionada la opción **Todas las redes**. El espacio de nombres de Service Bus acepta conexiones desde cualquier dirección IP. Esta configuración predeterminada es equivalente a una regla que acepta el intervalo de direcciones IP 0.0.0.0/0. 

    ![Firewall: opción Todas las redes seleccionada](./media/service-bus-ip-filtering/firewall-all-networks-selected.png)
1. Elija la opción **Redes seleccionadas** en la parte superior de la página. En la sección **Firewall**, haga lo siguiente:
    1. Seleccione la opción **Agregar la dirección IP del cliente** para dar acceso a esa IP de cliente actual al espacio de nombres. 
    2. En **Intervalo de direcciones**, escriba una dirección IPv4 específica o un intervalo de direcciones IPv4 en notación CIDR. 
    3. Especifique si quiere **permitir que los servicios de confianza de Microsoft omitan este firewall**. 

        ![Firewall: opción Todas las redes seleccionada](./media/service-bus-ip-filtering/firewall-selected-networks-trusted-access-disabled.png)
3. Seleccione **Guardar** en la barra de herramientas para guardar la configuración. Espere unos minutos hasta que la confirmación se muestre en las notificaciones de Azure Portal.

## <a name="use-resource-manager-template"></a>Uso de plantillas de Resource Manager
En esta sección se incluye una plantilla de ejemplo de Azure Resource Manager que crea una red virtual y una regla de firewall.


La siguiente plantilla de Resource Manager permite agregar una regla de red virtual a un espacio de nombres de Service Bus.

Parámetros de plantilla:

- **ipMask** es una única dirección IPv4 o un bloque de direcciones IP en la notación CIDR. Por ejemplo, en notación CIDR 70.37.104.0/24 representa las 256 direcciones IPv4 de 70.37.104.0 a 70.37.104.255, donde 24 indica el número de bits de prefijo significativos para el intervalo.

> [!NOTE]
> Si bien no hay reglas de denegación posibles, la plantilla de Azure Resource Manager tiene la acción predeterminada establecida en **"Permitir"** , que no restringe las conexiones.
> Cuando se realizan las reglas de Virtual Network o de firewall, es necesario cambiar el valor ***"defaultAction"***
> 
> desde
> ```json
> "defaultAction": "Allow"
> ```
> to
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "servicebusNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Service Bus namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('servicebusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Premium",
          "tier": "Premium"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,          
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Para implementar la plantilla, siga las instrucciones para [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Pasos siguientes

Para restringir el acceso a Service Bus a redes virtuales de Azure, visite el siguiente vínculo:

- [Puntos de conexión de servicio de red virtual para Service Bus][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
