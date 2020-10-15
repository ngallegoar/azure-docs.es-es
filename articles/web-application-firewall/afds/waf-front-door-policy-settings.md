---
title: Configuración de directivas para el Firewall de aplicaciones web con Azure Front Door
description: Obtenga información sobre el firewall de aplicaciones web (WAF).
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 08b21ccd7f7958f00546583f680ecb8cde4a20c8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75932612"
---
# <a name="policy-settings-for-web-application-firewall-on-azure-front-door"></a>Configuración de directivas para el firewall de aplicaciones web con Azure Front Door

Una directiva de firewall de aplicaciones web (WAF) permite controlar el acceso a sus aplicaciones web mediante un conjunto de reglas personalizadas y administradas. El nombre de la directiva WAF debe ser único. Recibirá un error de validación si intenta utilizar un nombre existente. Hay varias opciones de configuración del nivel de directiva que se aplican a todas las reglas especificadas para esa directiva, como se describe en este artículo.

## <a name="waf-state"></a>Estado de WAF

Una directiva WAF para Front Door puede tener uno de los dos estados siguientes:
- **Habilitada:** Cuando se habilita una directiva, WAF inspecciona activamente las solicitudes entrantes y realiza las acciones correspondientes según las definiciones de la regla.
- **Deshabilitada:** : cuando una directiva está deshabilitada, la inspección de WAF está en pausa. Las solicitudes entrantes omiten WAF y se envían a los servidores back-end en función del enrutamiento de Front Door.

## <a name="waf-mode"></a>Modo WAF

La directiva WAF se puede configurar para ejecutarse en los siguientes dos modos:

- **Modo de detección** Cuando se ejecuta en el modo de detección, WAF no realiza ninguna acción que no sea supervisar y registrar la solicitud y su regla WAF coincidente en los registros de WAF. Active el registro de diagnósticos de Front Door (para hacerlo desde el portal, vaya a la sección **Diagnósticos** de Azure Portal).

- **Modo de prevención** Cuando se configura para ejecutarse en el modo de prevención, WAF realiza la acción especificada si una solicitud coincide con una regla. Todas las solicitudes coincidentes también se registran en los registros de WAF.

## <a name="waf-response-for-blocked-requests"></a>Respuesta de WAF para las solicitudes bloqueadas

De forma predeterminada, cuando WAF bloquea una solicitud debido a una regla coincidente, devuelve un código de estado 403 con el mensaje **La solicitud está bloqueada**. También se devuelve una cadena de referencia para el registro.

Puede definir un mensaje de respuesta y código de estado de respuesta personalizado cuando WAF bloquea una solicitud. Se admiten los siguientes códigos de estado personalizados:

- 200    Correcto
- 403    Prohibido
- 405    Método no permitido
- 406    No aceptable
- 429    Demasiadas solicitudes

El mensaje de respuesta y código de estado de respuesta personalizado es una configuración de nivel de directiva. Una vez configurado, todas las solicitudes bloqueadas obtienen el mismo mensaje de respuesta y estado de respuesta personalizado.

## <a name="uri-for-redirect-action"></a>URI de acción de redirección

Es necesario definir un URI al que redirigir las solicitudes si la acción **REDIRIGIR** se selecciona para cualquiera de las reglas incluidas en una directiva WAF. Este URI de redirección debe ser un sitio HTTP(S) válido y, una vez configurado, todas las solicitudes que coincidan con las reglas con una acción "REDIRIGIR" se redirigirán al sitio especificado.


## <a name="next-steps"></a>Pasos siguientes
- Obtener información sobre cómo definir [respuestas WAF personalizadas](waf-front-door-configure-custom-response-code.md)
