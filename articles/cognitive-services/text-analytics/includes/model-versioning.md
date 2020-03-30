---
title: Control de versiones de los modelos
titleSuffix: Azure Cognitive Services
description: Especificar versiones del modelo en los puntos de conexión V3
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: 4cd8d0901ce23fb227bb6919dee18f5aa3d063ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77089047"
---
La versión 3 de Text Analytics API le permite elegir la versión del modelo más actual para los datos. Use el parámetro opcional `model-version` para seleccionar la versión del modelo que desee para sus solicitudes. Si no se especifica este parámetro, la API usará de forma predeterminada `latest`, la versión estable más reciente. Aunque puede usar la versión más reciente del modelo en cualquier solicitud, solo se actualizan algunas características en cada versión. En la tabla siguiente se describen las características que se han actualizado en cada versión del modelo:

| Versión del modelo           | Características actualizadas         | Versión más reciente para:           |
|-------------------------|--------------------------|--------------------------|
| `2020-02-01`            | Reconocimiento de entidades                      | Reconocimiento de entidades                      |
| `2019-10-01`            | Reconocimiento de entidades, análisis de opiniones  | Detección de idioma, extracción de frases clave y análisis de sentimiento|


Cada respuesta de los puntos de conexión V3 incluye un campo `model-version` que especifica la versión del modelo que se usó.

```json
{
    "documents": […]
    "errors": []
    "model-version": "2019-10-01"
}
```
Consulte [Novedades](../whats-new.md) para más información sobre las actualizaciones de estas versiones del modelo.
