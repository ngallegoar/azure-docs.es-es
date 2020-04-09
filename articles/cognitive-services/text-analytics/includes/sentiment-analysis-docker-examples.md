---
title: Ejemplos de Docker de contenedor de análisis de sentimiento
titleSuffix: Azure Cognitive Services
description: Ejemplos de Docker de contenedor de análisis de sentimiento
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: e1d80f2ee869b2d71a536e499f900c7e8d6e425d
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878696"
---
### <a name="sentiment-analysis-container-docker-examples"></a>Ejemplos de Docker de contenedor de análisis de sentimiento

Los siguientes ejemplos de Docker son para el contenedor de análisis de sentimiento.

#### <a name="basic-example"></a>Ejemplo básico 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} 
  ```

#### <a name="logging-example"></a>Ejemplo de registro 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
  ```
