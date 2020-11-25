---
title: 'Análisis de opiniones: LUIS'
titleSuffix: Azure Cognitive Services
description: Si el análisis de sentimiento está configurado, la respuesta JSON de LUIS incluye el análisis de sentimiento.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/01/2020
ms.openlocfilehash: 2e8e5a127741625fde7910aaabd421836148fc35
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95018674"
---
# <a name="sentiment-analysis"></a>análisis de opiniones
Si el análisis de sentimiento está configurado, la respuesta JSON de LUIS incluye el análisis de sentimiento. Obtenga más información sobre el análisis de sentimiento en la documentación de [Text Analytics](../text-analytics/index.yml).

LUIS usa Text Analytics v2. 

## <a name="resolution-for-sentiment"></a>Resolución de opiniones

Los datos de opinión son una puntuación entre 1 y 0 que indica el valor de opinión positiva (más cercano a 1) o negativa (más cercano a 0) de los datos.

#### <a name="english-language"></a>[Lengua inglesa](#tab/english)

Cuando la referencia cultural es `en-us`, la respuesta es:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

#### <a name="other-languages"></a>[Otros idiomas](#tab/other-languages)

Para todas las demás referencias culturales, la respuesta es:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```
* * *

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).