---
title: ¿Qué es Form Recognizer?
titleSuffix: Azure Cognitive Services
description: El Azure Cognitive Services Form Recognizer le permite identificar y extraer pares clave-valor y datos de tabla de documentos de formulario.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 6b19dc11438274ecf6218d5c0bd8c9ef3dafbf01
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80052431"
---
# <a name="what-is-form-recognizer"></a>¿Qué es Form Recognizer?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Form Recognizer es un servicio cognitivo que utiliza tecnología de aprendizaje automático para identificar y extraer texto, pares clave-valor, así como datos de tablas de documentos de formularios. Ingiere texto de formularios y genera datos estructurados que incluyen las relaciones del archivo original. Obtendrá rápidamente resultados precisos a la medida de su contenido específico sin necesidad de una intervención manual pesada o una amplia experiencia en ciencia de datos. Form Recognizer consta de modelos personalizados, el modelo de recibo pregenerado y la API de diseño. Puede llamar a los modelos de Form Recognizer mediante una API REST para reducir la complejidad e integrarla en su flujo de trabajo o aplicación.

Form Recognizer se compone de los siguientes servicios:
* **Modelos personalizados**: extraen los pares clave-valor y los datos de tablas de los formularios. Puede entrenar estos modelos con sus propios datos para que se adapten a sus formularios.
* **Modelo de recibo pregenerado**: extrae datos de los recibos de compras de EE. UU. con un modelo pregenerado.
* **API de diseño**: extrae estructuras de texto y tabla, junto con sus coordenadas de los cuadros de límite, desde los documentos.

<!-- add diagram -->

## <a name="custom-models"></a>Modelos personalizados

Puede entrenar los modelos personalizados de Form Recognizer con sus propios datos y solo necesita cinco formularios de entrada de ejemplo para empezar. Un modelo entrenado puede generar datos estructurados que incluyen las relaciones del documento de formulario original. Después de entrenar el modelo, puede probarlo, volver a entrenarlo y finalmente usarlo para extraer datos de forma confiable de más formularios en función de las propias necesidades.

Tiene las siguientes opciones al entrenar modelos personalizados: entrenamiento con datos etiquetados y sin ellos.

### <a name="train-without-labels"></a>Entrenamiento sin etiquetas

De forma predeterminada, Form Recognizer usa el aprendizaje sin supervisión para comprender el diseño y las relaciones entre los campos y las entradas de los formularios. Cuando se envían los formularios de entrada, el algoritmo agrupa los formularios por tipos, descubre qué claves y tablas están presentes y asocia los valores a las claves y entradas a las tablas. Esto no requiere el etiquetado de datos manual ni una codificación intensiva, ni mantenimiento y se recomienda probar este método primero.

### <a name="train-with-labels"></a>Entrenamiento con etiquetas

Al entrenar con datos etiquetados, el modelo realiza un aprendizaje supervisado para extraer valores de interés mediante los formularios etiquetados que se proporcionan. Esto da como resultado modelos con un mejor rendimiento y puede generar modelos que funcionen con formularios complejos o formularios que contengan valores sin claves.

Form Recognizer usa la [API de diseño](#layout-api) para aprender los tamaños y posiciones esperados de los elementos de texto impresos y manuscritos. A continuación, usa etiquetas especificadas por el usuario para aprender las asociaciones clave-valor de los documentos. Se recomienda usar cinco formularios etiquetados manualmente del mismo tipo para empezar a entrenar un nuevo modelo y agregar más datos etiquetados según sea necesario para mejorar la precisión del modelo.

## <a name="prebuilt-receipt-model"></a>Modelo de recibo pregenerado

Form Recognizer también incluye un modelo para leer los recibos de compras en inglés procedentes de Estados Unidos, el tipo de recibos que se usa en restaurantes, gasolineras, minoristas, etc. ([ejemplo de recibo](./media/contoso-receipt-small.png)). Este modelo extrae información clave como la hora y fecha de la transacción, información del comerciante, cantidades de impuestos, totales y mucho más. Además, el modelo de recibo pregenerado se entrena para reconocer y devolver todo el texto en un recibo.

## <a name="layout-api"></a>API de diseño

Form Recognizer también puede extraer texto y estructuras de tablas (los números de fila y columna asociados al texto) mediante el reconocimiento óptico de caracteres (OCR) de alta definición.

## <a name="get-started"></a>Introducción

Siga un inicio rápido para comenzar a extraer datos de los formularios. Se recomienda usar el servicio gratuito cuando se está aprendiendo la tecnología. Recuerde que el número de páginas gratuitas se limita a 500 al mes.

* Personalizado: entrene un modelo para sus formularios.
  * Entrenamiento sin etiquetas
    * [Inicio rápido: Entrenamiento de un modelo de Form Recognizer y extracción de los datos del formulario mediante la API REST con cURL](quickstarts/curl-train-extract.md)
    * [Inicio rápido: Entrenamiento de un modelo de Form Recognizer y extracción de datos del formulario mediante la API REST con Python](quickstarts/python-train-extract.md)
  * Entrenamiento con etiquetas
    * [Entrenamiento de un modelo de Form Recognizer con etiquetas mediante la herramienta de etiquetado de ejemplo](quickstarts/label-tool.md)
    * [Entrenamiento de un modelo de Form Recognizer con etiquetas mediante la API REST y Python](quickstarts/python-labeled-data.md)
* Recibos pregenerados: extraiga datos de recibos de compra de EE. UU.
  * [Inicio rápido: Extracción de datos de recibos con cURL](quickstarts/curl-receipts.md)
  * [Inicio rápido: Extracción de datos de recibos con Python](quickstarts/python-receipts.md)
* Diseño: extraiga texto y estructura de tablas de los formularios.
  * [Inicio rápido: Extracción de datos de diseño con Python](quickstarts/python-layout.md)

### <a name="review-the-rest-apis"></a>Revisión de las API REST

Va a utilizar las siguientes API para entrenar modelos y extraer datos estructurados de formularios.

|Nombre |Descripción |
|---|---|
| **Entrenamiento de modelos personalizados**| Entrene un nuevo modelo para analizar los formularios con cinco formularios del mismo tipo. Establezca el parámetro _useLabelFile_ en `true` para entrenar con datos etiquetados manualmente. |
| **Análisis de formularios** |Analice un documento único pasado como una secuencia para extraer texto, pares clave-valor y tablas del formulario con su modelo personalizado.  |
| **Análisis de recibos** |Analice un documento de entrada individual para extraer la información clave y cualquier otro texto del recibo.|
| **Análisis de diseño** |Analice el diseño de un formulario para extraer texto y estructuras de tablas.|

Explore la [documentación de referencia de API REST](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm) para más información. Si está familiarizado con una versión anterior de la API, consulte el artículo sobre [novedades](./whats-new.md) para más información sobre los cambios recientes.

## <a name="input-requirements"></a>Requisitos de entrada
### <a name="custom-model"></a>Modelo personalizado

[!INCLUDE [input requirements](./includes/input-requirements.md)]

### <a name="prebuilt-receipt-model"></a>Modelo de recibo pregenerado

Los requisitos de entrada para el modelo de recibo son ligeramente diferentes.

* El formato debe ser JPEG, PNG, BMP, PDF (texto o escaneado) o TIFF.
* El tamaño del archivo debe ser inferior a 20 MB.
* Las imágenes deben tener unas dimensiones entre 50 x 50 píxeles y 10 000 x 10 000 píxeles.
* Los archivos PDF deben tener unas dimensiones de 17 x 17 pulgadas como máximo, lo que se corresponde con los tamaños de papel Legal o A3 y más pequeños.
* En el caso de PDF y TIFF, solo se procesan las primeras 200 páginas (con una suscripción de nivel gratuitos, solo se procesan las dos primeras páginas).

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de datos

El servicio se ofrece como [versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) de un servicio de Azure bajo los [términos de los servicios en línea](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Form Recognizer deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Realice un [inicio rápido](quickstarts/curl-train-extract.md) para empezar a usar las [API de Form Recognizer](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm).
