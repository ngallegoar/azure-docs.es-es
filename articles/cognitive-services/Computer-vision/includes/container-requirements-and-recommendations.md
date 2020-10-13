---
title: Recomendaciones y requisitos del contenedor
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 3ab5462cc2b368bd1518606b03c8ec1027bc31c3
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2020
ms.locfileid: "91397202"
---
> [!NOTE]
> Los requisitos y las recomendaciones se basan en pruebas comparativas con una única solicitud por segundo, con una imagen de 8 MB de una carta comercial digitalizada que contiene 29 líneas y un total de 803 caracteres.

En la tabla siguiente se describe la asignación mínima y recomendada de recursos para cada contenedor de lectura.

| Contenedor | Mínima | Recomendado |
|-----------|---------|-------------|
| Versión preliminar de Read 2.0 | 1 núcleo, 8 GB de memoria |  8 núcleos, 16 GB de memoria |
| Read 3.0: versión preliminar | 8 núcleos, 16 GB de memoria | 8 núcleos, 24 GB de memoria |
| Read 3.1: versión preliminar | 8 núcleos, 16 GB de memoria | 8 núcleos, 24 GB de memoria |

* Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.
