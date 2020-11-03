---
title: 'Inicio rápido: Biblioteca cliente de Computer Vision para Node.js'
description: Empiece a trabajar con la biblioteca cliente de Computer Vision para Node.js con este inicio rápido.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 10/26/2020
ms.author: pafarley
ms.custom: devx-track-js
ms.openlocfilehash: 9c0ed50cc0f7ef3580d1441fe2f361065e6f8524
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92886740"
---
<a name="HOLTop"></a>

[Documentación de referencia](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-computervision) | [Paquete (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-computervision) | [Ejemplos](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/).
* La versión actual de [Node.js](https://nodejs.org/)
* Una vez que tenga la suscripción de Azure, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Creación de un recurso de Computer Vision"  target="_blank">cree un recurso de Computer Vision <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para obtener la clave y el punto de conexión. Una vez que se implemente, haga clic en **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación al servicio Computer Vision. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.


## <a name="setting-up"></a>Instalación

### <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él.

```console
mkdir myapp && cd myapp
```

Ejecute el comando `npm init` para crear una aplicación de nodo con un archivo `package.json`.

```console
npm init
```

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Instale los paquetes `ms-rest-azure` y `@azure/cognitiveservices-computervision` de NPM:

```console
npm install @azure/cognitiveservices-computervision
```

el archivo `package.json` de la aplicación se actualizará con las dependencias.

Cree un archivo, *index.js* y ábralo en un editor de texto. Agregue las siguientes instrucciones import.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imports)]

> [!TIP]
> ¿Desea ver todo el archivo de código de inicio rápido de una vez? Puede encontrarlo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js), que contiene los ejemplos de código de este inicio rápido.

Cree variables para el punto de conexión y la clave de Azure del recurso.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_vars)]

> [!IMPORTANT]
> Vaya a Azure Portal. Si el recurso de [nombre del producto] que ha creado en la sección **Requisitos previos** se ha implementado correctamente, haga clic en el botón **Ir al recurso** en **Pasos siguientes**. Puede encontrar su clave y punto de conexión en la página de **clave y punto de conexión** del recurso, en **Administración de recursos**. 
>
> Recuerde quitar la clave del código cuando haya terminado y no hacerla nunca pública. En el caso de producción, considere la posibilidad de usar alguna forma segura de almacenar las credenciales, y acceder a ellas. Para más información, consulte el artículo sobre la [seguridad](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security) de Cognitive Services.

## <a name="object-model"></a>Modelo de objetos

Las siguientes clases e interfaces controlan algunas de las características principales del SDK de Node.js para Computer Vision.

|Nombre|Descripción|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest) | Esta clase es necesaria para todas las funcionalidades de Computer Vision. Cree una instancia de ella con la información de suscripción y úsela para realizar la mayoría de las operaciones con imágenes.|
|[VisualFeatureTypes](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/visualfeaturetypes?view=azure-node-latest)| Esta enumeración define los diferentes tipos de análisis de imágenes que se pueden realizar en una operación de análisis estándar. Debe especificar un conjunto de valores de **VisualFeatureTypes** en función de sus necesidades. |

## <a name="code-examples"></a>Ejemplos de código

En estos fragmentos de código se muestra cómo realizar las siguientes tareas con la biblioteca cliente de Computer Vision para Node.js:

* [Autenticar el cliente](#authenticate-the-client)
* [Analizar una imagen](#analyze-an-image)
* [Leer texto manuscrito e impreso](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>Autenticar el cliente


Cree una instancia de un cliente con la clave y el punto de conexión. Cree un objeto [ApiKeyCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.apikeycredentials?view=azure-python) con su clave y punto de conexión y úselo para crear un objeto [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest).

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_client)]

Luego, defina una función `computerVision` y declare una serie asincrónica con la función principal y la función de devolución de llamada. Se agregará el código de inicio rápido a la función principal y se llamará a `computerVision` en la parte inferior del script. El resto del código de este inicio rápido va incluido dentro de la función `computerVision`.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_functiondef_begin)]

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_functiondef_end)]

## <a name="analyze-an-image"></a>Análisis de una imagen

El código de esta sección analiza las imágenes remotas para extraer varias características visuales. Puede realizar estas operaciones como parte del método **analyzeImage** del objeto cliente, o puede llamarlas mediante métodos individuales. Para más información, consulte la [documentación de referencia](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest).

> [!NOTE]
> También puede analizar una imagen local. Consulte los métodos [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest), como **analyzeImageInStream**. O bien, consulte el código de ejemplo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) para escenarios relacionados con imágenes locales.

### <a name="get-image-description"></a>Obtención de la descripción de la imagen

El código siguiente obtiene la lista de títulos generados para la imagen. Consulte [Descripción de imágenes](../../concept-describing-images.md) para más detalles.

En primer lugar, defina la dirección URL de la imagen que se va a analizar:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_describe_image)]

Después, agregue el código siguiente para obtener la descripción de la imagen e imprímala en la consola.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_describe)]

### <a name="get-image-category"></a>Obtención de la categoría de imagen

El código siguiente obtiene la categoría detectada de la imagen. Consulte [Categorización de imágenes](../../concept-categorizing-images.md) para más detalles.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_categories)]

Defina la función auxiliar `formatCategories`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_categories_format)]

### <a name="get-image-tags"></a>Obtención de etiquetas de imagen

El código siguiente obtiene el conjunto de las etiquetas detectadas en la imagen. Consulte [Etiquetas de contenido](../../concept-tagging-images.md) para más detalles.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_tags)]

Defina la función auxiliar `formatTags`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_tagsformat)]

### <a name="detect-objects"></a>Detección de objetos

El código siguiente detecta objetos comunes en la imagen y los imprime en la consola. Consulte [Detección de objetos](../../concept-object-detection.md) para más información.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_objects)]

Defina la función auxiliar `formatRectObjects` para devolver las coordenadas superior, izquierda, inferior y derecha, junto con el ancho y el alto.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_objectformat)]

### <a name="detect-brands"></a>Detección de marcas

El código siguiente detecta marcas corporativas y logotipos en la imagen y los imprime en la consola. Consulte [Detección de marcas](../../concept-brand-detection.md) para más información.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_brands)]

### <a name="detect-faces"></a>Detección de caras

El código siguiente devuelve las caras detectadas en la imagen con sus coordenadas de rectángulo y selecciona los atributos de cara. Consulte [Detección de caras](../../concept-detecting-faces.md) para más información.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_faces)]

Defina la función auxiliar `formatRectFaces`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_formatfaces)]

### <a name="detect-adult-racy-or-gory-content"></a>Detección de contenido para adultos, explícito o sangriento

El siguiente código imprime la presencia detectada de contenido para adultos en la imagen. Para más información, consulte [Contenido para adultos, subido de tono y sangriento](../../concept-detecting-adult-content.md).

Defina la dirección URL de la imagen que se va a usar:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_adult_image)]

Después, agregue el código siguiente para detectar el contenido para adultos e imprima los resultados en la consola.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Obtención de la combinación de colores de imagen

El código siguiente imprime los atributos de color detectados en la imagen, como los colores dominantes y el color de énfasis. Consulte [Combinaciones de colores](../../concept-detecting-color-schemes.md) para más detalles.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_colors)]

Defina la función auxiliar `printColorScheme` para imprimir los detalles de la combinación de color en la consola.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_colors_print)]

### <a name="get-domain-specific-content"></a>Obtención de contenido específico del dominio

Computer Vision puede usar un modelo especializado para realizar análisis adicionales en las imágenes. Consulte [Contenido específico del dominio](../../concept-detecting-domain-content.md) para más detalles.

En primer lugar, defina la dirección URL de la imagen que se va a analizar:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_domain_image)]

En el código siguiente se analizan los datos sobre los paisajes detectados en la imagen.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_landmarks)]

Defina la función auxiliar `formatRectDomain` para analizar los datos de ubicación de los puntos de referencia detectados.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_landmarks_rect)]

### <a name="get-the-image-type"></a>Obtención del tipo de imagen

El código siguiente imprime información sobre el tipo de imagen (si es una imagen prediseñada o dibujo lineal).

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imagetype)]

Defina la función auxiliar `describeType`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imagetype_describe)]

## <a name="extract-text-ocr-with-read"></a>Extraer texto (OCR) con lectura

Computer Vision puede leer texto visible de una imagen y convertirlo en un flujo de caracteres. Este ejemplo utiliza las operaciones de lectura.

### <a name="set-up-test-images"></a>Configuración de imágenes de prueba

Guarde una referencia de la dirección URL de las imágenes de las que quiere extraer texto.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_images)]

> [!NOTE]
> También puede leer el texto de una imagen local. Consulte los métodos [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest), como **readInStream**. O bien, consulte el código de ejemplo en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) para escenarios relacionados con imágenes locales.

### <a name="call-the-read-api"></a>Llamada a la API Read

Defina los campos siguientes en la función para indicar los valores de estado de la llamada a Read.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_statuses)]


Agregue el código siguiente, que llama a la función `readTextFromURL` para las imágenes proporcionadas.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_call)]

Defina la función `readTextFromURL`. Esta acción llama al método **read** en el objeto de cliente, que devuelve un identificador de operación e inicia un proceso asincrónico para leer el contenido de la imagen. A continuación, usa el identificador de operación para comprobar el estado de la operación hasta que se devuelvan los resultados. Después, devuelve los resultados extraídos.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_helper)]

Seguidamente, defina la función auxiliar `printRecText`, que imprime los resultados de la operación de lectura en la consola.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_print)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `node` en el archivo de inicio rápido.

```console
node index.js
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Referencia de la API Computer Vision (Node.js)](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest)

* [¿Qué es Computer Vision?](../../overview.md)
* El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js).
