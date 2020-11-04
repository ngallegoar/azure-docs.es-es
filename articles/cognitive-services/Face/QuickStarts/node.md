---
title: 'Inicio rápido: Detección de caras en una imagen con la API REST de Azure y Node.js'
titleSuffix: Azure Cognitive Services
description: En este tutorial, usará la API REST de Azure Face con Node.js para detectar caras en una imagen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 08/05/2020
ms.author: pafarley
ms.custom: devx-track-js
ms.openlocfilehash: f77282502a0032a5b1811e32d9aa47d9b4ede569
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "92912827"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-nodejs"></a>Inicio rápido: Detección de caras en una imagen mediante Face REST API y Node.js

En este inicio rápido, usará la API REST de Azure Face con Node.js para detectar caras humanas en una imagen.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/) antes de empezar. 

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/)
* Una vez que tenga la suscripción de Azure, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Creación de un recurso de Face"  target="_blank">cree un recurso de Face <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para obtener la clave y el punto de conexión. Una vez que se implemente, haga clic en **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación a Face API. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.
- Un editor de código como [Visual Studio Code](https://code.visualstudio.com/download)

## <a name="set-up-the-node-environment"></a>Configuración del entorno de nodo

Vaya a la carpeta donde desea crear el proyecto y cree un nuevo archivo, *facedetection.js*. A continuación, instale el módulo `axios`para este proyecto. De este modo, permite que los scripts realicen solicitudes HTTP.

```shell
npm install axios --save
```

## <a name="write-the-nodejs-script"></a>Escritura del script de Node.js

Pegue el siguiente código en el archivo *facedetection.js*. Estos campos especifican cómo conectarse al servicio Face y dónde obtener los datos de entrada. [Cree las variables de entorno](../../cognitive-services-apis-create-account.md?tabs=multiservice%252cwindows#configure-an-environment-variable-for-authentication) y agregue la clave de suscripción Face y el punto de conexión para ellas. Es posible que desee cambiar el campo `imageUrl` para que apunte a su propia imagen de entrada.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/Face/rest/detect.js" id="environment":::

Después, agregue el código siguiente para llamar a Face API y obtener los datos de los atributos de una imagen de entrada. El campo `returnFaceAttributes` especifica qué atributos de cara recuperar. Es posible que desee cambiar esta cadena según su uso previsto.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/Face/rest/detect.js" id="main":::

## <a name="save-and-run-the-script"></a>Guardado y ejecución del script

Una vez realizados los cambios, abra un símbolo del sistema y ejecute el archivo con el comando `node`.

```
node facedetection.js
```

Una respuesta correcta mostrará datos de las caras en formato JSON fácilmente legible. Por ejemplo:

```json
[
   {
      "faceId": "f7eda569-4603-44b4-8add-cd73c6dec644",
      "faceRectangle": {
         "top": 131,
         "left": 177,
         "width": 162,
         "height": 162
      }
   }
]
```

## <a name="extract-face-attributes"></a>Extracción de los atributos de la cara
 
Para extraer atributos de la cara, use el modelo de detección 1 y agregue el parámetro de consulta `returnFaceAttributes`. Edite los parámetros de la manera siguiente:

```javascript
    params : {
        detectionModel: 'detection_01',
        returnFaceAttributes: 'age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise',
        returnFaceId: true
    },
```

La respuesta ahora incluye atributos de la cara. Por ejemplo:

```json
[
  {
    "faceId": "ae8952c1-7b5e-4a5a-a330-a6aa351262c9",
    "faceRectangle": {
      "top": 621,
      "left": 616,
      "width": 195,
      "height": 195
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 6.8,
        "yaw": 3.7
      },
      "gender": "male",
      "age": 37,
      "facialHair": {
        "moustache": 0.4,
        "beard": 0.4,
        "sideburns": 0.1
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.999,
        "sadness": 0.001,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.89
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.51
      },
      "noise": {
        "noiseLevel": "medium",
        "value": 0.59
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.04,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.98
          },
          {
            "color": "brown",
            "confidence": 0.87
          },
          {
            "color": "gray",
            "confidence": 0.85
          },
          {
            "color": "other",
            "confidence": 0.25
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.02
          }
        ]
      }
    }
  },
  {
    "faceId": "b1bb3cbe-5a73-4f8d-96c8-836a5aca9415",
    "faceRectangle": {
      "top": 693,
      "left": 1503,
      "width": 180,
      "height": 180
    },
    "faceAttributes": {
      "smile": 0.003,
      "headPose": {
        "pitch": 0,
        "roll": 2,
        "yaw": -2.2
      },
      "gender": "female",
      "age": 56,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0.001,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.003,
        "neutral": 0.984,
        "sadness": 0.011,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "high",
        "value": 0.83
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.41
      },
      "noise": {
        "noiseLevel": "high",
        "value": 0.76
      },
      "makeup": {
        "eyeMakeup": false,
        "lipMakeup": false
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.06,
        "invisible": false,
        "hairColor": [
          {
            "color": "black",
            "confidence": 0.99
          },
          {
            "color": "gray",
            "confidence": 0.89
          },
          {
            "color": "other",
            "confidence": 0.64
          },
          {
            "color": "brown",
            "confidence": 0.34
          },
          {
            "color": "blond",
            "confidence": 0.07
          },
          {
            "color": "red",
            "confidence": 0.03
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha escrito un script de Node.js que llama al servicio de Azure Face para detectar caras en una imagen y devolver sus atributos. A continuación, explore la documentación de referencia de Face API para más información.

> [!div class="nextstepaction"]
> [Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)