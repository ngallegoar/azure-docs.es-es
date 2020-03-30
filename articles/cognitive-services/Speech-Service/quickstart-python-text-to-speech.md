---
title: 'Conversión de texto a voz en Python: servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, obtendrá información sobre cómo convertir texto a voz con Python y la API de REST Texto a voz. El texto de ejemplo incluido en esta guía se estructura como lenguaje de marcado de síntesis de voz (SSML). Esto le permite elegir la voz y el idioma de la respuesta de voz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: how-to
ms.date: 02/10/2020
ms.author: dapine
ms.openlocfilehash: 79562049f48ba90a4f9a123919185521a82d7be6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80365786"
---
# <a name="convert-text-to-speech-using-python"></a>Conversión de texto a voz con Python

En este artículo, obtendrá información sobre cómo convertir texto a voz con Python y la API de REST Texto a voz. El cuerpo de solicitud de esta guía se estructura como [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md), que permite elegir la voz y el idioma de la respuesta.

Para este artículo se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso del servicio de voz. Si no tiene una cuenta, puede usar la [evaluación gratuita](get-started.md) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Prerequisites

* Python 2.7.x o 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código
* Una clave de suscripción de Azure para el servicio de voz.

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un nuevo proyecto de Python con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `tts.py`.

```python
import os
import requests
import time
from xml.etree import ElementTree
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `pip install requests`.

Estos módulos se usan para escribir la respuesta de voz en un archivo con una marca de tiempo, crear la solicitud HTTP y llamar a la API Texto a voz.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Establecimiento de la clave de suscripción y creación de un símbolo del sistema para TTS

En las secciones siguientes podrá crear métodos para controlar la autorización, llamar a la API Texto a voz y validar la respuesta. Comencemos agregando código que garantice que este ejemplo funcione con Python 2.7. x y 3.x.

```python
try:
    input = raw_input
except NameError:
    pass
```

A continuación, crearemos una clase. Aquí pondremos nuestros métodos de intercambio de token y llamaremos a la API Texto a voz.

```python
class TextToSpeech(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.tts = input("What would you like to convert to speech: ")
        self.timestr = time.strftime("%Y%m%d-%H%M")
        self.access_token = None
```

`subscription_key` es la clave única de Azure Portal. `tts` pide al usuario que escriba el texto que se convertirá en voz. Esta entrada es un literal de cadena, por lo que no es necesario anteponer caracteres. Por último, `timestr` obtiene la hora actual, que vamos a usar para ponerle nombre al archivo.

## <a name="get-an-access-token"></a>Obtención de un token de acceso

La API REST Texto a voz requiere un token de acceso para la autenticación. Para obtener un token de acceso, es necesario un intercambio. En este ejemplo se intercambia la clave de suscripción del servicio de voz para obtener un token de acceso usando el punto de conexión `issueToken`.

En este ejemplo se da por supuesto que su suscripción del servicio de voz está en la región Oeste de EE. UU. Si usa una región diferente, actualice el valor de `fetch_token_url`. Para obtener una lista completa, consulte [Regiones](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Copie este código en la clase `TextToSpeech`:

```python
def get_token(self):
    fetch_token_url = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    headers = {
        'Ocp-Apim-Subscription-Key': self.subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    self.access_token = str(response.text)
```

> [!NOTE]
> Para obtener más información, vea [Autenticación con un token de autenticación](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Realización de solicitud y guardado de la respuesta

Aquí va a generar la solicitud y guardar la respuesta de voz. En primer lugar, deberá establecer `base_url` y `path`. En este ejemplo se da por hecho que está utilizando el punto de conexión de Oeste de EE. UU. Si el recurso está registrado en una región diferente, asegúrese de actualizar `base_url`. Para más información, consulte [Regiones del servicio de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

A continuación, deberá agregar los encabezados necesarios para la solicitud. Asegúrese de actualizar `User-Agent` con el nombre del recurso (que se encuentra en Azure Portal) y establezca `X-Microsoft-OutputFormat` en la salida de audio preferida. Para obtener una lista completa de los formatos de salida, vea [Salidas de Audio](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

A continuación, cree el cuerpo de solicitud mediante el lenguaje de marcado de síntesis de voz (SSML). Este ejemplo define la estructura y usa la entrada `tts` que creó anteriormente.

>[!NOTE]
> Este ejemplo se utiliza la fuente de voz `Guy24kRUS`. Para obtener una lista completa de los idiomas y las voces que proporciona Microsoft, consulte [Compatibilidad con idiomas](language-support.md).
> Si está interesado en crear una voz única y reconocible para su marca, vea [Crear fuentes de voz personalizada](how-to-customize-voice-font.md).

Por último, deberá realizar una solicitud al servicio. Si la solicitud es correcta y se devuelve un código de estado 200, la respuesta de voz se escribe en un archivo con marca de tiempo.

Copie este código en la clase `TextToSpeech`:

```python
def save_audio(self):
    base_url = 'https://westus.tts.speech.microsoft.com/'
    path = 'cognitiveservices/v1'
    constructed_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token,
        'Content-Type': 'application/ssml+xml',
        'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
        'User-Agent': 'YOUR_RESOURCE_NAME'
    }
    xml_body = ElementTree.Element('speak', version='1.0')
    xml_body.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-us')
    voice = ElementTree.SubElement(xml_body, 'voice')
    voice.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-US')
    voice.set(
        'name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)')
    voice.text = self.tts
    body = ElementTree.tostring(xml_body)

    response = requests.post(constructed_url, headers=headers, data=body)
    if response.status_code == 200:
        with open('sample-' + self.timestr + '.wav', 'wb') as audio:
            audio.write(response.content)
            print("\nStatus code: " + str(response.status_code) +
                  "\nYour TTS is ready for playback.\n")
    else:
        print("\nStatus code: " + str(response.status_code) +
              "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Colocación de todo junto

Ya casi ha terminado. El último paso es crear una instancia de la clase y llamar a las funciones.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = TextToSpeech(subscription_key)
    app.get_token()
    app.save_audio()
```

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

Eso es todo; ya está listo para ejecutar la aplicación de ejemplo de Texto a voz. Desde la línea de comandos (o sesión de terminal), vaya al directorio del proyecto y ejecute:

```console
python tts.py
```

Cuando se le solicite, escriba todo lo que gustaría convertir de texto a voz. Si se realiza correctamente, el archivo se ubicará en la carpeta del proyecto. Reprodúzcalo con el reproductor que prefiera.

## <a name="clean-up-resources"></a>Limpieza de recursos

Asegúrese de quitar cualquier información confidencial del código fuente de la aplicación de ejemplo como, por ejemplo, las claves de suscripción.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de los ejemplos de Python en GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Consulte también

* [Referencia de Text-to-speech API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Uso de Python y el SDK de Voz para convertir texto a voz](quickstarts/speech-to-text-from-microphone.md)
* [Crear fuentes de voz personalizada](how-to-customize-voice-font.md)
* [Grabación de muestras de voz para crear una voz personalizada](record-custom-voice-samples.md)
