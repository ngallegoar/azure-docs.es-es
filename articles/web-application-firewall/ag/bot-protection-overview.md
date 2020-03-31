---
title: Información general sobre la protección contra bots de WAF en Azure Application Gateway
titleSuffix: Azure Web Application Firewall
description: En este artículo se proporciona información general sobre la protección contra bots del firewall de aplicaciones web (WAF) en Application Gateway.
services: web-application-firewall
author: winthrop28
ms.service: web-application-firewall
ms.date: 02/04/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 3bc481cfc35ac94699d2795862f1fe8e4decf875
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77026516"
---
# <a name="azure-web-application-firewall-on-azure-application-gateway-bot-protection-overview"></a>Información general de protección contra bots del firewall de aplicaciones web de Azure en Azure Application Gateway

Aproximadamente el 20 % de todo el tráfico de Internet procede de bots maliciosos. Hacen cosas como recopilar, analizar y buscar vulnerabilidades en sus aplicaciones web. Cuando estos bots se detienen en el firewall de aplicaciones web (WAF), no pueden atacarle. Tampoco pueden usar sus recursos y servicios, como los back-ends y demás infraestructura subyacente.

Puede habilitar un conjunto de reglas de protección contra bots administrado para que WAF bloquee o registre las solicitudes de direcciones IP malintencionadas conocidas. Las direcciones IP proceden de la fuente Inteligencia sobre amenazas de Microsoft. Intelligent Security Graph impulsa la inteligencia sobre amenazas de Microsoft y lo utilizan numerosos servicios, incluido Azure Security Center.

> [!IMPORTANT]
> El conjunto de reglas de protección contra bots se encuentra en versión preliminar pública y se proporciona con un Acuerdo de nivel de servicio de versión preliminar. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte  [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) .

## <a name="use-with-owasp-rulesets"></a>Uso con conjuntos de reglas de OWASP

Puede usar el conjunto de reglas de protección contra bots junto con cualquiera de los conjuntos de reglas de OWASP (2.2.9, 3.0 y 3.1). Solo se puede usar un conjunto de reglas de OWASP en un momento dado. El conjunto de reglas de protección contra bots contiene una regla adicional que aparece en su propio conjunto de reglas. Se titula **MIcrosoft_BotManagerRuleSet_0.1**, y puede habilitarla o deshabilitarla al igual que las demás reglas de OWASP.

![Conjunto de reglas de bots](../media/bot-protection-overview/bot-ruleset.png)

## <a name="ruleset-update"></a>Actualización del conjunto de reglas

La lista del conjunto de reglas de mitigación de bots de direcciones IP no válidas conocidas se actualiza varias veces al día desde la fuente de Inteligencia sobre amenazas de Microsoft para mantenerse sincronizada con los bots. Sus aplicaciones web están protegidas continuamente, incluso cuando cambian los vectores de ataque de los bots.

## <a name="next-steps"></a>Pasos siguientes

- [Configure la protección contra bots para el Firewall de aplicaciones web en Azure Application Gateway (versión preliminar)](bot-protection.md)
