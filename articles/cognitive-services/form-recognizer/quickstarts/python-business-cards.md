---
title: 'Inicio rápido: Extracción de datos de tarjetas de presentación con Form Recognizer en Python'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, usará la API REST Form Recognizer con Python para extraer datos de imágenes de tarjetas de presentación en inglés.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 08/17/2020
ms.author: pafarley
ms.custom: devx-track-python
ms.openlocfilehash: 8132358dcd0ad9d87dc6687afd2adef1942f3b67
ms.sourcegitcommit: ac7ae29773faaa6b1f7836868565517cd48561b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2020
ms.locfileid: "88823911"
---
# <a name="quickstart-extract-business-card-data-using-the-form-recognizer-rest-api-with-python"></a>Inicio rápido: Extracción de datos de tarjetas de presentación mediante la API REST Form Recognizer con Python

En este inicio rápido, usará la API REST Azure Form Recognizer con Python para extraer e identificar información pertinente de las tarjetas de presentación.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido, debe cumplir los siguientes requisitos:
- [Python](https://www.python.org/downloads/) instalado (si desea ejecutar el ejemplo localmente).
- Imagen de una tarjeta de presentación. En este inicio rápido, puede usar una [imagen de ejemplo](../media/business-card-english.jpg).

> [!NOTE]
> En esta guía de inicio rápido se usa un archivo local. Para usar una imagen de tarjeta de presentación remota a la que se tiene acceso mediante una dirección URL, consulte la [documentación de referencia](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeReceiptAsync).

## <a name="create-a-form-recognizer-resource"></a>Creación de un recurso de Form Recognizer

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-a-business-card"></a>Análisis de una tarjeta de presentación

Para empezar a analizar una tarjeta de presentación, llame a la API **[Analyze Business Card](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeReceiptAsync)** mediante el siguiente script de Python. Antes de ejecutar el script, realice estos cambios:

1. Reemplace `<Endpoint>` por el punto de conexión que obtuvo con la suscripción de Form Recognizer.
1. Reemplace `<path to your business card>` por la ruta de acceso al documento de formulario local.
1. Reemplace `<subscription key>` con la clave de suscripción que copió en el paso anterior.

    ```python
    ########### Python Form Recognizer Async Business cards #############

    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<Endpoint>"
    apim_key = "<subscription key>"
    post_url = endpoint + "/formrecognizer/v2.1-preview.1/prebuilt/businessCard/analyze"
    source = r"<path to your business card>"
    
    headers = {
        # Request headers
        'Content-Type': '<file type>',
        'Ocp-Apim-Subscription-Key': apim_key,
    }
    
    params = {
        "includeTextDetails": True
    }
    
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % resp.text)
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit()
    ```

1. Guarde el código en un archivo con una extensión .py. Por ejemplo, *form-recognizer-businesscards.py*.
1. Abra una ventana de símbolo del sistema.
1. En el símbolo del sistema, utilice el comando `python` para ejecutar el ejemplo. Por ejemplo, `python form-recognizer-businesscards.py`.

Recibirá una respuesta `202 (Success)` que incluye un encabezado **Operation-Location**, que el script imprimirá en la consola. Este encabezado contiene un identificador de operación que puede usar para consultar el estado de la operación asincrónica y obtener los resultados. En el siguiente valor de ejemplo, la cadena después de `operations/` es el identificador de operación.

```console
https://cognitiveservice/formrecognizer/v2.1-preview.1/prebuilt/businessCard/analyzeresults/54f0b076-4e38-43e5-81bd-b85b8835fdfb
```

## <a name="get-the-business-card-results"></a>Obtención de los resultados de la tarjeta de presentación

Tras la llamada a la API **Analyze Business Card**, llame a la API **[Get Analyze Business Card Result](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetAnalyzeReceiptResult)** para obtener el estado de la operación y los datos extraídos. Agregue el código siguiente al final del script de Python. Este código usa el valor de identificador de la operación en una nueva llamada API. El script llama a la API a intervalos regulares hasta que los resultados están disponibles. Se recomienda un intervalo de uno o varios segundos.

```python
n_tries = 10
n_try = 0
wait_sec = 6
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
        resp_json = json.loads(resp.text)
        if resp.status_code != 200:
            print("GET Business card results failed:\n%s" % resp_json)
            quit()
        status = resp_json["status"]
        if status == "succeeded":
            print("Business card Analysis succeeded:\n%s" % resp_json)
            quit()
        if status == "failed":
            print("Analysis failed:\n%s" % resp_json)
            quit()
        # Analysis still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1     
    except Exception as e:
        msg = "GET analyze results failed:\n%s" % str(e)
        print(msg)
        quit()
```

1. Guarde el script.
1. De nuevo, use el comando `python` para ejecutar el ejemplo. Por ejemplo, `python form-recognizer-businesscards.py`.

### <a name="examine-the-response"></a>Examen de la respuesta
![Una tarjeta de presentación de la compañía Contoso](../media/business-card-english.jpg)

Este ejemplo muestra la salida JSON que Form Recognizer devuelve. Estos ejemplos se han truncado para facilitar la lectura.

```json
{
    "status": "succeeded",
    "createdDateTime": "2020-06-04T08:19:29Z",
    "lastUpdatedDateTime": "2020-06-04T08:19:35Z",
    "analyzeResult": {
        "version": "2.1.1",
        "readResults": [
            {
                "page": 1,
                "angle": -17.0956,
                "width": 4032,
                "height": 3024,
                "unit": "pixel"
            }
        ],
        "documentResults": [
            {
                "docType": "prebuilt:businesscard",
                "pageRange": [
                    1,
                    1
                ],
                "fields": {
                    "ContactNames": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "object",
                                "valueObject": {
                                    "FirstName": {
                                        "type": "string",
                                        "valueString": "Avery",
                                        "text": "Avery",
                                        "boundingBox": [
                                            703,
                                            1096,
                                            1134,
                                            989,
                                            1165,
                                            1109,
                                            733,
                                            1206
                                        ],
                                        "page": 1
                                },
                                "text": "Dr. Avery Smith",
                                "boundingBox": [
                                    419.3,
                                    1154.6,
                                    1589.6,
                                    877.9,
                                    1618.9,
                                    1001.7,
                                    448.6,
                                    1278.4
                                ],
                                "confidence": 0.993
                            }
                        ]
                    },
                    "Emails": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "avery.smith@contoso.com",
                                "text": "avery.smith@contoso.com",
                                "boundingBox": [
                                    2107,
                                    934,
                                    2917,
                                    696,
                                    2935,
                                    764,
                                    2126,
                                    995
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Websites": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "https://www.contoso.com/",
                                "text": "https://www.contoso.com/",
                                "boundingBox": [
                                    2121,
                                    1002,
                                    2992,
                                    755,
                                    3014,
                                    826,
                                    2143,
                                    1077
                                ],
                                "page": 1,
                                "confidence": 0.995
                            }
                        ]
                    }
                }
            }
        ]
    }
}
```

El script imprimirá las respuestas en la consola hasta que se complete la operación de **Analyze Business Card**. El nodo `"readResults"` contiene todo el texto reconocido. El texto se organiza por página, después, por líneas y, finalmente, por palabras individuales. El nodo `"documentResults"` contiene los valores específicos de la tarjeta de presentción que el modelo haya descubierto. Aquí encontrará pares clave-valor útiles como el nombre de la empresa, el nombre, el apellido, el teléfono, etc.


## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, usó la API REST de Form Recognizer con Python para extraer el contenido de una tarjeta de presención. A continuación, consulte la documentación de referencia para explorar la API de Form Recognizer con mayor profundidad.

> [!div class="nextstepaction"]
> [Documentación de referencia de API REST](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeReceiptAsync)
