---
title: 'Conversión de datos: LUIS'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo se pueden modificar las expresiones antes de las predicciones en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: aeb9094db83b14af988f70485788934a7854200c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "68619792"
---
# <a name="convert-data-format-of-utterances"></a>Conversión del formato de datos de expresiones
LUIS proporciona las siguientes conversiones de una expresión de usuario antes de la predicción.

* Conversión de voz en texto mediante el servicio [Speech de Cognitive Services](../Speech-Service/overview.md). 

## <a name="speech-to-text"></a>Conversión de voz en texto

La conversión de voz en texto se proporciona como una integración con LUIS. 

### <a name="intent-conversion-concepts"></a>Conceptos de la conversión en intención
La conversión de voz en texto en LUIS permite enviar expresiones de voz a un punto de conexión y recibir una respuesta de predicción de LUIS. El proceso consiste en integrar el servicio [Speech](https://docs.microsoft.com/azure/cognitive-services/Speech) con LUIS. Más información sobre la conversión de voz en intención con un [tutorial](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Requisitos principales
No es necesario crear una clave de **Bing Speech API** para esta integración. Para esta integración, solo servirá una clave de **Language Understanding** creada en Azure Portal. No utilice la clave de inicio de LUIS.

### <a name="pricing-tier"></a>Nivel de precios
Esta integración usa otro modelo de [precios](luis-boundaries.md#key-limits) distinto de los planes de tarifa habituales de Language Understanding. 

### <a name="quota-usage"></a>Uso de cuota
Vea los [Límites clave](luis-boundaries.md#key-limits) para obtener información. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Extracción de datos](luis-concept-data-extraction.md)

