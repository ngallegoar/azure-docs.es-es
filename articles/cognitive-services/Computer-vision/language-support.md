---
title: 'Compatibilidad con idiomas: Computer Vision'
titleSuffix: Azure Cognitive Services
description: 'En este artículo se proporciona una lista de lenguajes naturales admitidos por las características de Computer Vision: OCR, Reconocer texto y Lectura.'
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: a834c68119340d796f87971912a07fc0524a6d21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218858"
---
# <a name="language-support-for-computer-vision"></a>Compatibilidad con idiomas para Computer Vision

Algunas características de Computer Vision admiten varios idiomas; las características que no se mencionan aquí solo admiten inglés.

## <a name="text-recognition"></a>Reconocimiento de texto

Computer Vision puede reconocer texto en varios idiomas. Específicamente, la API de [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) admite varios idiomas, mientras que la API de [Leer](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) y [Reconocer texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) solo son admiten el inglés. Consulte [Reconocimiento de texto manuscrito e impreso](concept-recognizing-text.md) para obtener más información sobre esta funcionalidad y las ventajas de cada API.

OCR detecta automáticamente el idioma del material de entrada, por lo que no es necesario especificar un código de idioma en la llamada a la API. Sin embargo, siempre se devuelven los códigos de idioma como el valor del nodo `"language"` en la respuesta JSON.

|Idioma| Código de lenguaje | API de OCR |
|:-----|:----:|:-----:|
|Árabe | `ar`|✔ |
|Chino (simplificado) | `zh-Hans`|✔ |
|Chino (tradicional) | `zh-Hant`|✔ |
|Checo | `cs` |✔ |
|Danés | `da` |✔ |
|Neerlandés | `nl` |✔ |
|Inglés | `en` |✔ |
|Finés | `fi` |✔ |
|Francés | `fr` |✔ |
|Alemán | `de` |✔ |
|Griego | `el` |✔ |
|Húngaro | `hu` |✔ |
|Italiano | `it` |✔ |
|Japonés | `ja` |✔ |
|Coreano | `ko` |✔ |
|Noruego | `nb` |✔ |
|Polaco | `pl` |✔ |
|Portugués | `pt` |✔ |
|Rumano | `ro` |✔ |
|Ruso | `ru` |✔ |
|Serbio (cirílico) | `sr-Cyrl` |✔ |
|Serbio (latino) | `sr-Latn` |✔ |
|Eslovaco | `sk` |✔ |
|Español | `es` |✔ |
|Sueco | `sw` |✔ |
|Turco | `tr` |✔ |

## <a name="image-analysis"></a>Análisis de imágenes

Algunas acciones de la API [analizar: imagen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) pueden devolver resultados en otros idiomas, especificados con el parámetro de consulta `language`. Otras acciones devuelven resultados en inglés, independientemente del idioma que se especifique, y otros generan una excepción para los idiomas no admitidos. Las acciones se especifican con los parámetros de consulta `visualFeatures` y `details`; vea la [información general](home.md) para obtener una lista de todas las acciones que puede hacer con el análisis de imágenes.

|Idioma | Código de lenguaje | Categorías | Etiquetas | Descripción | Adultos | Marcas | Color | Caras | ImageType | Objetos | Celebridades | Puntos de referencia |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Chino | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Inglés | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japonés | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portugués | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Español | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Pasos siguientes

Conozca el uso de las características de Computer Vision mencionadas en esta guía.

* [Análisis de imágenes locales (REST)](./quickstarts/csharp-analyze.md)
* [Extracción de texto impreso (REST)](./quickstarts/csharp-print-text.md)