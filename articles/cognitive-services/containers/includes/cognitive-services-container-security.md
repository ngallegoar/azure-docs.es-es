---
title: Seguridad del contenedor
titleSuffix: Azure Cognitive Services
description: Aprenda a proteger su contenedor
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: fd2a6cdad01302501e30ec60a4d3ccf6efd9c266
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80876845"
---
## <a name="azure-cognitive-services-container-security"></a>Protección de los contenedores en Azure Cognitive Services

La seguridad debe ser parte central al desarrollar aplicaciones. La importancia de la seguridad es una métrica del éxito. Al diseñar una solución de software que incluye contenedores de Cognitive Services, es fundamental comprender las limitaciones y las funcionalidades disponibles. Para obtener más información sobre la seguridad de red, consulte [Configure Azure Cognitive Services virtual networks][az-security] (configuración de las redes virtuales de Azure Cognitive Services).

> [!IMPORTANT]
> De forma predeterminada, *no hay seguridad* en la API de contenedor de Cognitive Services. Esto es así porque la mayoría de las veces el contenedor se ejecuta como parte de un pod que está protegido desde fuera por un puente de red. Sin embargo, es posible habilitar la autenticación, que funciona de forma idéntica a la autenticación que se usa al acceder a [Cognitive Services en la nube][request-authentication].

En el diagrama siguiente se muestra el enfoque predeterminado, que **no es seguro**:

![Seguridad del contenedor](../media/container-security.svg)

Como método alternativo y *seguro*, los consumidores de contenedores de Cognitive Services podrían aumentar un contenedor con un componente frontal y mantener el punto de conexión del contenedor privado. Veamos un escenario en el que usamos [Istio][istio] como puerta de enlace de entrada. Istio admite HTTPS/TLS y la autenticación con certificado de cliente. En este escenario, el front-end de Istio expone el acceso al contenedor, al presentar de antemano el certificado de cliente que se encuentra en la lista de permitidos con Istio.

[Nginx][nginx] es otra opción popular de la misma categoría. Tanto Istio como Nginx actúan como malla de servicio y ofrecen características adicionales, como equilibrio de carga, enrutamiento y control de velocidad.

### <a name="container-networking"></a>Redes de contenedores

Los contenedores de Cognitive Services son necesarios para enviar información de medición con fines de facturación. La única excepción es *Contenedores sin conexión*, ya que siguen otra metodología de facturación. Si no se permite la enumeración de varios canales de red en los que se basan los contenedores de Cognitive Services, el contenedor no funcionará.

#### <a name="allow-list-cognitive-services-domains-and-ports"></a>Permiso para enumerar los dominios y puertos de Cognitive Services

El host debe permitir la enumeración del **puerto 443** y de los siguientes dominios:

* `*.cognitive.microsoft.com`
* `*.cognitiveservices.azure.com`

#### <a name="disable-deep-packet-inspection"></a>Deshabilitación de la inspección profunda de paquetes

> [Inspección profunda de paquetes](https://en.wikipedia.org/wiki/Deep_packet_inspection) (DPI) es un tipo de procesamiento de datos que inspecciona detalladamente los datos que se envían a través de una red de equipos y, por lo general, realiza un bloqueo, reenrutamiento o registro, la acción que corresponda.

Deshabilite DPI en los canales seguros que los contenedores de Cognitive Services crean en los servidores de Microsoft. Si no lo hace, el contenedor no funcionará correctamente.

[istio]: https://istio.io/
[nginx]: https://www.nginx.com
[request-authentication]: ../../authentication.md
[az-security]: ../../cognitive-services-virtual-networks.md
