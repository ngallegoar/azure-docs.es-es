---
title: Docker pull para el contenedor de análisis de sentimiento
titleSuffix: Azure Cognitive Services
description: Comando pull de Docker para el contenedor de análisis de sentimiento
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: df00d469052fa30c3f2aaa5afe1881ef74587f9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "70966665"
---
#### <a name="docker-pull-for-the-sentiment-analysis-container"></a>Docker pull para el contenedor de análisis de sentimiento

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor de [análisis de sentimiento](https://go.microsoft.com/fwlink/?linkid=2018654) en Docker Hub.

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```
