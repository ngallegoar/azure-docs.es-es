---
title: Filtrado basado en inteligencia sobre amenazas de Azure Firewall
description: El filtrado basado en inteligencia sobre amenazas puede habilitarse para que el firewall alerte y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: c291dbe9c1eb37e68174a2353e296a376c7d0896
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74168676"
---
# <a name="azure-firewall-threat-intelligence-based-filtering"></a>Filtrado basado en inteligencia sobre amenazas de Azure Firewall

El filtrado basado en inteligencia sobre amenazas puede habilitarse para que el firewall alerte y deniegue el tráfico desde y hacia los dominios y las direcciones IP malintencionados. La direcciones IP y los dominios proceden de la fuente Inteligencia sobre amenazas de Microsoft. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) impulsa la inteligencia sobre amenazas de Microsoft y lo utilizan numerosos servicios, incluido Azure Security Center.

![Inteligencia sobre amenazas de Firewall](media/threat-intel/firewall-threat.png)

Si ha habilitado el filtrado basado en inteligencia sobre amenazas, las reglas asociadas se procesan antes que cualquiera de las reglas NAT, reglas de red o reglas de aplicación.

Puede optar por registrar solo una alerta cuando se desencadena una regla o puede elegir el modo de alerta y denegación.

De forma predeterminada, el filtrado basado en inteligencia sobre amenazas está habilitado en el modo de alerta. No se puede desactivar esta característica o cambiar el modo hasta que la interfaz del portal esté disponible en su región.

![Interfaz del portal del filtrado basado en inteligencia sobre amenazas](media/threat-intel/threat-intel-ui.png)

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

- Consulte [Ejemplos de Log Analytics de Azure Firewall](log-analytics-samples.md)
- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md)
- Revise el [informe de inteligencia de seguridad de Microsoft](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)