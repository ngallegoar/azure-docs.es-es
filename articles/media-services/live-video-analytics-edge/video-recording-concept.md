---
title: 'Grabación de vídeo: Azure'
description: En el contexto de un sistema de administración de vídeo para cámaras CCTV, la grabación de vídeo se refiere al proceso de capturar vídeo a través de las cámaras y grabarlo para su visualización mediante aplicaciones móviles y de explorador. La grabación de vídeo se puede clasificar en grabación continua de vídeo y grabación de vídeo basada en eventos.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: c03472f577e67d5b2705a2ebe3f53e85e0849819
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84260198"
---
# <a name="video-recording"></a>Grabación de vídeo

En el contexto de un sistema de administración de vídeo para cámaras CCTV, la grabación de vídeo se refiere al proceso de capturar vídeo a través de las cámaras y grabarlo para su visualización mediante aplicaciones móviles y de explorador. La grabación de vídeo se puede clasificar en grabación continua de vídeo y grabación de vídeo basada en eventos. 

## <a name="continuous-video-recording"></a>Grabación de vídeo continua  

La grabación de vídeo continua (CVR) consiste en grabar de forma continua todo el vídeo que se captura a través de un origen de vídeo. La CVR garantiza que todo el vídeo esté disponible (según la [directiva de grabación](#recording-policy)) para su posterior análisis o auditoría.

## <a name="event-based-video-recording"></a>Grabación de vídeo basada en eventos  

La grabación de vídeo basada en eventos (EVR) hace referencia al proceso de grabación de vídeo desencadenado por un evento. El evento en cuestión podría generarse a partir del procesamiento de la propia señal de vídeo o desde un origen independiente (por ejemplo, el sensor de una puerta). La grabación de vídeo basada en eventos ayuda a ahorrar en el almacenamiento, pero requiere componentes adicionales que generen los eventos y desencadenen la grabación de vídeo (según una directiva predefinida). En otras palabras, la EVR requiere tomar decisiones adicionales sobre los eventos que deben desencadenar la grabación de vídeo y la directiva asociada a esta (también conocida como directiva de grabación). Un ejemplo de directiva podría ser grabar el vídeo durante dos minutos a partir de 30 segundos antes de la hora del evento. Los eventos en cuestión podrían originarse debido al procesamiento del vídeo en la propia cámara. Por ejemplo, varias cámaras permiten generar un evento de señal de detección del movimiento cuando se detecta movimiento en una zona de interés preconfigurada en el área de visualización de la cámara. Los eventos también se podrían generar al procesar el vídeo en otro dispositivo que captura el vídeo de la cámara y lo analiza mediante sencillas técnicas de procesamiento de imágenes o modelos sofisticados de Machine Learning. 

## <a name="choosing-recording-modes"></a>Selección del modo de grabación  

La opción de usar CVR o EVR depende de los objetivos empresariales. Live Video Analytics on IoT Edge proporciona funciones de plataforma tanto para CVR como para EVR. Puede obtener más información en los artículos de escenarios de [CVR](continuous-video-recording-concept.md) and [EVR](event-based-video-recording-concept.md).

## <a name="recording-policy"></a>Directiva de grabación  

La directiva de grabación hace referencia a las directivas que dictan el tiempo de inicio y detención de la grabación (en el caso de la EVR), la duración de la grabación y su eliminación. Las directivas de grabación permiten equilibrar el costo del almacenamiento con los requisitos empresariales. Las directivas de grabación difieren entre la CVR y la EVR. En el caso de la CVR, la directiva de grabación define durante cuántos días se debe almacenar el vídeo (por ejemplo, los últimos siete días). En cuanto a la EVR, la directiva de grabación define cuándo debe empezar y finalizar la grabación, así como la duración del vídeo. Puede obtener más información en los artículos de escenarios de [CVR](continuous-video-recording-concept.md) and [EVR](event-based-video-recording-concept.md).

## <a name="next-steps"></a>Pasos siguientes

* [Detección de movimiento y grabación de vídeo en Azure Media Services](detect-motion-record-video-clips-media-services-quickstart.md)
* [Detección de movimiento y grabación de vídeo en dispositivos perimetrales](detect-motion-record-video-clips-edge-devices-quickstart.md)

