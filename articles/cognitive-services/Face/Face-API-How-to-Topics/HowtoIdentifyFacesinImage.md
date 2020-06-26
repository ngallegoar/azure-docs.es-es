---
title: 'Ejemplo: Identificación de caras en imágenes: Face'
titleSuffix: Azure Cognitive Services
description: En esta guía se muestra cómo identificar caras desconocidas con los objetos PersonGroup, que se crean a partir de personas conocidas de antemano.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 04/10/2019
ms.author: sbowles
ms.openlocfilehash: 248bae81db1bc8cb69bac4618bd7593658336636
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2020
ms.locfileid: "84986703"
---
# <a name="example-identify-faces-in-images"></a>Ejemplo: Identificación de caras en imágenes

En esta guía se muestra cómo identificar caras desconocidas con los objetos PersonGroup, que se crean a partir de personas conocidas de antemano. Los ejemplos se escriben en C# mediante el uso de la biblioteca cliente de Face de Azure Cognitive Services.

## <a name="preparation"></a>Preparación

En este ejemplo se muestra:

- Cómo crear un elemento PersonGroup. Este elemento PersonGroup contiene una lista de personas conocidas.
- Cómo asignar caras a cada persona. Estas caras se usan como base de referencia para identificar personas. Le recomendamos usar vistas frontales claras de caras. Un ejemplo es un identificador de fotografía. Un buen conjunto de fotos incluye caras de la misma persona en distintas posturas, colores de ropa o peinados.

Para demostrar este ejemplo, prepare:

- Unas fotos con la cara de la persona. [Descargue fotos de ejemplo](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) para Anna, Bill y Clare.
- Una serie de fotos de prueba. Las fotos podrían contener o no las caras de Anna, Bill o Clare. Se usan para probar la identificación. Seleccione también algunas imágenes de ejemplo del vínculo anterior.

## <a name="step-1-authorize-the-api-call"></a>Paso 1: Autorización de la llamada a la API

Cada llamada a Face API requiere una clave de suscripción. Esta clave puede pasarse a través de un parámetro de cadena de consulta o especificarse en la cabecera de la solicitud. Para pasar la clave de suscripción a través de una cadena de consulta, consulte la dirección URL de la solicitud para [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como un ejemplo:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Como alternativa, especifique la clave de suscripción en el encabezado de la solicitud HTTP **ocp-apim-subscription-key: &lt;Clave de suscripción&gt;** .
Cuando usa una biblioteca cliente, la clave de suscripción se pasa a través del constructor de la clase FaceClient. Por ejemplo:
 
```csharp 
private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials("<subscription key>"),
            new System.Net.Http.DelegatingHandler[] { });
```
 
Siga estas instrucciones para obtener una clave.

1. Cree una [cuenta de Azure](https://azure.microsoft.com/free/cognitive-services/). Si ya tiene una, puede pasar a la siguiente sección.
2. Cree un [recurso de Face ](https://portal.azure.com/#create/Microsoft.CognitiveServicesFace) en Azure Portal para obtener la clave. Asegúrese de seleccionar el nivel gratis (F0) durante la instalación. 
3. Una vez implementados los recursos, haga clic en **Ir al recurso** para recopilar la clave. 

## <a name="step-2-create-the-persongroup"></a>Paso 2: Creación del elemento PersonGroup

En este paso, un PersonGroup denominado "MyFriends" contiene Anna, Bill y Clare. Cada persona tiene varias caras registrados. Las caras se deben detectar en las imágenes. Después de todos estos pasos, tiene un elemento PersonGroup similar al de la siguiente imagen:

![MyFriends](../Images/group.image.1.jpg)

### <a name="step-21-define-people-for-the-persongroup"></a>Paso 2.1: Definir personas para el elemento PersonGroup
Una persona es una unidad básica de identificación. Una persona puede tener una o más caras conocidas registradas. Un elemento PersonGroup es una colección de personas. Cada persona se define dentro de un elemento PersonGroup determinado. La identificación se hace en un elemento PersonGroup. La tarea consiste en crear un elemento PersonGroup y, después, crear personas en él, como Anna, Bill y Clare.

En primer lugar, cree un elemento PersonGroup mediante [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API. La API de la biblioteca cliente correspondiente es el método CreatePersonGroupAsync para la clase FaceClient. El identificador de grupo que está especificado para crear el grupo es único para cada suscripción. También puede obtener, actualizar o eliminar elementos PersonGroup mediante otras API PersonGroup. 

Una vez que se define un grupo, puede definir las personas dentro de él mediante [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API. El método de la biblioteca cliente es CreatePersonAsync. Puede agregar una cara a cada persona una vez creada.

```csharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceClient.PersonGroupPerson.CreateAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step-22-detect-faces-and-register-them-to-the-correct-person"></a><a name="step2-2"></a> Paso 2.2: Detectar caras y registrarlas en la persona correcta
La detección se realiza enviando una petición web "POST" a [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API con el archivo de imagen en el cuerpo de la solicitud HTTP. Cuando se usa la biblioteca cliente, la detección de caras se realiza mediante uno de los métodos DetectAsync de la clase FaceClient.

Para cada cara detectada llame a [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) para agregarla a la persona correcta.

El siguiente código demuestra el proceso de cómo detectar una cara en una imagen y agregarla a una persona:

```csharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceClient.PersonGroupPerson.AddFaceFromStreamAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Si la imagen contiene más de una cara, solo se agrega la cara más grande. Puede agregar otras caras a la persona. Pase una cadena con el formato "targetFace = left, top, width, height" al parámetro de consulta targetFace de [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API. También puede usar el parámetro opcional targetFace para que el método AddPersonFaceAsync agregue otras caras. Cada cara agregada a la persona tiene un identificador de cara persistente único. Puede usar este identificador en [PersonGroup Person – Delete Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) y en [Face – Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="step-3-train-the-persongroup"></a>Paso 3: Entrenamiento del elemento PersonGroup

El elemento PersonGroup debe entrenarse antes de que se pueda realizar una identificación con ella. El elemento PersonGroup se debe volver a entrenar después de agregar o quitar cualquier persona o si edita la cara registrada de una persona. El entrenamiento se realiza mediante [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API. Cuando usa la biblioteca cliente, se trata de una llamada al método TrainPersonGroupAsync:
 
```csharp 
await faceClient.PersonGroup.TrainAsync(personGroupId);
```
 
El entrenamiento es un proceso asincrónico. Es posible que no se haya terminado incluso después de que se haya devuelto el método TrainPersonGroupAsync. Quizás necesite consultar el estado del entrenamiento. Use la API [PersonGroup - Get Training Status](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API o el método GetPersonGroupTrainingStatusAsync de la biblioteca cliente. El código siguiente muestra una lógica sencilla de esperar que se complete el entrenamiento del elemento PersonGroup:
 
```csharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceClient.PersonGroup.GetTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != TrainingStatusType.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>Paso 4: Identificación de una cara con un elemento PersonGroup definido

Cuando el servicio de Face realiza las identificaciones, calcula la similitud de una cara de prueba entre todas las caras dentro de un grupo. Devuelve las personas más comparables con la cara de prueba. Este proceso se realiza con [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API o con el método IdentifyAsync de la biblioteca cliente.

La cara de prueba se debe detectar con los pasos anteriores. Luego, el identificador de cara se pasa a la API de identificación como segundo argumento. Es posible identificar varios id. de caras a la vez. El resultado contiene todos los resultados identificados. De manera predeterminada, el proceso de identificación devuelve solo una persona que coincida mejor con la cara de la prueba. Si lo prefiere, especifique el parámetro opcional maxNumOfCandidatesReturned para que el proceso de identificación devuelva más candidatos.

El código siguiente muestra el proceso de identificación:

```csharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceClient.Face.DetectWithStreamAsync(s);
    var faceIds = faces.Select(face => face.FaceId.Value).ToArray();
 
    var results = await faceClient.Face.IdentifyAsync(faceIds, personGroupId);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceClient.PersonGroupPerson.GetAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Una vez que complete los pasos, intente identificar las distintas caras. Vea si las caras de Anna, Bill o Clare se pueden identificar correctamente según las imágenes cargadas para realizar la detección de caras. Consulte los siguientes ejemplos:

![Identificación de distintas caras](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>Paso 5: Solicitud de gran escala

Un elemento PersonGroup puede contener hasta 10 000 personas según la limitación de diseño anterior.
Para más información sobre escenarios de hasta un millón de escalas, consulte [Uso de la característica a gran escala](how-to-use-large-scale.md).

## <a name="summary"></a>Resumen

En esta guía, aprendió el proceso de creación de un elemento PersonGroup y a identificar una persona. Se explicaron y mostraron las siguientes características:

- Detección de caras con [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API.
- Creación de un elemento PersonGroups con [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API.
- Creación de personas con [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API.
- Entrenamiento de un elemento PersonGroup con [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API.
- Identificación de caras desconocidas en el elemento PersonGroup con [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API.

## <a name="related-topics"></a>Temas relacionados

- [Face recognition concepts](../concepts/face-recognition.md) (Conceptos del reconocimiento facial)
- [Detección de caras en una imagen](HowtoDetectFacesinImage.md)
- [Agregar caras](how-to-add-faces.md)
- [Uso de la característica a gran escala](how-to-use-large-scale.md)
