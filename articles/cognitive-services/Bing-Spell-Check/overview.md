---
title: ¿Qué es Bing Spell Check API?
titleSuffix: Azure Cognitive Services
description: Conozca Bing Spell Check API, que usa el aprendizaje automático y la traducción automática estadística para la revisión ortográfica contextual.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 12/19/2019
ms.author: aahi
ms.openlocfilehash: 905ae56ab8d64f7342c4ee6b9f25d188c907710d
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93084194"
---
# <a name="what-is-the-bing-spell-check-api"></a>¿Qué es Bing Spell Check API?

> [!WARNING]
> Las Bing Search API se mueven de Cognitive Services a Bing Search Services. A partir del  **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de las Bing Search API mediante Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que antes suceda.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Bing Spell Check API permite realizar la revisión ortográfica y gramatical contextual del texto. Aunque la mayoría de los correctores ortográficos dependen de conjuntos de reglas basadas en diccionario, el corrector ortográfico de Bing aprovecha el aprendizaje automático y la traducción automática estadística para proporcionar correcciones precisas y contextuales. 

## <a name="features"></a>Características

| Característica | Descripción |
|---------|---------|
|Varios modos de revisión ortográfica     | Gracias a los diversos modos de revisión ortográfica, puede obtener correcciones centradas en la gramática o la ortografía. |
|Reconocimiento de lenguaje coloquial e informal     | Reconoce las expresiones comunes y los términos informales usados en el texto.         |
|Diferenciación entre palabras similares     | Busca el uso correcto entre palabras que suenan parecido pero cuyo significado varía (por ejemplo, "see" y "sea").        |
|Compatibilidad con uso popular, marca y título     | Reconoce nuevas marcas, títulos y otras expresiones populares que surgen. |

## <a name="workflow"></a>Flujo de trabajo

Bing Spell Check API es fácil de llamar desde cualquier lenguaje de programación con el que se puedan crear solicitudes HTTP y analizar respuestas JSON. Se puede acceder al servicio mediante la API REST o los SDK de Bing Spell Check. 

1. Cree una [cuenta de API de Cognitive Services](../cognitive-services-apis-create-account.md) con acceso a Bing Search APIs. Si no tiene una suscripción a Azure, puede crear una cuenta gratuita. 
2. Envíe una solicitud a Bing Web Search API.
3. Procese la respuesta JSON.

## <a name="next-steps"></a>Pasos siguientes

En primer lugar, pruebe la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/spell-check/) de Bing Spell Check Search API para ver cómo puede revisar rápidamente una variedad de textos.

Cuando esté listo para llamar a la API, cree una [cuenta de API de Cognitive Services](../../cognitive-services/cognitive-services-apis-create-account.md). Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/free/cognitive-services/) gratuita.

Puede también visitar la [página central de Bing Search API](../bing-web-search/search-the-web.md) para explorar las otras API disponibles.