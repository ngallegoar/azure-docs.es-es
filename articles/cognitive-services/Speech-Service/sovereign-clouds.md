---
title: 'Nubes soberanas: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a usar las nubes soberanas.
services: cognitive-services
author: cbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 1/14/2020
ms.author: cbasoglu
ms.openlocfilehash: b41967033b00144ca5bd52ce23cf8aabcea6749e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78228087"
---
# <a name="speech-services-with-sovereign-clouds"></a>Servicios de voz con nubes soberanas

## <a name="azure-government-united-states"></a>Azure Government (Estados Unidos)

Solo los gobiernos federales, estatales, locales y tribales y sus asociados tienen acceso a esta instancia dedicada con operaciones controladas por ciudadanos autorizados de EE. UU.
- Regiones: US Gov - Virginia
- SR en SpeechSDK: *config.FromHost("wss://virginia.stt.speech.azure.us", "\<your-key\>");*
- TTS en SpeechSDK: *config.FromHost("https[]()://virginia.tts.speech.azure.us", "\<your-key\>");*
- Tokens de autenticación: https[]()://virginia.api.cognitive.microsoft.us/sts/v1.0/issueToken
- Azure Portal: https://portal.azure.us  
- Portal de Habla personalizada: https://virginia.cris.azure.us/Home/CustomSpeech
- SKU disponibles: S0
- Características admitidas:
  - Voz a texto
  - Habla personalizada (adaptación acústica/idioma)
  - Text-to-Speech
  - Translator Speech
- Características no admitidas
  - Voz personalizada
  - Voces neuronales para la conversión de texto a voz
- Configuraciones regionales admitidas: Se admiten las configuraciones regionales de los siguientes idiomas.
  - Árabe (ar-*)
  - Chino (zh-*)
  - Inglés (en-*)
  - Francés (fr-*)
  - Alemán (de-*)
  - Hindi
  - Coreano
  - Ruso
  - Español (es-*)

## <a name="microsoft-azure-china"></a>Microsoft Azure China

Se ubica en China, un centro de datos de Azure con acceso directo a China Mobile, China Telecom, China Unicom y a otras redes troncales de los principales operadores, para proporcionar a los usuarios chinos una experiencia de acceso estable y de alta velocidad a la red local.
- Regiones: Este de China 2 (Shanghái)
- SR en SpeechSDK: *config.FromHost("wss://chinaeast2.stt.speech.azure.cn", "\<your-key\>");*
- TTS en SpeechSDK:  *config.FromHost("https[]()://chinaeast2.tts.speech.azure.cn", "\<your-key\>");*
- Tokens de autenticación: https[]()://chinaeast2.api.cognitive.azure.cn/sts/v1.0/issueToken
- Azure Portal: https://portal.azure.cn
- Portal de Habla personalizada: https://speech.azure.cn/CustomSpeech
- SKU disponibles: S0
- Características admitidas:
  - Voz a texto
  - Habla personalizada (adaptación acústica/idioma)
  - Text-to-Speech
  - Translator Speech
- Características no admitidas
  - Voz personalizada
  - Voces neuronales para la conversión de texto a voz
- Configuraciones regionales admitidas: Se admiten las configuraciones regionales de los siguientes idiomas.
  - Árabe (ar-*)
  - Chino (zh-*)
  - Inglés (en-*)
  - Francés (fr-*)
  - Alemán (de-*)
  - Hindi
  - Coreano
  - Ruso
  - Español (es-*)

