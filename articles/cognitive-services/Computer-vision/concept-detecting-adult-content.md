---
title: 'Contenido para adultos y subido de tono: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la detección de contenido para adultos en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5cc8a4508ceeda245fbc10a81e16f3ecf05284c7
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95013619"
---
# <a name="detect-adult-content"></a>Detección de contenido para adultos

Computer Vision puede detectar material para adultos en imágenes para que los programadores puedan restringir la presentación de dichas imágenes en su software. Las marcas de contenido se aplican con una puntuación entre cero y uno para que los desarrolladores puedan interpretar los resultados según sus preferencias.

> [!NOTE]
> Una gran parte de esta funcionalidad está disponible en el servicio [Azure Content Moderator](../content-moderator/overview.md). Consulte esta alternativa para las soluciones en escenarios de moderación de contenido más rigurosas, como la moderación de texto y los flujos de trabajo de revisión humana.

## <a name="content-flag-definitions"></a>Definiciones de las marcas de contenido

Dentro de la clasificación "adulto" hay varias categorías diferentes:

- Las imágenes **para adultos** se definen como aquellas de naturaleza explícitamente sexual que suelen representar desnudos y actos sexuales.
- Las imágenes **subidas de tono** se definen como las imágenes de naturaleza sexualmente sugerente y que a menudo contienen material sexual menos explícito que las imágenes etiquetadas como **para adultos**.
- Las imágenes **gore** son las que representan experiencias de ese tipo.

## <a name="use-the-api"></a>Uso de la API

Puede detectar contenido para adultos con la API [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b). Al agregar el valor de `Adult` al parámetro de consulta **visualFeatures**, la API devuelve tres propiedades booleanas &mdash;`isAdultContent`, `isRacyContent` y `isGoryContent`&mdash; en su respuesta JSON. El método también devuelve las propiedades correspondientes&mdash;`adultScore`, `racyScore` y `goreScore`&mdash; que representan las puntuaciones de confianza entre cero y uno para cada categoría correspondiente.

- [Inicio rápido: Análisis de imágenes (SDK .NET)](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
- [Inicio rápido: Análisis de imágenes (API REST)](./quickstarts/csharp-analyze.md)