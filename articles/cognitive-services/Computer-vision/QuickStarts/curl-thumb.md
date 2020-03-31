---
title: 'Inicio rápido: Generación de una miniatura (REST, cURL)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, generará una miniatura de una imagen mediante Computer Vision API con cURL.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 6cb9dadc107c6907f1ccb28a876270e577f10395
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74977308"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-curl"></a>Inicio rápido: Generación de una miniatura mediante la API REST Computer Vision y cURL

En esta guía de inicio rápido, generará una miniatura de una imagen mediante la API REST de Computer Vision. Debe especificar el alto y el ancho deseados, con una relación de aspecto que puede ser distinta a la de la imagen de entrada. Computer Vision usa el recorte inteligente para identificar el área de interés de forma inteligente y generar coordenadas de recorte alrededor de esa región.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

- Debe tener [cURL](https://curl.haxx.se/windows).
- Debe tener una clave de suscripción para Computer Vision. Puede obtener una clave de evaluación gratuita en la página [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). O bien, siga las instrucciones que se indican en [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse a Computer Vision y obtener su clave.

## <a name="get-thumbnail-request"></a>Solicitud Get Thumbnail

Con el método [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), puede generar una miniatura de una imagen.

Para ejecutar el ejemplo, siga estos pasos:

1. Copie el código siguiente en un editor.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Reemplace `<File>` por la ruta de acceso y el nombre de archivo para guardar la miniatura.
1. Si es necesario, cambie la dirección URL de solicitud (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.1`) para usar la ubicación donde obtuvo las claves de suscripción.
1. Además, puede cambiar la imagen (`{\"url\":\"...`) que se va a analizar.
1. Abra una ventana de comandos en un equipo que tenga instalado cURL.
1. Pegue el código en la ventana y ejecute el comando.

>[!NOTE]
>Debe utilizar la misma ubicación en la llamada de REST que utilizó para obtener las claves de la suscripción. Por ejemplo, si obtuvo sus claves de suscripción de westus, reemplace "westcentralus" en la siguiente URL por "westus".

## <a name="create-and-run-the-sample-command"></a>Creación y ejecución del comando de ejemplo

Para crear y ejecutar el ejemplo, siga estos pasos:

1. Copie el comando siguiente en un editor de texto.
1. Realice los siguientes cambios en el comando donde sea necesario:
    1. Reemplace el valor de `<subscriptionKey>` por la clave de suscripción.
    1. Reemplace el valor de `<thumbnailFile>` con la ruta de acceso y el nombre del archivo donde se va a guardar la miniatura.
    1. Reemplace la primera parte de la dirección URL de la solicitud (`westcentralus`) por el texto de la dirección URL de su punto de conexión.
        [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
    1. Si lo desea, cambie la dirección URL de la imagen del cuerpo de la solicitud (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) por la dirección URL de una imagen diferente de la que desea generar una miniatura.
1. Abra una ventana de símbolo del sistema.
1. Pegue el comando del editor de texto en la ventana del símbolo del sistema y después ejecute el comando.

    ```bash
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.1/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>Examen de la respuesta

Una respuesta correcta escribe la imagen en miniatura en el archivo especificado en `<thumbnailFile>`. Si se produce un error en la solicitud, la respuesta contiene un código de error y un mensaje para ayudar a determinar qué salió mal. Si la solicitud parece realizarse correctamente pero la miniatura creada no es un archivo de imagen válido, es posible que su clave de suscripción no sea válida.

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API para ver cómo analizar una imagen, detectar celebridades y sitios emblemáticos, crear una miniatura y extraer texto impreso y manuscrito. Para experimentar rápidamente con la versión de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Explore Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
