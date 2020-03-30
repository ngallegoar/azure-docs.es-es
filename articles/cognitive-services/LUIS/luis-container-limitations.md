---
title: 'Limitaciones de los contenedores: LUIS'
titleSuffix: Azure Cognitive Services
description: Idiomas de los contenedores LUIS que se admiten.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: bd8a7a63113bcf4e972ab08655aa58b35ddff03d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73506815"
---
# <a name="language-understanding-luis-container-limitations"></a>Limitaciones de los contenedores de Language Understanding (LUIS)

Los contenedores LUIS tienen pocas limitaciones notables. Desde las dependencias no admitidas hasta un subconjunto de idiomas admitidos, en este artículo se detallan estas restricciones.

## <a name="supported-dependencies-for-latest-container"></a>Dependencias admitidas del contenedor `latest`

El contenedor LUIS más reciente, que se lanzó en [//build/ 2019](https://news.microsoft.com/build2019/), admitirá:

* [Nuevos dominios creados previamente](luis-reference-prebuilt-domains.md): estos dominios empresariales incluyen entidades, expresiones de ejemplo y patrones. Amplíe estos dominios para su propio uso.

## <a name="unsupported-dependencies-for-latest-container"></a>Dependencias no admitidas del contenedor `latest`

Para [exportar para contenedor](luis-container-howto.md#export-packaged-app-from-luis), debe quitar las dependencias no admitidas de la aplicación de LUIS. Si intenta exportar el contenedor, el portal de LUIS informa de estas características no admitidas que debe quitar.

Puede usar una aplicación de LUIS si **no incluye** ninguna de las siguientes dependencias:

Configuraciones de aplicaciones no admitidas|Detalles|
|--|--|
|Referencias culturales de contenedor no admitidas| Neerlandés (`nl-NL`)<br>Japonés (`ja-JP`)<br>El alemán solo es compatible con el [tokenizador 1.0.2](luis-language-support.md#custom-tokenizer-versions).|
|Entidades no compatibles con ninguna referencia cultural|Entidad [KeyPhrase](luis-reference-prebuilt-keyphrase.md) pregenerada para todas las referencias culturales|
|Entidades no compatibles con la referencia cultural Inglés (`en-US`)|Entidades [GeographyV2](luis-reference-prebuilt-geographyV2.md) pregeneradas|
|Preparación para la voz|El contenedor no admite dependencias externas.|
|análisis de opiniones|El contenedor no admite dependencias externas.|
|Bing Spell Check|El contenedor no admite dependencias externas.|

## <a name="languages-supported"></a>Idiomas admitidos

Los contenedores LUIS admiten un subconjunto de los [idiomas admitidos](luis-language-support.md#languages-supported) por LUIS. Los contenedores LUIS son capaces de comprender expresiones en los siguientes idiomas:

| Idioma | Configuración regional | Dominio creado previamente | Entidad creada previamente | Recomendaciones de la lista de frases | \**[Análisis de texto](../text-analytics/language-support.md)<br>(Las opiniones y<br>palabras clave)|
|--|--|:--:|:--:|:--:|:--:|
| Inglés de Estados Unidos | `en-US` | ✔️ | ✔️ | ✔️ | ✔️ |
| *[Chino](#chinese-support-notes) |`zh-CN` | ✔️ | ✔️ | ✔️ | ❌ |
| Francés (Francia) |`fr-FR` | ✔️ | ✔️ | ✔️ | ✔️ |
| Francés (Canadá) |`fr-CA` | ❌ | ❌ | ❌ | ✔️ |
| Alemán |`de-DE` | ✔️ | ✔️ | ✔️ | ✔️ |
| Hindi | `hi-IN`| ❌ | ❌ | ❌ | ❌ |
| Italiano |`it-IT` | ✔️ | ✔️ | ✔️ | ✔️ |
| Coreano |`ko-KR` | ✔️ | ❌ | ❌ | Solo la *frase clave* |
| Portugués (Brasil) |`pt-BR` | ✔️ | ✔️ | ✔️ | No todas las referencias culturales secundarias |
| Español (España) |`es-ES` | ✔️ | ✔️ |✔️|✔️|
| Español (México)|`es-MX` | ❌ | ❌ |✔️|✔️|
| Turco | `tr-TR` |✔️| ❌ | ❌ | Solo *opiniones* |

[!INCLUDE [Chinese language support notes](includes/chinese-language-support-notes.md)]

[!INCLUDE [Text Analytics support notes](includes/text-analytics-support-notes.md)]