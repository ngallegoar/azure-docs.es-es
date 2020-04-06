---
title: 'Descripciones de imágenes: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la característica de descripción de imágenes de Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7bf95a2b49608ef1f031a3b443db92b42cdae624
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80244722"
---
# <a name="describe-images-with-human-readable-language"></a>Descripción de imágenes con lenguaje natural

Computer Vision puede analizar una imagen y generar una frase inteligible que describa su contenido. El algoritmo realmente devuelve varias descripciones según diferentes características visuales y cada descripción tiene una puntuación de confianza. El resultado final es una lista de descripciones ordenadas de mayor a menor confianza.

## <a name="image-description-example"></a>Ejemplo de descripción de imagen

La siguiente respuesta JSON muestra lo que devuelve Computer Vision al describir la imagen de ejemplo según sus características visuales.

![Una imagen en blanco y negro de edificios en Manhattan](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="use-the-api"></a>Uso de la API

La característica de descripción de imágenes forma parte de la API [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa). Puede llamar a esta API mediante una SDK nativa o con llamadas a REST. Incluya `Description` en el parámetro de consulta **visualFeatures**. Después, cuando llegue la respuesta JSON completa, simplemente analice la cadena con el contenido de la sección `"description"`.

* [Inicio rápido: SDK de Computer Vision para .NET](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Inicio rápido: Análisis de imágenes (API REST)](./quickstarts/csharp-analyze.md)

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos relacionados de [etiquetado de imágenes](concept-tagging-images.md) y de [categorización de imágenes](concept-categorizing-images.md).
