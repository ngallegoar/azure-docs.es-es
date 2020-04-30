---
title: Docker pull para el contenedor de detección de idioma
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de detección de idioma
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 3444d6267c09056697abf8339cc688a606b6dc17
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877102"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Docker pull para el contenedor de detección de idioma

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor [Detección de idioma](https://go.microsoft.com/fwlink/?linkid=2018759) en Docker Hub.

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
