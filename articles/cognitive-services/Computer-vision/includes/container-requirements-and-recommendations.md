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
ms.openlocfilehash: c62044582cb488a5ef2d20b3f407c0865b3994ba
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877988"
---
> [!NOTE]
> Los requisitos y las recomendaciones se basan en pruebas comparativas con una única solicitud por segundo, con una imagen de 8 MB de una carta comercial digitalizada que contiene 29 líneas y un total de 803 caracteres.

En la tabla siguiente se describe la asignación mínima y recomendada de recursos para cada contenedor de lectura.

| Contenedor | Mínima | Recomendado |TPS<br>(mínimo, máximo)|
|-----------|---------|-------------|--|
| Lectura | 1 núcleo, 8 GB de memoria, 0,24 TPS | 8 núcleos, 16 GB de memoria, 1,17 TPS | 0,24, 1,17 |

* Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.
* TPS: transacciones por segundo

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.
