---
title: 'Inicio rápido: Generación de una miniatura (REST, JavaScript)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, generará una miniatura de una imagen mediante Computer Vision API con JavaScript.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 1d07bc12f33df7253a849b605fdaff1f2f0123dd
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974553"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-javascript"></a>Inicio rápido: Generación de una miniatura mediante la API REST Computer Vision y JavaScript

En esta guía de inicio rápido, generará una miniatura de una imagen mediante la API REST de Computer Vision. Debe especificar el alto y el ancho, que pueden ser diferentes a la relación de aspecto de la imagen de entrada. Computer Vision usa el recorte inteligente para identificar el área de interés de forma inteligente y generar coordenadas de recorte en función de esa región.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Debe tener una clave de suscripción para Computer Vision. Puede obtener una clave de evaluación gratuita en la página [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). O bien, siga las instrucciones que se indican en [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse a Computer Vision y obtener su clave. Después, [cree variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para la cadena de punto de conexión del servicio y la clave denominadas `COMPUTER_VISION_SUBSCRIPTION_KEY` y `COMPUTER_VISION_ENDPOINT`, respectivamente.

## <a name="create-and-run-the-sample"></a>Creación y ejecución del ejemplo

Para crear y ejecutar el ejemplo, siga estos pasos:

1. Copie el código siguiente en un editor de texto.
1. También puede reemplazar el valor del atributo `value` para el control `inputImage` por la dirección URL de una imagen diferente que desee analizar.
1. Guarde el código como un archivo con la extensión `.html`. Por ejemplo, `get-thumbnail.html`.
1. Abra una ventana del explorador.
1. En el explorador, arrastre y coloque el archivo en la ventana del explorador.
1. Cuando la página web se muestra en el explorador, seleccione el botón **Generar miniatura**.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Thumbnail Sample</title>
</head>
<body>

<script type="text/javascript">
    function processImage() {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        let subscriptionKey = process.env['COMPUTER_VISION_SUBSCRIPTION_KEY'];
        let endpoint = process.env['COMPUTER_VISION_ENDPOINT']
        if (!subscriptionKey) { throw new Error('Set your environment variables for your subscription key and endpoint.'); }
        
        var uriBase = endpoint + "vision/v2.1/generateThumbnail";

        // Request parameters.
        var params = "?width=100&height=150&smartCropping=true";

        // Display the source image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;

        // Prepare the REST API call:

        // Create the HTTP Request object.
        var xhr = new XMLHttpRequest();

        // Identify the request as a POST, with the URL and parameters.
        xhr.open("POST", uriBase + params);

        // Add the request headers.
        xhr.setRequestHeader("Content-Type","application/json");
        xhr.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

        // Set the response type to "blob" for the thumbnail image data.
        xhr.responseType = "blob";

        // Process the result of the REST API call.
        xhr.onreadystatechange = function(e) {
            if(xhr.readyState === XMLHttpRequest.DONE) {

                // Thumbnail successfully created.
                if (xhr.status === 200) {
                    // Show response headers.
                    var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                    document.getElementById("responseTextArea").value =
                        JSON.stringify(xhr.getAllResponseHeaders(), null, 2);

                    // Show thumbnail image.
                    var urlCreator = window.URL || window.webkitURL;
                    var imageUrl = urlCreator.createObjectURL(this.response);
                    document.querySelector("#thumbnailImage").src = imageUrl;
                } else {
                    // Display the error message. The error message is the response
                    // body as a JSON string. The code in this code block extracts
                    // the JSON string from the blob response.
                    var reader = new FileReader();

                    // This event fires after the blob has been read.
                    reader.addEventListener('loadend', (e) => {
                        document.getElementById("responseTextArea").value =
                            JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                    });

                    // Start reading the blob as text.
                    reader.readAsText(xhr.response);
                }
            }
        }

        // Make the REST API call.
        xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
    };
</script>

<h1>Generate thumbnail image:</h1>
Enter the URL to an image to use in creating a thumbnail image,
then click the <strong>Generate thumbnail</strong> button.
<br><br>
Image for thumbnail:
<input type="text" name="inputImage" id="inputImage"
    value="https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg" />
<button onclick="processImage()">Generate thumbnail</button>
<br><br>
<div id="wrapper" style="width:1160px; display:table;">
    <div id="jsonOutput" style="width:600px; display:table-cell;">
        Response:
        <br><br>
        <textarea id="responseTextArea" class="UIInput"
                  style="width:580px; height:400px;"></textarea>
    </div>
    <div id="imageDiv" style="width:420px; display:table-cell;">
        Source image:
        <br><br>
        <img id="sourceImage" width="400" />
    </div>
    <div id="thumbnailDiv" style="width:140px; display:table-cell;">
        Thumbnail:
        <br><br>
        <img id="thumbnailImage" />
    </div>
</div>
</body>
</html>
```

## <a name="examine-the-response"></a>Examen de la respuesta

Se devuelve una respuesta correcta como datos binarios, que representan los datos de imagen para la miniatura. Si la solicitud se realiza correctamente, la miniatura se genera a partir de los datos binarios de la respuesta y se muestra en la ventana del explorador. Si se produce un error en la solicitud, la respuesta se muestra en la ventana de consola. La respuesta de la solicitud con error contiene un código de error y un mensaje para ayudar a determinar qué salió mal.

## <a name="next-steps"></a>Pasos siguientes

Explore una aplicación de JavaScript que usa Computer Vision para realizar el reconocimiento óptico de caracteres (OCR); crear miniaturas con recorte inteligente; y detectar, clasificar, etiquetar y describir características visuales, como caras, en una imagen. Para experimentar rápidamente con la versión de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Tutorial de Computer Vision API para JavaScript](../Tutorials/javascript-tutorial.md)
