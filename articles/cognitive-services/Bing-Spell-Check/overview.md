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
ms.openlocfilehash: be864e04143a381a08dedb5693026f05c42c5421
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96349306"
---
# <a name="what-is-the-bing-spell-check-api"></a>¿Qué es Bing Spell Check API?

> [!WARNING]
> Las Bing Search API se mueven de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020**, las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

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

Puede también visitar la [página central de Bing Search API](../bing-web-search/overview.md) para explorar las otras API disponibles.