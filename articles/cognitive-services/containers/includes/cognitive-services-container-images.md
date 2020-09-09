---
title: Repositorios e imágenes de contenedor
services: cognitive-services
author: aahill
manager: nitinme
description: Dos tablas que representan los registros de contenedor, los repositorios y los nombres de imagen de todas las ofertas de Cognitive Service.
ms.service: cognitive-services
ms.topic: include
ms.date: 04/24/2020
ms.author: aahi
ms.openlocfilehash: 911e9efe6a6b404b0ba543c585d10f96b289672a
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89321450"
---
### <a name="container-repositories-and-images"></a>Repositorios e imágenes de contenedor

Las tablas siguientes son una lista de las imágenes de contenedor disponibles que ofrece Azure Cognitive Services. Para obtener una lista completa de todos los nombres de imagen de contenedor y sus etiquetas disponibles, consulte [Etiquetas de imágenes de contenedor de Azure Cognitive Services](../container-image-tags.md). 

#### <a name="generally-available"></a>Disponibilidad general 

Microsoft Container Registry (MCR) sindica todos los contenedores con disponibilidad general para Cognitive Services. Los contenedores también están disponibles directamente desde [Docker Hub](https://hub.docker.com/_/microsoft-azure-cognitive-services).

#### <a name="luis"></a>[LUIS](#tab/luis)

| Contenedor de LUIS | Container Registry/Repositorio/Nombre de imagen |
|--|--|
| LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |

Consulte [Cómo ejecutar e instalar contenedores de LUIS](../../LUIS/luis-container-howto.md) para obtener más información.

#### <a name="text-analytics"></a>[Text Analytics](#tab/text-analytics)

| Contenedor de Text Analytics | Container Registry/Repositorio/Nombre de imagen |
|--|--|
| Análisis de sentimiento, versión 3 (inglés) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-en` |
| Análisis de sentimiento, versión 3 (español) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-es` |
| Análisis de sentimiento, versión 3 (francés) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-fr` |
| Análisis de sentimiento, versión 3 (italiano) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-it` |
| Análisis de sentimiento, versión 3 (alemán) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-de` |
| Análisis de sentimiento, versión 3 (chino simplificado) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-zh` |
| Análisis de sentimiento, versión 3 (chino tradicional) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-zht` |
| Análisis de sentimiento, versión 3 (japonés) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-ja` |
| Análisis de sentimiento, versión 3 (portugués) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-pt` |
| Análisis de sentimiento, versión 3 (holandés) | `mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-nl` |

Consulte [Cómo ejecutar e instalar contenedores de Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) para obtener más información.

---

#### <a name="public-ungated-preview-container-registry-mcrmicrosoftcom"></a>Versión preliminar pública "no validada" (registro de contenedor: `mcr.microsoft.com`)

Los siguientes contenedores de versión preliminar están disponibles públicamente. Microsoft Container Registry sindica todos los contenedores no validados con disponibilidad general para Cognitive Services. Los contenedores también están disponibles directamente desde [Docker Hub](https://hub.docker.com/_/microsoft-azure-cognitive-services).

| Servicio | Contenedor | Container Registry/Repositorio/Nombre de imagen |
|--|--|--|
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Extracción de frases clave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Detección de idiomas | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Anomaly Detector](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomaly Detector | `mcr.microsoft.com/azure-cognitive-services/anomaly-detector` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Versión preliminar pública "validada" (registro de contenedor: `containerpreview.azurecr.io`)

Los siguientes contenedores de la versión preliminar validada se hospedan en el registro de versiones preliminares de contenedores y requieren una aplicación para acceder a ellos. Consulte los siguientes artículos de contenedor para obtener más información.

| Servicio | Contenedor | Container Registry/Repositorio/Nombre de imagen |
|--|--|--|
| [Computer Vision](../../Computer-vision/computer-vision-how-to-install-containers.md) | Lectura | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Face](../../face/face-how-to-install-containers.md) | Caras | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form Recognizer](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Recognizer | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=stt) | Voz a texto | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=cstt) | Conversión de voz a texto personalizada | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=tts) | Texto a voz | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=ctts) | Conversión de texto a voz personalizada | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=ntts) | Texto a voz neuronal | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
| [Text Analytics for Health](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=health) | Text Analytics for Health | `containerpreview.azurecr.io/microsoft/cognitive-services-healthcare` |
