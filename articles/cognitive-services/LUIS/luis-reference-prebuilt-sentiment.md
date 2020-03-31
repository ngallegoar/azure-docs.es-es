---
title: 'Análisis de opiniones: LUIS'
titleSuffix: Azure Cognitive Services
description: Si el análisis de sentimiento está configurado, la respuesta JSON de LUIS incluye el análisis de sentimiento.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/22/2019
ms.author: diberry
ms.openlocfilehash: dee07859da97f76d9a2db3ccbb71ae74d8ff4846
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270418"
---
# <a name="sentiment-analysis"></a>análisis de opiniones
Si el análisis de sentimiento está configurado, la respuesta JSON de LUIS incluye el análisis de sentimiento. Obtenga más información sobre el análisis de sentimiento en la documentación de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).


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

