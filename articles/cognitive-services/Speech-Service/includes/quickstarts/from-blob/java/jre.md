---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 01/13/2020
ms.author: trbye
ms.openlocfilehash: 804cc2213379364002a093509f2393e375c3971b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400949"
---
## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?tabs=jre&pivots=programmming-language-java)
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha cargado de un archivo de origen en un blob de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)


## <a name="open-your-project-in-eclipse"></a>Apertura del proyecto en Eclipse

El primer paso es asegurarse de que tiene el proyecto abierto en Eclipse.

1. Inicie Eclipse
2. Cargue el proyecto y abra `Main.java`.

## <a name="add-a-reference-to-gson"></a>Adición de una referencia a Gson
En este inicio rápido, se usará un serializador o deserializador JSON externos. Para Java, hemos elegido [Gson](https://github.com/google/gson).

Abra el archivo pom.xml y agregue la siguiente referencia.

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/pom.xml?range=19-25)]

## <a name="start-with-some-boilerplate-code"></a>Inicio con código reutilizable

Vamos a agregar código que funcione como el esqueleto del proyecto.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=1-13,95-105,206-207)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="json-wrappers"></a>Contenedores de JSON

Como las API REST toman las solicitudes en formato JSON y también devuelven resultados en formato JSON, se podría interactuar con ellas solo con el uso de cadenas, aunque no se recomienda.
Para facilitar la administración de solicitudes y respuestas, se declararán algunas clases para la serialización y deserialización del código JSON.

Continúe y coloque sus declaraciones delante de `Main`.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=15-93)]

## <a name="create-and-configure-an-http-client"></a>Creación y configuración de un cliente HTTP
Lo primero que necesitamos es un cliente HTTP con una dirección URL base y un conjunto de autenticación correctos.
Inserte este código en `Main` [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=106-113)]

## <a name="generate-a-transcription-request"></a>Generación de una solicitud de transcripción
A continuación, se generará la solicitud de transcripción. Agregue este código a `Main` [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=115-116)]

## <a name="send-the-request-and-check-its-status"></a>Envío de la solicitud y comprobación de su estado
Ahora, se va a publicar la solicitud en el servicio de voz y se va a comprobar el código de respuesta inicial. Este código de respuesta indicará simplemente si el servicio ha recibido la solicitud. El servicio devolverá una dirección URL en los encabezados de respuesta, que es la ubicación en la que se almacenará el estado de la transcripción.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=118-129)]

## <a name="wait-for-the-transcription-to-complete"></a>Espere a que finalice la operación.
Dado que el servicio procesa la transcripción de forma asincrónica, es necesario sondear su estado cada cierto tiempo. Aquí se va a comprobar cada cinco segundos.

Para comprobar el estado se puede recuperar el contenido en la dirección URL que obtuvimos al publicar la solicitud. Cuando se recupera el contenido, se deserializa en una de nuestras clases auxiliares para facilitar la interacción con él.

Este es el código de sondeo con la visualización del estado de todas las tareas, excepto el de una finalización correcta, que se hará a continuación.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=131-159,192-204)]

## <a name="display-the-transcription-results"></a>Visualización de los resultados de la transcripción
Cuando el servicio haya finalizado correctamente la transcripción, los resultados se almacenarán en otra dirección URL que se puede obtener de la respuesta de estado.

Se descargará el contenido de esa dirección URL, se deserializará el código JSON y se recorrerán en bucle los resultados, con la impresión del texto en pantalla a medida que avanzamos.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=6-160-190)]

## <a name="check-your-code"></a>Comprobación del código
En este momento, el código debe tener esta apariencia: (Se han agregado algunos comentarios a esta versión) [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java]

## <a name="build-and-run-your-app"></a>Compilación y ejecución de la aplicación

Ya está listo para compilar la aplicación y probar el reconocimiento de voz con el servicio Voz.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]
