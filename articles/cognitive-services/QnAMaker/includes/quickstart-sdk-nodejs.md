---
title: 'Inicio rápido: Biblioteca cliente de QnA Maker para Node.js'
description: Este inicio rápido le muestra cómo empezar a trabajar con la biblioteca cliente de QnA Maker para Node.js.
ms.topic: quickstart
ms.date: 01/13/2020
ms.openlocfilehash: 05e6d2c77a351c22c73f0bdb54daeaf0e03f4a9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76021039"
---
Use la biblioteca cliente de QnA Maker para Node.js con los siguientes fines:

* Creación de una base de conocimientos
* Actualización de una base de conocimientos
* Publicación de una base de conocimientos
* Obtención de la clave de un punto de conexión publicado
* Espera de una tarea de ejecución prolongada
* Eliminación de una base de conocimiento

[Documentación de referencia](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/?view=azure-node-latest) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker) | [Paquete (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker) | [Ejemplos de Node.js](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* La versión actual de [Node.js](https://nodejs.org).

## <a name="setting-up"></a>Instalación

### <a name="create-a-qna-maker-azure-resource"></a>Creación de un recurso de Azure en QnA Maker

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para QnA Maker mediante [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local.

Después de obtener la clave y el punto de conexión del recurso, obtenga los valores de Azure Portal para el nuevo recurso en la página de inicio rápido.

[Cree variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) denominadas `QNAMAKER_AUTHORING_KEY` y `QNAMAKER_ENDPOINT`. Puede copiar el archivo [.env.sample](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/.env.sample) en `.env` y usar las variables de entorno de ese archivo.

### <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él.

```console
mkdir qnamaker_quickstart && cd qnamaker_quickstart
```

Ejecute el comando `npm init -y` para crear una aplicación de nodo con un archivo `package.json`.

```console
npm init -y
```

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Instale los paquetes obligatorios y opcionales de NPM:

```console
npm install @azure/cognitiveservices-qnamaker @azure/ms-rest-js dotenv
```

El archivo `package.json` de la aplicación se actualiza con las dependencias. `dotenv` es opcional y se usa para que pueda establecer las variables de entorno en un archivo de texto. No seleccione `.env` en el control de código fuente.


## <a name="object-model"></a>Modelo de objetos

El cliente de QnA Maker es un objeto [QnAMakerClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest) que se autentica en Azure mediante ServiceClientCredentials, que contiene la clave.

Una vez creado el cliente, utilice la propiedad [Knowledge base](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#knowledgebase) para crear, administrar y publicar la base de conocimiento.

Administre la base de conocimiento mediante el envío de un objeto JSON. En el caso de operaciones inmediatas, un método suele devolver un objeto JSON que indica el estado. En el caso de operaciones de ejecución prolongada, la respuesta es el identificador de la operación. Llame al método [client.Operations.getDetails](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest#getdetails-string--servicecallback-operation--) con el identificador de la operación para determinar el [estado de la solicitud](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operationstatetype?view=azure-node-latest).


## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes acciones con la biblioteca cliente de QnA Maker para Node.js:

* [Creación de una base de conocimientos](#create-a-knowledge-base)
* [Actualización de una base de conocimientos](#update-a-knowledge-base)
* [Publicación de una base de conocimientos](#publish-a-knowledge-base)
* [Eliminación de una base de conocimiento](#delete-a-knowledge-base)
* [Obtención de un punto de conexión publicado](#get-published-endpoint)
* [Obtención del estado de una operación](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Adición de las dependencias

Cree un archivo llamado `index.js`. Agregue la biblioteca de QnA Maker y las dependencias al archivo.

[!code-javascript[Require statements](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=dependencies)]

Cree variables para el punto de conexión y la clave de Azure del recurso. Si ha creado la variable de entorno después de haber iniciado la aplicación, deberá cerrar y volver a abrir el editor, el IDE o el shell que lo ejecuta para acceder a la variable.

|Variable de entorno|Variable de Node.js|Ejemplo|
|--|--|--|
|`QNAMAKER_AUTHORING_KEY`|`authoring_key`|La clave es una cadena de 32 caracteres y está disponible en Azure Portal, en el recurso de QnA Maker, en la página de inicio rápido. Esta clave no es la misma que la clave de punto de conexión de predicción.|
|`QNAMAKER_ENDPOINT`|`endpoint`| El punto de conexión de creación, con el formato `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`, incluye el **nombre del recurso**. Esta no es la misma dirección URL que se utiliza para consultar el punto de conexión de predicción.|
||||

[!code-javascript[Azure resource variables](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=resourceSettings)]

## <a name="authenticate-the-client"></a>Autenticar el cliente

A continuación, cree un objeto ApiKeyCredentials con la clave y úselo con el punto de conexión para crear un objeto [QnAMakerClient](https://docs.microsoft.com/javascript/api/%40azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#qnamakerclient-serviceclientcredentials--string--msrest-serviceclientoptions-). Use el objeto de cliente para obtener un objeto de [cliente de base de conocimiento](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest).


[!code-javascript[Authorization to resource key](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=authorization)]

## <a name="create-a-knowledge-base"></a>Creación de una base de conocimientos

Una base de conocimiento almacena pares de preguntas y respuestas para el objeto [CreateKbDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/createkbdto?view=azure-node-latest) procedentes de tres orígenes:

* Para **contenido editorial**, use el objeto [QnADTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnadto?view=azure-node-latest).
* Para **archivos**, use el objeto [FileDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/filedto?view=azure-node-latest).
* Para **direcciones URL**, use una lista de cadenas.

Llame al método [create](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#create-createkbdto--servicecallback-operation--) con la información de la base de conocimiento. La información de la base de conocimiento es básicamente un objeto JSON.

Cuando el método create realice una devolución, pase el identificador de la operación devuelta al método [wait_for_operation](#get-status-of-an-operation) para sondear el estado. El método wait_for_operation devuelve una salida cuando finaliza la operación. Analice el valor del encabezado `resourceLocation` de la operación devuelta para obtener el nuevo identificador de la base de conocimiento.

[!code-javascript[Create a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=createKnowledgeBase&highlight=15,30)]

Asegúrese de incluir la función [`wait_for_operation`](#get-status-of-an-operation), a la que se hace referencia en el código anterior, con el fin de crear correctamente una base de conocimiento.

## <a name="update-a-knowledge-base"></a>Actualización de una base de conocimientos

Para actualizar una base de conocimiento, pase el identificador de la base de conocimiento y un objeto [UpdateKbOperationDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest) que contenga los objetos de DTO [add](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#add), [update](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#update) y [delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#deleteproperty) al método [update](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#update-string--updatekboperationdto--msrest-requestoptionsbase-). Los DTO son también básicamente objetos JSON. Use el método [wait_for_operation](#get-status-of-an-operation) para determinar si la actualización se realizó correctamente.

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=updateKnowledgeBase&highlight=28)]

Asegúrese de incluir la función [`wait_for_operation`](#get-status-of-an-operation), a la que se hace referencia en el código anterior, con el fin de actualizar correctamente una base de conocimiento.

## <a name="publish-a-knowledge-base"></a>Publicación de una base de conocimientos

Publique la base de conocimiento mediante el método [publish](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#publish-string--msrest-requestoptionsbase-). Esto toma el modelo actual guardado y entrenado, al que hace referencia el identificador de la base de conocimiento, y lo publica en un punto de conexión. Compruebe el código de respuesta HTTP para validar que la publicación se realizó correctamente.

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=publishKnowledgeBase&highlight=3)]


## <a name="get-published-endpoint"></a>Obtención de un punto de conexión publicado

Una vez publicada la base de conocimiento, el acceso a la base de conocimiento publicada se realiza a través de la API generateAnswer del entorno de ejecución de la predicción de consultas. Para ello, necesita la clave del punto de conexión del entorno de ejecución. Esta es diferente a la clave de creación.

[!code-javascript[Get endpoint keys](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=getEndpointKeys&highlight=3)]

Se devuelven dos claves de punto de conexión desde la llamada. Solo se necesita una para acceder al punto de conexión en tiempo de ejecución.

## <a name="delete-a-knowledge-base"></a>Eliminación de una base de conocimiento

Elimine la base de conocimiento con el método [delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#deletemethod-string--msrest-requestoptionsbase-) y el parámetro del identificador de la base de conocimiento.

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=deleteKnowledgeBase&highlight=3)]

## <a name="get-status-of-an-operation"></a>Obtención del estado de una operación

Algunos métodos, como Create y Update, pueden tardar bastante tiempo en que en lugar de esperar a que finalice el proceso, se devuelva una [operación](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest). Use el [identificador](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operation?view=azure-node-latest#operationid) de la operación para realizar un sondeo (con lógica de reintento) para determinar el estado del método original.

La llamada _delayTimer_ del siguiente bloque de código se usa para simular la lógica de reintentos. Reemplácela por su propia lógica de reintentos.

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=monitorOperation&highlight=8,13)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `node index.js` desde el directorio de la aplicación.

Todos los fragmentos de código de este artículo están [disponibles](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js) y se pueden ejecutar como un solo archivo.

```console
node index.js
```

El programa imprime el estado en la consola:

```console
qnamaker_quickstart@1.0.0 start C:\samples\cognitive-services-quickstart-code\javascript\QnAMaker\sdk> node index.js

Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Succeeded
Create operation 200, KB ID 99df758d-f23f-4931-ab83-e738fe978e69
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Succeeded
Update operation state 200 - HTTP status 200
Publish request succeeded - HTTP status 204
GetEndpointKeys request succeeded - HTTP status 200 - primary key 8482830b-681e-400e-b8a3-4016278aba64
QnA Maker FAQ stored in English language with 1 sources, last updated 2020-01-12T16:54:40Z
New KB name stored in English language with 1 sources, last updated 2020-01-12T17:32:16Z
New KB name stored in English language with 1 sources, last updated 2020-01-13T00:27:46Z
Delete operation state succeeded - HTTP status 204
done
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)