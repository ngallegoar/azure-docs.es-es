---
title: Conceptos del reconocimiento facial
titleSuffix: Azure Cognitive Services
description: En este artículo se explican los conceptos de las operaciones de reconocimiento facial Verificar, Buscar similares, Grupo e Identificar, y las estructuras de datos subyacentes.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pafarley
ms.openlocfilehash: 3957a9cde957c8e92806f10d39c949d73f20153e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91323029"
---
# <a name="face-recognition-concepts"></a>Conceptos del reconocimiento facial

En este artículo se explican los conceptos de las operaciones de reconocimiento facial Verificar, Buscar similares, Grupo e Identificar, y las estructuras de datos subyacentes. En términos generales, el reconocimiento describe el trabajo de comparar dos caras diferentes para determinar si son similares o pertenecen a la misma persona.

## <a name="recognition-related-data-structures"></a>Estructuras de datos relacionados con el reconocimiento

Las operaciones de reconocimiento usan principalmente las siguientes estructuras de datos. Estos objetos se almacenan en la nube y se puede hacer referencia a ellos por sus cadenas de identificador. Las cadenas de identificador siempre son únicas dentro de una suscripción. Se pueden duplicar los campos de nombre.

|Nombre|Descripción|
|:--|:--|
|DetectedFace| Esta representación facial única se recupera mediante la operación de [detección de caras](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md). Su identificador expira 24 horas después de su creación.|
|PersistedFace| Cuando se agregan objetos DetectedFace a un grupo, como FaceList o Person, se convierten en objetos PersistedFace. Se pueden [recuperar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) en cualquier momento y no caducan.|
|[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b) o [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)| Esta estructura de datos es una lista variada de objetos PersistedFace. Un objeto FaceList tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario.|
|[Person](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)| Esta estructura de datos es una lista de objetos PersistedFace que pertenecen a la misma persona. Tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario.|
|[PersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) o [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)| Esta estructura de datos es una lista variada de objetos Person. Tiene un identificador único, una cadena de nombre y, opcionalmente, una cadena de datos de usuario. Se debe [entrenar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) un objeto PersonGroup antes de que se pueda utilizar en operaciones de reconocimiento.|

## <a name="recognition-operations"></a>Operaciones de reconocimiento

En esta sección se detalla el modo en que las cuatro operaciones de reconocimiento utilizan las estructuras de datos que se han descrito anteriormente. Para una amplia descripción de cada operación de reconocimiento, consulte [Introducción](../Overview.md).

### <a name="verify"></a>Verify

La operación [Verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) toma un identificador de cara de DetectedFace o PersistedFace y otro identificador de cara o un objeto Person, y determina si pertenecen a la misma persona. Si pasa un objeto Person, puede pasar, opcionalmente, un objeto PersonGroup al que pertenece ese objeto Person para mejorar el rendimiento.

### <a name="find-similar"></a>Buscar similares

La operación [Buscar similares](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) toma un identificador de cada objeto DetectedFace o PersistedFace, y un objeto FaceList o una matriz de otros identificadores de caras. Con un objeto FaceList, devuelve un objeto FaceList más pequeño de caras que son similares a la cara determinada. Con una matriz de identificadores de caras, devuelve de forma similar una matriz más pequeña.

### <a name="group"></a>Grupo

La operación [Grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) toma una matriz de identificadores de caras variados de DetectedFace o PersistedFace y devuelve los mismos identificadores agrupados en varias matrices más pequeñas. Cada matriz de "grupos" contiene identificadores de caras que parecen similares. Una matriz única "messyGroup" contiene identificadores de caras para las que no se han encontrado similitudes.

### <a name="identify"></a>Identificar

La operación [Identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) toma uno o varios identificadores de caras de DetectedFace o PersistedFace y un objeto PersonGroup, y devuelve una lista de objetos Person a los que puede pertenecer cada cara. Los objetos Person devueltos se encapsulan como objetos Candidate, que tienen un valor de confianza de predicción.

## <a name="input-data"></a>Datos de entrada

Utilice las siguientes sugerencias para asegurarse de que las imágenes de entrada proporcionan los resultados de reconocimiento más precisos:

* Los formatos de imagen de entrada admitidos son JPEG, PNG, GIF(el primer fotograma) y BMP.
* El tamaño del archivo de imagen no debe ser superior a 4 MB.
* Cuando crea objetos Person, use las fotos que presentan distintos tipos de ángulos e iluminaciones.
* Es posible que no se puedan reconocer algunas caras debido a desafíos técnicos como, por ejemplo:
  * Imágenes con iluminación extrema, por ejemplo, fuerte contraluz.
  * Obstáculos que bloquean uno o los dos ojos.
  * Diferencias en el tipo de pelo o vello facial.
  * Cambios en la apariencia facial debido a la edad.
  * Expresiones faciales extremas.

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con los conceptos de reconocimiento facial, escriba un script que identifique las caras con un objeto PersonGroup entrenado.

* [Inicio rápido de la biblioteca cliente de Face](../Quickstarts/client-libraries.md)