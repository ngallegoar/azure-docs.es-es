---
title: 'Moderación de vídeo con revisión humana: Content Moderator'
titleSuffix: Azure Cognitive Services
description: Uso de la moderación de vídeo asistida por máquina y la herramienta de revisión para moderar contenido inapropiado
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 0c031a890efc7fad7e5d9caefce3b0e66c515d90
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "81404251"
---
# <a name="video-moderation-with-human-review"></a>Moderación de vídeo con revisión humana

Utilice la [moderación de vídeo](video-moderation-api.md) asistida por máquina de Content Moderator y la [herramienta de revisión](Review-Tool-User-Guide/human-in-the-loop.md) para moderar vídeos y transcripciones de contenido para adultos (explícito) y subido de tono (sugerente) a fin de obtener los mejores resultados para su negocio.

## <a name="video-trained-classifier-preview"></a>Clasificador entrenado con vídeo (vista previa)

La clasificación de vídeo asistido automáticamente se logra con modelos entrenados de imagen y modelos entrenados de vídeo. A diferencia de los clasificadores de vídeo entrenados con imágenes, el clasificador de vídeos con contenido para adultos y explícito de Microsoft se entrena con vídeos. Este método da como resultado una mejor calidad de las coincidencias.

## <a name="shot-detection"></a>Detección de cortes

Cuando se generan los detalles de clasificación, la inteligencia adicional de vídeo contribuye con una mayor flexibilidad al análisis de los vídeos. En lugar de generar simplemente los fotogramas, el servicio de moderación de vídeo de Microsoft proporciona también información de los cortes. Ahora tiene la opción de analizar los vídeos en los niveles de corte y fotograma.

## <a name="key-frame-detection"></a>Detección de fotogramas principales

En lugar de generar fotogramas a intervalos regulares, el servicio de moderación de vídeo identifica y genera solo los fotogramas que es probable que estén completos (los buenos). La característica permite la generación eficaz de fotogramas para análisis de contenido para adultos y explícito en el nivel de fotograma.

El extracto siguiente muestra una respuesta parcial con cortes posibles, fotogramas principales y puntuaciones de contenido para adultos y explícito:

```json
"fragments":[  
  {  
    "start":0,
    "duration":18000
  },
  {  
    "start":18000,
    "duration":3600,
    "interval":3600,
    "events":[  
      [  
        {  
          "reviewRecommended":false,
          "adultScore":0.00001,
          "racyScore":0.03077,
          "index":5,
          "timestamp":18000,
          "shotIndex":0
        }
      ]
    ]
  },
  {  
    "start":18386372,
    "duration":119149,
    "interval":119149,
    "events":[  
      [  
        {  
          "reviewRecommended":true,
          "adultScore":0.00000,
          "racyScore":0.91902,
          "index":5085,
          "timestamp":18386372,
          "shotIndex":62
        }
      ]
    ]
```

## <a name="visualization-for-human-reviews"></a>Visualización para revisiones humanas

En casos más matizados, las empresas necesitan una solución de revisión humana para presentar el vídeo, sus fotogramas y las etiquetas asignadas automáticamente. Los moderadores humanos que revisan los vídeos y fotogramas obtienen una perspectiva completa de la información, cambian las etiquetas y envían sus decisiones.

![vista predeterminada de herramienta de revisión de vídeo](images/video-review-default-view.png)

## <a name="player-view-for-video-level-review"></a>Vista del reproductor para revisión de nivel de vídeo

Las decisiones binarias de nivel de vídeo son posibles gracias a una vista de reproductor de vídeo que muestra los fotogramas de contenido para adultos y explícito. Los revisores humanos se desplazan por el vídeo con varias opciones de velocidad para examinar las escenas. Para confirmar sus decisiones alternan las etiquetas.

![vista de reproductor de herramienta de revisión de vídeo](images/video-review-player-view.PNG)

## <a name="frames-view-for-detailed-reviews"></a>Vista de fotogramas para revisiones detalladas

Una revisión de vídeo detallada para un análisis fotograma a fotograma es posible con una vista basada en fotogramas. Los revisores humanos revisan y seleccionan uno o varios fotogramas y alternan etiquetas para confirmar sus decisiones. Un siguiente paso opcional es la redacción de fotogramas o contenido ofensivos.

![vista de fotogramas de herramienta de revisión de vídeo](images/video-review-frames-view-apply-tags.PNG)

## <a name="transcript-moderation"></a>Moderación de transcripciones

Los vídeos tienen normalmente voz de narración o habla ofensiva que necesitan moderación. Use el servicio Azure Media Indexer para convertir voz a texto y la API de revisión de Content Moderator para enviar la transcripción para la moderación de texto dentro de la herramienta de revisión.

![vista de transcripción de herramienta de revisión de vídeo](images/video-review-transcript-view.png)

## <a name="next-steps"></a>Pasos siguientes

- Empiece con el [inicio rápido de moderación de vídeo](video-moderation-api.md).
- Aprenda a generar [revisiones de vídeo](video-reviews-quickstart-dotnet.md) para los revisores humanos desde su salida moderada.
- Agregue [revisiones de transcripción de vídeo](video-transcript-reviews-quickstart-dotnet.md) a las revisiones de vídeo.
- Consulte el tutorial detallado sobre cómo desarrollar una [solución completa de moderación de vídeo](video-transcript-moderation-review-tutorial-dotnet.md).