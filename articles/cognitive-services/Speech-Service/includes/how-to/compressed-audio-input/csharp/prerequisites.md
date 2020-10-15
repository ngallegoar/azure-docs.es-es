---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 64eab53d24a14bb51633be3f979db20963677d93
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "81421863"
---
El control del audio comprimido se implementa mediante [GStreamer](https://gstreamer.freedesktop.org). Por motivos de licencia, los binarios de GStreamer no se compilan ni se vinculan con el SDK de Voz. Los desarrolladores deben instalar varias dependencias y complementos; vea [Instalación en Windows](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c). Los archivos binarios de Gstreamer deben estar en la ruta de acceso del sistema, con el fin de que el SDK de voz pueda cargarlos durante el tiempo de ejecución. Si el SDK de voz es capaz de encontrar libgstreamer-1.0-0.dll durante el tiempo de ejecución, significa que los archivos binarios de gstreamer se encuentran en la ruta de acceso del sistema.

