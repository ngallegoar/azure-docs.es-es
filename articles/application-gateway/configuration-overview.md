---
title: Introducción a la configuración de Azure Application Gateway
description: En este artículo se describe cómo configurar los componentes de Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: 0012ac99600c77dce5940387e1da54f29c3f6ab7
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89652002"
---
# <a name="application-gateway-configuration-overview"></a>Introducción a la configuración de Application Gateway

Azure Application Gateway consta de varios componentes que se pueden configurar de varias maneras para diferentes escenarios. En este artículo se muestra cómo configurar cada componente.

![Diagrama de flujo de los componentes de Application Gateway](./media/configuration-overview/configuration-overview1.png)

Esta imagen muestra una aplicación que tiene tres clientes de escucha. Los dos primeros son clientes de escucha multisitio para `http://acme.com/*` y `http://fabrikam.com/*`, respectivamente. Ambos escuchan en el puerto 80. El tercero es un cliente de escucha básico que tiene una terminación de seguridad de la capa de transporte (TLS) de un extremo a otro, antes conocida como terminación de Capa de sockets seguros (SSL).

## <a name="infrastructure"></a>Infraestructura

La infraestructura de Application Gateway incluye la red virtual, las subredes, los grupos de seguridad de red y las rutas definidas por el usuario.

Para más información, consulte [Configuración de la infraestructura de Application Gateway](configuration-infrastructure.md).



## <a name="front-end-ip-address"></a>Dirección IP de front-end

Puede configurar la puerta de enlace de aplicaciones para que tenga una dirección IP pública, una dirección IP privada o ambas. Se necesita una dirección IP pública si hospeda un back-end al que los clientes deben acceder desde Internet mediante una dirección IP virtual (VIP) accesible desde Internet.

Para más información, consulte [Configuración de la dirección IP de front-end de Application Gateway](configuration-front-end-ip.md).

## <a name="listeners"></a>Clientes de escucha

Un cliente de escucha es una entidad lógica que comprueba las solicitudes de conexión entrantes mediante el puerto, protocolo, host y dirección IP. Al configurar el cliente de escucha, debe especificar valores que coincidan con los valores correspondientes de la solicitud entrante de la puerta de enlace.

Para más información, consulte [Configuración del agente de escucha de Application Gateway](configuration-listeners.md).

## <a name="request-routing-rules"></a>Reglas de enrutamiento de solicitudes

Si crea una puerta de enlace de aplicaciones mediante Azure Portal, puede crear una regla predeterminada (*rule1*). Esta regla enlaza el cliente de escucha predeterminado (*appGatewayHttpListener*) con el grupo de servidores back-end predeterminado (*appGatewayBackendPool*) y la configuración de HTTP de back-end predeterminada (*appGatewayBackendHttpSettings*). Después de crear la puerta de enlace, puede modificar la configuración de la regla predeterminada o crear nuevas reglas.

Para más información, consulte [Reglas de enrutamiento de solicitudes de Application Gateway](configuration-request-routing-rules.md).

## <a name="http-settings"></a>Configuración de HTTP

La puerta de enlace de aplicaciones enrutará el tráfico a los servidores back-end mediante la configuración que especifique aquí. Después de crear una configuración de HTTP, debe asociarla con una o varias reglas de enrutamiento de solicitudes.

Para más información, consulte [Configuración HTTP de Application Gateway](configuration-http-settings.md).

## <a name="back-end-pool"></a>Grupo de servidores back-end

Puede apuntar un grupo de servidores back-end a cuatro tipos de miembros de back-end: una máquina virtual específica, un conjunto de escalado de máquinas virtuales, una dirección IP o FQDN, o una instancia de App Service. 

Después de crear un grupo de servidores back-end, debe asociarlo con una o varias reglas de enrutamiento de solicitudes. También debe configurar sondeos de estado para cada grupo de servidores back-end de la puerta de enlace de aplicaciones. Cuando se cumple una condición de la regla de enrutamiento de solicitudes, la puerta de enlace de aplicaciones reenvía el tráfico a los servidores correctos (los que determinen los sondeos de estado) del grupo de back-end correspondiente.

## <a name="health-probes"></a>Sondeos de estado

Una puerta de enlace de aplicaciones supervisa el estado de todos los recursos en su back-end de forma predeterminada. Pero se recomienda encarecidamente que cree un sondeo personalizado para cada configuración de HTTP de back-end para conseguir un mayor control sobre la supervisión del estado. Para más información sobre cómo configurar un sondeo personalizado, consulte [Configuración de sondeo de estado personalizado](application-gateway-probe-overview.md#custom-health-probe-settings).

> [!NOTE]
> Después de crear un sondeo de estado personalizado, debe asociarlo a una configuración de HTTP de back-end. Un sondeo personalizado no supervisará el estado del grupo de servidores back-end a menos que la configuración HTTP correspondiente esté explícitamente asociada a un cliente de escucha mediante una regla.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya conoce los componentes de Application Gateway, puede:

- [Crear una puerta de enlace de aplicaciones mediante Azure Portal](quick-create-portal.md)
- [Crear una puerta de enlace de aplicaciones mediante PowerShell](quick-create-powershell.md)
- [Creación de una puerta de enlace de aplicaciones mediante la CLI de Azure](quick-create-cli.md)
