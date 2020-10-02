---
title: Configuración de la dirección IP de front-end de Azure Application Gateway
description: En este artículo se describe cómo configurar la dirección IP de front-end de Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: d6cfac7f0fb3939e57ce64f552556138ce9feacd
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89652735"
---
# <a name="application-gateway-front-end-ip-address-configuration"></a>Configuración de la dirección IP de front-end de Application Gateway

Puede configurar la puerta de enlace de aplicaciones para que tenga una dirección IP pública, una dirección IP privada o ambas. Se necesita una dirección IP pública si hospeda un back-end al que los clientes deben acceder desde Internet mediante una dirección IP virtual (VIP) accesible desde Internet.

## <a name="public-and-private-ip-address-support"></a>Compatibilidad con direcciones IP públicas y privadas

Actualmente, Application Gateway V2 no admite solo el modo IP privada. Admite las siguientes combinaciones:

* Dirección IP privada y dirección IP pública
* Solo dirección IP pública

Para obtener más información, vea las [preguntas más frecuentes sobre Application Gateway](application-gateway-faq.md#how-do-i-use-application-gateway-v2-with-only-private-frontend-ip-address).


No se necesita ninguna dirección IP pública para un punto de conexión interno que no está expuesto a Internet. Eso se conoce como un punto de conexión de *equilibrador de carga interno* (ILB) o una dirección IP de front-end privada. Un equilibrador de carga interno de puerta de enlace de aplicaciones es útil para aplicaciones de línea de negocio internas no expuestas a Internet. También es útil para los servicios y niveles de una aplicación de varios niveles dentro de un límite de seguridad que no están expuestos a Internet, pero que siguen necesitando distribución de carga round robin, permanencia de sesión o terminación TLS.

Se admite solo una dirección IP pública o una dirección IP privada. Puede elegir la dirección IP de front-end cuando cree la puerta de enlace de aplicaciones.

- Para una dirección IP pública, puede crear una nueva dirección IP pública o usar una ya existente en la misma ubicación que la puerta de enlace de aplicación. Para más información, consulte esta comparación entre [direcciones IP públicas estáticas y dinámicas](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-versus-dynamic-public-ip-address).

- Para una dirección IP privada, puede especificar una dirección IP privada de la subred en la que se creó la puerta de enlace de aplicación. Si no especifica ninguna, se seleccionará automáticamente una dirección IP aleatoria de la subred. El tipo de dirección IP que seleccione (estática o dinámica) no se podrá cambiar más adelante. Para más información, consulte [Creación de una puerta de enlace de aplicaciones con un equilibrador de carga interno](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm).

Una dirección IP de front-end está asociada a un *cliente de escucha* que comprueba las solicitudes entrantes en esta dirección IP.

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre la configuración del agente de escucha](configuration-listeners.md)