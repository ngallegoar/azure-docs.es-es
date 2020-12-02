---
title: 'Diseños: Form Recognizer'
titleSuffix: Azure Cognitive Services
description: 'Conozca los conceptos relacionados con el análisis de diseños mediante API Form Recognizer: uso y límites.'
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 11/18/2020
ms.author: pafarley
ms.openlocfilehash: 60ef490bbba4dca4f01b82967ad6ea8636e7bd4e
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96353704"
---
# <a name="form-recognizer-layout-service"></a>Servicio Layout de Form Recognizer

Azure Form Recognizer puede extraer texto, tablas, marcas de selección e información de la estructura de los documentos mediante su servicio Layout. La API Layout permite a los clientes tomar documentos de varios formatos y devolver no solo datos estructurados, sino también una representación del documento. Combina nuestras eficaces funcionalidades de [reconocimiento óptico de caracteres (OCR)](../computer-vision/concept-recognizing-text.md) con modelos de aprendizaje profundo de reconocimiento de documentos para extraer texto, tablas, marcas de selección e información de la estructura de los documentos. 

## <a name="what-does-the-layout-service-do"></a>¿Qué hace el servicio Layout?

La API Layout extrae texto, tablas, marcas de selección e información de la estructura de documentos con una precisión excepcional y los devuelve en una respuesta JSON estructurada organizada. Los documentos pueden ser de varios formatos y tener distintos grados de calidad, lo que incluye imágenes capturadas por un teléfono, documentos digitalizados y archivos PDF digitales. La API Layout extraerá la salida estructurada de todos estos documentos.

![Ejemplo de diseño](./media/layout-tool-example.JPG)

## <a name="try-it-out"></a>Prueba

Para probar el servicio Layout de Form Recognizer, vaya a la herramienta de interfaz de usuario de ejemplo en línea:

> [!div class="nextstepaction"]
> [Ejemplo de interfaz de usuario](https://fott-preview.azurewebsites.net/)

Necesitará una suscripción de Azure ([cree una gratis](https://azure.microsoft.com/free/cognitive-services)) y un punto de conexión de [recursos de Form Recognizer](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer), así como la clave para probar la API Layout de Form Recognizer. 

![Captura de pantalla de la interfaz de usuario de ejemplo; se analizan el texto, las tablas y las marcas de selección de un documento](./media/analyze-layout.png)

### <a name="input-requirements"></a>Requisitos de entrada 

[!INCLUDE [input requirements](./includes/input-requirements-receipts.md)]

## <a name="the-analyze-layout-operation"></a>La operación Analyze Layout

La operación [Analyze Layout](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeLayoutAsync) toma un documento (archivo de imagen, TIFF o PDF) como entrada y extrae el texto, las tablas, las marcas de selección y la estructura del mismo. La llamada devuelve un campo de encabezado de respuesta denominado `Operation-Location`. El valor `Operation-Location` es una dirección URL que contiene el id. de resultado que se va a usar en el paso siguiente.

|Encabezado de respuesta| Dirección URL del resultado |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.1-preview.2/prebuilt/layout/analyzeResults/44a436324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="the-get-analyze-layout-result-operation"></a>La operación Get Analyze Layout Result

El segundo paso consiste en llamar a la operación [Get Analyze Layout Result](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/GetAnalyzeLayoutResult). Esta operación toma como entrada el identificador de resultado que la operación Analyze Layout ha creado. Devuelve una respuesta JSON que contiene un campo de **estado** con los siguientes valores posibles. 

|Campo| Tipo | Valores posibles |
|:-----|:----:|:----|
|status | string | `notStarted`: la operación de análisis no se ha iniciado.<br /><br />`running`: la operación de análisis está en curso.<br /><br />`failed`: error en la operación de análisis.<br /><br />`succeeded`: la operación de análisis se realizó correctamente.|

Llamará a esta operación de forma iterativa hasta que se devuelva con el valor `succeeded`. Use un intervalo de 3 a 5 segundos para evitar superar la tasa de solicitudes por segundo (RPS).

Cuando el campo de **estado** tiene el valor `succeeded`, la respuesta de JSON incluirá los resultados de la extracción del diseño, el texto, las tablas y las marcas de selección extraídas. Los datos extraídos contienen las líneas de texto y palabras extraídas, el cuadro de límite, la indicación manuscrita de apariencia del texto, las tablas y las marcas de selección con una indicación de seleccionada o no seleccionada. 

### <a name="sample-json-output"></a>Salida de JSON de ejemplo

La respuesta a la operación Get Analyze Layout Result será la representación estructurada del documento con toda la información extraída. Aquí encontrará un [archivo con un documento de ejemplo](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-layout.pdf) y su salida estructurada, una [salida de un diseño de ejemplo](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-layout-output.json).

La salida JSON tiene dos partes: 
* El nodo `"readResults"` contiene todo el texto reconocido y las marcas de selección. El texto se organiza por página, después, por líneas y, finalmente, por palabras individuales. 
* El nodo `"pageResults"` contiene las tablas y celdas extraídas con sus rectángulos delimitadores, su confianza y una referencia a las líneas y palabras de "readResults".

## <a name="example-output"></a>Ejemplo de salida

### <a name="text"></a>Texto

Layout extrae texto de documentos (PDF y TIFF) e imágenes (JPG, PNG y BMP) con distintos ángulos de texto, colores, ángulos, fotos de documentos, faxes, texto impresos, texto manuscrito (solo en inglés) y modos mixtos. El texto se extrae con información sobre las líneas, palabras, cuadros de límite, puntuaciones de confianza y estilo (manuscrito u otro). Toda la información del texto se incluye en la sección `"readResults"` de la salida JSON. 

### <a name="tables"></a>Tablas

Layout extrae tablas de documentos (PDF y TIFF) e imágenes (JPG, PNG y BMP). Los documentos se pueden digitalizar o fotografiar. Las tablas pueden ser tablas complejas con celdas o columnas combinadas, con o sin bordes, y con ángulos impares. Las tablas extraídas incluyen el número de columnas y filas, el intervalo de filas y el intervalo de columnas. Cada celda se extrae con su cuadro de límite y referencia al texto extraído en la sección `"readResults"`. La información de la tabla se encuentra en la sección `"pageResults"` de la salida JSON. 

![Tablas de ejemplo](./media/tables-example.jpg)

### <a name="selection-marks"></a>Marcas de selección

Layout también extrae marcas de selección de los documentos. Entre las marcas de selección extraídas se incluyen el cuadro de límite, la confianza y el estado (seleccionado o no seleccionado). La información de la marca de selección se extrae en la sección `"readResults"` de la salida JSON. 

## <a name="next-steps"></a>Pasos siguientes

- Pruebe su propia extracción de diseño mediante el [interfaz de usuario de ejemplo de Form Recognizer](https://fott-preview.azurewebsites.net/)
- Consulte el [inicio rápido de la biblioteca cliente de Form Recognizer](quickstarts/client-library.md) para empezar a extraer diseños en el lenguaje que prefiera.
- O bien, siga el inicio rápido [Extracción de datos de diseño](./QuickStarts/python-layout.md) para implementar la extracción de datos de diseño con Python y la API REST.

## <a name="see-also"></a>Consulte también

* [¿Qué es Form Recognizer?](./overview.md)
* [Documentos de referencia de la API de REST](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeLayoutAsync)