---
title: 'Idiomas admitidos: Translator'
titleSuffix: Azure Cognitive Services
description: Translator de Cognitive Services admite los siguientes idiomas para la conversión de texto a texto con traducción automática neuronal (NMT).
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 6339b86166aff008bf17b5096d42629daf6e3401
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2020
ms.locfileid: "84434288"
---
# <a name="language-and-region-support-for-translator"></a>Compatibilidad de idiomas y regiones para Translator

Translator admite los siguientes idiomas para la traducción de texto a texto. La traducción automática neuronal (NMT) es el nuevo estándar de traducción automática de alta calidad con tecnologías de inteligencia artificial, y está disponible de forma predeterminada con la versión V3 de Translator cuando hay un sistema neuronal disponible.

[Más información sobre cómo funciona la traducción automática](https://www.microsoft.com/translator/mt.aspx)

## <a name="translation"></a>Traducción

**Translator V2**

> [!NOTE]
> V2 quedó en desuso el 30 de abril de 2018. Migre sus aplicaciones a V3 para aprovechar la nueva funcionalidad disponible exclusivamente en V3.

* Solo estadísticas: no hay ningún sistema neuronal disponible para este idioma.
* Traducción neuronal disponible: hay un sistema neuronal disponible. Utilice el parámetro `category=generalnn` para acceder al sistema neuronal.
* Neuronal como sistema predeterminado: el sistema de traducción neuronal es el predeterminado. Utilice el parámetro `category=smt` para acceder al sistema estadístico y utilizarlo con Microsoft Translator Hub.
* Solo neuronal: la traducción neuronal es la única que está disponible.

**Translator V3** Translator V3 utiliza el sistema neuronal de forma predeterminada, por lo que los sistemas estadísticos solamente están disponibles cuando no existe un sistema neuronal.

> [!NOTE]
> Actualmente, un subconjunto de los idiomas neuronales está disponible en Traductor personalizado y agregamos otros adicionales de forma gradual. [Vea los idiomas disponibles actualmente en Traductor personalizado](#customization).

|Idioma|    Código de lenguaje|    API V3|
|:-----|:-----:|:-----|
|Afrikáans|    `af`|    Neuronal|
|Árabe|    `ar`    |    Neuronal|
|Bengalí|    `bn`    |    Neuronal|
|Bosnio (latino)|    `bs`    |    Neuronal|
|Búlgaro|    `bg`    |    Neuronal|
|Cantonés (tradicional)|    `yue`|    Estadística|
|Catalán|    `ca`    |    Estadística|
|Chino simplificado|    `zh-Hans`|Neuronal|
|Chino tradicional|    `zh-Hant`        |Neuronal|
|Croata|    `hr`    |Neuronal|
|Checo|    `cs`    |    Neuronal|
|Danés|    `da`        |Neuronal|
|Neerlandés|    `nl`|    Neuronal|
|Inglés|    `en`    |    Neuronal|
|Estonio|    `et`    |    Neuronal|
|Fiyiano|    `fj`    |    Estadística|
|Filipino|    `fil`    |    Estadística|
|Finés|    `fi`    |    Neuronal|
|Francés|    `fr`    |    Neuronal|
|Alemán|    `de`    |    Neuronal|
|Griego|    `el`    |    Neuronal|
|Gujarati|    `gu`    |    Neuronal|
|Criollo haitiano|    `ht`        |Estadística|
|Hebreo    |`he`    |Neuronal
|Hindi|    `hi`    |    Neuronal|
|Hmong Daw|    `mww`    |    Estadística|
|Húngaro|    `hu`    |    Neuronal|
|Islandés|    `is`    |    Neuronal|
|Indonesio|    `id`    |    Estadística|
|Irlandés | `ga`| Neuronal
|Italiano|    `it`    |    Neuronal|
|Japonés|    `ja`    |    Neuronal|
|Canarés|`kn`| Neuronal|
|Kazajo |`kk`| Neuronal|
|Kiswahili|    `sw`    |    Estadística|
|Klingon|    `tlh-Latn`    |    Estadística|
|Klingon (plqaD)|    `tlh-Piqd`    |    Estadística|
|Coreano    |`ko`    |    Neuronal|
|Letón|    `lv`    |    Neuronal|
|Lituano|    `lt`    |    Neuronal|
|Malgache|    `mg`    |    Estadística|
|Malayo|    `ms`        |Estadística|
|Malayalam| `ml` | Neuronal
|Maltés|    `mt`    |    Estadística|
|Maori| `mi`  | Neuronal|
|Maratí| `mr`  | Neuronal|
|Noruego|    `nb`    |    Neuronal|
|Persa|    `fa`    |    Neuronal|
|Polaco|    `pl`    |    Neuronal|
|Portugués (Brasil)|    `pt-br`    |    Neuronal|
|Portugués (Portugal)| `pt-pt` | Neuronal
|Punjabi|`pa`|Neuronal
|Otomí Querétaro|    `otq`    |    Estadística|
|Rumano|    `ro`    |    Neuronal|
|Ruso|    `ru`    |    Neuronal|
|Samoano|    `sm`    |    Estadística|
|Serbio (cirílico)|    `sr-Cyrl`|    Estadística|
|Serbio (latino)|    `sr-Latn`        |Estadística|
|Eslovaco|    `sk`    |    Neuronal|
|Esloveno|    `sl`    |    Neuronal|
|Español|    `es`    |    Neuronal|
|Sueco|    `sv`    |Neuronal|
|Tahitiano|    `ty`    |Estadística|
|Tamil|    `ta`    |    Neuronal|
|Telugu|    `te`    |    Neuronal|
|Tailandés|    `th`    |    Neuronal|
|Tongano|    `to`    |    Estadística|
|Turco|    `tr`        |Neuronal|
|Ucraniano|    `uk`    |    Neuronal|
|Urdu|    `ur`    |    Estadística|
|Vietnamita|    `vi`    |    Neuronal|
|Galés|    `cy`    |    Neuronal|
|Maya Yucateco|    `yua`    |    Estadística|

> [!NOTE]
> El código de idioma `pt` se establece de forma predeterminada en `pt-br`, Portugués (Brasil).

## <a name="transliteration"></a>Transliteración

El método Transliterate admite los siguientes idiomas. En "Hacia/Desde", "<-->" indica que el idioma se puede transliterar hacia o desde cualquiera de los alfabetos enumerados. "-->" indica que el idioma solo se puede transliterar de un idioma al otro.

| Idioma    | Código de lenguaje | Script | Hacia/Desde | Script|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Árabe | `ar` | Árabe `Arab` | <--> | Latino `Latn` |
|Bengalí  | `bn` | Bengalí `Beng` | <--> | Latino `Latn` |
| Chino (simplificado) | `zh-Hans` | Chino simplificado `Hans`| <--> | Latino `Latn` |
| Chino (simplificado) | `zh-Hans` | Chino simplificado `Hans`| <--> | Chino tradicional `Hant`|
| Chino (tradicional) | `zh-Hant` | Chino tradicional `Hant`| <--> | Latino `Latn` |
| Chino (tradicional) | `zh-Hant` | Chino tradicional `Hant`| <--> | Chino simplificado `Hans` |
| Gujarati | `gu`  | Guyaratí `Gujr` | <--> | Latino `Latn` |
| Hebreo | `he` | Hebreo `Hebr` | <--> | Latino `Latn` |
| Hindi | `hi` | Devanagari `Deva` | <--> | Latino `Latn` |
| Japonés | `ja` | Japonés `Jpan` | <--> | Latino `Latn` |
| Canarés | `kn` | Canarés `Knda` | <--> | Latino `Latn` |
| Malayalam | `ml` | Malayalam `Mlym` | <--> | Latino `Latn` |
| Maratí | `mr` | Devanagari `Deva` | <--> | Latino `Latn` |
| Odia | `or` | Odia `Orya` | <--> | Latino `Latn` |
| Punjabi | `pa` | Gurmukhi `Guru`  | <--> | Latino `Latn`  |
| Serbio (cirílico) | `sr-Cyrl` | Cirílico `Cyrl`  | --> | Latino `Latn` |
| Serbio (latino) | `sr-Latn` | Latino `Latn` | --> | Cirílico `Cyrl`|
| Tamil | `ta` | Tamil `Taml` | <--> | Latino `Latn` |
| Telugu | `te` | Telugu `Telu` | <--> | Latino `Latn` |
| Tailandés | `th` | Tailandés `Thai` | <--> | Latino `Latn` |

## <a name="dictionary"></a>Diccionario

El diccionario admite los siguientes idiomas desde o hacia el inglés con los métodos Lookup y Examples.

| Idioma    | Código de lenguaje |
|:----------- |:-------------:|
| Afrikáans      | `af`          |
| Árabe       | `ar`          |
| Bengalí      | `bn`          |
| Bosnio (latino)      | `bs`          |
| Búlgaro      | `bg`          |
| Catalán      | `ca`          |
| Chino simplificado      | `zh-Hans`          |
| Croata      | `hr`          |
| Checo      | `cs`          |
| Danés      | `da`          |
| Neerlandés      | `nl`          |
| Estonio      | `et`          |
| Finés      | `fi`          |
| Francés      | `fr`          |
| Alemán      | `de`          |
| Griego      | `el`          |
| Criollo haitiano      | `ht`          |
| Hebreo      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Húngaro      | `hu`          |
| Islandés    | `is`  |
| Indonesio      | `id`          |
| Italiano      | `it`          |
| Japonés      | `ja`          |
| Kiswahili      | `sw`          |
| Klingon      | `tlh-Latn`          |
| Coreano      | `ko`          |
| Letón      | `lv`          |
| Lituano      | `lt`          |
| Malayo      | `ms`          |
| Maltés      | `mt`          |
| Noruego      | `nb`          |
| Persa      | `fa`          |
| Polaco      | `pl`          |
| Portugués (Brasil)     | `pt-br`          |
| Rumano      | `ro`          |
| Ruso      | `ru`          |
| Serbio (latino)      | `sr-Latn`          |
| Eslovaco     | `sk`          |
| Esloveno      | `sl`          |
| Español      | `es`          |
| Sueco      | `sv`          |
| Tamil      | `ta`          |
| Tailandés      | `th`          |
| Turco      | `tr`          |
| Ucraniano      | `uk`          |
| Urdu      | `ur`          |
| Vietnamita      | `vi`          |
| Galés      | `cy`          |

## <a name="detect"></a>Detect

Translator detecta todos los idiomas disponibles para traducción y transliteración.


## <a name="access-the-translator-language-list-programmatically"></a>Acceso a la lista de idiomas de Translator mediante programación

Puede recuperar una lista de los idiomas admitidos por Translator v3.0 mediante el método Languages. Puede ver la lista por característica, código de idioma o por nombre del idioma en inglés o en cualquier otro idioma admitido. El servicio Microsoft Translator actualiza esta lista automáticamente cuando hay nuevos idiomas disponibles.

[Ver la documentación de referencia de la operación Languages](reference/v3-0-languages.md)

## <a name="customization"></a>Personalización

Los siguientes idiomas están disponibles para personalización al y del inglés mediante [Traductor personalizado](https://aka.ms/CustomTranslator).

| Idioma    | Código de lenguaje |
|:----------- |:-------------:|
| Árabe       | `ar`          |
| Bengalí      | `bn`          |
| Bosnio (latino)      | `bs`          |
| Búlgaro      | `bg`          |
| Chino simplificado      | `zh-Hans`          |
|Chino tradicional|    `zh-Hant`    |
| Croata      | `hr`          |
| Checo      | `cs`          |
| Danés      | `da`          |
| Neerlandés      | `nl`          |
| Inglés    | `en`     |
| Estonio      | `et`          |
| Finés      | `fi`          |
| Francés      | `fr`          |
| Alemán      | `de`          |
| Griego      | `el`          |
| Hebreo      | `he`          |
| Hindi      | `hi`          |
| Húngaro      | `hu`          |
| Islandés | `is` |
| Indonesio|    `id`    |
| Irlandés | `ga`    |
| Italiano      | `it`          |
| Japonés      | `ja`          |
| Kiswahili|    `sw`    |
| Coreano      | `ko`          |
| Letón      | `lv`          |
| Lituano      | `lt`          |
| Malgache|    `mg`    |
| Maori| `mi`  |
| Noruego      | `nb`          |
| Persa      | `fa`          |
| Polaco      | `pl`          |
| Portugués (Brasil) | `pt-br` |
| Rumano      | `ro`          |
| Ruso      | `ru`          |
| Samoano|    `sm`    |
| Serbio (latino)      | `sr-Latn`          |
| Eslovaco     | `sk`          |
| Esloveno      | `sl`          |
| Español      | `es`          |
| Sueco      | `sv`          |
| Tailandés      | `th`          |
| Turco      | `tr`          |
| Ucraniano      | `uk`          |
| Vietnamita      | `vi`          |
| Galés | `cy` |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Acceso a la lista en el sitio web de Microsoft Translator

Para echar un vistazo rápido a los idiomas, el sitio web de Microsoft Translator muestra todos los idiomas admitidos por las API Translator y Voz. Esta lista no incluye información específica para desarrolladores, por ejemplo, los códigos de idioma.

[Ver la lista de idiomas](https://www.microsoft.com/translator/languages.aspx)
