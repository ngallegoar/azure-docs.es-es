---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: 3ca50a9bad36e0174dc4ee0059c9d01fcc18a5f1
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400988"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha cargado de un archivo de origen en un blob de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

## <a name="download-and-install-the-api-client-library"></a>Descarga e instalación de la biblioteca cliente de API

Para ejecutar el ejemplo, debe generar la biblioteca de Python para la API REST que se genera mediante [Swagger](https://swagger.io).

Siga estos pasos para la instalación:

1. Ir a https://editor.swagger.io.
1. Haga clic en **File** (Archivo) y, luego, en **Import URL** (URL de importación).
1. Escriba la dirección URL de Swagger, incluida la región de la suscripción del servicio de voz: `https://<your-region>.cris.ai/docs/v2.0/swagger`.
1. Haga clic en **Generate Client** (Generar cliente) y seleccione **Python**.
1. Guarde la biblioteca cliente.
1. Extraiga el archivo python-client-generated.zip descargado en alguna parte del sistema de archivos.
1. Instale el módulo python-client extraído en el entorno de Python mediante pip: `pip install path/to/package/python-client`.
1. El paquete instalado tiene el nombre `swagger_client`. Puede comprobar que la instalación ha funcionado con el comando `python -c "import swagger_client"`.

> [!NOTE]
> Debido a un [error conocido en la generación automática de Swagger](https://github.com/swagger-api/swagger-codegen/issues/7541), pueden producirse errores al importar el paquete `swagger_client`.
> Para corregirlos, elimine la línea con el contenido
> ```py
> from swagger_client.models.model import Model  # noqa: F401,E501
> ```
> del archivo `swagger_client/models/model.py` y la línea con el contenido
> ```py
> from swagger_client.models.inner_error import InnerError  # noqa: F401,E501
> ```
> del archivo `swagger_client/models/inner_error.py` dentro del paquete instalado. El mensaje de error le indicará dónde se encuentran estos archivos para su instalación.

## <a name="install-other-dependencies"></a>Instalación de otras dependencias

En el ejemplo se usa la biblioteca `requests`. Puede instalarla con el comando

```bash
pip install requests
```

## <a name="start-with-some-boilerplate-code"></a>Inicio con código reutilizable

Vamos a agregar código que funcione como el esqueleto del proyecto.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-2,7-34,115-119)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="create-and-configure-an-http-client"></a>Creación y configuración de un cliente HTTP
Lo primero que necesitamos es un cliente HTTP con una dirección URL base y un conjunto de autenticación correctos.
Inserte este código en `transcribe` [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=37-45)]

## <a name="generate-a-transcription-request"></a>Generación de una solicitud de transcripción
A continuación, se generará la solicitud de transcripción. Agregue este código a `transcribe` [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=52-54)]

## <a name="send-the-request-and-check-its-status"></a>Envío de la solicitud y comprobación de su estado
Ahora, se va a publicar la solicitud en el servicio de voz y se va a comprobar el código de respuesta inicial. Este código de respuesta indicará simplemente si el servicio ha recibido la solicitud. El servicio devolverá una dirección URL en los encabezados de respuesta, que es la ubicación en la que se almacenará el estado de la transcripción.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=65-73)]

## <a name="wait-for-the-transcription-to-complete"></a>Espere a que finalice la operación.
Dado que el servicio procesa la transcripción de forma asincrónica, es necesario sondear su estado cada cierto tiempo. Aquí se va a comprobar cada cinco segundos.

Se enumerarán todas las transcripciones que está procesando este recurso del servicio de voz y se buscará la que se ha creado.

Este es el código de sondeo con la visualización del estado de todas las tareas, excepto el de una finalización correcta, que se hará a continuación.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=75-94,99-112)]

## <a name="display-the-transcription-results"></a>Visualización de los resultados de la transcripción
Cuando el servicio haya finalizado correctamente la transcripción, los resultados se almacenarán en otra dirección URL que se puede obtener de la respuesta de estado.

Aquí obtendremos ese archivo JSON de resultados y lo mostraremos.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=95-98)]

## <a name="check-your-code"></a>Comprobación del código
En este momento, el código debe tener esta apariencia: (Se han agregado algunos comentarios a esta versión) [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-118)]

## <a name="build-and-run-your-app"></a>Compilación y ejecución de la aplicación

Ya está listo para compilar la aplicación y probar el reconocimiento de voz con el servicio Voz.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]
