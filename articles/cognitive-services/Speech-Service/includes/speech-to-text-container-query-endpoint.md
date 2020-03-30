---
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/21/2020
ms.author: dapine
ms.openlocfilehash: 9b4317064196c4ea3d761fd1a0bd43a764054fe6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77563283"
---
### <a name="speech-to-text-or-custom-speech-to-text"></a>Conversión de voz a texto o Conversión de voz a texto personalizada

El contenedor proporciona las API de punto de conexión de consulta basadas en WebSocket, a las que se accede mediante el [SDK de Voz](../index.yml). De forma predeterminada, el SDK de Voz usa servicios de voz en línea. Para usar el contenedor, deberá cambiar el método de inicialización.

> [!TIP]
> Al usar el SDK de Voz con los contenedores, no es necesario proporcionar la [clave de suscripción del recurso de Voz de Azure o un token de portador de autenticación](../rest-speech-to-text.md#authentication).

Consulte los ejemplos siguientes.

# <a name="c"></a>[C#](#tab/csharp)

Cambie de usar esta llamada de inicialización en la nube de Azure:

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

por esta llamada mediante el [host](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-dotnet) de contenedor:

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```
# <a name="python"></a>[Python](#tab/python)

Cambie de usar esta llamada de inicialización en la nube de Azure:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

por esta llamada mediante el [host](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) de contenedor:

```python
speech_config = speechsdk.SpeechConfig(
    host="ws://localhost:5000")
```

***
