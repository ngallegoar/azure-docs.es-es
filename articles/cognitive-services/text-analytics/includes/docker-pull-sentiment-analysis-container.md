---
title: Docker pull para el contenedor de análisis de sentimiento
titleSuffix: Azure Cognitive Services
description: Comando pull de Docker para el contenedor de análisis de sentimiento
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: a3db0e2ffdd4a75f02634ca2227c3c41416d4f65
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588423"
---
#### <a name="docker-pull-for-the-sentiment-analysis-v3-container"></a>Docker pull para el contenedor de Análisis de sentimiento v3

El contenedor de Análisis de sentimiento v3 está disponible en varios idiomas. Para descargar el contenedor en inglés, use el siguiente comando. 

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-en
```

Para descargar el contenedor para otro idioma, reemplace `en` por uno de los siguientes códigos de idioma. 

| Contenedor de Text Analytics | Código de lenguaje |
|--|--|
| Inglés | `en` |
| Español | `es` |
| Francés | `fr` |
| Italiano | `it` |
| Alemán | `de` |
| Chino (simplificado) | `zh` |
| Chino (tradicional) | `zht` |
| Japonés | `ja` |
| Portugués | `pt` |
| Neerlandés | `nl` |

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte [Docker Hub](https://go.microsoft.com/fwlink/?linkid=2018654).