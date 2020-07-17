---
title: Configuración de inteligencia sobre amenazas de Azure Firewall
description: El filtrado basado en inteligencia sobre amenazas puede habilitarse para que el firewall alerte y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: article
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 136ceeb271bec29bdbfc4572626936ee67f05556
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568130"
---
# <a name="azure-firewall-threat-intelligence-configuration"></a>Configuración de inteligencia sobre amenazas de Azure Firewall

El filtrado basado en inteligencia sobre amenazas puede configurarse para que una directiva de Azure Firewall envíe una alerta y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) impulsa la inteligencia sobre amenazas de Microsoft y lo utilizan numerosos servicios, incluido Azure Security Center.<br>

Si ha configurado el filtrado basado en inteligencia sobre amenazas, las reglas asociadas se procesan antes que cualquiera de las reglas NAT, reglas de red o reglas de aplicación.

:::image type="content" source="media/threat-intelligence-settings/threat-intelligence-policy.png" alt-text="Directiva de inteligencia sobre amenazas":::

## <a name="threat-intelligence-mode"></a>Modo de inteligencia sobre amenazas

Puede optar por registrar solo una alerta cuando se desencadene una regla o puede elegir el modo de alerta y denegación.

De forma predeterminada, el filtrado basado en inteligencia sobre amenazas está habilitado en el modo de alerta.

## <a name="allowed-list-addresses"></a>Lista de direcciones permitidas

Puede configurar una lista de direcciones IP permitidas para que la inteligencia sobre amenazas no filtre ninguna de las direcciones, rangos o subredes que especifique.



## <a name="logs"></a>Registros

El siguiente extracto del registro muestra una regla desencadenada:

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Prueba

- **Pruebas de salida**: las alertas de tráfico de salida deben ser un caso poco habitual, ya que significa que se ha puesto en peligro su entorno. Para ayudar a probar que las alertas de salida funcionan, se ha creado un FQDN de prueba que desencadena una alerta. Use **testmaliciousdomain.eastus.cloudapp.azure.com** para las pruebas de salida.

- **Pruebas de entrada**: puede esperar ver las alertas en el tráfico de entrada si se configuran reglas DNAT en el firewall. Esto es cierto incluso si solo se permiten orígenes específicos en la regla DNAT y se deniega el tráfico de otra manera. Azure Firewall no alerta sobre todos los escáneres de puerto conocidos, solo sobre aquellos que se sabe que también participan en actividades malintencionadas.

## <a name="next-steps"></a>Pasos siguientes

- Revise el [informe de inteligencia de seguridad de Microsoft](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)