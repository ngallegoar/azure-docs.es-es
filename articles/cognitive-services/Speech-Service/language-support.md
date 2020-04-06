---
title: 'Compatibilidad con idiomas: servicio de voz'
titleSuffix: Azure Cognitive Services
description: El servicio de voz admite un gran número de idiomas para la conversión de texto a voz y voz a texto, junto con la traducción de voz. En este artículo se proporciona una lista completa de idiomas compatibles por servicio.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: d01ab60790311649e424a98d5a08c6af0bca90f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336037"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Compatibilidad con idiomas y voces en el servicio de voz

La compatibilidad con los idiomas varía según la funcionalidad del servicio de voz. En las tablas siguientes se resume la compatibilidad con idiomas para la [conversión de voz a texto](#speech-to-text), [conversión de texto a voz](#text-to-speech) y las ofertas del servicio [Speech Translation](#speech-translation).

## <a name="speech-to-text"></a>Voz a texto

El SDK de Voz de Microsoft y la API REST admiten los siguientes idiomas (configuraciones regionales). Para mejorar la precisión, se ofrece la personalización para un subconjunto de idiomas mediante la carga de audio y transcripciones con etiqueta humana o texto relacionado: Oraciones. La personalización de la pronunciación solo está disponible actualmente para `en-US` y `de-DE`. Aprenda más sobre la personalización [aquí](how-to-custom-speech.md).

<!--
To get the AM and ML bits:
https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20models%3A/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Configuración regional  | Idioma                          | Compatible | Personalizaciones                                    |
|---------|-----------------------------------|-----------|---------------------------------------------------|
| `ar-AE` | Árabe (Emiratos Árabes Unidos)                      | Sí       | No                                                |
| `ar-BH` | Árabe (Bahréin), estándar moderno | Sí       | Modelo de lenguaje                                    |
| `ar-EG` | Árabe (Egipto)                    | Sí       | Modelo de lenguaje                                    |
| `ar-KW` | Árabe (Kuwait)                   | Sí       | No                                                |
| `ar-QA` | Árabe (Qatar)                    | Sí       | No                                                |
| `ar-SA` | Árabe (Arabia Saudí)             | Sí       | No                                                |
| `ar-SY` | Árabe (Siria)                    | Sí       | Modelo de lenguaje                                    |
| `ca-ES` | Catalán                           | Sí       | Modelo de lenguaje                                    |
| `da-DK` | Danés (Dinamarca)                  | Sí       | Modelo de lenguaje                                    |
| `de-DE` | Alemán (Alemania)                  | Sí       | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `en-AU` | Inglés (Australia)               | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `en-CA` | Inglés (Canadá)                  | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `en-GB` | Inglés (Reino Unido)          | Sí       | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `en-IN` | Inglés (India)                   | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `en-NZ` | Inglés (Nueva Zelanda)             | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `en-US` | Spanish (Traditional Sort) - Spain           | Sí       | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `es-ES` | Español (España)                   | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `es-MX` | Español (México)                  | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `fi-FI` | Finés (Finlandia)                 | Sí       | Modelo de lenguaje                                    |
| `fr-CA` | Francés (Canadá)                   | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `fr-FR` | Francés (Francia)                   | Sí       | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `gu-IN` | Gujarati (India)                 | Sí       | Modelo de lenguaje                                    |
| `hi-IN` | Hindi (India)                     | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `it-IT` | Italiano (Italia)                   | Sí       | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `ja-JP` | Japonés (Japón)                  | Sí       | Modelo de lenguaje                                    |
| `ko-KR` | Coreano (Corea)                    | Sí       | Modelo de lenguaje                                    |
| `mr-IN` | Maratí (India)                   | Sí       | Modelo de lenguaje                                    |
| `nb-NO` | Noruego, Bokmål (Noruego)       | Sí       | Modelo de lenguaje                                    |
| `nl-NL` | Neerlandés (Países Bajos)               | Sí       | Modelo de lenguaje                                    |
| `pl-PL` | Polaco (Polonia)                   | Sí       | Modelo de lenguaje                                    |
| `pt-BR` | Portugués (Brasil)               | Sí       | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `pt-PT` | Portugués (Portugal)             | Sí       | Modelo de lenguaje                                    |
| `ru-RU` | Ruso (Rusia)                  | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `sv-SE` | Sueco (Suecia)                  | Sí       | Modelo de lenguaje                                    |
| `ta-IN` | Tamil (India)                     | Sí       | Modelo de lenguaje                                    |
| `te-IN` | Telugu (India)                    | Sí       | No                                                |
| `th-TH` | Tailandés (Tailandia)                   | Sí       | No                                                |
| `tr-TR` | Turco (Turquía)                  | Sí       | No                                                |
| `zh-CN` | Chino (mandarín, simplificado)    | Sí       | Modelo acústico<br>Modelo de lenguaje                  |
| `zh-HK` | Chino (cantonés, tradicional)  | Sí       | Modelo de lenguaje                                    |
| `zh-TW` | Chino (mandarín, Taiwán)      | Sí       | Modelo de lenguaje                                    |

## <a name="text-to-speech"></a>Texto a voz

Tanto el SDK de Voz de Microsoft como las API REST admiten estas voces, y cada una de ellas admite un idioma y un dialecto específicos, que se identifican mediante la configuración regional.

> [!IMPORTANT]
> Los precios son distintos para voces estándar, personalizadas y neuronales. Consulte la página de [precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para más información.

### <a name="neural-voices"></a>Voces neuronales

Texto a voz neuronal es un nuevo tipo de síntesis de voz con tecnología de redes neuronales profundas. Cuando se usa una voz neuronal, es prácticamente imposible distinguir la voz sintetizada de las grabaciones humanas.

Las voces neuronales se pueden usar para que las interacciones con los bots de chat y los asistentes de voz sean más naturales y atractivas, para convertir textos digitales (por ejemplo, los libros electrónicos) en audiolibros y para mejorar los sistemas de navegación de los automóviles. Con su prosodia natural similar a la humana y la clara articulación de las palabras, las voces neuronales reducen considerablemente la fatiga de la escucha que aparece cuando los usuarios interactúan con sistemas de inteligencia artificial.

Para más información acerca de la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

| Configuración regional  | Idioma            | Sexo | Asignación de nombre de servicio completo                                               | Nombre corto de voz        |
|---------|---------------------|--------|-------------------------------------------------------------------------|-------------------------|
| `de-DE` | Alemán (Alemania)    | Female | "Microsoft Server Speech Text to Speech Voice (de-DE, KatjaNeural)"     | "de-DE-KatjaNeural"     |
| `en-US` | Inglés (EE. UU.)        | Female | "Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)"      | "en-US-AriaNeural"      |
| `en-US` | Inglés (EE. UU.)        | Male   | "Microsoft Server Speech Text to Speech Voice (en-US, GuyNeural)"       | "en-US-GuyNeural"       |
| `it-IT` | Italiano (Italia)     | Female | "Microsoft Server Speech Text to Speech Voice (it-IT, ElsaNeural)"      | "it-IT-ElsaNeural"      |
| `pt-BR` | Portugués (Brasil) | Female | "Microsoft Server Speech Text to Speech Voice (pt-BR, FranciscaNeural)" | "pt-BR-FranciscaNeural" |
| `zh-CN` | Chino (mandarín, simplificado)  | Female | "Microsoft Server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)"  | "zh-CN-XiaoxiaoNeural"  |

> [!IMPORTANT]
> La voz `en-US-JessaNeural` ha cambiado a `en-US-AriaNeural`. Si usaba "Jessa" antes, conviértalo a "Aria".

Para obtener información sobre cómo configurar y ajustar las voces neuronales, consulte [Lenguaje de marcado de síntesis de voz](speech-synthesis-markup.md#adjust-speaking-styles).

> [!TIP]
> Puede usar la asignación de nombre de servicio completo o el nombre corto de voz en las solicitudes de síntesis de voz.

### <a name="standard-voices"></a>Voces estándar

Hay más de 75 voces estándar disponibles en más de 45 idiomas y configuraciones regionales, lo que le permite convertir texto en voz sintetizada. Para más información acerca de la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

| Configuración regional | Idioma | Sexo | Asignación de nombre de servicio completo | Nombre corto |
|--|--|--|--|--|
| <sup>1</sup>`ar-EG` | Árabe (Egipto) | Female | "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)" | "ar-EG-Hoda" |
| `ar-SA` | Árabe (Arabia Saudí) | Male | "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)" | "ar-SA-Naayf" |
| `bg-BG` | Búlgaro | Male | "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)" | "bg-BG-Ivan" |
| `ca-ES` | Catalán (España) | Female | "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)" | "ca-ES-HerenaRUS" |
| `cs-CZ` | Checo | Male | "Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)" | "cs-CZ-Jakub" |
| `da-DK` | Danés | Female | "Microsoft Server Speech Text to Speech Voice (da-DK, HelleRUS)" | "da-DK-HelleRUS" |
| `de-AT` | Alemán (Austria) | Male | "Microsoft Server Speech Text to Speech Voice (de-AT, Michael)" | "de-AT-Michael" |
| `de-CH` | Alemán (Suiza) | Male | "Microsoft Server Speech Text to Speech Voice (de-CH, Karsten)" | "de-CH-Karsten" |
| `de-DE` | Alemán (Alemania) | Female | "Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)" | "de-DE-Hedda" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (de-DE, HeddaRUS)" | "de-DE-HeddaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)" | "de-DE-Stefan-Apollo" |
| `el-GR` | Griego | Male | "Microsoft Server Speech Text to Speech Voice (el-GR, Stefanos)" | "el-GR-Stefanos" |
| `en-AU` | Inglés (Australia) | Female | "Microsoft Server Speech Text to Speech Voice (en-AU, Catherine)" | "en-AU-Catherine" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-AU, HayleyRUS)" | "en-AU-HayleyRUS" |
| `en-CA` | Inglés (Canadá) | Female | "Microsoft Server Speech Text to Speech Voice (en-CA, Linda)" | "en-CA-Linda" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-CA, HeatherRUS)" | "en-CA-HeatherRUS" |
| `en-GB` | English (Reino Unido) | Female | "Microsoft Server Speech Text to Speech Voice (en-GB, Susan, Apollo)" | "en-GB-Susan-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-GB, HazelRUS)" | "en-GB-HazelRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (en-GB, George, Apollo)" | "en-GB-George-Apollo" |
| `en-IE` | Inglés (Irlanda) | Male | "Microsoft Server Speech Text to Speech Voice (en-IE, Sean)" | "en-IE-Sean" |
| `en-IN` | Inglés (India) | Female | "Microsoft Server Speech Text to Speech Voice (en-IN, Heera, Apollo)" | "en-IN-Heera-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-IN, PriyaRUS)" | "en-IN-PriyaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (en-IN, Ravi, Apollo)" | "en-IN-Ravi-Apollo" |
| `en-US` | Inglés (EE. UU.) | Female | "Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)" | "en-US-ZiraRUS" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)" | "en-US-AriaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)" | "en-US-BenjaminRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)" | "en-US-Guy24kRUS" |
| `es-ES` | Español (España) | Female | "Microsoft Server Speech Text to Speech Voice (es-ES, Laura, Apollo)" | "es-ES-Laura-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (es-ES, HelenaRUS)" | "es-ES-HelenaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (es-ES, Pablo, Apollo)" | "es-ES-Pablo-Apollo" |
| `es-MX` | Español (México) | Female | "Microsoft Server Speech Text to Speech Voice (es-MX, HildaRUS)" | "es-MX-HildaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (es-MX, Raul, Apollo)" | "es-MX-Raul-Apollo" |
| `fi-FI` | Finés | Female | "Microsoft Server Speech Text to Speech Voice (fi-FI, HeidiRUS)" | "fi-FI-HeidiRUS" |
| `fr-CA` | Francés (Canadá) | Female | "Microsoft Server Speech Text to Speech Voice (fr-CA, Caroline)" | "fr-CA-Caroline" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (fr-CA, HarmonieRUS)" | "fr-CA-HarmonieRUS" |
| `fr-CH` | Francés (Suiza) | Male | "Microsoft Server Speech Text to Speech Voice (fr-CH, Guillaume)" | "fr-CH-Guillaume" |
| `fr-FR` | Francés (Francia) | Female | "Microsoft Server Speech Text to Speech Voice (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo" |
| `he-IL` | Hebreo (Israel) | Male | "Microsoft Server Speech Text to Speech Voice (he-IL, Asaf)" | "he-IL-Asaf" |
| `hi-IN` | Hindi (India) | Female | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana, Apollo)" | "hi-IN-Kalpana-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana)" | "hi-IN-Kalpana" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (hi-IN, Hemant)" | "hi-IN-Hemant" |
| `hr-HR` | Croata | Male | "Microsoft Server Speech Text to Speech Voice (hr-HR, Matej)" | "hr-HR-Matej" |
| `hu-HU` | Húngaro | Male | "Microsoft Server Speech Text to Speech Voice (hu-HU, Szabolcs)" | "hu-HU-Szabolcs" |
| `id-ID` | Indonesio | Male | "Microsoft Server Speech Text to Speech Voice (id-ID, Andika)" | "id-ID-Andika" |
| `it-IT` | Italiano | Male | "Microsoft Server Speech Text to Speech Voice (it-IT, Cosimo, Apollo)" | "it-IT-Cosimo-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (it-IT, LuciaRUS)" | "it-IT-LuciaRUS" |
| `ja-JP` | Japonés | Female | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS" |
| `ko-KR` | Coreano | Female | "Microsoft Server Speech Text to Speech Voice (ko-KR, HeamiRUS)" | "ko-KR-HeamiRUS" |
| `ms-MY` | Malayo | Male | "Microsoft Server Speech Text to Speech Voice (ms-MY, Rizwan)" | "ms-MY-Rizwan" |
| `nb-NO` | Noruego | Female | "Microsoft Server Speech Text to Speech Voice (nb-NO, HuldaRUS)" | "nb-NO-HuldaRUS" |
| `nl-NL` | Neerlandés | Female | "Microsoft Server Speech Text to Speech Voice (nl-NL, HannaRUS)" | "nl-NL-HannaRUS" |
| `pl-PL` | Polaco | Female | "Microsoft Server Speech Text to Speech Voice (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS" |
| `pt-BR` | Portugués (Brasil) | Female | "Microsoft Server Speech Text to Speech Voice (pt-BR, HeloisaRUS)" | "pt-BR-HeloisaRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (pt-BR, Daniel, Apollo)" | "pt-BR-Daniel-Apollo" |
| `pt-PT` | Portugués (Portugal) | Female | "Microsoft Server Speech Text to Speech Voice (pt-PT, HeliaRUS)" | "pt-PT-HeliaRUS" |
| `ro-RO` | Rumano | Male | "Microsoft Server Speech Text to Speech Voice (ro-RO, Andrei)" | "ro-RO-Andrei" |
| `ru-RU` | Ruso | Female | "Microsoft Server Speech Text to Speech Voice (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (ru-RU, Pavel, Apollo)" | "ru-RU-Pavel-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (ru-RU, EkaterinaRUS)" | ru-RU-EkaterinaRUS |
| `sk-SK` | Eslovaco | Male | "Microsoft Server Speech Text to Speech Voice (sk-SK, Filip)" | "sk-SK-Filip" |
| `sl-SI` | Esloveno | Male | "Microsoft Server Speech Text to Speech Voice (sl-SI, Lado)" | "sl-SI-Lado" |
| `sv-SE` | Sueco | Female | "Microsoft Server Speech Text to Speech Voice (sv-SE, HedvigRUS)" | "sv-SE-HedvigRUS" |
| `ta-IN` | Tamil (India) | Male | "Microsoft Server Speech Text to Speech Voice (ta-IN, Valluvar)" | "ta-IN-Valluvar" |
| `te-IN` | Telugu (India) | Female | "Microsoft Server Speech Text to Speech Voice (te-IN, Chitra)" | "te-IN-Chitra" |
| `th-TH` | Tailandés | Male | "Microsoft Server Speech Text to Speech Voice (th-TH, Pattara)" | "th-TH-Pattara" |
| `tr-TR` | Turco (Turquía) | Female | "Microsoft Server Speech Text to Speech Voice (tr-TR, SedaRUS)" | "tr-TR-SedaRUS" |
| `vi-VN` | Vietnamita | Male | "Microsoft Server Speech Text to Speech Voice (vi-VN, An)" | "vi-VN-An" |
| `zh-CN` | Chino (mandarín, simplificado) | Female | "Microsoft Server Speech Text to Speech Voice (zh-CN, HuihuiRUS)" | "zh-CN-HuihuiRUS" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo" |
| `zh-HK` | Chino (cantonés, tradicional) | Female | "Microsoft Server Speech Text to Speech Voice (zh-HK, Tracy, Apollo)" | "zh-HK-Tracy-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (zh-HK, TracyRUS)" | "zh-HK-TracyRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (zh-HK, Danny, Apollo)" | "zh-HK-Danny-Apollo" |
| `zh-TW` | Chino (mandarín, Taiwán) | Female | "Microsoft Server Speech Text to Speech Voice (zh-TW, Yating, Apollo)" | "zh-TW-Yating-Apollo" |
|  |  | Female | "Microsoft Server Speech Text to Speech Voice (zh-TW, HanHanRUS)" | "zh-TW-HanHanRUS" |
|  |  | Male | "Microsoft Server Speech Text to Speech Voice (zh-TW, Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo" |

**1** *ar-EG admite el árabe estándar moderno (MSA).*

> [!IMPORTANT]
> La voz `en-US-Jessa` ha cambiado a `en-US-Aria`. Si usaba "Jessa" antes, conviértalo a "Aria".

> [!TIP]
> Puede usar la asignación de nombre de servicio completo o el nombre corto de voz en las solicitudes de síntesis de voz.

### <a name="customization"></a>Personalización

La personalización de la voz está disponible para `de-DE`, `en-GB`, `en-IN`, `en-US`, `es-MX`, `fr-FR`, `it-IT`, `pt-BR` y `zh-CN`. Seleccione la configuración regional adecuada que coincida con los datos de entrenamiento que tiene para entrenar un modelo de voz personalizado. Por ejemplo, si los datos de grabación que tienen que hablar en inglés con acento británico, seleccione `en-GB`.

> [!NOTE]
> No se admite el entrenamiento de modelos bilingües en la funcionalidad de voz personalizada, excepto en el caso de chino-inglés bilingüe. Seleccione la opción de chino-inglés bilingüe si quiere entrenar una voz china que pueda hablar también inglés. El entrenamiento de la voz de todas las configuraciones regionales se inicia con un conjunto de datos de más de 2000 expresiones, excepto en el caso de `en-US` y `zh-CN`, que puede comenzar con cualquier tamaño de datos de entrenamiento.

## <a name="speech-translation"></a>Traducción de voz

**Speech Translation** API admite varios idiomas para la traducción de voz a voz y de texto a voz. El idioma de origen debe incluirse siempre en la siguiente tabla de conversión de voz en texto. Los idiomas de destino admitidos dependen de si el destino de traducción es voz o texto. Puede traducir la voz entrante en más de [60 idiomas](https://www.microsoft.com/translator/business/languages/). Un subconjunto de estos idiomas está disponible para la [síntesis de voz](language-support.md#text-languages).

### <a name="text-languages"></a>Idiomas de texto

| Idioma de texto           | Código de lenguaje |
|:------------------------|:-------------:|
| Afrikáans               | `af`          |
| Árabe                  | `ar`          |
| Bengalí                  | `bn`          |
| Bosnio (latino)         | `bs`          |
| Búlgaro               | `bg`          |
| Cantonés (tradicional) | `yue`         |
| Catalán                 | `ca`          |
| Chino simplificado      | `zh-Hans`     |
| Chino tradicional     | `zh-Hant`     |
| Croata                | `hr`          |
| Checo                   | `cs`          |
| Danés                  | `da`          |
| Neerlandés                   | `nl`          |
| Inglés                 | `en`          |
| Estonio                | `et`          |
| Fiyiano                  | `fj`          |
| Filipino                | `fil`         |
| Finés                 | `fi`          |
| Francés                  | `fr`          |
| Alemán                  | `de`          |
| Griego                   | `el`          |
| Criollo haitiano          | `ht`          |
| Hebreo                  | `he`          |
| Hindi                   | `hi`          |
| Hmong Daw               | `mww`         |
| Húngaro               | `hu`          |
| Indonesio              | `id`          |
| Irlandés                   | `ga`          |
| Italiano                 | `it`          |
| Japonés                | `ja`          |
| Canarés                 | `kn`          |
| Kiswahili               | `sw`          |
| Klingon                 | `tlh`         |
| Klingon (plqaD)         | `tlh-Qaak`    |
| Coreano                  | `ko`          |
| Letón                 | `lv`          |
| Lituano              | `lt`          |
| Malgache                | `mg`          |
| Malayo                   | `ms`          |
| Malayalam               | `ml`          |
| Maltés                 | `mt`          |
| Noruego               | `nb`          |
| Persa                 | `fa`          |
| Polaco                  | `pl`          |
| Portugués (Brasil)     | `pt-br`       |
| Portugués (Portugal)   | `pt-pt`       |
| Punjabi                 | `pa`          |
| Otomí Querétaro         | `otq`         |
| Rumano                | `ro`          |
| Ruso                 | `ru`          |
| Samoano                  | `sm`          |
| Serbio (cirílico)      | `sr-Cyrl`     |
| Serbio (latino)         | `sr-Latn`     |
| Eslovaco                  | `sk`          |
| Esloveno               | `sl`          |
| Español                 | `es`          |
| Sueco                 | `sv`          |
| Tahitiano                | `ty`          |
| Tamil                   | `ta`          |
| Telugu                  | `te`          |
| Tailandés                    | `th`          |
| Tongano                  | `to`          |
| Turco                 | `tr`          |
| Ucraniano               | `uk`          |
| Urdu                    | `ur`          |
| Vietnamita              | `vi`          |
| Galés                   | `cy`          |
| Maya Yucateco            | `yua`         |

## <a name="next-steps"></a>Pasos siguientes

* [Obtener la suscripción de evaluación gratuita del servicio de voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
