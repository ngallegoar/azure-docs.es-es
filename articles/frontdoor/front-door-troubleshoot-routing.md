---
title: Solución de problemas de configuración de Azure Front Door
description: En este tutorial, aprenderá a solucionar por sí mismo algunos de los problemas comunes que pueden aparecer en Front Door.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/30/2020
ms.author: duau
ms.openlocfilehash: dbce9019e33c07dd4faa91ffd490eba4d313c675
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91630617"
---
# <a name="troubleshooting-common-routing-issues"></a>Solución de problemas comunes de enrutamiento

En este artículo se describe cómo solucionar algunos de los problemas comunes de enrutamiento que pueden surgir con la configuración de Azure Front Door.

## <a name="503-response-from-front-door-after-a-few-seconds"></a>Respuesta 503 de Front Door después de unos segundos

### <a name="symptom"></a>Síntoma

* Las solicitudes normales enviadas al back-end sin pasar por Front Door se realizan correctamente, pero el paso por Front Door produce respuestas de error 503.
* El error de Front Door se muestra después de unos segundos (normalmente, al cabo de unos 30 segundos).

### <a name="cause"></a>Causa

La causa de este problema puede ser una de estas dos:
 
* El back-end es mayor que el tiempo de expiración configurado (el valor predeterminado es 30 segundos) para recibir la solicitud de Front Door.
* El tiempo que se tarda en enviar una respuesta a la solicitud desde Front Door es mayor que el valor de tiempo de expiración. 

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

* Envíe la solicitud al back-end directamente (sin pasar por Front Door) y vea cuál es el tiempo habitual que tarda el back-end en responder.
* Envíe la solicitud a través de Front Door y vea si se obtienen respuestas 503. Si no es así, es posible que no se trate de un problema de tiempo de expiración. Póngase en contacto con el servicio de soporte técnico.
* Si se analizan los resultados de Front Door en el código de respuesta de error, configure el campo `sendReceiveTimeout` para su instancia de Front Door. Puede ampliar el tiempo de expiración predeterminado hasta 4 minutos (240 segundos). La opción de configuración está en `backendPoolSettings` y se denomina `sendRecvTimeoutSeconds`. 

## <a name="requests-sent-to-the-custom-domain-returns-400-status-code"></a>Las solicitudes enviadas al dominio personalizado devuelven el código de estado 400

### <a name="symptom"></a>Síntoma

* Creó una instancia de Front Door, pero una solicitud al dominio o el host de front-end devuelve un código de estado HTTP 400.
* Creó una asignación de DNS para un dominio personalizado para el host de front-end que configuró. Sin embargo, el envío de una solicitud al nombre de host de dominio personalizado devuelve un código de estado HTTP 400. No parece enrutarse al back-end que configuró.

### <a name="cause"></a>Causa

El problema se produce si no configuró una regla de enrutamiento para el dominio personalizado que se agregó como host de front-end. Es necesario agregar explícitamente una regla de enrutamiento para ese host de front-end. Incluso si ya se ha configurado una para el host de front-end en el subdominio de Front Door (*. azurefd.net).

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

Agregue una regla de enrutamiento para que el dominio personalizado dirija el tráfico al grupo de back-end seleccionado.

## <a name="front-door-doesnt-redirect-http-to-https"></a>Front Door no redirige HTTP a HTTPS

### <a name="symptom"></a>Síntoma

Su instancia de Front Door tiene una regla de enrutamiento que indica redirigir HTTP a HTTPS, pero el acceso al dominio sigue manteniendo HTTP como el protocolo.

### <a name="cause"></a>Causa

Este comportamiento puede producirse si no configuró correctamente las reglas de enrutamiento para su instancia de Front Door. Básicamente, la configuración actual no se ha especificado y puede tener reglas en conflicto.

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>La solicitud al nombre de host de front-end devuelve el código de estado 404

### <a name="symptom"></a>Síntoma

 Creó una instancia de Front Door configurando un host de front-end, un grupo de back-end con al menos un back-end y una regla de enrutamiento que conecta el host de front-end con el grupo de back-end. El contenido no está disponible al realizar una solicitud al host de front-end configurado, ya que se devuelve un código de estado HTTP 404.

### <a name="cause"></a>Causa

Hay varias causas posibles para este síntoma:

* El back-end no es un back-end orientado al público y no es visible para Front Door.
* La configuración del back-end es errónea, lo que hace que Front Door envíe la solicitud equivocada. Es decir, el back-end solo acepta HTTP y no ha desactivado la autorización de HTTPS. Así pues, Front Door intenta reenviar solicitudes HTTPS.
* El back-end rechaza el encabezado de host que se ha reenviado con la solicitud al back-end.
* La configuración para el back-end aún no se ha implementado por completo.

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

1. Tiempo de implementación
   * Asegúrese de haber esperado unos 10 minutos para que se implemente la configuración.

2. Compruebe la configuración de back-end
    * Navegue hasta el grupo de back-ends al que debería enrutarse la solicitud (depende de cómo haya configurado la regla de enrutamiento). Compruebe que el *tipo de host de back-end* y el nombre de host de back-end sean correctos. Si el back-end es un host personalizado, asegúrese de haberlo escrito correctamente. 

    * Compruebe sus puertos HTTP y HTTPS. En la mayoría de los casos, 80 y 443 (respectivamente) son correctos y no será necesario realizar ningún cambio. Sin embargo, existe la posibilidad de que el back-end no esté configurado de este modo y esté escuchando en otro puerto.

        * Compruebe el _Encabezado host de back-end_ configurado para los back-ends a los que debería enrutarse el host de front-end. En la mayoría de los casos, este encabezado debería ser el mismo que el *Nombre de host de back-end*. Sin embargo, un valor incorrecto puede provocar distintos códigos de estado HTTP 4xx si el back-end espera algo diferente. Si escribe la dirección IP de su back-end, es posible que deba establecer el *Encabezado host de back-end* en el nombre de host del backend.

3. Compruebe la configuración de la regla de enrutamiento:
    * Vaya a la regla de enrutamiento que debe enrutar del nombre de host de front-end en cuestión a un grupo de back-end. Asegúrese de que los protocolos aceptados estén configurados correctamente al reenviar la solicitud. El campo *Protocolos aceptados* determina qué solicitudes debe aceptar Front Door. El *protocolo de reenvío* determina qué protocolo debe usar Front Door para reenviar la solicitud al back-end.
         * Por ejemplo, si el back-end solo acepta solicitudes HTTP, las siguientes configuraciones serían válidas:
            * Los *protocolos aceptados* son HTTP y HTTPS. El *protocolo de reenvío* es HTTP. Confrontar solicitud no funcionará, ya que HTTPS es un protocolo permitido y, si llegó una solicitud como HTTPS, Front Door intentaría reenviarla mediante HTTPS.

            * Los *protocolos aceptados* son HTTP. El *protocolo de reenvío* es Confrontar solicitud o HTTPS.

    - *Reescritura de direcciones URL* está deshabilitada de forma predeterminada. Este campo solo se usa si quiere restringir el ámbito de los recursos hospedados en back-end que quiera que estén disponibles. Cuando esté deshabilitado, Front Door reenviará la misma ruta de acceso de solicitud que reciba. Se puede configurar incorrectamente este campo. Por tanto, cuando Front Door solicita un recurso del back-end que no está disponible, devolverá un código de estado HTTP 404.
