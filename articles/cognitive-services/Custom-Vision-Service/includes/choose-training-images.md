---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: ce67062f83ad10703dfb1f0ee1f3741e698fb450
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70130116"
---
Como mínimo, se recomienda que use 30 imágenes por etiqueta en el conjunto de entrenamiento inicial. También conviene recopilar algunas imágenes adicionales para probar el modelo una vez que está entrenado.

Para entrenar el modelo de forma eficaz, use imágenes con variedad visual. Seleccione imágenes que varíen en:
* ángulos de cámara
* iluminación
* background
* estilo visual
* sujetos individuales o grupos
* tamaño
* type

Además, asegúrese de que todas las imágenes de entrenamiento cumplen los criterios siguientes:
* formato .jpg, .png, .bmp o .gif
* tienen menos de 6 MB de tamaño (4 MB en el caso de imágenes de predicción)
* tienen más de 256 píxeles en el borde más corto. Custom Vision Service escalará verticalmente y de forma automática todas las imágenes que sean más cortas
