---
title: 'Miniaturas de recorte inteligente: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la generación de miniaturas de imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/11/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 4874910f37b49990a659b48af0cf27921c3fcd5e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "68945226"
---
# <a name="generating-smart-cropped-thumbnails-with-computer-vision"></a>Generación de miniaturas de recorte inteligente con Computer Vision

Una miniatura es una representación reducida de una imagen. Las miniaturas se utilizan para representar imágenes y otros datos de un modo más económico y más compatible con el diseño. Computer Vision API utiliza la funcionalidad de recorte inteligente junto con el cambio de tamaño para crear miniaturas intuitivas de una determinada imagen.

El algoritmo de generación de miniaturas de Computer Vision funciona del modo siguiente:

1. Quita los elementos prescindibles de la imagen e identifica el _área de interés_&mdash;; es decir, el área de la imagen en la que aparecen los objetos principales.
1. Recorta la imagen en función del _área de interés_ identificada.
1. Cambia la relación de aspecto para adaptarla a las dimensiones de la miniatura de destino.

## <a name="area-of-interest"></a>Área de interés

Cuando se carga una imagen, Computer Vision API la analiza para determinar el *área de interés*. También puede usar esta región para determinar cómo debe recortarse la imagen. Sin embargo, la operación de recorte siempre se adaptará a la relación de aspecto deseada, si se especifica una.

También puede obtener las coordenadas del rectángulo de selección de esta misma *área de interés* llamando a **areaOfInterest** API en su lugar. Puede utilizar esta información para modificar la imagen original como desee.

## <a name="examples"></a>Ejemplos

La miniatura generada puede variar enormemente en función de la altura, anchura y la función de recorte inteligente que se especifiquen, tal como se muestra en la siguiente imagen.

![Una imagen de una montaña junto a diversas configuraciones de recorte](./Images/thumbnail-demo.png)

En la tabla siguiente se muestran las típicas miniaturas que se pueden generar con Computer Vision, teniendo en cuenta las imágenes de ejemplo. Las miniaturas se crearon para un destino con una altura y anchura específicas de 50 píxeles, y tienen la función de recorte inteligente habilitada.

| Imagen | Miniatura |
|-------|-----------|
|![Montaña al aire libre al atardecer, con la silueta de una persona](./Images/mountain_vista.png) | ![Miniatura de una montaña al aire libre al atardecer, con la silueta de una persona](./Images/mountain_vista_thumbnail.png) |
|![Una flor blanca con un fondo verde](./Images/flower.png) | ![Miniatura Vision Analyze Flower](./Images/flower_thumbnail.png) |
|![Una mujer en el tejado de un edificio](./Images/woman_roof.png) | ![miniatura de una mujer en el tejado de un edificio de apartamentos](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Pasos siguientes

Mas información sobre el [etiquetado de imágenes](concept-tagging-images.md) y la [categorización de imágenes](concept-categorizing-images.md).
