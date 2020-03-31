---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 01/13/2020
ms.author: dapine
ms.openlocfilehash: 20dfc49acdaa30bea6f0652640e007c16f08c572
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "78925042"
---
## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?tabs=linux&pivots=programmming-language-cpp)
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha cargado de un archivo de origen en un blob de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

## <a name="open-your-project-in-visual-studio"></a>Abra el proyecto en Visual Studio.

El primer paso es asegurarse de que tiene el proyecto abierto en Visual Studio.

1. Inicie Visual Studio 2019.
2. Cargue el proyecto y abra `helloworld.cpp`.

## <a name="add-a-references"></a>Adición de referencias

Para acelerar el desarrollo de código, se usarán un par de componentes externos:
* [SDK de REST de CPP](https://github.com/microsoft/cpprestsdk): una biblioteca cliente para realizar llamadas de REST a un servicio REST.
* [nlohmann/JSON](https://github.com/nlohmann/json): práctica biblioteca de análisis, serialización y deserialización de JSON.

Ambos se pueden instalar mediante [vcpkg](https://github.com/Microsoft/vcpkg/).

```
vcpkg install cpprestsdk cpprestsdk:x64-windows
vcpkg install nlohmann-json
```

## <a name="start-with-some-boilerplate-code"></a>Inicio con código reutilizable

Vamos a agregar código que funcione como el esqueleto del proyecto.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=7-32,187-190,300-309)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="json-wrappers"></a>Contenedores de JSON

Como las API REST toman las solicitudes en formato JSON y también devuelven resultados en formato JSON, se podría interactuar con ellas solo con el uso de cadenas, aunque no se recomienda.
Para facilitar la administración de solicitudes y respuestas, se declararán algunas clases para la serialización y deserialización del código JSON y algunos métodos para ayudar a nlohmann/JSON.

Continúe y coloque sus declaraciones delante de `recognizeSpeech`.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=33-185)]

## <a name="create-and-configure-an-http-client"></a>Creación y configuración de un cliente HTTP
Lo primero que necesitamos es un cliente HTTP con una dirección URL base y un conjunto de autenticación correctos.
Inserte este código en `recognizeSpeech`.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=191-197)]

## <a name="generate-a-transcription-request"></a>Generación de una solicitud de transcripción
A continuación, se generará la solicitud de transcripción. Agregue este código a `recognizeSpeech`.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=199-203)]

## <a name="send-the-request-and-check-its-status"></a>Envío de la solicitud y comprobación de su estado
Ahora, se va a publicar la solicitud en el servicio de voz y se va a comprobar el código de respuesta inicial. Este código de respuesta indicará simplemente si el servicio ha recibido la solicitud. El servicio devolverá una dirección URL en los encabezados de respuesta, que es la ubicación en la que se almacenará el estado de la transcripción.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=204-216)]

## <a name="wait-for-the-transcription-to-complete"></a>Espere a que finalice la operación.
Dado que el servicio procesa la transcripción de forma asincrónica, es necesario sondear su estado cada cierto tiempo. Aquí se va a comprobar cada cinco segundos.

Para comprobar el estado se puede recuperar el contenido en la dirección URL que obtuvimos al publicar la solicitud. Cuando se recupera el contenido, se deserializa en una de nuestras clases auxiliares para facilitar la interacción con él.

Este es el código de sondeo con la visualización del estado de todas las tareas, excepto el de una finalización correcta, que se hará a continuación.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=222-245,285-299)]

## <a name="display-the-transcription-results"></a>Visualización de los resultados de la transcripción
Cuando el servicio haya finalizado correctamente la transcripción, los resultados se almacenarán en otra dirección URL que se puede obtener de la respuesta de estado.

Se descargará el contenido de esa dirección URL, se deserializará el código JSON y se recorrerán en bucle los resultados, con la impresión del texto en pantalla a medida que avanzamos.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=246-284)]

## <a name="check-your-code"></a>Comprobación del código
En este momento, el código debe tener esta apariencia: (Se han agregado algunos comentarios a esta versión)

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=7-308)]

## <a name="build-and-run-your-app"></a>Compilación y ejecución de la aplicación

Ya está listo para compilar la aplicación y probar el reconocimiento de voz con el servicio Voz.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]
