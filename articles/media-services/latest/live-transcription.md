---
title: Transcripción en directo
titleSuffix: Azure Media Services
description: Más información sobre la transcripción en directo de Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/19/2019
ms.author: inhenkel
ms.openlocfilehash: 9481b4ee2f225c7f76337d73b27630e4c67cc780
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193603"
---
# <a name="live-transcription-preview"></a>Transcripción en directo (versión preliminar)

Azure Media Services ofrece vídeo, audio y texto en diferentes protocolos. Cuando publica el streaming en vivo mediante MPEG-DASH o HLS/CMAF, el servicio proporcionará junto con el vídeo y el audio, el texto transcrito en formato IMSC1.1 compatible con TTML. La entrega se empaqueta en fragmentos MPEG-4 parte 30 (ISO/IEC 14496-30). Si la entrega se realiza mediante HLS/TS, el texto se proporcionará como VTT fragmentado.

En este artículo se describe cómo habilitar la transcripción en directo al hacer streaming de un evento en directo con Azure Media Services v3. Antes de continuar, asegúrese de que está familiarizado con el uso de las API REST de Media Services v3 (consulte [este tutorial](stream-files-tutorial-with-rest.md) para más información). También debe estar familiarizado con el concepto de [streaming en vivo](live-streaming-overview.md). Se recomienda completar el tutorial [Streaming en vivo con Media Services](stream-live-tutorial-with-api.md).

> [!NOTE]
> Actualmente, la transcripción en directo solo está disponible como una característica en vista previa en la región Oeste de EE. UU. 2. Admite la transcripción de texto oral en inglés a texto. La referencia de la API para esta característica se encuentra a continuación; como está en versión preliminar, los detalles no están disponibles en nuestros documentos de REST.

## <a name="creating-the-live-event"></a>Creación del evento en directo

Para crear el evento en directo, se envía la operación PUT a la versión preliminar 2019-05-01, por ejemplo:

```
PUT https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/liveEvents/:liveEventName?api-version=2019-05-01-preview&autoStart=true 
```

La operación tiene el cuerpo siguiente (en el que se crea un evento en directo con paso a través mediante RTMP como protocolo de ingesta). Observe que se ha agregado una propiedad de transcripciones. El único valor permitido para el idioma es "en-US".

```
{
  "properties": {
    "description": "Demonstrate how to enable live transcriptions",
    "input": {
      "streamingProtocol": "RTMP",
      "accessControl": {
        "ip": {
          "allow": [
            {
              "name": "Allow All",
              "address": "0.0.0.0",
              "subnetPrefixLength": 0
            }
          ]
        }
      }
    },
    "preview": {
      "accessControl": {
        "ip": {
          "allow": [
            {
              "name": "Allow All",
              "address": "0.0.0.0",
              "subnetPrefixLength": 0
            }
          ]
        }
      }
    },
    "encoding": {
      "encodingType": "None"
    },
    "transcriptions": [
      {
        "language": "en-US"
      }
    ],
    "vanityUrl": false,
    "streamOptions": [
      "Default"
    ]
  },
  "location": "West US 2"
}
```

Sondee el estado del evento en directo hasta que este pase al estado "En ejecución", que indica que ahora puede enviar una fuente RTMP de contribución. Ahora puede seguir los mismos pasos que en este tutorial, como comprobar la fuente de vista previa y crear salidas en directo.

## <a name="start-transcription-after-live-event-has-started"></a>Inicio de la transcripción después del inicio del evento en directo

La transcripción en directo se puede iniciar después de que haya comenzado un evento en directo. Para activar las transcripciones en vivo, revise el evento en directo para incluir la propiedad "transcriptions". Para desactivar las transcripciones en directo, se quitará la propiedad "transcriptions" del objeto de evento activo.

> [!NOTE]
> La activación o desactivación de la transcripción más de una vez durante el evento en directo no es un escenario admitido.

Esta es la llamada de ejemplo para activar las transcripciones en directo.

PATCH: ```https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/liveEvents/:liveEventName?api-version=2019-05-01-preview```

```
{
  "properties": {
    "description": "Demonstrate how to enable live transcriptions", 
    "input": {
      "streamingProtocol": "RTMP",
      "accessControl": {
        "ip": {
          "allow": [
            {
              "name": "Allow All",
              "address": "0.0.0.0",
              "subnetPrefixLength": 0
            }
          ]
        }
      }
    },
    "preview": {
      "accessControl": {
        "ip": {
          "allow": [
            {
              "name": "Allow All",
              "address": "0.0.0.0",
              "subnetPrefixLength": 0
            }
          ]
        }
      }
    },
    "encoding": {
      "encodingType": "None"
    },
    "transcriptions": [
      {
        "language": "en-US"
      }
    ],
    "vanityUrl": false,
    "streamOptions": [
      "Default"
    ]
  },
  "location": "West US 2"
}
```

## <a name="transcription-delivery-and-playback"></a>Entrega y reproducción de la transcripción

Revise el artículo [Introducción al empaquetado dinámico](dynamic-packaging-overview.md#to-prepare-your-source-files-for-delivery) que describe cómo usa el servicio el empaquetado dinámico para ofrecer vídeo, audio y texto en diferentes protocolos. Cuando publica el streaming en vivo mediante MPEG-DASH o HLS/CMAF, el servicio proporcionará junto con el vídeo y el audio, el texto transcrito en formato IMSC1.1 compatible con TTML. La entrega se empaqueta en fragmentos MPEG-4 parte 30 (ISO/IEC 14496-30). Si la entrega se realiza mediante HLS/TS, el texto se proporcionará como VTT fragmentado. Puede usar un reproductor web como [Azure Media Player](use-azure-media-player.md) para reproducir la secuencia.  

> [!NOTE]
> Si usa Azure Media Player, use la versión 2.3.3 o posterior.

## <a name="known-issues"></a>Problemas conocidos

En la versión preliminar, estos son los problemas conocidos de la transcripción en directo:

* La característica solo está disponible en la región Oeste de EE. UU. 2.
* Las aplicaciones deben usar las API de la versión preliminar que se describen en la [especificación de Media Services v3 OpenAPI](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2019-05-01-preview/streamingservice.json).
* El único idioma compatible es el inglés (en-es).
* Con respecto a la protección del contenido, solo se admite el cifrado de sobre AES.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Media Services](media-services-overview.md)
