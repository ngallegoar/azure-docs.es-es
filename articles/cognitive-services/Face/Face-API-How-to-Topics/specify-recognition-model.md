---
title: 'Especificación de un modelo de reconocimiento: Face'
titleSuffix: Azure Cognitive Services
description: En este artículo se mostrará cómo elegir qué modelo de reconocimiento usar con su aplicación Face de Azure.
services: cognitive-services
author: longli0
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: longl
ms.openlocfilehash: da9ad5576d146c007e45124668875e9681860ce6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76938825"
---
# <a name="specify-a-face-recognition-model"></a>Especificación de un modelo de reconocimiento facial

En esta guía se muestra cómo especificar un modelo de reconocimiento de caras para la detección de caras, la identificación y la búsqueda de similitud con el servicio Face de Azure.

El servicio Face emplea modelos de aprendizaje automático para realizar operaciones en caras humanas de imágenes. Seguimos mejorando la precisión de nuestros modelos gracias a los comentarios de los clientes y los avances en la investigación, y estas mejoras se ofrecen como actualizaciones del modelo. Los desarrolladores tienen la opción de especificar qué versión del modelo de reconocimiento de caras le gustaría usar; pueden elegir el modelo que mejor se adapte a sus casos de uso.

Si es un nuevo usuario, se recomienda usar el modelo más reciente. Siga leyendo para saber cómo especificarlo en diferentes operaciones de caras y evitar al mismo tiempo conflictos entre modelos. Si es un usuario avanzado y no está seguro de si debe cambiar al modelo más reciente, vaya a la sección [Evaluación de modelos diferentes](#evaluate-different-models) para evaluar el nuevo modelo y comparar los resultados con el conjunto de datos actual.

## <a name="prerequisites"></a>Prerequisites

Debe estar familiarizado con los conceptos de identificación y detección de caras de AI. Si no es así, consulte primero estas guías paso a paso:

* [Detección de caras en una imagen](HowtoDetectFacesinImage.md)
* [Identificación de caras en imágenes](HowtoIdentifyFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Detección de caras con el modelo especificado

La detección de caras identifica los puntos de referencia visuales de caras humanas y encuentra las ubicaciones de su cuadro delimitador. También extrae las características de la cara y las almacena para su uso en la identificación. Toda esta información constituye la representación de una cara.

El modelo de reconocimiento se usa cuando se extraen características faciales, por lo que puede especificar una versión de modelo al realizar la operación de detección.

Al usar la API [Face - Detect], asigne la versión del modelo con el parámetro `recognitionModel`. Los valores disponibles son:

* `recognition_01`
* `recognition_02`

Opcionalmente, puede especificar el parámetro _returnRecognitionModel_ (valor predeterminado **false**) para indicar si _recognitionModel_ se debe devolver en la respuesta. Por lo tanto, una dirección URL de solicitud para la API REST [Face - Detect] tendrá este aspecto:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel]&subscription-key=<Subscription key>`

Si usa la biblioteca cliente, puede asignar el valor de `recognitionModel` si pasa una cadena que represente la versión.
Si la deja sin asignar, se usará la versión predeterminada del modelo (_recognition_01_). Consulte el siguiente ejemplo de código de la biblioteca cliente .NET.

```csharp
string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, recognitionModel: "recognition_02", returnRecognitionModel: true);
```

## <a name="identify-faces-with-specified-model"></a>Identificación de caras con el modelo especificado

El servicio Face puede extraer datos faciales de una imagen y asociarlos con un objeto **Person** (mediante la llamada API [Add face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b), por ejemplo) y varios objetos **Person** se pueden almacenar juntos en un objeto **PersonGroup**. A continuación, se puede comparar una nueva cara con un objeto **PersonGroup** (con la llamada [Face - Identify]), y se puede identificar la persona coincidente dentro de ese grupo.

Un objeto **PersonGroup** debe tener un único modelo de reconocimiento para todos los objetos **Person**, y para especificarlo, debe usar el parámetro `recognitionModel` al crear el grupo ([PersonGroup - Create] o [LargePersonGroup - Create]). Si no especifica este parámetro, se usa el modelo original, `recognition_01`. Un grupo siempre usará el modelo de reconocimiento con el que se creó, y las nuevas caras se asociarán con este modelo cuando se agreguen a él; esta acción no se puede cambiar después de la creación de un grupo. Para ver el modelo con el que está configurado un objeto **PersonGroup**, use la API [PersonGroup - Get] con el parámetro _returnRecognitionModel_ establecido en **true**.

Consulte el siguiente ejemplo de código de la biblioteca cliente .NET.

```csharp
// Create an empty PersonGroup with "recognition_02" model
string personGroupId = "mypersongroupid";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");
```

En este código, se crea un objeto **PersonGroup** con el identificador `mypersongroupid`, y se configura para usar el modelo _recognition_02_ para extraer las características faciales.

En consecuencia, deberá especificar qué modelo usar al detectar caras para comparar con este objeto **PersonGroup** (mediante la API [Face - Detect]). El modelo que use siempre debe ser coherente con la configuración de **PersonGroup**, o la operación generará un error debido a la incompatibilidad de los modelos.

No hay cambios en la API [Face - Identify], solo debe especificar la versión del modelo en la detección.

## <a name="find-similar-faces-with-specified-model"></a>Búsqueda de caras parecidas con el modelo especificado

También puede especificar un modelo de reconocimiento para la búsqueda de similitudes. Puede asignar la versión del modelo con `recognitionModel` al crear la lista de caras con la API [FaceList - Create] o [LargeFaceList - Create]. Si no especifica este parámetro, se usa el modelo original, `recognition_01`. Una lista de caras siempre usará el modelo de reconocimiento con el que se creó, y las nuevas caras se asociarán con este modelo cuando se agreguen a él; esta acción no se puede cambiar después de la creación. Para ver el modelo con el que está configurada una lista de caras, use la API [FaceList - Get] con el parámetro _returnRecognitionModel_ establecido en **true**.

Consulte el siguiente ejemplo de código de la biblioteca cliente .NET.

```csharp
await faceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");
```

Este código crea una lista de caras denominada `My face collection`, con el modelo _recognition_02_ para la extracción de características. Al buscar en esta lista de caras rostros parecidos a una nueva cara detectada, la cara se debe haber detectado con ([Face - Detect]) mediante el modelo _recognition_02_. Al igual que en la sección anterior, el modelo debe ser coherente.

No hay cambios en la API [Face - Find Similar]; solo se especifica la versión del modelo en la detección.

## <a name="verify-faces-with-specified-model"></a>Comprobación de las caras con el modelo especificado

La API [Face - Verify] comprueba si dos caras pertenecen a la misma persona. Aunque no hay ningún cambio en la API de comprobación con respecto a los modelos de reconocimiento, solo se pueden comparar caras que se hayan detectado con el mismo modelo. Por lo tanto, ambas caras se deben haber detectado con `recognition_01` o `recognition_02`.

## <a name="evaluate-different-models"></a>Evaluación de modelos diferentes

Si quiere comparar el rendimiento de los modelos _recognition_01_ y _recognition_02_, deberá hacer lo siguiente:

1. Crear dos objetos **PersonGroup** con _recognition_01_ y _recognition_02_, respectivamente.
1. Usar los datos de imagen para detectar caras y registrarlas en el objeto **Persona** de estos dos objetos **PersonGroup**, y desencadenar el proceso de entrenamiento con la API [PersonGroup - Train].
1. Probar con [Face - Identify] en ambos objetos **PersonGroup** y comparar los resultados.

Si normalmente especifica un umbral de confianza (un valor entre cero y uno que determina lo confiable que debe ser el modelo para identificar una cara), puede que deba usar diferentes umbrales para diferentes modelos. El umbral de un modelo no tiene porqué compartirse con otro y no produce necesariamente los mismos resultados.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a especificar el modelo de reconocimiento que se debe usar con diferentes API del servicio Face. A continuación, siga un inicio rápido para empezar a usar la detección de caras.

* [SDK de .NET para Face](../Quickstarts/csharp-sdk.md)
* [SDK de Python para Face](../Quickstarts/python-sdk.md)
* [SDK de Go para Face](../Quickstarts/go-sdk.md)

[Face - Detect]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Face - Find Similar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Face - Identify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Face - Verify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Train]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[LargeFaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc
