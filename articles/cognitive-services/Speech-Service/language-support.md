---
title: 'Compatibilidad con idiomas: servicio de voz'
titleSuffix: Azure Cognitive Services
description: El servicio de voz admite un gran número de idiomas para la conversión de texto a voz y voz a texto, junto con la traducción de voz. En este artículo se proporciona una lista completa de idiomas compatibles por servicio.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: trbye
ms.custom: references_regions
ms.openlocfilehash: a430333f57b3734e5977db5c104c8f6d37bfd3c4
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94949095"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Compatibilidad con idiomas y voces en el servicio de voz

La compatibilidad con los idiomas varía según la funcionalidad del servicio de voz. En las tablas siguientes se resume la compatibilidad con idiomas para la [conversión de voz a texto](#speech-to-text), [conversión de texto a voz](#text-to-speech) y las ofertas del servicio [Speech Translation](#speech-translation).

## <a name="speech-to-text"></a>Voz a texto

El SDK de Voz de Microsoft y la API REST admiten los siguientes idiomas (configuraciones regionales). 

Para mejorar la precisión, se ofrece la posibilidad de personalizar un subconjunto de idiomas mediante la carga de **audio y transcripciones etiquetadas por humanos** o **texto relacionado: oraciones**. Para más información sobre la personalización, consulte [Introducción a Custom Speech](how-to-custom-speech.md).

<!--
To get the AM and ML bits:
https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Idioma                          | Configuración regional (BCP-47) | Personalizaciones                                   |
|-----------------------------------|--------|--------------------------------------------------|
|Árabe (Bahréin), estándar moderno  |`ar-BH` | Modelo de lenguaje                                   |
|Árabe (Egipto)                     |`ar-EG` | Modelo de lenguaje                                   |
|Árabe (Iraq)                      |`ar-IQ` | Modelo de lenguaje                                   |
|Árabe (Jordania)                    |`ar-JO` | Modelo de lenguaje                                   |
|Árabe (Kuwait)                    |`ar-KW` | Modelo de lenguaje                                   |
|Árabe (Líbano)                   |`ar-LB` | Modelo de lenguaje                                   |
|Árabe (Omán)                      |`ar-OM` | Modelo de lenguaje                                   |
|Árabe (Qatar)                     |`ar-QA` | Modelo de lenguaje                                   |
|Árabe (Arabia Saudí)              |`ar-SA` | Modelo de lenguaje                                   |
|Árabe (Siria)                     |`ar-SY` | Modelo de lenguaje                                   |
|Árabe (Emiratos Árabes Unidos)      |`ar-AE` | Modelo de lenguaje                                   |
|Búlgaro (Bulgaria)               |`bg-BG` | Modelo de lenguaje                                   |
|Catalán (España)                    |`ca-ES` | Modelo de lenguaje                                   |
|Chino (cantonés, tradicional)   |`zh-HK` | Modelo de lenguaje                                   |
|Chino (mandarín, simplificado)     |`zh-CN` | Modelo acústico<br>Modelo de lenguaje                 |
|Chino (mandarín, Taiwán)       |`zh-TW` | Modelo de lenguaje                                   |
|Croata (Croacia)                 |`hr-HR` | Modelo de lenguaje                                   |
|Checo (República Checa)             |`cs-CZ` | Modelo de lenguaje                                   |
|Danés (Dinamarca)                   |`da-DK` | Modelo de lenguaje                                   |
|Neerlandés (Países Bajos)                |`nl-NL` | Modelo de lenguaje                                   |
|Inglés (Australia)                |`en-AU` | Modelo acústico<br>Modelo de lenguaje                 |
|Inglés (Canadá)                   |`en-CA` | Modelo acústico<br>Modelo de lenguaje                 |
|Inglés (Hong Kong)                |`en-HK` | Modelo de lenguaje                                   |
|Inglés (India)                    |`en-IN` | Modelo acústico<br>Modelo de lenguaje                 |
|Inglés (Irlanda)                  |`en-IE` | Modelo de lenguaje                                   |
|Inglés (Nueva Zelanda)              |`en-NZ` | Modelo acústico<br>Modelo de lenguaje                 |
|Inglés (Filipinas)              |`en-PH` | Modelo de lenguaje                                   |
|Inglés (Singapur)                |`en-SG` | Modelo de lenguaje                                   |
|Inglés (Sudáfrica)             |`en-ZA` | Modelo de lenguaje                                   |
|Inglés (Reino Unido)           |`en-GB` | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación|
|Spanish (Traditional Sort) - Spain            |`en-US` | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación|
|Estonio (Estonia)                  |`et-EE` | Modelo de lenguaje                                   |
|Finés (Finlandia)                  |`fi-FI` | Modelo de lenguaje                                   |
|Francés (Canadá)                    |`fr-CA` | Modelo acústico<br>Modelo de lenguaje                 |
|Francés (Francia)                    |`fr-FR` | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación|
|Alemán (Alemania)                   |`de-DE` | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación|
|Griego (Grecia)                     |`el-GR` | Modelo de lenguaje                                   |
|Gujarati (India)                  |`gu-IN` | Modelo de lenguaje                                   |
|Hindi (India)                      |`hi-IN` | Modelo acústico<br>Modelo de lenguaje                 |
|Húngaro (Hungría)                |`hu-HU` | Modelo de lenguaje                                   |
|Irlandés (Irlanda)                     |`ga-IE` | Modelo de lenguaje                                   |
|Italiano (Italia)                    |`it-IT` | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación|
|Japonés (Japón)                   |`ja-JP` | Modelo de lenguaje                                   |
|Coreano (Corea)                     |`ko-KR` | Modelo de lenguaje                                   |
|Letón (Letonia)                   |`lv-LV` | Modelo de lenguaje                                   |
|Lituano (Lituania)             |`lt-LT` | Modelo de lenguaje                                   |
|Maltés (Malta)                     |`mt-MT` | Modelo de lenguaje                                   |
|Maratí (India)                    |`mr-IN` | Modelo de lenguaje                                   |
|Noruego (Bokmål, Noruega)         |`nb-NO` | Modelo de lenguaje                                   |
|Polaco (Polonia)                    |`pl-PL` | Modelo de lenguaje                                   |
|Portugués (Brasil)                |`pt-BR` | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación|
|Portugués (Portugal)              |`pt-PT` | Modelo de lenguaje                                   |
|Rumano (Rumanía)                 |`ro-RO` | Modelo de lenguaje                                   |
|Ruso (Rusia)                   |`ru-RU` | Modelo acústico<br>Modelo de lenguaje                 |
|Eslovaco (Eslovaquia)                  |`sk-SK` | Modelo de lenguaje                                   |
|Esloveno (Eslovenia)               |`sl-SI` | Modelo de lenguaje                                   |
|Español (Argentina)                |`es-AR` | Modelo de lenguaje                                   |
|Español (Bolivia)                  |`es-BO` | Modelo de lenguaje                                   |
|Español (Chile)                    |`es-CL` | Modelo de lenguaje                                   |
|Español (Colombia)                 |`es-CO` | Modelo de lenguaje                                   |
|Español (Costa Rica)               |`es-CR` | Modelo de lenguaje                                   |
|Español (Cuba)                     |`es-CU` | Modelo de lenguaje                                   |
|Español (República Dominicana)       |`es-DO` | Modelo de lenguaje                                   |
|Español (Ecuador)                  |`es-EC` | Modelo de lenguaje                                   |
|Español (El Salvador)              |`es-SV` | Modelo de lenguaje                                   |
|Español (Guatemala)                |`es-GT` | Modelo de lenguaje                                   |
|Español (Honduras)                 |`es-HN` | Modelo de lenguaje                                   |
|Español (México)                   |`es-MX` | Modelo acústico<br>Modelo de lenguaje                 |
|Español (Nicaragua)                |`es-NI` | Modelo de lenguaje                                   |
|Español (Panamá)                   |`es-PA` | Modelo de lenguaje                                   |
|Español (Paraguay)                 |`es-PY` | Modelo de lenguaje                                   |
|Español (Perú)                     |`es-PE` | Modelo de lenguaje                                   |
|Español (Puerto Rico)              |`es-PR` | Modelo de lenguaje                                   |
|Español (España)                    |`es-ES` | Modelo acústico<br>Modelo de lenguaje                 |
|Español (Uruguay)                  |`es-UY` | Modelo de lenguaje                                   |
|Español (EE. UU.)                      |`es-US` | Modelo de lenguaje                                   |
|Español (Venezuela)                |`es-VE` | Modelo de lenguaje                                   |
|Sueco (Suecia)                   |`sv-SE` | Modelo de lenguaje                                   |
|Tamil (India)                      |`ta-IN` | Modelo de lenguaje                                   |
|Telugu (India)                     |`te-IN` | Modelo de lenguaje                                   |
|Tailandés (Tailandia)                    |`th-TH` | Modelo de lenguaje                                   |
|Turco (Turquía)                   |`tr-TR` | Modelo de lenguaje                                   |

## <a name="text-to-speech"></a>Texto a voz

Tanto el SDK de Voz de Microsoft como las API REST admiten estas voces, y cada una de ellas admite un idioma y un dialecto específicos, que se identifican mediante la configuración regional. También puede obtener una lista completa de los idiomas y las voces que se admiten para cada región o punto de conexión específico a través de [voices/list API](rest-text-to-speech.md#get-a-list-of-voices). 

> [!IMPORTANT]
> Los precios son distintos para voces estándar, personalizadas y neuronales. Consulte la página de [precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para más información.

### <a name="neural-voices"></a>Voces neuronales

Texto a voz neuronal es un nuevo tipo de síntesis de voz con tecnología de redes neuronales profundas. Cuando se usa una voz neuronal, es prácticamente imposible distinguir la voz sintetizada de las grabaciones humanas.

Las voces neuronales se pueden usar para que las interacciones con los bots de chat y los asistentes de voz sean más naturales y atractivas, para convertir textos digitales (por ejemplo, los libros electrónicos) en audiolibros y para mejorar los sistemas de navegación de los automóviles. Con su prosodia natural similar a la humana y la clara articulación de las palabras, las voces neuronales reducen considerablemente la fatiga de la escucha que aparece cuando los usuarios interactúan con sistemas de inteligencia artificial.

| Idioma | Configuración regional | Sexo | Nombre de voz | Compatibilidad de estilo |
|---|---|---|---|---|
| Árabe (Egipto) | `ar-EG` | Female | `ar-EG-SalmaNeural` | General |
| Árabe (Arabia Saudí) | `ar-SA` | Female | `ar-SA-ZariyahNeural` | General |
| Búlgaro (Bulgaria) | `bg-BG` | Female | `bg-BG-KalinaNeural` <sup>Nuevo</sup> | General |
| Catalán (España) | `ca-ES` | Female | `ca-ES-AlbaNeural` | General |
| Chino (cantonés, tradicional) | `zh-HK` | Female | `zh-HK-HiuGaaiNeural` | General |
| Chino (mandarín, simplificado) | `zh-CN` | Female | `zh-CN-XiaoxiaoNeural` | General, varios estilos de voz disponibles [mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chino (mandarín, simplificado) | `zh-CN` | Female | `zh-CN-XiaoyouNeural` | Voz de niño, optimizado para la narración de historias |
| Chino (mandarín, simplificado) | `zh-CN` | Male   | `zh-CN-YunyangNeural` | Optimizado para lectura de noticias,<br /> varios estilos de voz disponibles [mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chino (mandarín, simplificado) | `zh-CN` | Male   | `zh-CN-YunyeNeural` | Optimizado para la narración de historias |
| Chino (mandarín, Taiwán) | `zh-TW` | Female | `zh-TW-HsiaoYuNeural` | General |
| Croata (Croacia) | `hr-HR` | Female | `hr-HR-GabrijelaNeural` <sup>Nuevo</sup> | General |
| Checo (República Checa) | `cs-CZ` | Female | `cs-CZ-VlastaNeural` <sup>Nuevo</sup>    | General |
| Danés (Dinamarca) | `da-DK` | Female | `da-DK-ChristelNeural` | General |
| Neerlandés (Países Bajos) | `nl-NL` | Female | `nl-NL-ColetteNeural` | General |
| Inglés (Australia) | `en-AU` | Female | `en-AU-NatashaNeural` | General |
| Inglés (Australia) | `en-AU` | Male   | `en-AU-WilliamNeural` <sup>Nuevo</sup>  | General |
| Inglés (Canadá) | `en-CA` | Female | `en-CA-ClaraNeural` | General |
| Inglés (India) | `en-IN` | Female | `en-IN-NeerjaNeural` | General |
| Inglés (Irlanda) | `en-IE` | Female | `en-IE-EmilyNeural` <sup>Nuevo</sup> | General |
| Inglés (Reino Unido) | `en-GB` | Female | `en-GB-LibbyNeural` | General |
| Inglés (Reino Unido) | `en-GB` | Female | `en-GB-MiaNeural` | General |
| Inglés (Reino Unido) | `en-GB` | Male | `en-GB-RyanNeural` <sup>Nuevo</sup> | General |
| Spanish (Traditional Sort) - Spain | `en-US` | Female | `en-US-AriaNeural` | General, varios estilos de voz disponibles [mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Spanish (Traditional Sort) - Spain | `en-US` | Male | `en-US-GuyNeural` | General |
| Spanish (Traditional Sort) - Spain | `en-US` | Female | `en-US-JennyNeural` <sup>Nuevo</sup> | General, varios estilos de voz disponibles [mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Finés (Finlandia) | `fi-FI` | Female | `fi-FI-NooraNeural` | General |
| Francés (Canadá) | `fr-CA` | Female | `fr-CA-SylvieNeural` | General |
| Francés (Canadá) | `fr-CA` | Male | `fr-CA-JeanNeural` <sup>Nuevo</sup> | General |
| Francés (Francia) | `fr-FR` | Female | `fr-FR-DeniseNeural` | General |
| Francés (Francia) | `fr-FR` | Male | `fr-FR-HenriNeural` <sup>Nuevo</sup> | General |
| Francés (Suiza) | `fr-CH` | Female | `fr-CH-ArianeNeural` <sup>Nuevo</sup> | General |
| Alemán (Austria) | `de-AT` | Female | `de-AT-IngridNeural` <sup>Nuevo</sup> | General |
| Alemán (Alemania) | `de-DE` | Female | `de-DE-KatjaNeural` | General |
| Alemán (Alemania) | `de-DE` | Male | `de-DE-ConradNeural` <sup>Nuevo</sup> | General |
| Alemán (Suiza) | `de-CH` | Female | `de-CH-LeniNeural` <sup>Nuevo</sup> | General |
| Griego (Grecia) | `el-GR` | Female | `el-GR-AthinaNeural` <sup>Nuevo</sup> | General |
| Hebreo (Israel) | `he-IL` | Female | `he-IL-HilaNeural` <sup>Nuevo</sup> | General |
| Hindi (India) | `hi-IN` | Female | `hi-IN-SwaraNeural` | General |
| Húngaro (Hungría) | `hu-HU` | Female | `hu-HU-NoemiNeural` <sup>Nuevo</sup> | General |
| Indonesio (Indonesia) | `id-ID` | Male | `id-ID-ArdiNeural` <sup>Nuevo</sup> | General |
| Italiano (Italia) | `it-IT` | Female | `it-IT-ElsaNeural` | General |
| Italiano (Italia) | `it-IT` | Female | `it-IT-IsabellaNeural` <sup>Nuevo</sup> | General |
| Italiano (Italia) | `it-IT` | Male | `it-IT-DiegoNeural` <sup>Nuevo</sup> | General |
| Japonés (Japón) | `ja-JP` | Female | `ja-JP-NanamiNeural` | General |
| Japonés (Japón) | `ja-JP` | Male | `ja-JP-KeitaNeural` <sup>Nuevo</sup> | General |
| Coreano (Corea) | `ko-KR` | Female | `ko-KR-SunHiNeural` | General |
| Coreano (Corea) | `ko-KR` | Male | `ko-KR-InJoonNeural` <sup>Nuevo</sup> | General |
| Malayo (Malasia) | `ms-MY` | Female | `ms-MY-YasminNeural` <sup>Nuevo</sup> | General |
| Noruego (Bokmål, Noruega) | `nb-NO` | Female | `nb-NO-IselinNeural` | General |
| Polaco (Polonia) | `pl-PL` | Female | `pl-PL-ZofiaNeural` | General |
| Portugués (Brasil) | `pt-BR` | Female | `pt-BR-FranciscaNeural` | General, varios estilos de voz disponibles [mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Portugués (Brasil) | `pt-BR` | Male | `pt-BR-AntonioNeural` <sup>Nuevo</sup> | General |
| Portugués (Portugal) | `pt-PT` | Female | `pt-PT-FernandaNeural` | General |
| Rumano (Rumanía) | `ro-RO` | Female | `ro-RO-AlinaNeural` <sup>Nuevo</sup> | General |
| Ruso (Rusia) | `ru-RU` | Female | `ru-RU-DariyaNeural` | General |
| Eslovaco (Eslovaquia) | `sk-SK` | Female | `sk-SK-ViktoriaNeural` <sup>Nuevo</sup> | General |
| Esloveno (Eslovenia) | `sl-SI` | Female | `sl-SI-PetraNeural` <sup>Nuevo</sup> | General |
| Español (México) | `es-MX` | Female | `es-MX-DaliaNeural` | General |
| Español (México) | `es-MX` | Male | `es-MX-JorgeNeural` <sup>Nuevo</sup> | General |
| Español (España) | `es-ES` | Female | `es-ES-ElviraNeural` | General |
| Español (España) | `es-ES` | Male | `es-ES-AlvaroNeural` <sup>Nuevo</sup> | General |
| Sueco (Suecia) | `sv-SE` | Female | `sv-SE-HilleviNeural` | General |
| Tamil (India) | `ta-IN` | Female | `ta-IN-PallaviNeural` <sup>Nuevo</sup> | General |
| Telugu (India) | `te-IN` | Female | `te-IN-ShrutiNeural` <sup>Nuevo</sup> | General |
| Tailandés (Tailandia) | `th-TH` | Female | `th-TH-AcharaNeural` | General |
| Tailandés (Tailandia) | `th-TH` | Female | `th-TH-PremwadeeNeural` <sup>Nuevo</sup> | General |
| Turco (Turquía) | `tr-TR` | Female | `tr-TR-EmelNeural` | General |
| Vietnamita (Vietnam) | `vi-VN` <sup>Nuevo</sup> | Female | `vi-VN-HoaiMyNeural` | General|

#### <a name="neural-voices-in-preview"></a>Voces neuronales en versión preliminar

Las siguientes voces neuronales se encuentran en versión preliminar pública. 

| Idioma                         | Configuración regional  | Sexo | Nombre de voz                             | Compatibilidad de estilo |
|----------------------------------|---------|--------|----------------------------------------|---------------|
| Chino (mandarín, simplificado) | `zh-CN` | Male   | `zh-CN-YunxiNeural` <sup>Nuevo</sup> | General, varios estilos disponibles [mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chino (mandarín, simplificado) | `zh-CN` | Female | `zh-CN-XiaohanNeural` <sup>Nuevo</sup> | General, varios estilos disponibles [mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chino (mandarín, simplificado) | `zh-CN` | Female | `zh-CN-XiaoxuanNeural` <sup>Nuevo</sup> | General, varios estilos y [representaciones disponibles mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chino (mandarín, simplificado) | `zh-CN` | Female | `zh-CN-XiaomoNeural` <sup>Nuevo</sup> | General, varios estilos y [representaciones disponibles mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chino (mandarín, simplificado) | `zh-CN` | Female | `zh-CN-XiaoruiNeural` <sup>Nuevo</sup> | Voz de adulto de edad avanzada, varios estilos disponibles [mediante SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Estonio (Estonia) | `et-EE` | Female | `et-EE-AnuNeural` <sup>Nuevo</sup> | General |
| Gaélico (Irlanda) | `ga-IE` | Female | `ga-IE-OrlaNeural` <sup>Nuevo</sup> | General |
| Lituano (Lituania) | `lt-LT` | Female | `lt-LT-OnaNeural` <sup>Nuevo</sup> | General |
| Letón (Letonia) | `lv-LV` | Female | `lv-LV-EveritaNeural` <sup>Nuevo</sup> | General |
| Maltés (Malta) | `mt-MT` | Female | `mt-MT-GraceNeural` <sup>Nuevo</sup> | General |

> [!IMPORTANT]
> Las voces en versión preliminar pública solo están disponibles en tres regiones del servicio: Este de EE. UU., Oeste de Europa y Sudeste de Asia.

Para más información acerca de la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

Para más información sobre cómo configurar y ajustar las voces neuronales, por ejemplo, los estilos de habla, consulte [Lenguaje de marcado de síntesis de voz](speech-synthesis-markup.md#adjust-speaking-styles).

> [!IMPORTANT]
> La voz `en-US-JessaNeural` ha cambiado a `en-US-AriaNeural`. Si usaba "Jessa" antes, conviértalo a "Aria".

> [!TIP]
> Puede seguir usando la asignación de nombre completo de servicio, como "Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)", en sus solicitudes de síntesis de voz.

### <a name="standard-voices"></a>Voces estándar

Hay más de 75 voces estándar disponibles en más de 45 idiomas y configuraciones regionales, lo que le permite convertir texto en voz sintetizada. Para más información acerca de la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

| Idioma | Configuración regional (BCP-47) | Sexo | Nombre de voz |
|--|--|--|--|
| Árabe (árabe) | `ar-EG` | Female | `ar-EG-Hoda`|
| Árabe (Arabia Saudí) | `ar-SA` | Male | `ar-SA-Naayf`|
| Búlgaro (Bulgaria) | `bg-BG` | Male | `bg-BG-Ivan`|
| Catalán (España) | `ca-ES` | Female | `ca-ES-HerenaRUS`|
| Chino (cantonés, tradicional) | `zh-HK` | Male | `zh-HK-Danny`|
| Chino (cantonés, tradicional) | `zh-HK` | Female | `zh-HK-TracyRUS`|
| Chino (mandarín, simplificado) | `zh-CN` | Female | `zh-CN-HuihuiRUS`|
| Chino (mandarín, simplificado) | `zh-CN` | Male | `zh-CN-Kangkang`|
| Chino (mandarín, simplificado) | `zh-CN` | Female | `zh-CN-Yaoyao`|
| Chino (mandarín, Taiwán) |  `zh-TW` | Female | `zh-TW-HanHanRUS`|
| Chino (mandarín, Taiwán) |  `zh-TW` | Female | `zh-TW-Yating`|
| Chino (mandarín, Taiwán) |  `zh-TW` | Male | `zh-TW-Zhiwei`|
| Croata (Croacia) | `hr-HR` | Male | `hr-HR-Matej`|
| Checo (República Checa) | `cs-CZ` | Male | `cs-CZ-Jakub`|
| Danés (Dinamarca) | `da-DK` | Female | `da-DK-HelleRUS`|
| Neerlandés (Países Bajos) | `nl-NL` | Female | `nl-NL-HannaRUS`|
| Inglés (Australia) | `en-AU` | Female | `en-AU-Catherine`|
| Inglés (Australia) | `en-AU` | Female | `en-AU-HayleyRUS`|
| Inglés (Canadá) | `en-CA` | Female | `en-CA-HeatherRUS`|
| Inglés (Canadá) | `en-CA` | Female | `en-CA-Linda`|
| Inglés (India) | `en-IN` | Female | `en-IN-Heera`|
| Inglés (India) | `en-IN` | Female | `en-IN-PriyaRUS`|
| Inglés (India) | `en-IN` | Male | `en-IN-Ravi`|
| Inglés (Irlanda) | `en-IE` | Male | `en-IE-Sean`|
| Inglés (Reino Unido) | `en-GB` | Male | `en-GB-George`|
| Inglés (Reino Unido) | `en-GB` | Female | `en-GB-HazelRUS`|
| Inglés (Reino Unido) | `en-GB` | Female | `en-GB-Susan`|
| Spanish (Traditional Sort) - Spain | `en-US` | Male | `en-US-BenjaminRUS`|
| Spanish (Traditional Sort) - Spain | `en-US` | Male | `en-US-GuyRUS`|
| Spanish (Traditional Sort) - Spain | `en-US` | Female | `en-US-JessaRUS`|
| Spanish (Traditional Sort) - Spain | `en-US` | Female | `en-US-ZiraRUS`|
| Finés (Finlandia) | `fi-FI` | Female | `fi-FI-HeidiRUS`|
| Francés (Canadá) | `fr-CA` | Female | `fr-CA-Caroline`|
| Francés (Canadá) | `fr-CA` | Female | `fr-CA-HarmonieRUS`|
| Francés (Francia) | `fr-FR` | Female | `fr-FR-HortenseRUS`|
| Francés (Francia) | `fr-FR` | Female | `fr-FR-Julie`|
| Francés (Francia) | `fr-FR` | Male | `fr-FR-Paul`|
| Francés (Suiza) | `fr-CH` | Male | `fr-CH-Guillaume`|
| Alemán (Austria) | `de-AT` | Male | `de-AT-Michael`|
| Alemán (Alemania) | `de-DE` | Female | `de-DE-HeddaRUS`|
| Alemán (Alemania) | `de-DE` | Male | `de-DE-Stefan`|
| Alemán (Suiza) | `de-CH` | Male | `de-CH-Karsten`|
| Griego (Grecia) | `el-GR` | Male | `el-GR-Stefanos`|
| Hebreo (Israel) | `he-IL` | Male | `he-IL-Asaf`|
| Hindi (India) | `hi-IN` | Male | `hi-IN-Hemant`|
| Hindi (India) | `hi-IN` | Female | `hi-IN-Kalpana`|
| Húngaro (Hungría) | `hu-HU` | Male | `hu-HU-Szabolcs`|
| Indonesio (Indonesia) | `id-ID` | Male | `id-ID-Andika`|
| Italiano (Italia) | `it-IT` | Male | `it-IT-Cosimo`|
| Italiano (Italia) | `it-IT` | Female | `it-IT-LuciaRUS`|
| Japonés (Japón) | `ja-JP` | Female | `ja-JP-Ayumi`|
| Japonés (Japón) | `ja-JP` | Female | `ja-JP-HarukaRUS`|
| Japonés (Japón) | `ja-JP` | Male | `ja-JP-Ichiro`|
| Coreano (Corea) | `ko-KR` | Female | `ko-KR-HeamiRUS`|
| Malayo (Malasia) | `ms-MY` | Male | `ms-MY-Rizwan`|
| Noruego (Bokmål, Noruega) | `nb-NO` | Female | `nb-NO-HuldaRUS`|
| Polaco (Polonia) | `pl-PL` | Female | `pl-PL-PaulinaRUS`|
| Portugués (Brasil) | `pt-BR` | Male | `pt-BR-Daniel`|
| Portugués (Brasil) | `pt-BR` | Female | `pt-BR-HeloisaRUS`|
| Portugués (Portugal) | `pt-PT` | Female | `pt-PT-HeliaRUS`|
| Rumano (Rumanía) | `ro-RO` | Male | `ro-RO-Andrei`|
| Ruso (Rusia) | `ru-RU` | Female | `ru-RU-EkaterinaRUS`|
| Ruso (Rusia) | `ru-RU` | Female | `ru-RU-Irina`|
| Ruso (Rusia) | `ru-RU` | Male | `ru-RU-Pavel`|
| Eslovaco (Eslovaquia) | `sk-SK` | Male | `sk-SK-Filip`|
| Esloveno (Eslovenia) | `sl-SI` | Male | `sl-SI-Lado`|
| Español (México) | `es-MX` | Female | `es-MX-HildaRUS`|
| Español (México) | `es-MX` | Male | `es-MX-Raul`|
| Español (España) | `es-ES` | Female | `es-ES-HelenaRUS`|
| Español (España) | `es-ES` | Female | `es-ES-Laura`|
| Español (España) | `es-ES` | Male | `es-ES-Pablo`|
| Sueco (Suecia) | `sv-SE` | Female | `sv-SE-HedvigRUS`|
| Tamil (India) | `ta-IN` | Male | `ta-IN-Valluvar`|
| Telugu (India) | `te-IN` | Female | `te-IN-Chitra`|
| Tailandés (Tailandia) | `th-TH` | Male | `th-TH-Pattara`|
| Turco (Turquía) | `tr-TR` | Female | `tr-TR-SedaRUS`|
| Vietnamita (Vietnam) | `vi-VN` | Male | `vi-VN-An` |

> [!IMPORTANT]
> La voz `en-US-Jessa` ha cambiado a `en-US-Aria`. Si usaba "Jessa" antes, conviértalo a "Aria".

> [!TIP]
> Puede seguir usando la asignación de nombre completo de servicio, como "Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)", en sus solicitudes de síntesis de voz.

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
| Gujarati                | `gu`          |
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
| Klingon                 | `tlh-Latn`    |
| Klingon (plqaD)         | `tlh-Piqd`    |
| Coreano                  | `ko`          |
| Letón                 | `lv`          |
| Lituano              | `lt`          |
| Malgache                | `mg`          |
| Malayo                   | `ms`          |
| Malayalam               | `ml`          |
| Maltés                 | `mt`          |
| Maori                   | `mi`          |
| Maratí                 | `mr`          |
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

## <a name="speaker-recognition"></a>Speaker Recognition

Consulte la tabla siguiente para ver los idiomas compatibles con las distintas API de Speaker Recognition. Consulte la [introducción](speaker-recognition-overview.md) para obtener información adicional sobre Speaker Recognition.

| Idioma | Configuración regional (BCP-47) | Comprobación dependiente del texto | Comprobación independiente del texto | Identificación independiente del texto |
|----|----|----|----|----|
|Inglés (EE.UU.)  |  es-ES  |  sí  |  sí  |  sí |
|Chino (mandarín, simplificado) | zh-CN     |     N/D |     sí |     sí|
|Inglés (Australia)     | en-AU     | N/D     | sí     | sí|
|Inglés (Canadá)     | en-CA     | N/D |     sí |     sí|
|English (Reino Unido)     | en-GB     | N/D     | sí     | sí|
|Francés (Canadá)     | fr-CA     | N/D     | sí |     sí|
|Francés (Francia)     | fr-FR     | N/D     | sí     | sí|
|Alemán (Alemania)     | de-DE     | N/D     | sí     | sí|
|Italiano | it-IT     |     N/D     | sí |     sí|
|Japonés     | ja-JP | N/D     | sí     | sí|
|Portugués (Brasil) | pt-BR |     N/D |     sí |     sí|
|Español (México)     | es-MX     | N/D |     sí |     sí|
|Español (España)     | es-ES | N/D     | sí |     sí|

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una cuenta de Azure gratuita](https://azure.microsoft.com/free/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
