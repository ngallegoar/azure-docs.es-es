---
title: Hospedaje de varios sitios en Azure Application Gateway
description: Este artículo proporciona información general de la compatibilidad multisitio de Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 03/11/2020
ms.author: amsriva
ms.topic: conceptual
ms.openlocfilehash: 4d945a255dacd35c61c3c80574b7d46b56de4aab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80257417"
---
# <a name="application-gateway-multiple-site-hosting"></a>Hospedaje de varios sitios de Application Gateway

El hospedaje de varios sitios permite configurar más de una aplicación web en el mismo puerto de una puerta de enlace de aplicaciones. Esta característica permite configurar una topología más eficaz para las implementaciones al agregar hasta 100 sitios web en una puerta de enlace de aplicaciones. Cada sitio web se puede dirigir a su propio grupo de back-end. En el ejemplo siguiente, la puerta de enlace de aplicaciones atiende el tráfico de `contoso.com` y `fabrikam.com` desde dos grupos de servidores back-end llamados ContosoServerPool y FabrikamServerPool.

![imageURLroute](./media/multiple-site-overview/multisite.png)

> [!IMPORTANT]
> La reglas se procesan en el orden en que aparecen en el portal para la SKU v1. En el caso de la SKU v2, las coincidencias exactas tienen mayor prioridad. Es muy recomendable configurar a los agentes de escucha multisitio antes de configurar un agente de escucha básico.  De esta forma se asegura de que el tráfico se enruta al back-end adecuado. Si un agente de escucha básico aparece en primer lugar y coincide con una solicitud entrante, lo procesa ese agente de escucha.

Las solicitudes para `http://contoso.com` se enrutan a ContosoServerPool y para `http://fabrikam.com` se enrutan a FabrikamServerPool.

De forma similar, puede hospedar varios subdominios del mismo dominio principal en la misma implementación de la puerta de enlace de aplicaciones. Por ejemplo, puede hospedar `http://blog.contoso.com` y `http://app.contoso.com` en la misma implementación de puerta de enlace de aplicaciones.

## <a name="host-headers-and-server-name-indication-sni"></a>Encabezados de host e Indicación de nombre de servidor (SNI)

Existen tres mecanismos comunes para habilitar el hospedaje de varios sitios en la misma infraestructura.

1. Hospede varias aplicaciones web, cada una en una dirección IP única.
2. Use el nombre de host para hospedar varias aplicaciones web en la misma dirección IP.
3. Use puertos distintos para hospedar varias aplicaciones web en la misma dirección IP.

En la actualidad, Application Gateway admite una dirección IP pública única en la que escucha el tráfico. Por lo tanto, actualmente no se permite tener varias aplicaciones, cada una con su propia dirección IP. 

Application Gateway permite que haya varias aplicaciones y cada una escuche en un puerto diferente. Sin embargo, este escenario requiere que las aplicaciones acepten el tráfico en puertos no estándar. Esta no suele ser la configuración deseada.

Application Gateway se basa en los encabezados de host HTTP 1.1 para hospedar más de un sitio web en la misma dirección IP pública y en el mismo puerto. Los sitios que se hospedan en la puerta de enlace de aplicaciones también pueden admitir la descarga TLS con la extensión TLS de Indicación de nombre de servidor (SNI). Este escenario significa que el explorador cliente y la granja de servidores web back-end deben admitir la extensión TLS y HTTP/1.1, como se define en RFC 6066.

## <a name="listener-configuration-element"></a>Elemento de configuración de agente de escucha

Los elementos de configuración de HTTPListener existentes se han mejorado para admitir los elementos que indican el nombre del host y el nombre del servidor. Se utilizan en Application Gateway para enrutar el tráfico al grupo de back-ends adecuado. 

El ejemplo de código siguiente es el fragmento del elemento HttpListeners de un archivo de plantilla:

```json
"httpListeners": [
    {
        "name": "appGatewayHttpsListener1",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
            },
            "Protocol": "Https",
            "SslCertificate": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
            },
            "HostName": "contoso.com",
            "RequireServerNameIndication": "true"
        }
    },
    {
        "name": "appGatewayHttpListener2",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
            },
            "Protocol": "Http",
            "HostName": "fabrikam.com",
            "RequireServerNameIndication": "false"
        }
    }
],
```

Puede consultar la [plantilla de Resource Manager con hospedaje de múltiples sitios](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) para ver una implementación completa basada en una plantilla.

## <a name="routing-rule"></a>Regla de enrutamiento

No es necesario realizar ningún cambio en la regla de enrutamiento. La regla de enrutamiento 'básica' debería seguir eligiéndose para vincular el agente de escucha del sitio adecuado al grupo de direcciones de back-end correspondiente.

```json
"requestRoutingRules": [
{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener1')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

},
{
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener2')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/FabrikamServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}
]
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que conoce el hospedaje de varios sitios, vaya a la sección sobre cómo [crear una puerta de enlace de aplicaciones mediante el hospedaje de varios sitios](tutorial-multiple-sites-powershell.md) para crear una puerta de enlace de aplicaciones con la capacidad de admitir más de una aplicación web.

