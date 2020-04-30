---
title: 'Inicio rápido: Síntesis asincrónica para audio de formato largo (versión preliminar): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Use Long Audio API para convertir texto a voz de forma asincrónica y recuperar la salida de audio de un URI proporcionado por el servicio. Esta API REST es ideal para los proveedores de contenido que necesitan convertir archivos de texto de más de 10 000 caracteres o 50 párrafos en voz sintetizada.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: dcdc942999e45eb779e54cd5f92432c54d65fc6a
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82561988"
---
# <a name="quickstart-asynchronous-synthesis-for-long-form-audio-in-python-preview"></a>Inicio rápido: síntesis asincrónica para audio de formato largo en Python (versión preliminar)

En esta guía de inicio rápido, usará Long Audio API para convertir texto a voz de forma asincrónica y recuperar la salida de audio de un URI proporcionado por el servicio. Esta API REST es idónea para los proveedores de contenido que necesitan sintetizar el audio de un texto mayor de 5000 caracteres (o más de 10 minutos de longitud). Para obtener más información, consulte [Long Audio API](../../long-audio-api.md).

> [!NOTE]
> La síntesis asincrónica de audio de formato largo solo se puede usar con [voces neuronales personalizadas](../../how-to-custom-voice.md#custom-neural-voices).

## <a name="prerequisites"></a>Prerrequisitos

Esta guía de inicio rápido requiere:

* Python 2.7.x o 3.x.
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código.
* Una suscripción a Azure y una clave de suscripción al servicio de Voz. [Cree una cuenta de Azure](../../get-started.md#new-resource) y [cree un recurso de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-resource) para obtener la clave. Al crear el recurso de voz, asegúrese de que el plan de tarifa está establecido en **S0** y la ubicación está establecida en una [región compatible](../../regions.md#standard-and-neural-voices).

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un nuevo proyecto de Python con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `voice_synthesis_client.py`.

```python
import argparse
import json
import ntpath
import urllib3
import requests
import time
from json import dumps, loads, JSONEncoder, JSONDecoder
import pickle

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
```

> [!NOTE]
> Si no ha usado estos módulos, tendrá que instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `pip install requests urllib3`.

Estos módulos se usan para analizar argumentos, construir la solicitud HTTP y llamar a la API REST de audios largos para convertir texto a voz.

## <a name="get-a-list-of-supported-voices"></a>Obtención de una lista de voces compatibles

Este código obtiene una lista de las voces disponibles que puede usar para convertir texto a voz. Agregue el código a `voice_synthesis_client.py`:

```python
parser = argparse.ArgumentParser(description='Text-to-speech client tool to submit voice synthesis requests.')
parser.add_argument('--voices', action="store_true", default=False, help='print voice list')
parser.add_argument('-key', action="store", dest="key", required=True, help='the speech subscription key, like fg1f763i01d94768bda32u7a******** ')
parser.add_argument('-region', action="store", dest="region", required=True, help='the region information, could be centralindia, canadacentral or uksouth')
args = parser.parse_args()
baseAddress = 'https://%s.customvoice.api.speech.microsoft.com/api/texttospeech/v3.0-beta1/' % args.region

def getVoices():
    response=requests.get(baseAddress+"voicesynthesis/voices", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    voices = json.loads(response.text)
    return voices

if args.voices:
    voices = getVoices()
    print("There are %d voices available:" % len(voices))
    for voice in voices:
        print ("Name: %s, Description: %s, Id: %s, Locale: %s, Gender: %s, PublicVoice: %s, Created: %s" % (voice['name'], voice['description'], voice['id'], voice['locale'], voice['gender'], voice['isPublicVoice'], voice['created']))
```

### <a name="test-your-code"></a>Prueba del código

Probemos lo que ha hecho hasta ahora. Deberá actualizar algunos detalles en la solicitud siguiente:

* Reemplace `<your_key>` por la clave de suscripción del servicio Speech. Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).
* Reemplace `<region>` por la región donde se creó el recurso de Voz (por ejemplo: `eastus` o `westus`). Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).

Ejecute este comando:

```console
python voice_synthesis_client.py --voices -key <your_key> -region <Region>
```

Verá una salida similar a la siguiente:

```console
There are xx voices available:

Name: Microsoft Server Speech Text to Speech Voice (en-US, xxx), Description: xxx , Id: xxx, Locale: en-US, Gender: Male, PublicVoice: xxx, Created: 2019-07-22T09:38:14Z
Name: Microsoft Server Speech Text to Speech Voice (zh-CN, xxx), Description: xxx , Id: xxx, Locale: zh-CN, Gender: Female, PublicVoice: xxx, Created: 2019-08-26T04:55:39Z
```

## <a name="prepare-input-files"></a>Preparación de los archivos de entrada

Prepare un archivo de texto de entrada. Puede ser de texto sin formato o texto SSML. Para obtener los requisitos del archivo de entrada, vea el procedimiento de [preparación del contenido para la síntesis](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#prepare-content-for-synthesis).

## <a name="convert-text-to-speech"></a>Conversión de texto en voz

Después de preparar el archivo de texto de entrada, agregue este código para la síntesis de voz a `voice_synthesis_client.py`:

> [!NOTE]
> "concatenateResult" es un parámetro opcional. Si este parámetro no se establece, las salidas de audio se generarán por párrafo. También puede concatenar los audios en una salida si establece el parámetro. De forma predeterminada, la salida de audio se establece en riff-16khz-16bit-mono-pcm. Para más información sobre las salidas de audio compatibles, vea [Formatos de salida de audio](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#audio-output-formats).

```python
parser.add_argument('--submit', action="store_true", default=False, help='submit a synthesis request')
parser.add_argument('--concatenateResult', action="store_true", default=False, help='If concatenate result in a single wave file')
parser.add_argument('-file', action="store", dest="file", help='the input text script file path')
parser.add_argument('-voiceId', action="store", nargs='+', dest="voiceId", help='the id of the voice which used to synthesis')
parser.add_argument('-locale', action="store", dest="locale", help='the locale information like zh-CN/en-US')
parser.add_argument('-format', action="store", dest="format", default='riff-16khz-16bit-mono-pcm', help='the output audio format')

def submitSynthesis():
    modelList = args.voiceId
    data={'name': 'simple test', 'description': 'desc...', 'models': json.dumps(modelList), 'locale': args.locale, 'outputformat': args.format}
    if args.concatenateResult:
        properties={'ConcatenateResult': 'true'}
        data['properties'] = json.dumps(properties)
    if args.file is not None:
        scriptfilename=ntpath.basename(args.file)
        files = {'script': (scriptfilename, open(args.file, 'rb'), 'text/plain')}
    response = requests.post(baseAddress+"voicesynthesis", data, headers={"Ocp-Apim-Subscription-Key":args.key}, files=files, verify=False)
    if response.status_code == 202:
        location = response.headers['Location']
        id = location.split("/")[-1]
        print("Submit synthesis request successful")
        return id
    else:
        print("Submit synthesis request failed")
        print("response.status_code: %d" % response.status_code)
        print("response.text: %s" % response.text)
        return 0

def getSubmittedSynthesis(id):
    response=requests.get(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    synthesis = json.loads(response.text)
    return synthesis

if args.submit:
    id = submitSynthesis()
    if (id == 0):
        exit(1)

    while(1):
        print("\r\nChecking status")
        synthesis=getSubmittedSynthesis(id)
        if synthesis['status'] == "Succeeded":
            r = requests.get(synthesis['resultsUrl'])
            filename=id + ".zip"
            with open(filename, 'wb') as f:  
                f.write(r.content)
                print("Succeeded... Result file downloaded : " + filename)
            break
        elif synthesis['status'] == "Failed":
            print("Failed...")
            break
        elif synthesis['status'] == "Running":
            print("Running...")
        elif synthesis['status'] == "NotStarted":
            print("NotStarted...")
        time.sleep(10)
```

### <a name="test-your-code"></a>Prueba del código

Vamos a realizar una solicitud para sintetizar el texto mediante el archivo de entrada como origen. Deberá actualizar algunos detalles en la solicitud siguiente:

* Reemplace `<your_key>` por la clave de suscripción del servicio Speech. Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).
* Reemplace `<region>` por la región donde se creó el recurso de Voz (por ejemplo: `eastus` o `westus`). Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).
* Reemplace `<input>` por la ruta de acceso al archivo de texto que ha preparado para convertir de texto a voz.
* Reemplace `<locale>` por la configuración regional de salida deseada. Para obtener más información, consulte [Compatibilidad de idioma](../../language-support.md#neural-voices).
* Reemplace `<voice_guid>` por la voz de salida deseada. Use una de las voces devueltas por [Obtención de una lista de voces compatibles](#get-a-list-of-supported-voices).

Convierta texto a voz con este comando:

```console
python voice_synthesis_client.py --submit -key <your_key> -region <Region> -file <input> -locale <locale> -voiceId <voice_guid>
```

> [!NOTE]
> Si tiene más de un archivo de entrada, tendrá que enviar varias solicitudes. Existen algunas limitaciones que tener en cuenta. 
> * El cliente puede enviar hasta **5** solicitudes al servidor por segundo para cada cuenta de suscripción de Azure. Si supera la limitación, el cliente obtendrá un código de error 429 (demasiadas solicitudes). Reduzca la cantidad de solicitudes por segundo.
> * El servidor puede ejecutar y poner en cola hasta **120** solicitudes para cada cuenta de suscripción de Azure. Si supera la limitación, el servidor devolverá un código de error 429 (demasiadas solicitudes). Espere y evite enviar una nueva solicitud hasta que se completen algunas solicitudes.

Verá una salida similar a la siguiente:

```console
Submit synthesis request successful

Checking status
NotStarted...

Checking status
Running...

Checking status
Running...

Checking status
Succeeded... Result file downloaded : xxxx.zip
```

El resultado contiene el texto de entrada y los archivos de salida de audio generados por el servicio. Puede descargar estos archivos en un archivo ZIP.

## <a name="remove-previous-requests"></a>Eliminación de solicitudes anteriores

El servidor mantendrá hasta **20 000** solicitudes para cada cuenta de suscripción de Azure. Si la cantidad de solicitudes supera esta limitación, elimine las solicitudes anteriores antes de realizar otras nuevas. Si no quita las solicitudes existentes, recibirá una notificación de error.

Agregue el código a `voice_synthesis_client.py`:

```python
parser.add_argument('--syntheses', action="store_true", default=False, help='print synthesis list')
parser.add_argument('--delete', action="store_true", default=False, help='delete a synthesis request')
parser.add_argument('-synthesisId', action="store", nargs='+', dest="synthesisId", help='the id of the voice synthesis which need to be deleted')

def getSubmittedSyntheses():
    response=requests.get(baseAddress+"voicesynthesis", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    syntheses = json.loads(response.text)
    return syntheses

def deleteSynthesis(ids):
    for id in ids:
        print("delete voice synthesis %s " % id)
        response = requests.delete(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
        if (response.status_code == 204):
            print("delete successful")
        else:
            print("delete failed, response.status_code: %d, response.text: %s " % (response.status_code, response.text))

if args.syntheses:
    synthese = getSubmittedSyntheses()
    print("There are %d synthesis requests submitted:" % len(synthese))
    for synthesis in synthese:
        print ("ID : %s , Name : %s, Status : %s " % (synthesis['id'], synthesis['name'], synthesis['status']))

if args.delete:
    deleteSynthesis(args.synthesisId)
```

### <a name="test-your-code"></a>Prueba del código

Ahora, se comprobará qué solicitudes ha enviado anteriormente. Antes de continuar, tendrá que actualizar algunos aspectos en esta solicitud:

* Reemplace `<your_key>` por la clave de suscripción del servicio Speech. Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).
* Reemplace `<region>` por la región donde se creó el recurso de Voz (por ejemplo: `eastus` o `westus`). Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).

Ejecute este comando:

```console
python voice_synthesis_client.py --syntheses -key <your_key> -region <Region>
```

Esto devolverá una lista de las solicitudes de síntesis realizadas. Verá una salida similar a la siguiente:

```console
There are <number> synthesis requests submitted:
ID : xxx , Name : xxx, Status : Succeeded
ID : xxx , Name : xxx, Status : Running
ID : xxx , Name : xxx : Succeeded
```

Ahora, se quitará una solicitud enviada previamente. Tendrá que actualizar algunos aspectos en el código siguiente:

* Reemplace `<your_key>` por la clave de suscripción del servicio Speech. Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).
* Reemplace `<region>` por la región donde se creó el recurso de Voz (por ejemplo: `eastus` o `westus`). Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).
* Reemplace `<synthesis_id>` por el valor devuelto en la solicitud anterior.

> [!NOTE]
> Las solicitudes con el estado "En ejecución" o "En espera" no se pueden quitar ni eliminar.

Ejecute este comando:

```console
python voice_synthesis_client.py --delete -key <your_key> -region <Region> -synthesisId <synthesis_id>
```

Verá una salida similar a la siguiente:

```console
delete voice synthesis xxx
delete successful
```

## <a name="get-the-full-client"></a>Obtención del cliente completo

El archivo `voice_synthesis_client.py` completo está disponible para descargar en [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Python/voiceclient.py).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre Long Audio API](../../long-audio-api.md)
