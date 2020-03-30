---
title: Docker pull para el contenedor de detección de idioma
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de detección de idioma
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: 23669a5440799ff071b3a30e3e41ab5d49f212df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "70966786"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Docker pull para el contenedor de detección de idioma

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor [Detección de idioma](https://go.microsoft.com/fwlink/?linkid=2018759) en Docker Hub.

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
