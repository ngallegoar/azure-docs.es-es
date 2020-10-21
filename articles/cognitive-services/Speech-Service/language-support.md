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
ms.custom: seodec18
ms.openlocfilehash: 3b449913c9d1abbbec84f0ff77f7a1c1bc5321bf
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92048464"
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

| Configuración regional  | Idioma                          | Personalizaciones                                    |
|---------|-----------------------------------|---------------------------------------------------|
| `ar-AE` | Árabe (Emiratos Árabes Unidos)     | Modelo de lenguaje                                    |
| `ar-BH` | Árabe (Bahréin), estándar moderno | Modelo de lenguaje                                    |
| `ar-EG` | Árabe (Egipto)                    | Modelo de lenguaje                                    |
| `ar-IQ` | Árabe (Iraq)                     | Modelo de lenguaje                                    |
| `ar-JO` | Árabe (Jordania)                   | Modelo de lenguaje                                    |
| `ar-KW` | Árabe (Kuwait)                   | Modelo de lenguaje                                    |
| `ar-LB` | Árabe (Líbano)                  | Modelo de lenguaje                                    |
| `ar-OM` | Árabe (Omán)                     | Modelo de lenguaje                                    |
| `ar-QA` | Árabe (Qatar)                    | Modelo de lenguaje                                    |
| `ar-SA` | Árabe (Arabia Saudí)             | Modelo de lenguaje                                    |
| `ar-SY` | Árabe (Siria)                    | Modelo de lenguaje                                    |
| `bg-BG` | Búlgaro (Bulgaria)              | Modelo de lenguaje                                    |
| `ca-ES` | Catalán (España)                   | Modelo de lenguaje                                    |
| `cs-CZ` | Checo (República Checa)            | Modelo de lenguaje                                    | 
| `da-DK` | Danés (Dinamarca)                  | Modelo de lenguaje                                    |
| `de-DE` | Alemán (Alemania)                  | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `el-GR` | Griego (Grecia)                    | Modelo de lenguaje                                    |
| `en-AU` | Inglés (Australia)               | Modelo acústico<br>Modelo de lenguaje                  |
| `en-CA` | Inglés (Canadá)                  | Modelo acústico<br>Modelo de lenguaje                  |
| `en-GB` | Inglés (Reino Unido)          | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `en-HK` | Inglés (Hong Kong)               | Modelo de lenguaje                                    | 
| `en-IE` | Inglés (Irlanda)                 | Modelo de lenguaje                                    | 
| `en-IN` | Inglés (India)                   | Modelo acústico<br>Modelo de lenguaje                  |
| `en-NZ` | Inglés (Nueva Zelanda)             | Modelo acústico<br>Modelo de lenguaje                  |
| `en-PH` | Inglés (Filipinas)             | Modelo de lenguaje                                    | 
| `en-SG` | Inglés (Singapur)               | Modelo de lenguaje                                    | 
| `en-US` | Spanish (Traditional Sort) - Spain           | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `en-ZA` | Inglés (Sudáfrica)            | Modelo de lenguaje                                    | 
| `es-AR` | Español (Argentina)               | Modelo de lenguaje                                    | 
| `es-BO` | Español (Bolivia)                 | Modelo de lenguaje                                    | 
| `es-CL` | Español (Chile)                   | Modelo de lenguaje                                    | 
| `es-CO` | Español (Colombia)                | Modelo de lenguaje                                    | 
| `es-CR` | Español (Costa Rica)              | Modelo de lenguaje                                    | 
| `es-CU` | Español (Cuba)                    | Modelo de lenguaje                                    | 
| `es-DO` | Español (República Dominicana)      | Modelo de lenguaje                                    | 
| `es-EC` | Español (Ecuador)                 | Modelo de lenguaje                                    | 
| `es-ES` | Español (España)                   | Modelo acústico<br>Modelo de lenguaje                  |
| `es-GT` | Español (Guatemala)               | Modelo de lenguaje                                    | 
| `es-HN` | Español (Honduras)                | Modelo de lenguaje                                    | 
| `es-MX` | Español (México)                  | Modelo acústico<br>Modelo de lenguaje                  |
| `es-NI` | Español (Nicaragua)               | Modelo de lenguaje                                    | 
| `es-PA` | Español (Panamá)                  | Modelo de lenguaje                                    | 
| `es-PE` | Español (Perú)                    | Modelo de lenguaje                                    | 
| `es-PR` | Español (Puerto Rico)             | Modelo de lenguaje                                    | 
| `es-PY` | Español (Paraguay)                | Modelo de lenguaje                                    | 
| `es-SV` | Español (El Salvador)             | Modelo de lenguaje                                    | 
| `es-US` | Español (EE. UU.)                     | Modelo de lenguaje                                    | 
| `es-UY` | Español (Uruguay)                 | Modelo de lenguaje                                    | 
| `es-VE` | Español (Venezuela)               | Modelo de lenguaje                                    |
| `et-EE` | Estonio (Estonia)                 | Modelo de lenguaje                                    | 
| `fi-FI` | Finés (Finlandia)                 | Modelo de lenguaje                                    |
| `fr-CA` | Francés (Canadá)                   | Modelo acústico<br>Modelo de lenguaje                  |
| `fr-FR` | Francés (Francia)                   | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `ga-IE` | Irlandés (Irlanda)                    | Modelo de lenguaje                                    |
| `gu-IN` | Gujarati (India)                 | Modelo de lenguaje                                    |
| `hi-IN` | Hindi (India)                     | Modelo acústico<br>Modelo de lenguaje                  |
| `hr-HR` | Croata (Croacia)                | Modelo de lenguaje                                    |
| `hu-HU` | Húngaro (Hungría)               | Modelo de lenguaje                                    | 
| `it-IT` | Italiano (Italia)                   | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `ja-JP` | Japonés (Japón)                  | Modelo de lenguaje                                    |
| `ko-KR` | Coreano (Corea)                    | Modelo de lenguaje                                    |
| `lt-LT` | Lituano (Lituania)            | Modelo de lenguaje                                    |
| `lv-LV` | Letón (Letonia)                  | Modelo de lenguaje                                    |
| `mr-IN` | Maratí (India)                   | Modelo de lenguaje                                    |
| `mt-MT` | Maltés (Malta)                    | Modelo de lenguaje                                    |
| `nb-NO` | Noruego, Bokmål (Noruego)       | Modelo de lenguaje                                    |
| `nl-NL` | Neerlandés (Países Bajos)               | Modelo de lenguaje                                    |
| `pl-PL` | Polaco (Polonia)                   | Modelo de lenguaje                                    |
| `pt-BR` | Portugués (Brasil)               | Modelo acústico<br>Modelo de lenguaje<br>Pronunciación |
| `pt-PT` | Portugués (Portugal)             | Modelo de lenguaje                                    |
| `ro-RO` | Rumano (Rumanía)                | Modelo de lenguaje                                    |
| `ru-RU` | Ruso (Rusia)                  | Modelo acústico<br>Modelo de lenguaje                  |
| `sk-SK` | Eslovaco (Eslovaquia)                 | Modelo de lenguaje                                    |
| `sl-SI` | Esloveno (Eslovenia)              | Modelo de lenguaje                                    |
| `sv-SE` | Sueco (Suecia)                  | Modelo de lenguaje                                    |
| `ta-IN` | Tamil (India)                     | Modelo de lenguaje                                    |
| `te-IN` | Telugu (India)                    | Modelo de lenguaje                                    |
| `th-TH` | Tailandés (Tailandia)                   | Modelo de lenguaje                                    |
| `tr-TR` | Turco (Turquía)                  | Modelo de lenguaje                                    |
| `zh-CN` | Chino (mandarín, simplificado)    | Modelo acústico<br>Modelo de lenguaje                  |
| `zh-HK` | Chino (cantonés, tradicional)  | Modelo de lenguaje                                    |
| `zh-TW` | Chino (mandarín, Taiwán)      | Modelo de lenguaje                                    |

## <a name="text-to-speech"></a>Texto a voz

Tanto el SDK de Voz de Microsoft como las API REST admiten estas voces, y cada una de ellas admite un idioma y un dialecto específicos, que se identifican mediante la configuración regional. También puede obtener una lista completa de los idiomas y las voces que se admiten para cada región o punto de conexión específico a través de [voices/list API](rest-text-to-speech.md#get-a-list-of-voices). 

> [!IMPORTANT]
> Los precios son distintos para voces estándar, personalizadas y neuronales. Consulte la página de [precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para más información.

### <a name="neural-voices"></a>Voces neuronales

Texto a voz neuronal es un nuevo tipo de síntesis de voz con tecnología de redes neuronales profundas. Cuando se usa una voz neuronal, es prácticamente imposible distinguir la voz sintetizada de las grabaciones humanas.

Las voces neuronales se pueden usar para que las interacciones con los bots de chat y los asistentes de voz sean más naturales y atractivas, para convertir textos digitales (por ejemplo, los libros electrónicos) en audiolibros y para mejorar los sistemas de navegación de los automóviles. Con su prosodia natural similar a la humana y la clara articulación de las palabras, las voces neuronales reducen considerablemente la fatiga de la escucha que aparece cuando los usuarios interactúan con sistemas de inteligencia artificial.

Para más información acerca de la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

|Configuración regional  | Idioma            | Sexo | Nombre de voz | Compatibilidad de estilo |
|--|--|--|--|--|
| `ar-EG` | Árabe (Egipto) | Female | `ar-EG-SalmaNeural` | General |
| `ar-SA` | Árabe (Arabia Saudí) | Female | `ar-SA-ZariyahNeural` | General |
| `bg-BG` <sup>Nuevo</sup> | Búlgaro (Bulgaria) | Female | `bg-BG-KalinaNeural` | General |
| `ca-ES` | Catalán (España) | Female | `ca-ES-AlbaNeural` | General |
| `cs-CZ` <sup>Nuevo</sup> | Checo (República Checa)  | Female | `cs-CZ-VlastaNeural` | General |
| `da-DK` | Danés (Dinamarca) | Female | `da-DK-ChristelNeural` | General |
| `de-AT` <sup>Nuevo</sup> | Alemán (Austria) | Female | `de-AT-IngridNeural` | General |
| `de-CH` <sup>Nuevo</sup> | Alemán (Suiza) | Female | `de-CH-LeniNeural` | General |
| `de-DE` | Alemán (Alemania) | Female | `de-DE-KatjaNeural` | General |
| `de-DE` <sup>Nuevo</sup> | Alemán (Alemania) | Male | `de-DE-ConradNeural` | General |
| `el-GR` <sup>Nuevo</sup> | Griego (Grecia) | Female | `el-GR-AthinaNeural` | General |
| `en-AU` | Inglés (Australia) | Female | `en-AU-NatashaNeural` | General |
| `en-AU` <sup>Nuevo</sup> | Australiano (Australia) | Male | `en-AU-WilliamNeural` | General |
| `en-CA` | Inglés (Canadá) | Female | `en-CA-ClaraNeural` | General |
| `en-GB` | Inglés (Reino Unido) | Female | `en-GB-LibbyNeural` | General |
| `en-GB` | Inglés (Reino Unido) | Female | `en-GB-MiaNeural` | General |
| `en-GB` <sup>Nuevo</sup> | Inglés (Reino Unido) | Male | `en-GB-RyanNeural` | General |
| `en-IE` <sup>Nuevo</sup> | Inglés de Irlanda (Irlanda) | Female | `en-IE-EmilyNeural` | General |
| `en-IN` | Inglés (India) | Female | `en-IN-NeerjaNeural` | General |
| `en-US` | Inglés (Estados Unidos) | Female | `en-US-AriaNeural` | General, varios estilos de voz disponibles |
| `en-US` | Inglés (Estados Unidos) | Male | `en-US-GuyNeural` | General |
| `en-US` <sup>Nuevo</sup> | Inglés (Estados Unidos) | Female | `en-US-JennyNeural` | General, varios estilos de voz disponibles |
| `es-ES` | Español (España) | Female | `es-ES-ElviraNeural` | General |
| `es-ES` <sup>Nuevo</sup> | Español (España) | Male | `es-ES-AlvaroNeural` | General |
| `es-MX` | Español (México) | Female | `es-MX-DaliaNeural` | General |
| `es-MX` <sup>Nuevo</sup> | Español (México) | Male | `es-MX-JorgeNeural` | General |
| `fi-FI` | Finés (Finlandia) | Female | `fi-FI-NooraNeural` | General |
| `fr-CA` | Francés (Canadá) | Female | `fr-CA-SylvieNeural` | General |
| `fr-CA` <sup>Nuevo</sup> | Francés (Canadá) | Male | `fr-CA-JeanNeural` | General |
| `fr-CH` <sup>Nuevo</sup> | Francés (Suiza) | Female | `fr-CH-ArianeNeural` | General |
| `fr-FR` | Francés (Francia) | Female | `fr-FR-DeniseNeural` | General |
| `fr-FR` <sup>Nuevo</sup> | Francés (Francia) | Male | `fr-FR-HenriNeural` | General |
| `he-IL` <sup>Nuevo</sup> | Hebreo (Israel) | Female | `he-IL-HilaNeural` | General |
| `hi-IN` | Hindi (India) | Female | `hi-IN-SwaraNeural` | General |
| `hr-HR` <sup>Nuevo</sup> | Croata (Croacia) | Female | `hr-HR-GabrijelaNeural` | General |
| `hu-HU` <sup>Nuevo</sup> | Húngaro (Hungría) | Female | `hu-HU-NoemiNeural` | General |
| `id-ID` <sup>Nuevo</sup> | Indonesio bahasa (Indonesia) | Male | `id-ID-ArdiNeural` | General |
| `it-IT` | Italiano (Italia) | Female | `it-IT-ElsaNeural` | General |
| `it-IT` <sup>Nuevo</sup> | Italiano (Italia) | Female | `it-IT-IsabellaNeural` | General |
| `it-IT` <sup>Nuevo</sup> | Italiano (Italia) | Male | `it-IT-DiegoNeural` | General |
| `ja-JP` | Japonés (Japón) | Female | `ja-JP-NanamiNeural` | General |
| `ja-JP` <sup>Nuevo</sup> | Japonés (Japón) | Male | `ja-JP-KeitaNeural` | General |
| `ko-KR` | Coreano (Corea del Sur) | Female | `ko-KR-SunHiNeural` | General |
| `ko-KR` <sup>Nuevo</sup> | Coreano (Corea) | Male | `ko-KR-InJoonNeural` | General |
| `ms-MY` <sup>Nuevo</sup> | Malayo (Malasia) | Female | `ms-MY-YasminNeural` | General |
| `nb-NO` | Noruego, Bokmål (Noruega) | Female | `nb-NO-IselinNeural` | General |
| `nl-NL` | Neerlandés (Países Bajos) | Female | `nl-NL-ColetteNeural` | General |
| `pl-PL` | Polaco (Polonia) | Female | `pl-PL-ZofiaNeural` | General |
| `pt-BR` | Portugués (Brasil) | Female | `pt-BR-FranciscaNeural` | General, varios estilos de voz disponibles |
| `pt-BR` <sup>Nuevo</sup> | Portugués de Brasil (Brasil) | Male | `pt-BR-AntonioNeural` | General |
| `pt-PT` | Portugués (Portugal) | Female | `pt-PT-FernandaNeural` | General |
| `ro-RO` <sup>Nuevo</sup> | Rumano (Rumanía) | Female | `ro-RO-AlinaNeural` | General |
| `ru-RU` | Ruso (Rusia) | Female | `ru-RU-DariyaNeural` | General |
| `sk-SK` <sup>Nuevo</sup> | Eslovaco (Eslovaquia) | Female | `sk-SK-ViktoriaNeural` | General |
| `sl-SI` <sup>Nuevo</sup> | Esloveno (Eslovenia) | Female | `sl-SI-PetraNeural` | General |
| `sv-SE` | Sueco (Suecia) | Female | `sv-SE-HilleviNeural` | General |
| `ta-IN` <sup>Nuevo</sup> | Tamil (India) | Female | `ta-IN-PallaviNeural` | General |
| `te-IN` <sup>Nuevo</sup> | Telugu (India) | Female | `te-IN-ShrutiNeural` | General |
| `th-TH` | Tailandés (Tailandia) | Female | `th-TH-AcharaNeural` | General |
| `th-TH` <sup>Nuevo</sup> | Tailandés (Tailandia) | Female | `th-TH-PremwadeeNeural` | General |
| `tr-TR` | Turco (Turquía) | Female | `tr-TR-EmelNeural` | General |
| `vi-VN` <sup>Nuevo</sup> | Vietnamita (Vietnam) | Female | `vi-VN-HoaiMyNeural` | General |
| `zh-CN` | Mandarín (chino simplificado, China) | Female | `zh-CN-XiaoxiaoNeural` | General, varios estilos de voz disponibles |
| `zh-CN` | Mandarín (chino simplificado, China) | Female | `zh-CN-XiaoyouNeural` | Voz de niño, optimizado para la narración de historias |
| `zh-CN` | Mandarín (chino simplificado, China) | Male | `zh-CN-YunyangNeural` | Optimizado para la lectura de noticias, varios estilos de voz disponibles |
| `zh-CN` | Mandarín (chino simplificado, China) | Male | `zh-CN-YunyeNeural` | Optimizado para la narración de historias |
| `zh-HK` | Cantonés (chino tradicional, Hong Kong) | Female | `zh-HK-HiuGaaiNeural` | General |
| `zh-TW` | Mandarín (chino tradicional, Taiwán) | Female | `zh-TW-HsiaoYuNeural` | General |


> [!IMPORTANT]
> La voz `en-US-JessaNeural` ha cambiado a `en-US-AriaNeural`. Si usaba "Jessa" antes, conviértalo a "Aria".

Para obtener información sobre cómo configurar y ajustar las voces neuronales, consulte [Lenguaje de marcado de síntesis de voz](speech-synthesis-markup.md#adjust-speaking-styles).

> [!TIP]
> Puede seguir usando la asignación de nombre completo de servicio, como "Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)", en sus solicitudes de síntesis de voz.

### <a name="standard-voices"></a>Voces estándar

Hay más de 75 voces estándar disponibles en más de 45 idiomas y configuraciones regionales, lo que le permite convertir texto en voz sintetizada. Para más información acerca de la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

| Configuración regional | Idioma | Sexo | Nombre de voz |
|--|--|--|--|
| `ar-EG` | Árabe (árabe) | Female | `ar-EG-Hoda` |
| `ar-SA` | Árabe (Arabia Saudí) | Male | `ar-SA-Naayf` |
| `bg-BG` | Búlgaro (Bulgaria) | Male | `bg-BG-Ivan` |
| `ca-ES` | Catalán (España) | Female | `ca-ES-HerenaRUS` |
| `cs-CZ` | Checo (República Checa) | Male | `cs-CZ-Jakub` |
| `da-DK` | Danés (Dinamarca) | Female | `da-DK-HelleRUS` |
| `de-AT` | Alemán (Austria) | Male | `de-AT-Michael` |
| `de-CH` | Alemán (Suiza) | Male | `de-CH-Karsten` |
| `de-DE` | Alemán (Alemania) | Female | `de-DE-HeddaRUS` |
| `de-DE` | Alemán (Alemania) | Male | `de-DE-Stefan` |
| `el-GR` | Griego (Grecia) | Male | `el-GR-Stefanos` |
| `en-AU` | Inglés (Australia) | Female | `en-AU-Catherine` |
| `en-AU` | Inglés (Australia) | Female | `en-AU-HayleyRUS` |
| `en-CA` | Inglés (Canadá) | Female | `en-CA-HeatherRUS` |
| `en-CA` | Inglés (Canadá) | Female | `en-CA-Linda` |
| `en-GB` | Inglés (Reino Unido) | Male | `en-GB-George` |
| `en-GB` | Inglés (Reino Unido) | Female | `en-GB-HazelRUS` |
| `en-GB` | Inglés (Reino Unido) | Female | `en-GB-Susan` |
| `en-IE` | Inglés (Irlanda) | Male | `en-IE-Sean` |
| `en-IN` | Inglés (India) | Female | `en-IN-Heera` |
| `en-IN` | Inglés (India) | Female | `en-IN-PriyaRUS` |
| `en-IN` | Inglés (India) | Male | `en-IN-Ravi` |
| `en-US` | Spanish (Traditional Sort) - Spain | Male | `en-US-BenjaminRUS` |
| `en-US` | Spanish (Traditional Sort) - Spain | Male | `en-US-GuyRUS` |
| `en-US` | Spanish (Traditional Sort) - Spain | Female | `en-US-JessaRUS` |
| `en-US` | Spanish (Traditional Sort) - Spain | Female | `en-US-ZiraRUS` |
| `es-ES` | Español (España) | Female | `es-ES-HelenaRUS` |
| `es-ES` | Español (España) | Female | `es-ES-Laura` |
| `es-ES` | Español (España) | Male | `es-ES-Pablo` |
| `es-MX` | Español (México) | Female | `es-MX-HildaRUS` |
| `es-MX` | Español (México) | Male | `es-MX-Raul` |
| `fi-FI` | Finés (Finlandia) | Female | `fi-FI-HeidiRUS` |
| `fr-CA` | Francés (Canadá) | Female | `fr-CA-Caroline` |
| `fr-CA` | Francés (Canadá) | Female | `fr-CA-HarmonieRUS` |
| `fr-CH` | Francés (Suiza) | Male | `fr-CH-Guillaume` |
| `fr-FR` | Francés (Francia) | Female | `fr-FR-HortenseRUS` |
| `fr-FR` | Francés (Francia) | Female | `fr-FR-Julie` |
| `fr-FR` | Francés (Francia) | Male | `fr-FR-Paul` |
| `he-IL` | Hebreo (Israel) | Male | `he-IL-Asaf` |
| `hi-IN` | Hindi (India) | Male | `hi-IN-Hemant` |
| `hi-IN` | Hindi (India) | Female | `hi-IN-Kalpana` |
| `hr-HR` | Croata (Croacia) | Male | `hr-HR-Matej` |
| `hu-HU` | Húngaro (Hungría) | Male | `hu-HU-Szabolcs` |
| `id-ID` | Indonesio (Indonesia) | Male | `id-ID-Andika` |
| `it-IT` | Italiano (Italia) | Male | `it-IT-Cosimo` |
| `it-IT` | Italiano (Italia) | Female | `it-IT-LuciaRUS` |
| `ja-JP` | Japonés (Japón) | Female | `ja-JP-Ayumi` |
| `ja-JP` | Japonés (Japón) | Female | `ja-JP-HarukaRUS` |
| `ja-JP` | Japonés (Japón) | Male | `ja-JP-Ichiro` |
| `ko-KR` | Coreano (Corea) | Female | `ko-KR-HeamiRUS` |
| `ms-MY` | Malayo (Malasia) | Male | `ms-MY-Rizwan` |
| `nb-NO` | Noruego, Bokmål (Noruega) | Female | `nb-NO-HuldaRUS` |
| `nl-NL` | Neerlandés (Países Bajos) | Female | `nl-NL-HannaRUS` |
| `pl-PL` | Polaco (Polonia) | Female | `pl-PL-PaulinaRUS` |
| `pt-BR` | Portugués (Brasil) | Male | `pt-BR-Daniel` |
| `pt-BR` | Portugués (Brasil) | Female | `pt-BR-HeloisaRUS` |
| `pt-PT` | Portugués (Portugal) | Female | `pt-PT-HeliaRUS` |
| `ro-RO` | Rumano (Rumanía) | Male | `ro-RO-Andrei` |
| `ru-RU` | Ruso (Rusia) | Female | `ru-RU-EkaterinaRUS` |
| `ru-RU` | Ruso (Rusia) | Female | `ru-RU-Irina` |
| `ru-RU` | Ruso (Rusia) | Male | `ru-RU-Pavel` |
| `sk-SK` | Eslovaco (Eslovaquia) | Male | `sk-SK-Filip` |
| `sl-SI` | Esloveno (Eslovenia) | Male | `sl-SI-Lado` |
| `sv-SE` | Sueco (Suecia) | Female | `sv-SE-HedvigRUS` |
| `ta-IN` | Tamil (India) | Male | `ta-IN-Valluvar` |
| `te-IN` | Telugu (India) | Female | `te-IN-Chitra` |
| `th-TH` | Tailandés (Tailandia) | Male | `th-TH-Pattara` |
| `tr-TR` | Turco (Turquía) | Female | `tr-TR-SedaRUS` |
| `vi-VN` | Vietnamita (Vietnam) | Male | `vi-VN-An` |
| `zh-CN` | Mandarín (chino simplificado, China) | Female | `zh-CN-HuihuiRUS` |
| `zh-CN` | Mandarín (chino simplificado, China) | Male | `zh-CN-Kangkang` |
| `zh-CN` | Mandarín (chino simplificado, China) | Female | `zh-CN-Yaoyao` |
| `zh-HK` | Cantonés (chino tradicional, Hong Kong) | Male | `zh-HK-Danny` |
| `zh-HK` | Cantonés (chino tradicional, Hong Kong) | Female | `zh-HK-TracyRUS` |
| `zh-TW` | Mandarín (chino tradicional, Taiwán) | Female | `zh-TW-HanHanRUS` |
| `zh-TW` | Mandarín (chino tradicional, Taiwán) | Female | `zh-TW-Yating` |
| `zh-TW` | Mandarín (chino tradicional, Taiwán) | Male | `zh-TW-Zhiwei` |

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

| Configuración regional | Idioma | Comprobación dependiente del texto | Comprobación independiente del texto | Identificación independiente del texto |
|----|----|----|----|----|
| es-ES | Inglés (EE. UU.) | sí | sí | sí |
|zh-CN    |Chino (mandarín, simplificado)|    N/D|    sí|    sí|
|de-DE    |Alemán (Alemania)    |N/D    |sí    |sí|
|en-GB    |English (Reino Unido)    |N/D    |sí    |sí|
|fr-FR    |Francés (Francia)    |N/D    |sí    |sí|
|en-AU    |Inglés (Australia)    |N/D    |sí    |sí|
|en-CA    |Inglés (Canadá)    |N/D|    sí|    sí|
|fr-CA    |Francés (Canadá)    |N/D    |sí|    sí|
|it-IT    |Italiano|    N/D    |sí|    sí|
|es-ES|    Español (España)    |N/D    |sí|    sí|
|es-MX    |Español (México)    |N/D|    sí|    sí|
|ja-JP|    Japonés    |N/D    |sí    |sí|
|pt-BR|    Portugués (Brasil)|    N/D|    sí|    sí|

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una cuenta de Azure gratuita](https://azure.microsoft.com/free/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
