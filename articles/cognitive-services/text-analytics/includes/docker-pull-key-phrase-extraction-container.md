---
title: Docker pull para el contenedor de extracción de frases clave
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de extracción de frases clave
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: db5d0cfecde32d3dd6afe181d67a6ce6c0826eda
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877122"
---
#### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker pull para el contenedor de extracción de frases clave

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor [Extracción de frases clave](https://go.microsoft.com/fwlink/?linkid=2018757) en Docker Hub.

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```
