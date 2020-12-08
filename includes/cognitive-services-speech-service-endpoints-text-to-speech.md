---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: 650ce0cc9586118b30593767c6a3ddb92f494ac3
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95972666"
---
### <a name="standard-and-neural-voices"></a>Voces estándares y neuronales

Utilice esta tabla para determinar la disponibilidad de las voces estándar y neuronales por región o punto de conexión:

| Region | Punto de conexión | Voces estándar | Voces neuronales |
|--------|----------|-----------------|---------------|
| Este de Australia | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Sur de Brasil | `https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Centro de Canadá | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Centro de EE. UU. | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Este de Asia | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Este de EE. UU. | `https://eastus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Este de EE. UU. 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Centro de Francia | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| India central | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Japón Oriental | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Japón Occidental | `https://japanwest.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Centro de Corea del Sur | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Centro-Norte de EE. UU | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Norte de Europa | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Centro-sur de EE. UU. | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Sudeste de Asia | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Sur de Reino Unido 2 | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Oeste de Europa | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Oeste de EE. UU. | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Oeste de EE. UU. 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |

> [!TIP]
> Las [voces en versión preliminar](../articles/cognitive-services/Speech-Service/language-support.md#neural-voices-in-preview) solo están disponibles en estas tres regiones: Este de EE. UU., Oeste de Europa y Sudeste de Asia.

### <a name="custom-voices"></a>Voces personalizadas

Si ha creado una fuente de voz personalizada, use el punto de conexión que ha creado. También puede utilizar los puntos de conexión que se indican a continuación; para ello, reemplace `{deploymentId}` por el identificador de implementación para el modelo de voz.

| Region | Punto de conexión |
|--------|----------|
| Este de Australia | `https://australiaeast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Sur de Brasil | `https://brazilsouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de Canadá | `https://canadacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de EE. UU. | `https://centralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de Asia | `https://eastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de EE. UU. | `https://eastus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de EE. UU. 2 | `https://eastus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de Francia | `https://francecentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| India central | `https://centralindia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Japón Oriental | `https://japaneast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Japón Occidental | `https://japanwest.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de Corea del Sur | `https://koreacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro-Norte de EE. UU | `https://northcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Norte de Europa | `https://northeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro-sur de EE. UU. | `https://southcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Sudeste de Asia | `https://southeastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Sur de Reino Unido 2 | `https://uksouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Oeste de Europa | `https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Oeste de EE. UU. | `https://westus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Oeste de EE. UU. 2 | `https://westus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
