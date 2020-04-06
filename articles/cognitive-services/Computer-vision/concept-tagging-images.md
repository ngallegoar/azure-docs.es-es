---
title: 'Etiquetas de contenido: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los conceptos relacionados con la característica de etiquetado de imágenes de Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 887f6d9163f800ecbe8a5ab9a0715fd171f1d3d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80244518"
---
# <a name="applying-content-tags-to-images"></a>Aplicación de etiquetas de contenido a imágenes

Computer Vision devuelve etiquetas en función de miles de objetos, seres vivos, paisajes y acciones reconocibles. Cuando las etiquetas son ambiguas o no muy comunes, la respuesta de la API contiene «indicaciones» para aclarar el significado de la etiqueta en el contexto de un entorno conocido. Las etiquetas no están organizadas por su taxonomía y no existen jerarquías de herencia. Una colección de etiquetas de contenido es la base de la "descripción" de una imagen que se muestra en lenguaje natural con formato de oraciones completas. Tenga en cuenta que, en este momento, el inglés es el único idioma admitido para la descripción de la imagen.

Después de cargar una imagen o especificar la URL de una imagen, los algoritmos de Computer Vision devuelven como salida etiquetas basadas en los objetos, seres vivos, paisajes y acciones identificados en la imagen. El etiquetado no se limita al sujeto principal, como una persona en primer plano, sino que también incluye el entorno (interior o exterior), muebles, herramientas, plantas, animales, accesorios, gadgets, etc.

## <a name="image-tagging-example"></a>Ejemplo de etiquetado de imágenes

La siguiente respuesta JSON muestra lo que devuelve Computer Vision al etiquetar las características visuales detectadas en la imagen de ejemplo.

![Una casa azul y el patio delantero](./Images/house_yard.png).

```json
{
    "tags": [
        {
            "name": "grass",
            "confidence": 0.9999995231628418
        },
        {
            "name": "outdoor",
            "confidence": 0.99992108345031738
        },
        {
            "name": "house",
            "confidence": 0.99685388803482056
        },
        {
            "name": "sky",
            "confidence": 0.99532157182693481
        },
        {
            "name": "building",
            "confidence": 0.99436837434768677
        },
        {
            "name": "tree",
            "confidence": 0.98880356550216675
        },
        {
            "name": "lawn",
            "confidence": 0.788884699344635
        },
        {
            "name": "green",
            "confidence": 0.71250593662261963
        },
        {
            "name": "residential",
            "confidence": 0.70859086513519287
        },
        {
            "name": "grassy",
            "confidence": 0.46624681353569031
        }
    ],
    "requestId": "06f39352-e445-42dc-96fb-0a1288ad9cf1",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="use-the-api"></a>Uso de la API

La característica de etiquetado forma parte de la API [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa). Puede llamar a esta API mediante una SDK nativa o con llamadas a REST. Incluya `Tags` en el parámetro de consulta **visualFeatures**. Después, cuando llegue la respuesta JSON completa, simplemente analice la cadena con el contenido de la sección `"tags"`.

* [Inicio rápido: SDK de Computer Vision para .NET](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Inicio rápido: Análisis de imágenes (API REST)](./quickstarts/csharp-analyze.md)

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos relacionados de [categorización de imágenes](concept-categorizing-images.md) y [descripción de imágenes](concept-describing-images.md).
