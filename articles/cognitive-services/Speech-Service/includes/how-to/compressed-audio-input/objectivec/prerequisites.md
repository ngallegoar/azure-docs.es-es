---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 7106e139108681e1908b20d2daac5e619a63555d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422252"
---
El control del audio comprimido se implementa mediante [GStreamer](https://gstreamer.freedesktop.org). Por motivos de licencia, los binarios de GStreamer no se compilan ni se vinculan con el SDK de Voz. En su lugar, es necesario compilar y enviar una biblioteca de contenedor que incluya estas funciones con las aplicaciones mediante el SDK.

Para compilar esta biblioteca de contenedor, primero descargue e instale el [SDK de GStreamer](https://gstreamer.freedesktop.org/data/pkg/ios/1.16.0/gstreamer-1.0-devel-1.16.0-ios-universal.pkg). Luego, descargue el proyecto de **Xcode** para la [biblioteca de contenedor](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/compressed-streams/GStreamerWrapper).

Abra el proyecto en **Xcode** y compílelo para el destino **Generic iOS Device** (Dispositivo iOS genérico); *no* sirve compilar para un destino específico.

El paso de compilación generará un conjunto dinámico de marcos con una biblioteca dinámica para todas las arquitecturas necesarias con el nombre de `GStreamerWrapper.framework`.

Esta plataforma debe estar incluida en todas las aplicaciones que usan secuencias de audio comprimidas con el SDK de voz.

Aplique la siguiente configuración en el proyecto de **Xcode** para lograr esto:

1. Copie el elemento `GStreamerWrapper.framework` que acaba de compilar y el marco del SDK de Voz de Cognitive Services, que puede descargar [aquí](https://aka.ms/csspeech/iosbinary), en el directorio que contiene el proyecto de ejemplo.
1. Ajuste las rutas de acceso para los marcos en la *Configuración del proyecto*.
   1. En la pestaña **General**, en el encabezado **Binarios incrustados**, agregue la biblioteca del SDK como un marco de trabajo: **Agregar binarios insertados** > **Agregar otros...** > vaya al directorio que eligió y seleccione ambos marcos.
   1. Vaya a la pestaña **Configuración de compilación** y active la configuración **Todos**.
1. Agregue el directorio `$(SRCROOT)/..` a _Rutas de búsqueda de marco_ bajo el encabezado **Rutas de búsqueda**.
