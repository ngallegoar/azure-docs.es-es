---
title: Configuración de la protección contra bots para el Firewall de aplicaciones web (WAF)
description: Más información sobre cómo configurar la protección contra bots para el Firewall de aplicaciones web (WAF) en Azure Application Gateway.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/31/2019
ms.author: victorh
ms.openlocfilehash: 89c863e85d9eab27a47bc1bf7b98cd1c8d89e900
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73511525"
---
# <a name="configure-bot-protection-for-web-application-firewall-on-azure-application-gateway-preview"></a>Configure la protección contra bots para el Firewall de aplicaciones web en Azure Application Gateway (versión preliminar)

En este artículo se muestra cómo configurar una regla de protección contra bots en el Firewall de aplicaciones web (WAF) para Application Gateway Azure Portal. 

Puede habilitar un conjunto de reglas de protección contra bots administrado para que WAF bloquee o registre las solicitudes de direcciones IP malintencionadas conocidas. Las direcciones IP proceden de la fuente Inteligencia sobre amenazas de Microsoft. Intelligent Security Graph impulsa la inteligencia sobre amenazas de Microsoft y lo utilizan numerosos servicios, incluido Azure Security Center.

> [!NOTE]
> El conjunto de reglas de protección contra bots se encuentra en versión preliminar pública y se proporciona con un Acuerdo de nivel de servicio de versión preliminar. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte  [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) .

## <a name="prerequisites"></a>Prerequisites

Cree una directiva de WAF básica para Application Gateway siguiendo las instrucciones descritas en [Crear directivas de Firewall de aplicaciones web para Application Gateway](create-waf-policy-ag.md).

## <a name="enable-bot-protection-rule-set"></a>Habilitación del conjunto de reglas de protección contra bots

1. En la página de la Directiva **básica** que creó anteriormente, en de **Configuración**, seleccione **Reglas**.  

2. En la página detalles, en la sección **Administrar reglas** , en el menú desplegable, active la casilla de la regla de protección contra bots y luego seleccione **Guardar**.

> [!div class="mx-imgBorder"]
> ![Protección contra bots](../media/bot-protection/bot-protection.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las reglas personalizadas, consulte [Reglas personalizadas para el Firewall de aplicaciones web V2 en Azure Application Gateway](custom-waf-rules-overview.md).