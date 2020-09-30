---
title: Obtención del modelo con la llamada de REST en Go
titleSuffix: Azure Cognitive Services
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 06/03/2020
ms.openlocfilehash: 0db01cbd281b2060c1e08e6c44202624faefc5d7
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91534660"
---
[Documentación de referencia](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c45) | [Ejemplo](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/LUIS/go-rest-model/model.go)

## <a name="prerequisites"></a>Requisitos previos

* Lenguaje de programación [Go](https://golang.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="create-pizza-app"></a>Creación de una aplicación de pizza

[!INCLUDE [Create pizza app](get-started-get-model-create-pizza-app.md)]

## <a name="change-model-programmatically"></a>Cambio de modelo mediante programación

1. Cree un nuevo archivo llamado `predict.go`. Agregue el siguiente código:

    [!code-go[Code snippet](~/cognitive-services-quickstart-code/go/LUIS/go-rest-model/model.go)]

1. Reemplace los valores a partir de `YOUR-` por sus propios valores.

    |Información|Propósito|
    |--|--|
    |`YOUR-APP-ID`| El identificador de la aplicación de LUIS. |
    |`YOUR-AUTHORING-KEY`|La clave de creación de 32 caracteres.|
    |`YOUR-AUTHORING-ENDPOINT`| El punto de conexión de la dirección URL de creación. Por ejemplo, `https://replace-with-your-resource-name.api.cognitive.microsoft.com/`. El nombre del recurso se establece al crear el recurso.|

    Las claves y recursos asignados son visibles en el portal de LUIS, en la sección Administrar de la página **Recursos de Azure**. El identificador de la aplicación está disponible en la misma sección Administrar, en la página **Configuración de la aplicación**.

1. Con un símbolo del sistema en el mismo directorio que donde creó el archivo, escriba el siguiente comando para compilar el archivo de Go:

    ```console
    go build model.go
    ```

1. En el símbolo del sistema, escriba el siguiente texto para ejecutar la aplicación de Go desde la línea de comandos:

    ```console
    go run model.go
    ```

1. Revise la respuesta de creación:

    ```console
    add example utterances requested
    body
    [{'text': 'order a pizza', 'intentName': 'ModifyOrder', 'entityLabels': [{'entityName': 'Order', 'startCharIndex': 6, 'endCharIndex': 12}]}, {'text': 'order a large pepperoni pizza', 'intentName': 'ModifyOrder', 'entityLabels': [{'entityName': 'Order', 'startCharIndex': 6, 'endCharIndex': 28}, {'entityName': 'FullPizzaWithModifiers', 'startCharIndex': 6, 'endCharIndex': 28}, {'entityName': 'PizzaType', 'startCharIndex': 14, 'endCharIndex': 28}, {'entityName': 'Size', 'startCharIndex': 8, 'endCharIndex': 12}]}, {'text': 'I want two large pepperoni pizzas on thin crust', 'intentName': 'ModifyOrder', 'entityLabels': [{'entityName': 'Order', 'startCharIndex': 7, 'endCharIndex': 46}, {'entityName': 'FullPizzaWithModifiers', 'startCharIndex': 7, 'endCharIndex': 46}, {'entityName': 'PizzaType', 'startCharIndex': 17, 'endCharIndex': 32}, {'entityName': 'Size', 'startCharIndex': 11, 'endCharIndex': 15}, {'entityName': 'Quantity', 'startCharIndex': 7, 'endCharIndex': 9}, {'entityName': 'Crust', 'startCharIndex': 37, 'endCharIndex': 46}]}]
        201
    [{"value":{"ExampleId":1137150691,"UtteranceText":"order a pizza"},"hasError":false},{"value":{"ExampleId":1137150692,"UtteranceText":"order a large pepperoni pizza"},"hasError":false},{"value":{"ExampleId":1137150693,"UtteranceText":"i want two large pepperoni pizzas on thin crust"},"hasError":false}]
    training selected
    body

        202
    {"statusId":9,"status":"Queued"}
    training status selected
    body

        200
    [{"modelId":"edb46abf-0000-41ab-beb2-a41a0fe1630f","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"a5030be2-616c-4648-bf2f-380fa9417d37","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"3f2b1f31-a3c3-4fbd-8182-e9d9dbc120b9","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"e4b6704b-1636-474c-9459-fe9ccbeba51c","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"031d3777-2a00-4a7a-9323-9a3280a30000","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"9250e7a1-06eb-4413-9432-ae132ed32583","details":{"statusId":9,"status":"Queued","exampleCount":0}}]
    ```

    Este es el resultado con formato para mejorar la legibilidad:

    ```json
    add example utterances requested
    body
    [
      {
        'text': 'order a pizza',
        'intentName': 'ModifyOrder',
        'entityLabels': [
          {
            'entityName': 'Order',
            'startCharIndex': 6,
            'endCharIndex': 12
          }
        ]
      },
      {
        'text': 'order a large pepperoni pizza',
        'intentName': 'ModifyOrder',
        'entityLabels': [
          {
            'entityName': 'Order',
            'startCharIndex': 6,
            'endCharIndex': 28
          },
          {
            'entityName': 'FullPizzaWithModifiers',
            'startCharIndex': 6,
            'endCharIndex': 28
          },
          {
            'entityName': 'PizzaType',
            'startCharIndex': 14,
            'endCharIndex': 28
          },
          {
            'entityName': 'Size',
            'startCharIndex': 8,
            'endCharIndex': 12
          }
        ]
      },
      {
        'text': 'I want two large pepperoni pizzas on thin crust',
        'intentName': 'ModifyOrder',
        'entityLabels': [
          {
            'entityName': 'Order',
            'startCharIndex': 7,
            'endCharIndex': 46
          },
          {
            'entityName': 'FullPizzaWithModifiers',
            'startCharIndex': 7,
            'endCharIndex': 46
          },
          {
            'entityName': 'PizzaType',
            'startCharIndex': 17,
            'endCharIndex': 32
          },
          {
            'entityName': 'Size',
            'startCharIndex': 11,
            'endCharIndex': 15
          },
          {
            'entityName': 'Quantity',
            'startCharIndex': 7,
            'endCharIndex': 9
          },
          {
            'entityName': 'Crust',
            'startCharIndex': 37,
            'endCharIndex': 46
          }
        ]
      }
    ]

        201
    [
      {
        "value": {
          "ExampleId": 1137150691,
          "UtteranceText": "order a pizza"
        },
        "hasError": false
      },
      {
        "value": {
          "ExampleId": 1137150692,
          "UtteranceText": "order a large pepperoni pizza"
        },
        "hasError": false
      },
      {
        "value": {
          "ExampleId": 1137150693,
          "UtteranceText": "i want two large pepperoni pizzas on thin crust"
        },
        "hasError": false
      }
    ]
    training selected
    body

        202
    {
      "statusId": 9,
      "status": "Queued"
    }
    training status selected
    body

        200
    [
      {
        "modelId": "edb46abf-0000-41ab-beb2-a41a0fe1630f",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "a5030be2-616c-4648-bf2f-380fa9417d37",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "3f2b1f31-a3c3-4fbd-8182-e9d9dbc120b9",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "e4b6704b-1636-474c-9459-fe9ccbeba51c",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "031d3777-2a00-4a7a-9323-9a3280a30000",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "9250e7a1-06eb-4413-9432-ae132ed32583",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      }
    ]
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para una aplicación](../luis-concept-best-practices.md)