---
title: 'Inicio rápido: Uso de cURL y REST para administrar la base de conocimiento: QnA Maker'
description: En este inicio rápido se muestra cómo crear, publicar y consultar la base de conocimiento mediante las API REST.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 04/13/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: quickstart
ms.openlocfilehash: 11a7bd8655d1b5606c3b53ed78e796bc42f85b2e
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91777426"
---
# <a name="quickstart-use-curl-and-rest-to-manage-knowledge-base"></a>Inicio rápido: Uso de cURL y REST para administrar la base de conocimiento

En este inicio rápido se explica cómo crear, publicar y consultar la base de conocimiento. QnA Maker extrae automáticamente preguntas y respuestas a partir de contenido semiestructurado, como las preguntas frecuentes, y de [orígenes de datos](../Concepts/knowledge-base.md). El modelo para la base de conocimiento se define en el JSON que se envía en el cuerpo de la solicitud de API.

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

* La versión actual de [cURL](https://curl.haxx.se/). En las guías de inicio rápido se usan varios modificadores de línea de comandos, que se indican en la [documentación de cURL](https://curl.haxx.se/docs/manpage.html).
* Para usar la clave y el nombre del recurso, debe tener un [recurso de QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Ha especificado el **nombre** del recurso durante su creación; la clave se creó automáticamente. El nombre del recurso se usa como subdominio personalizado para el punto de conexión. Para recuperar la clave y el nombre del recurso, seleccione **Inicio rápido** para el recurso en Azure Portal. El nombre del recurso es el primer subdominio de la dirección URL del punto de conexión:

    `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0`

> [!CAUTION]
> En los siguientes ejemplos de BASH se usa el carácter de continuación de línea `\`. Si la consola o el terminal usan un carácter de continuación de línea diferente, use este carácter.

## <a name="create-a-knowledge-base"></a>Creación de una base de conocimientos

Para crear una base de conocimiento con las API REST y cURL, debe tener la siguiente información:

|Information|Configuración de cURL|Propósito|
|--|--|--|
|Nombre del recurso de QnA Maker|URL|Se usa para construir la dirección URL|
|Clave del recurso de QnA Maker|Parámetro `-h` para el encabezado `Ocp-Apim-Subscription-Key`|Autenticación en el servicio QnA Maker|
|JSON que describe la base de conocimiento|Parámetro `-d`|[Ejemplos](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create#examples) de JSON|
|Tamaño del archivo JSON en bytes|Parámetro `-h` para el encabezado `Content-Size`||

El comando de cURL se ejecuta desde un shell de BASH. Edite este comando con su propio nombre de recurso, clave de recurso y valores y tamaño de JSON.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/create \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:107" \
-d '{ name: "QnA Maker FAQ",urls: [ "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs"]}'
```

La respuesta de cURL desde QnA Maker incluye el `operationId`, que es necesario para [obtener el estado de la operación](#get-status-of-operation).

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2020-02-27T04:11:22Z",
  "lastActionTimestamp": "2020-02-27T04:11:22Z",
  "userId": "9596077b3e0441eb93d5080d6a15c64b",
  "operationId": "95a4f700-9899-4c98-bda8-5449af9faef8"
}
```

## <a name="get-status-of-operation"></a>Obtención del estado de la operación

Al crear una base de conocimiento, dado que la operación es asincrónica, la respuesta incluye información para determinar el estado.

|Information|Configuración de cURL|Propósito|
|--|--|--|
|Nombre del recurso de QnA Maker|URL|Se usa para construir la dirección URL|
|Identificador de la operación|Ruta de dirección URL|`/operations/REPLACE-WITH-YOUR-OPERATION-ID`|
|Clave del recurso de QnA Maker|Parámetro `-h` para el encabezado `Ocp-Apim-Subscription-Key`|Autenticación en el servicio QnA Maker|

El comando de cURL se ejecuta desde un shell de BASH. Edite este comando con su propio nombre de recurso, clave de recurso e identificador de operación.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/operations/REPLACE-WITH-YOUR-OPERATION-ID \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

La respuesta de cURL incluye el estado. Si el estado de la operación es correcto, `resourceLocation` incluye el identificador de la base de conocimiento.

```json
{
   "operationState": "Succeeded",
   "createdTimestamp": "2020-02-27T04:54:07Z",
   "lastActionTimestamp": "2020-02-27T04:54:19Z",
   "resourceLocation": "/knowledgebases/fe3971b7-cfaa-41fa-8d9f-6ceb673eb865",
   "userId": "f596077b3e0441eb93d5080d6a15c64b",
   "operationId": "f293f218-d080-48f0-a766-47993e9b26a8"
}
```

## <a name="publish-knowledge-base"></a>Publicación de una base de conocimiento

Antes de consultar la base de conocimiento, debe hacer lo siguiente:
* Publicación de una base de conocimiento
* Obtención de la clave de punto de conexión en tiempo de ejecución

Esta tarea publica la base de conocimiento. La obtención de la clave de punto de conexión en tiempo de ejecución es una [tarea independiente](#get-published-knowledge-bases-runtime-endpoint-key).

|Information|Configuración de cURL|Propósito|
|--|--|--|
|Nombre del recurso de QnA Maker|URL|Se usa para construir la dirección URL|
|Clave del recurso de QnA Maker|Parámetro `-h` para el encabezado `Ocp-Apim-Subscription-Key`|Autenticación en el servicio QnA Maker|
|Identificador de base de conocimiento|Ruta de dirección URL|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

El comando de cURL se ejecuta desde un shell de BASH. Edite este comando con su propio nombre de recurso, clave de recurso e identificador de base de conocimiento.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-v \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
--data-raw ''
```

El estado de la respuesta es 204 sin resultados. Use el parámetro de línea de comandos `-v` para ver resultados detallados del comando de cURL. Esto incluirá el estado de HTTP.

## <a name="get-published-knowledge-bases-runtime-endpoint-key"></a>Obtención de la clave de punto de conexión en tiempo de ejecución de la base de conocimiento publicada

Antes de consultar la base de conocimiento, debe hacer lo siguiente:
* Publicación de una base de conocimiento
* Obtención de la clave de punto de conexión en tiempo de ejecución

Esta tarea obtiene la clave de punto de conexión en tiempo de ejecución. La publicación de la base de conocimiento es una [tarea independiente](#publish-knowledge-base).

La clave de punto de conexión en tiempo de ejecución es la misma clave para todas las bases de conocimiento que utilizan el recurso de QnA Maker.

|Information|Configuración de cURL|Propósito|
|--|--|--|
|Nombre del recurso de QnA Maker|URL|Se usa para construir la dirección URL|
|Clave del recurso de QnA Maker|Parámetro `-h` para el encabezado `Ocp-Apim-Subscription-Key`|Autenticación en el servicio QnA Maker|

El comando de cURL se ejecuta desde un shell de BASH. Edite este comando con su propio nombre de recurso y clave de recurso.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/endpointkeys \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```


La respuesta de cURL incluye las claves de punto de conexión en tiempo de ejecución. Use solo una de las claves al consultar para obtener una respuesta de la base de conocimiento.

```json
{
  "primaryEndpointKey": "93e88a14-694a-44d5-883b-184a68aa8530",
  "secondaryEndpointKey": "92c98c16-ca31-4294-8626-6c57454a5063",
  "installedVersion": "4.0.5",
  "lastStableVersion": "4.0.6"
}
```

## <a name="query-for-answer-from-published-knowledge-base"></a>Consulta para obtener respuesta de la base de conocimiento publicada

La obtención de una respuesta de la base de conocimiento se realiza desde un tiempo de ejecución independiente de la administración de la base de conocimiento. Dado que se trata de un entorno de ejecución independiente, debe autenticarse con una clave de tiempo de ejecución.

|Information|Configuración de cURL|Propósito|
|--|--|--|
|Nombre del recurso de QnA Maker|URL|Se usa para construir la dirección URL|
|Clave de tiempo de ejecución de QnA Maker|Parámetro `-h` para el encabezado `Authorization`|La clave forma parte de una cadena que incluye la palabra `Endpointkey `. Autenticación en el servicio QnA Maker|
|Identificador de base de conocimiento|Ruta de dirección URL|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|
|JSON que describe la consulta|Parámetro `-d`|[Parámetros del cuerpo de la solicitud](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#request-body) y [ejemplos](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#examples) de JSON|
|Tamaño del archivo JSON en bytes|Parámetro `-h` para el encabezado `Content-Size`||

El comando de cURL se ejecuta desde un shell de BASH. Edite este comando con su propio nombre de recurso, clave de recurso e identificador de base de conocimiento.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.azurewebsites.net/qnamaker/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID/generateAnswer \
-X POST \
-H "Authorization: EndpointKey REPLACE-WITH-YOUR-RUNTIME-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:159" \
-d '{"question": "How are QnA Maker and LUIS used together?","top": 6,"isTest": true,  "scoreThreshold": 20, "strictFilters": [], "userId": "sd53lsY="}'
```

Una respuesta correcta incluye la respuesta principal junto con otra información que una aplicación cliente, como un bot de chat, necesita para mostrar una respuesta al usuario.

## <a name="delete-knowledge-base"></a>Eliminación de una base de conocimiento

Cuando haya terminado de usar la base de conocimiento, elimínela.

|Information|Configuración de cURL|Propósito|
|--|--|--|
|Nombre del recurso de QnA Maker|URL|Se usa para construir la dirección URL|
|Clave del recurso de QnA Maker|Parámetro `-h` para el encabezado `Ocp-Apim-Subscription-Key`|Autenticación en el servicio QnA Maker|
|Identificador de base de conocimiento|Ruta de dirección URL|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

El comando de cURL se ejecuta desde un shell de BASH. Edite este comando con su propio nombre de recurso, clave de recurso e identificador de base de conocimiento.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-X DELETE \
-v \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

El estado de la respuesta es 204 sin resultados. Use el parámetro de línea de comandos `-v` para ver resultados detallados del comando de cURL. Esto incluirá el estado de HTTP.

## <a name="additional-resources"></a>Recursos adicionales

* Documentación de referencia de [creación](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* Documentación de referencia del [entorno de ejecución](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/)
* [Scripts de BASH de ejemplo con cURL](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/curl/QnAMaker)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de QnA Maker (V4) REST API](https://go.microsoft.com/fwlink/?linkid=2092179)
