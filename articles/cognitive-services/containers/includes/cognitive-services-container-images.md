---
title: Repositorios e imágenes de contenedor
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Dos tablas que representan los registros de contenedor, los repositorios y los nombres de imagen de todas las ofertas de Cognitive Service.
ms.service: cognitive-services
ms.topic: include
ms.date: 03/10/2020
ms.author: dapine
ms.openlocfilehash: 55a3bb5f894d3ab753cfec64687abc9c7cae53cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082397"
---
### <a name="container-repositories-and-images"></a>Repositorios e imágenes de contenedor

Las tablas siguientes son una lista de las imágenes de contenedor disponibles que ofrece Azure Cognitive Services. Para obtener una lista completa de todos los nombres de imagen de contenedor y sus etiquetas disponibles, consulte [Etiquetas de imágenes de contenedor de Azure Cognitive Services](../container-image-tags.md). En la actualidad, no hay contenedores de Cognitive Services disponibles con carácter general (GA). Por el momento, hasta que se realicen más anuncios, los contenedores están disponibles como *No controlado público* o *Controlado público (versión preliminar)* .

 - *No controlado público*: los contenedores están disponibles de forma pública sin un mecanismo de acceso.
 - *Controlado público (versión preliminar)* : los contenedores están disponibles de forma pública, pero primero necesitan una solicitud formal para acceder al registro de contenedor.

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>"No validado" público (registro de contenedor: `mcr.microsoft.com`)

Microsoft Container Registry sindica todos los contenedores "no validados" con disponibilidad general para Cognitive Services. Los contenedores también están disponibles directamente desde [Docker Hub](https://hub.docker.com/_/microsoft-azure-cognitive-services).

| Servicio | Contenedor | Container Registry/Repositorio/Nombre de imagen |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Extracción de frases clave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Detección de idiomas | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Análisis de sentimiento | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Versión preliminar pública "validada" (registro de contenedor: `containerpreview.azurecr.io`)

La versión preliminar del registro de contenedor hospeda todos los contenedores "validados" con disponibilidad general para Cognitive Services. Estos contenedores necesitan una solicitud formal para acceder a ellos desde su registro de contenedor.

| Servicio | Contenedor | Container Registry/Repositorio/Nombre de imagen |
|--|--|--|
| [Anomaly Detector](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomaly Detector | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Computer Vision](../../Computer-vision/computer-vision-how-to-install-containers.md) | Lectura | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Face](../../face/face-how-to-install-containers.md) | Caras | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form Recognizer](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Recognizer | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=stt) | Voz a texto | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=cstt) | Conversión de voz a texto personalizada | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=tts) | Texto a voz | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=ctts) | Conversión de texto a voz personalizada | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
