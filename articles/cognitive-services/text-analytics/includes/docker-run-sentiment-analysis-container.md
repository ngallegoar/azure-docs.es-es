---
title: Ejecución del ejemplo de contenedor del comando de ejecución de Docker
titleSuffix: Azure Cognitive Services
description: Comando docker run para el contenedor de análisis de sentimiento
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: f0e587fc39fa2cc6f5275ae16834372a206b37d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "70966715"
---
Para ejecutar el contenedor *Análisis de sentimiento*, ejecute el siguiente comando `docker run`.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Este comando:

* Ejecuta un contenedor de *Análisis de sentimiento* desde la imagen de contenedor.
* Asigna un núcleo de CPU y 4 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host.