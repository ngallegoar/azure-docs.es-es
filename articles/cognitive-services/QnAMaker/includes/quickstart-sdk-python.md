---
ms.openlocfilehash: 13dcdec4eff72f97e1af8ecb26e32c0ef5f881fd
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81637067"
---

Use la biblioteca cliente de QnA Maker para Python para:

* Creación de una base de conocimientos
* Administrar una base de conocimiento
* Publicación de una base de conocimientos

[Documentación de referencia](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker?view=azure-python) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-knowledge-qnamaker) | [Paquete (pypi)](https://pypi.org/project/azure-cognitiveservices-knowledge-qnamaker/) | [Ejemplos de Python](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Instalación

### <a name="create-a-qna-maker-azure-resource"></a>Creación de un recurso de Azure en QnA Maker

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para QnA Maker mediante [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local.

Después de obtener una clave del recurso, [cree las variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para el recurso, llamadas `QNAMAKER_KEY` y `QNAMAKER_HOST`. Use los valores de clave y de punto de conexión que se encuentran en Azure Portal.

### <a name="install-the-python-library-for-qna-maker"></a>Instalación de la biblioteca de Python para QnA Maker

Después de instalar Python, puede instalar la biblioteca cliente con:

```console
pip install azure-cognitiveservices-knowledge-qnamaker
```

## <a name="object-model"></a>Modelo de objetos

Cree un objeto [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) con la clave y úselo con el punto de conexión para crear un objeto [QnAMakerClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.qn_amaker_client?view=azure-python).

Una vez creado el cliente, utilice la propiedad [Knowledge base](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations?view=azure-python) para crear, administrar y publicar la base de conocimiento.

En el caso de operaciones inmediatas, un método suele devolver un objeto JSON que indica el estado. En el caso de operaciones de ejecución prolongada, la respuesta es el identificador de la operación. Llame al método [client.Operations.getDetails](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.operationstatetype?view=azure-python) con el identificador de la operación para determinar el [estado de la solicitud](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-python).


## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes acciones con la biblioteca cliente de QnA Maker para Python:

* [Creación de una base de conocimientos](#create-a-knowledge-base)
* [Actualización de una base de conocimientos](#update-a-knowledge-base)
* [Publicación de una base de conocimientos](#publish-a-knowledge-base)
* [Descarga de una base de conocimiento](#download-a-knowledge-base)
* [Eliminación de una base de conocimiento](#delete-a-knowledge-base)
* [Obtención del estado de una operación](#get-status-of-an-operation)

## <a name="create-a-new-python-application"></a>Creación de una nueva aplicación de Python

Cree una aplicación de Python en el IDE o editor que prefiera. A continuación, importe las bibliotecas siguientes.

[!code-python[Dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=dependencies)]

Cree variables para el punto de conexión y la clave de Azure del recurso. Si ha creado la variable de entorno después de haber iniciado la aplicación, deberá cerrar y volver a abrir el editor, el IDE o el shell que lo ejecuta para acceder a la variable.

|Variable de entorno|Variable|Ejemplo|
|--|--|--|
|`QNAMAKER_KEY`|`subscription_key`|La clave es una cadena de 32 caracteres y está disponible en Azure Portal, en el recurso de QnA Maker, en la página de inicio rápido. Esta clave no es la misma que la clave de punto de conexión de predicción.|
|`QNAMAKER_HOST`|`host`| El punto de conexión de creación, con el formato `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`, incluye el **nombre del recurso**. Esta no es la misma dirección URL que se utiliza para consultar el punto de conexión de predicción.|

[!code-python[Azure resource variables](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=resourcekeys)]

## <a name="authenticate-the-client"></a>Autenticar el cliente

A continuación, cree un objeto CognitiveServicesCredentials con la clave y úselo con el punto de conexión para crear un objeto [QnAMakerClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-python).


[!code-python[Authorization to resource key](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=authorization)]

## <a name="create-a-knowledge-base"></a>Creación de una base de conocimientos

 Use el objeto de cliente para obtener un objeto de [operaciones de base de conocimiento](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations?view=azure-python).

Una base de conocimiento almacena pares de preguntas y respuestas para el objeto [CreateKbDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.create_kb_dto?view=azure-python) procedentes de tres orígenes:

* Para **contenido editorial**, use el objeto [QnADTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.qn_adto?view=azure-python).
* Para **archivos**, use el objeto [FileDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.file_dto?view=azure-python).
* Para **direcciones URL**, use una lista de cadenas.

Llame al método [create](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#create-create-kb-payload--custom-headers-none--raw-false----operation-config-) y, luego, pase el identificador de operación devuelto al método [Operations.getDetails](#get-status-of-an-operation) para sondear el estado.

[!code-python[Create a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=createkb&highlight=15)]

Asegúrese de incluir la función [`_monitor_operation`](#get-status-of-an-operation), a la que se hace referencia en el código anterior, con el fin de crear correctamente una base de conocimiento.

## <a name="update-a-knowledge-base"></a>Actualización de una base de conocimientos

Para actualizar una base de conocimiento, pase el identificador de la base de conocimiento y un objeto [UpdateKbOperationDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto?view=azure-python) que contenga los objetos de DTO [add](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_add?view=azure-python), [update](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_update?view=azure-python) y [delete](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_delete?view=azure-python) al método [update](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#update-kb-id--update-kb--custom-headers-none--raw-false----operation-config-). Use el método [Operation.getDetail](#get-status-of-an-operation) para determinar si la actualización se realizó correctamente.

[!code-python[Update a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=updatekb&highlight=2)]

Asegúrese de incluir la función [`_monitor_operation`](#get-status-of-an-operation), a la que se hace referencia en el código anterior, con el fin de actualizar correctamente una base de conocimiento.

## <a name="publish-a-knowledge-base"></a>Publicación de una base de conocimientos

Publique la base de conocimiento mediante el método [publish](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#publish-kb-id--custom-headers-none--raw-false----operation-config-). Esto toma el modelo actual guardado y entrenado, al que hace referencia el identificador de la base de conocimiento, y lo publica en un punto de conexión.

[!code-python[Publish a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=publishkb&highlight=2)]

## <a name="download-a-knowledge-base"></a>Descarga de una base de conocimiento

Use el método [download](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#download-kb-id--environment--custom-headers-none--raw-false----operation-config-) para descargar la base de datos como una lista de [QnADocumentsDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.qn_adocuments_dto?view=azure-python). Esto _no_ equivale a las exportaciones del portal de QnA Maker desde la página **Configuración** ya que el resultado de este método no es un archivo TSV.

[!code-python[Download a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=downloadkb&highlight=2)]

## <a name="delete-a-knowledge-base"></a>Eliminación de una base de conocimiento

Elimine la base de conocimiento con el método [delete](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#delete-kb-id--custom-headers-none--raw-false----operation-config-) y el parámetro del identificador de la base de conocimiento.

[!code-python[Delete a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=deletekb&highlight=2)]

## <a name="get-status-of-an-operation"></a>Obtención del estado de una operación

Algunos métodos, como Create y Update, pueden tardar bastante tiempo en que en lugar de esperar a que finalice el proceso, se devuelva una [operación](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.operation.operation?view=azure-python). Use el identificador de la operación para realizar un sondeo (con lógica de reintento) para determinar el estado del método original.

La llamada _setTimeout_ del siguiente bloque de código se usa para simular código asincrónico. Reemplácela por la lógica de reintento.

[!code-python[Monitor an operation](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=monitorOperation&highlight=7)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `python knowledgebase_quickstart.py` desde el directorio de la aplicación.

Todos los fragmentos de código de este artículo están [disponibles](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py) y se pueden ejecutar como un solo archivo.

```console
python knowledgebase_quickstart.py
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)
