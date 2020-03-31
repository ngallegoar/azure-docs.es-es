---
title: 'Inicio rápido: Enumeración de voces de texto a voz en Node.js: servicio de voz'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, aprenderá a obtener la lista completa de voces estándar y neuronales para una región o punto de conexión mediante Node.js. La lista se devuelve como JSON y la disponibilidad de las voces varía por regiones.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: erhopf
ms.openlocfilehash: 7a929794ffaea4f863ffaef7227e58c7ccf901f0
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74976577"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-nodejs"></a>Inicio rápido: Obtención de la lista de voces de texto a voz mediante Node.js

En esta guía de inicio rápido, aprenderá a obtener la lista completa de voces estándar y neuronales para una región o punto de conexión mediante Node.js. La lista se devuelve como JSON y la disponibilidad de las voces varía por regiones. Para ver una lista de las regiones admitidas, consulte [Regiones](regions.md).

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso del servicio de voz. Si no tiene una cuenta, puede usar la [evaluación gratuita](get-started.md) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Prerrequisitos

Esta guía de inicio rápido requiere:

* [Node 8.12.x o posterior](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Creación de un proyecto y requerimiento de dependencias

Cree un proyecto de Node.js mediante su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `get-voices.js`.

```javascript
// Requires request and request-promise for HTTP requests
// e.g. npm install request request-promise
const rp = require('request-promise');
// Requires fs to write the list of languages to a file
const fs = require('fs');
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `npm install request request-promise`.

## <a name="get-an-access-token"></a>Obtención de un token de acceso

La API REST Texto a voz requiere un token de acceso para la autenticación. Para obtener un token de acceso, es necesario un intercambio. En esta función se intercambia la clave de suscripción del servicio de voz para obtener un token de acceso usando el punto de conexión `issueToken`.

En este ejemplo se da por supuesto que su suscripción del servicio de voz está en la región Oeste de EE. UU. Si usa una región diferente, actualice el valor de `uri`. Para obtener una lista completa, consulte [Regiones](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Copie este código en el proyecto:

```javascript
// Gets an access token.
function getAccessToken(subscriptionKey) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    }
    return rp(options);
}
```

> [!NOTE]
> Para obtener más información, vea [Autenticación con un token de autenticación](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

En la siguiente sección, vamos a crear la función para obtener la lista de las voces y guardar la salida JSON en un archivo.

## <a name="make-a-request-and-save-the-response"></a>Realización de solicitud y guardado de la respuesta

Aquí va a generar la solicitud y guardar la lista de voces devueltas. En este ejemplo se da por hecho que está utilizando el punto de conexión de Oeste de EE. UU. Si el recurso está registrado en una región diferente, asegúrese de actualizar `uri`. Para más información, consulte [Regiones del servicio de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

A continuación, agregue los encabezados necesarios para la solicitud. Por último, deberá realizar una solicitud al servicio. Si la solicitud es correcta y se devuelve un código de estado 200, la respuesta se escribe en el archivo.

```javascript
function textToSpeech(accessToken) {
    let options = {
        method: 'GET',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/voices/list',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'Content-Type': 'application/json'
        }
    }

    let request = rp(options)
        .on('response', (response) => {
            if (response.statusCode === 200) {
                request.pipe(fs.createWriteStream('voices.json'));
                console.log('\nYour file is ready.\n')
            }
        });
    return request;
}
```

## <a name="put-it-all-together"></a>Colocación de todo junto

Ya casi ha terminado. El último paso es crear una función asincrónica. Esta función leerá la clave de suscripción desde una variable de entorno, obtendrá un token, esperará a que la solicitud se complete y, a continuación, escribirá la respuesta JSON en un archivo.

Si no está familiarizado con las variables de entorno o si prefiere probar con su clave de suscripción codificada de forma rígida, sustituya `process.env.SPEECH_SERVICE_KEY` por la clave de suscripción como una cadena.

```javascript
// Use async and await to get the token before attempting
// to convert text to speech.
async function main() {
    // Reads subscription key from env variable.
    // You can replace this with a string containing your subscription key. If
    // you prefer not to read from an env variable.
    // e.g. const subscriptionKey = "your_key_here";
    const subscriptionKey = process.env.SPEECH_SERVICE_KEY;
    if (!subscriptionKey) {
        throw new Error('Environment variable for your subscription key is not set.')
    };
    try {
        const accessToken = await getAccessToken(subscriptionKey);
        await textToSpeech(accessToken);
    } catch (err) {
        console.log(`Something went wrong: ${err}`);
    }
}

main()
```

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

Eso es todo, ya está listo para ejecutar la aplicación de ejemplo. Desde la línea de comandos (o sesión de terminal), vaya al directorio del proyecto y ejecute:

```console
node get-voices.js
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Asegúrese de quitar cualquier información confidencial del código fuente de la aplicación de ejemplo como, por ejemplo, las claves de suscripción.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar ejemplos de Node.js en GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Consulte también

* [Referencia de Text-to-speech API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Crear fuentes de voz personalizada](how-to-customize-voice-font.md)
* [Grabación de muestras de voz para crear una voz personalizada](record-custom-voice-samples.md)
