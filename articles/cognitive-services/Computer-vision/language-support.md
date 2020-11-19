---
title: 'Compatibilidad con idiomas: Computer Vision'
titleSuffix: Azure Cognitive Services
description: 'En este artículo se proporciona una lista de lenguajes naturales que admiten las características de Computer Vision: OCR y análisis de imagen.'
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: b065b36103b69f0601daa1388b45865856543d2b
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94540525"
---
# <a name="language-support-for-computer-vision"></a>Compatibilidad con idiomas para Computer Vision

Algunas características de Computer Vision admiten varios idiomas; las características que no se mencionan aquí solo admiten inglés.

## <a name="optical-character-recognition-ocr"></a>Reconocimiento óptico de caracteres (OCR)

Las API de OCR de Computer Vision admiten varios idiomas. No requieren que especifique un código de idioma. Consulte [Reconocimiento óptico de caracteres (OCR)](concept-recognizing-text.md) para obtener más información.

|Idioma| Código de lenguaje | API de OCR | Read 3.0 y 3.1 | Read v3.2-preview.1 |
|:-----|:----:|:-----:|:---:|:---:|
|Árabe | `ar`|✔ | | |
|Chino (simplificado) | `zh-Hans`|✔ | |✔ |
|Chino (tradicional) | `zh-Hant`|✔ | | |
|Checo | `cs` |✔ | | |
|Danés | `da` |✔ | | |
|Neerlandés | `nl` |✔ |✔ |✔ |
|Inglés | `en` |✔ |✔ |✔ |
|Finés | `fi` |✔ | | |
|Francés | `fr` |✔ |✔ |✔ |
|Alemán | `de` |✔ |✔ |✔ |
|Griego | `el` |✔ | | |
|Húngaro | `hu` |✔ | | |
|Italiano | `it` |✔ |✔ |✔ |
|Japonés | `ja` |✔ | |✔ |
|Coreano | `ko` |✔ | | |
|Noruego | `nb` |✔ | | |
|Polaco | `pl` |✔ | | |
|Portugués | `pt` |✔ |✔ |✔ |
|Rumano | `ro` |✔ | | |
|Ruso | `ru` |✔ | | |
|Serbio (cirílico) | `sr-Cyrl` |✔ | | |
|Serbio (latino) | `sr-Latn` |✔ | | |
|Eslovaco | `sk` |✔ | | |
|Español | `es` |✔ |✔ |✔ |
|Sueco | `sw` |✔ | | |
|Turco | `tr` |✔ | | |

## <a name="image-analysis"></a>Análisis de imágenes

Algunas acciones de la API [analizar: imagen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b) pueden devolver resultados en otros idiomas, especificados con el parámetro de consulta `language`. Otras acciones devuelven resultados en inglés, independientemente del idioma que se especifique, y otros generan una excepción para los idiomas no admitidos. Las acciones se especifican con los parámetros de consulta `visualFeatures` y `details`; vea la [información general](overview.md) para obtener una lista de todas las acciones que puede hacer con el análisis de imágenes.

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
