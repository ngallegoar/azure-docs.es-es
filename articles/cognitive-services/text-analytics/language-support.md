---
title: 'Idiomas admitidos: Text Analytics API'
titleSuffix: Azure Cognitive Services
description: Una lista de los idiomas naturales admitidos por Text Analytics API. En este artículo se explica qué idiomas se admiten para cada operación.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: aahi
ms.openlocfilehash: b13d82780a01771c6bb8e87091a7808ea22ca111
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92371192"
---
# <a name="text-analytics-api-v3-language-support"></a>Idiomas admitidos en Text Analytics API v3 

[!INCLUDE [v3 region availability](includes/v3-region-availability.md)]


#### <a name="sentiment-analysis"></a>[Análisis de sentimiento](#tab/sentiment-analysis)

| Idioma              | Código de lenguaje | Compatibilidad con la versión 2 | Compatibilidad con la versión v3 | A partir de la versión del modelo v3: |              Notas |
|:----------------------|:-------------:|:----------:|:----------:|:--------------------------:|-------------------:|
| Chino simplificado    |   `zh-hans`   |     ✓      |     ✓      |         2019-10-01         | También se acepta `zh` |
| Chino (tradicional)   |   `zh-hant`   |            |     ✓      |         2019-10-01         |                    |
| Danés               |     `da`      |     ✓      |            |                            |                    |
| Neerlandés                 |     `nl`      |     ✓      |            |                            |                    |
| Inglés               |     `en`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Finés               |     `fi`      |     ✓      |            |                            |                    |
| Francés                |     `fr`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Alemán                |     `de`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Griego                 |     `el`      |     ✓      |            |                            |                    |
| Hindi                 |     `hi`      |           |      ✓      |          2020-04-01                  |                    |
| Italiano               |     `it`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Japonés              |     `ja`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Coreano                |     `ko`      |            |     ✓      |         2019-10-01         |                    |
| Noruego (Bokmål)   |     `no`      |     ✓      |     ✓       |        01-07-2020         |                    |
| Polaco                |     `pl`      |     ✓      |            |                            |                    |
| Portugués (Portugal) |    `pt-PT`    |     ✓      |     ✓      |         2019-10-01         | También se acepta `pt` |
| Ruso               |     `ru`      |     ✓      |            |                            |                    |
| Español               |     `es`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Sueco               |     `sv`      |     ✓      |            |                            |                    |
| Turco               |     `tr`      |     ✓      |     ✓       |         01-07-2020        |                    |

### <a name="opinion-mining-v31-preview-only"></a>Minería de opiniones (solo v3.1-preview)

| Idioma              | Código de lenguaje | A partir de la versión del modelo v3: |              Notas |
|:----------------------|:-------------:|:------------------------------------:|-------------------:|
| Inglés               |     `en`      |              2020-04-01              |                    |


#### <a name="named-entity-recognition-ner"></a>[Reconocimiento de entidades con nombre (NER)](#tab/named-entity-recognition)

> [!NOTE]
> * NER v3 actualmente solo admite los idiomas inglés y español. Si llama a NER v3 con un idioma diferente, la API devolverá los resultados de v2.1, siempre que el idioma sea compatible con la versión 2.1.
> * La versión 2.1 solo devuelve el conjunto completo de entidades para los idiomas inglés, chino simplificado, francés, alemán y español.  Se devuelven las entidades "Persona", "Ubicación" y "Organización" para los demás idiomas admitidos.

| Idioma               | Código de lenguaje | Compatibilidad con la versión 2.1 | Compatibilidad con la versión 3 | A partir de la versión del modelo de la versión 3: |       Notas        |
|:-----------------------|:-------------:|:----------:|:----------:|:-------------------------------:|:------------------:|
| Árabe                |     `ar`      |     ✓      |            |                                 |                    |
| Checo                 |     `cs`      |     ✓      |            |                                 |                    |
| Chino simplificado     |   `zh-hans`   |     ✓      |            |                                 | También se acepta `zh` |
| Chino (tradicional)   |   `zh-hant`   |     ✓      |            |                                 |                    |
| Danés                |     `da`      |     ✓      |            |                                 |                    |
| Neerlandés                 |     `nl`      |     ✓      |            |                                 |                    |
| Inglés                |     `en`      |     ✓      |     ✓      |           2019-10-01            |                    |
| Finés               |     `fi`      |     ✓      |            |                                 |                    |
| Francés                 |     `fr`      |     ✓      |            |                                 |                    |
| Alemán                 |     `de`      |     ✓      |            |                                 |                    |
| Hebreo                |     `he`      |     ✓      |            |                                 |                    |
| Húngaro             |     `hu`      |     ✓      |            |                                 |                    |
| Italiano               |     `it`      |     ✓      |            |                                 |                    |
| Japonés              |     `ja`      |     ✓      |            |                                 |                    |
| Coreano                |     `ko`      |     ✓      |            |                                 |                    |
| Noruego (Bokmål)   |     `no`      |     ✓      |            |                                 | También se acepta `nb` |
| Polaco                |     `pl`      |     ✓      |            |                                 |                    |
| Portugués (Portugal) |    `pt-PT`    |     ✓      |            |                                 | También se acepta `pt` |
| Portugués (Brasil)   |    `pt-BR`    |     ✓      |            |                                 |                    |
| Ruso              |     `ru`      |     ✓      |            |                                 |                    |
| Español               |     `es`      |     ✓      |     ✓       |              2020-04-01                   |                    |
| Sueco               |     `sv`      |     ✓      |            |                                 |                    |
| Turco               |     `tr`      |     ✓      |            |                                 |                    |

#### <a name="key-phrase-extraction"></a>[Extracción de frases clave](#tab/key-phrase-extraction)

> [!NOTE]
> Las versiones del modelo de Extracción de frases clave anteriores a la 2020-07-01 tienen un límite de 64 caracteres. Este límite no está presente en versiones posteriores del modelo.

| Idioma              | Código de lenguaje | Compatibilidad con la versión 2 | Compatibilidad con la versión 3 | Disponible a partir de la versión del modelo 3: |       Notas        |
|:----------------------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:------------------:|
| Neerlandés                 |     `nl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Inglés               |     `en`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Finés               |     `fi`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Francés                |     `fr`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Alemán                |     `de`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Italiano               |     `it`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Japonés              |     `ja`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Coreano                |     `ko`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Noruego (Bokmål)   |     `no`      |     ✓      |     ✓      |                2019-10-01                 | También se acepta `nb` |
| Polaco                |     `pl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Portugués (Portugal) |    `pt-PT`    |     ✓      |     ✓      |                2019-10-01                 | También se acepta `pt` |
| Portugués (Brasil)   |    `pt-BR`    |     ✓      |     ✓      |                2019-10-01                 |                    |
| Ruso               |     `ru`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Español               |     `es`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Sueco               |     `sv`      |     ✓      |     ✓      |                2019-10-01                 |                    |

#### <a name="entity-linking"></a>[Vinculación de entidad](#tab/entity-linking)

| Idioma | Código de lenguaje | Compatibilidad con la versión 2 | Compatibilidad con la versión 3 | Disponible a partir de la versión del modelo 3: | Notas |
|:---------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:-----:|
| Inglés  |     `en`      |     ✓      |     ✓      |                2019-10-01                 |       |
| Español  |     `es`      |     ✓      |     ✓      |                2019-10-01                 |       |

#### <a name="language-detection"></a>[Detección de idioma](#tab/language-detection)

Text Analytics API puede detectar una amplia variedad de idiomas, variantes, dialectos y algunos idiomas regionales o culturales, y puede devolver idiomas con su nombre y código. Los parámetros de código de idioma de Detección de idioma de Text Analytics se ajustan al estándar [BCP-47](https://tools.ietf.org/html/bcp47) y la mayoría de ellos cumplen con los identificadores [ISO-639-1](https://www.iso.org/iso-639-language-codes.html). 

Si tiene contenido que se expresa en un idioma que se usa con menos frecuencia, puede probar Detección de idioma para ver si devuelve un código. La respuesta para los idiomas que no se pueden detectar es `unknown`.

| Idioma | Código de lenguaje |  Compatibilidad con la versión 3 | Disponible a partir de la versión del modelo 3: |
|:---------|:-------------:|:----------:|:-----------------------------------------:|
|Afrikáans|`af`|✓|    |
|Albanés|`sq`|✓|    |
|Árabe|`ar`|✓|    |
|Armenio|`hy`|✓|    |
|Vasco|`eu`|✓|    |
|Bielorruso|`be`|✓|    |
|Bengalí|`bn`|✓|    |
|Bosnio|`bs`|✓|01-09-2020|
|Búlgaro|`bg`|✓|    |
|Birmano|`my`|✓|    |
|Catalán, valenciano|`ca`|✓|    |
|Camboyano|`km`|✓|    |
|Chino|`zh`|✓|    |
|Chino simplificado|`zh_chs`|✓|    |
|Chino tradicional|`zh_cht`|✓|    |
|Croata|`hr`|✓|    |
|Checo|`cs`|✓|    |
|Danés|`da`|✓|    |
|Dari|`prs`|✓|01-09-2020|
|Divehi, Dhivehi|`dv`|✓|    |
|Neerlandés, flamenco|`nl`|✓|    |
|Inglés|`en`|✓|    |
|Esperanto|`eo`|✓|    |
|Estonio|`et`|✓|    |
|Fiyiano|`fj`|✓|01-09-2020|
|Finés|`fi`|✓|    |
|Francés|`fr`|✓|    |
|Gallego|`gl`|✓|    |
|Georgiano|`ka`|✓|    |
|Alemán|`de`|✓|    |
|Griego|`el`|✓|    |
|Gujarati|`gu`|✓|    |
|Criollo haitiano|`ht`|✓|    |
|Hebreo|`he`|✓|    |
|Hindi|`hi`|✓|    |
|Hmong Daw|`mww`|✓|01-09-2020|
|Húngaro|`hu`|✓|    |
|Islandés|`is`|✓|    |
|Indonesio|`id`|✓|    |
|Inuktitut|`iu`|✓|    |
|Irlandés|`ga`|✓|    |
|Italiano|`it`|✓|    |
|Japonés|`ja`|✓|    |
|Canarés|`kn`|✓|    |
|Kazajo|`kk`|✓|01-09-2020|
|Coreano|`ko`|✓|    |
|Kurdo|`ku`|✓|    |
|Lao|`lo`|✓|    |
|Latín|`la`|✓|    |
|Letón|`lv`|✓|    |
|Lituano|`lt`|✓|    |
|Macedonio|`mk`|✓|    |
|Malgache|`mg`|✓|01-09-2020|
|Malayo|`ms`|✓|    |
|Malayalam|`ml`|✓|    |
|Maltés|`mt`|✓|    |
|Maori|`mi`|✓|01-09-2020|
|Maratí|`mr`|✓|01-09-2020|
|Noruego|`no`|✓|    |
|Noruego nynorsk|`nn`|✓|    |
|Odia|`or`|✓|    |
|Pastún|`ps`|✓|    |
|Persa|`fa`|✓|    |
|Polaco|`pl`|✓|    |
|Portugués|`pt`|✓|    |
|Punyabí, panjabi|`pa`|✓|    |
|Otomí Querétaro|`otq`|✓|01-09-2020|
|Rumano, moldavo|`ro`|✓|    |
|Ruso|`ru`|✓|    |
|Samoano|`sm`|✓|01-09-2020|
|Serbio|`sr`|✓|    |
|Cingalés|`si`|✓|    |
|Eslovaco|`sk`|✓|    |
|Esloveno|`sl`|✓|    |
|Somalí|`so`|✓|    |
|Español, castellano|`es`|✓|    |
|Swahili|`sw`|✓|    |
|Sueco|`sv`|✓|    |
|Tagalo|`tl`|✓|    |
|Tahitiano|`ty`|✓|01-09-2020|
|Tamil|`ta`|✓|    |
|Telugu|`te`|✓|    |
|Tailandés|`th`|✓|    |
|Tongano|`to`|✓|01-09-2020|
|Turco|`tr`|✓|    |
|Ucraniano|`uk`|✓|    |
|Urdu|`ur`|✓|    |
|Uzbeko|`uz`|✓|    |
|Vietnamita|`vi`|✓|    |
|Galés|`cy`|✓|    |
|Yidis|`yi`|✓|    |
|Maya Yucateco|`yua`|✓|    |


---


## <a name="see-also"></a>Consulte también

* [¿Qué es Text Analytics API?](overview.md)   
