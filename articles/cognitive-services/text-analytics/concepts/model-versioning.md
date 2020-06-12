---
title: Especificación de la versión de modelo en Text Analytics v3
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo especificar el modelo de Text Analytics API que se usa en los datos.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 04/21/2020
ms.author: aahi
ms.openlocfilehash: 9431ff862dd987a1a806087053014e7c880bf801
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2020
ms.locfileid: "84143261"
---
# <a name="model-versioning-in-the-text-analytics-api"></a>Control de versiones de modelos en Text Analytics API

La versión 3 de Text Analytics API le permite elegir la versión del modelo que se usará en los datos. Use el parámetro opcional `model-version` para seleccionar la versión del modelo en sus solicitudes de API. Por ejemplo: `<resource-url>/text/analytics/v3.0/sentiment?model-version=2020-04-01`. Si no se especifica este parámetro, la API usará de forma predeterminada la versión estable más reciente. 

## <a name="available-versions"></a>Versiones disponibles

Use la tabla siguiente para averiguar qué versiones del modelo son compatibles con cada punto de conexión.


| Punto de conexión                        | Versiones admitidas                       | Versión más reciente |
|---------------------------------|------------------------------------------|----------------|
| `/sentiment`                    | `2019-10-01`, `2020-04-01`               | `2020-04-01`   |
| `/languages`                    | `2019-10-01`                             | `2019-10-01`   |
| `/entities/linking`             | `2019-10-01`, `2020-02-01`               | `2020-02-01`   |
| `/entities/recognition/general` | `2019-10-01`, `2020-02-01`, `2020-04-01` | `2020-04-01`   |
| `/entities/recognition/pii`     | `2019-10-01`, `2020-02-01`, `2020-04-01` | `2020-04-01`   |
| `/keyphrases`                   | `2019-10-01`                             | `2019-10-01`   |


Encontrará más detalles sobre las actualizaciones de estos modelos en [Novedades](../whats-new.md).

## <a name="next-steps"></a>Pasos siguientes

* [Información general de Text Analytics](../overview.md)
* [Análisis de opiniones](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Reconocimiento de entidades](../how-tos/text-analytics-how-to-entity-linking.md)