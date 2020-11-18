---
title: Reconocimiento óptico de caracteres (OCR) en Computer Vision
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con el reconocimiento óptico de caracteres (OCR) de imágenes y documentos que contienen texto impreso y manuscrito mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/11/2020
ms.author: pafarley
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 4eb2835bd09e6548149ee90cb7232bf230de0300
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94538757"
---
# <a name="optical-character-recognition-ocr"></a>Reconocimiento óptico de caracteres (OCR)

Computer Vision API de Azure incluye funcionalidades para el reconocimiento óptico de caracteres (OCR) que permiten extraer texto impreso o manuscrito de imágenes. Puede extraer texto de imágenes, como fotos de matrículas o de contenedores con números de serie, así como de documentos; por ejemplo, facturas, recibos, informes financieros y artículos, entre otros.

## <a name="read-api"></a>Read API 

Computer Vision [Read API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) es la tecnología de OCR más reciente de Azure ([información sobre las novedades](./whats-new.md)) que permite extraer texto impreso (en varios idiomas), texto manuscrito (solo en inglés), dígitos y símbolos de divisa de imágenes, y documentos PDF con varias páginas. Esta tecnología está optimizada para extraer texto de imágenes con mucho texto y de documentos PDF con varias páginas y una mezcla de idiomas. Es compatible con la detección de texto impreso y manuscrito en un mismo documento o una misma imagen.

![Cómo OCR convierte imágenes y documentos en una salida estructurada con texto extraído](./Images/how-ocr-works.svg)

## <a name="input-requirements"></a>Requisitos de entrada
La llamada a **Read** usa las imágenes y los documentos como entrada. Tienen los siguientes requisitos:

* Formatos de archivos admitidos: JPEG, PNG, BMP, PDF y TIFF.
* En el caso de los archivos PDF y TIFF, se procesan hasta 2000 páginas (solo las primeras dos páginas en el nivel Gratis).
* El tamaño de archivo debe ser inferior a 50 MB (4 MB para el nivel Gratis); y sus dimensiones, de al menos 50x50 píxeles y, como máximo, de 10 000x10 000 píxeles. 
* Los archivos PDF deben tener unas dimensiones de 17x17 pulgadas como máximo, lo que se corresponde con los tamaños de papel Legal o A3 y más pequeños.

### <a name="read-32-preview-allows-selecting-pages"></a>Read 3.2 en versión preliminar permite seleccionar una o varias páginas.
Con la [API de Read 3.2 en versión preliminar](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-1/operations/5d986960601faab4bf452005), para documentos grandes de varias páginas, puede proporcionar números de página o intervalos de páginas específicos como parámetro de entrada para extraer texto solo de dichas páginas. Se trata de un nuevo parámetro de entrada además del parámetro de idioma opcional.

> [!NOTE]
> **Entrada de idioma** 
>
> La [llamada a Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) tiene un parámetro de solicitud opcional para el idioma. Este es el código de idioma BCP-47 del texto del documento. La lectura admite la identificación automática del idioma y documentos multilingües, por lo que solo debe proporcionar un código de idioma si desea forzar el procesamiento del documento como ese idioma específico.

## <a name="the-read-call"></a>Llamada a Read

La [llamada a Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) de Read API toma como entrada una imagen o un documento PDF y extrae texto de forma asincrónica. La llamada se devuelve un campo de encabezado de respuesta denominado `Operation-Location`. El valor `Operation-Location` es una dirección URL que contiene el identificador de la operación que se va a usar en el paso siguiente.

|Encabezado de respuesta| Dirección URL del resultado |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/vision/v3.1/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

> [!NOTE]
> **Facturación** 
>
> La página de [precios de Computer Vision](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/) incluye el plan de tarifa de Read. Cada imagen o página analizada cuenta como una transacción. Si llama a la operación con un documento PDF o TIFF con 100 páginas, la operación de Read la contabilizará como 100 transacciones para su facturación. Si realizó 50 llamadas a la operación, y cada llamada envió un documento con 100 páginas, se facturarán 50x100 = 5000 transacciones.

## <a name="the-get-read-results-call"></a>Llamada a Get Read Results

El segundo paso es llamar a la operación [Get Read Results](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d9869604be85dee480c8750). Esta operación toma como entrada el identificador de operación que la operación de lectura ha creado. Devuelve una respuesta JSON que contiene un campo de **estado** con los siguientes valores posibles. Llamará a esta operación de forma iterativa hasta que se devuelva con el valor **correcto**. Use un intervalo de 1 a 2 segundos para evitar superar la tasa de solicitudes por segundo (RPS).

|Campo| Tipo | Valores posibles |
|:-----|:----:|:----|
|status | string | notStarted: la operación no se ha iniciado. |
| |  | running: la operación se está procesando. |
| |  | failed: Se produjeron errores en la operación. |
| |  | succeeded: La operación se ha realizado correctamente. |

> [!NOTE]
> El nivel gratuito limita la tasa de solicitudes a 20 llamadas por minuto. El nivel de pago permite 10 solicitudes por segundo (RPS) que pueden aumentar a petición. Use el canal de soporte técnico de Azure o contacte con el equipo de cuentas para solicitar una tasa de solicitudes por segundo (RPS) más alta.

Cuando el campo **estado** tiene el valor **succeeded** (correcto), la respuesta JSON contiene el contenido de texto extraído de la imagen o el documento. La respuesta en JSON mantiene las agrupaciones de líneas originales de palabras reconocidas. Incluye las líneas de texto extraídas y las coordenadas del cuadro de límite correspondientes. Cada línea de texto incluye todas las palabras extraídas, con las coordenadas y las puntuaciones de confianza respectivas.

> [!NOTE]
> Los datos enviados a la operación `Read` se cifran y almacenan temporalmente en reposo y se eliminan en 48 horas. Esto permite que las aplicaciones recuperen el texto extraído como parte de la respuesta del servicio.

## <a name="sample-json-output"></a>Salida de JSON de ejemplo

Consulte el siguiente ejemplo de una respuesta JSON correcta:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0.8551,
        "width": 2661,
        "height": 1901,
        "unit": "pixel",
        "lines": [
          {
            "boundingBox": [
              67,
              646,
              2582,
              713,
              2580,
              876,
              67,
              821
            ],
            "text": "The quick brown fox jumps",
            "words": [
              {
                "boundingBox": [
                  143,
                  650,
                  435,
                  661,
                  436,
                  823,
                  144,
                  824
                ],
                "text": "The",
                "confidence": 0.958
              }
            ]
          }
        ]
      }
    ]
  }
}
```
### <a name="read-32-preview-adds-text-line-style-latin-languages-only"></a>Read 3.2 en versión preliminar agrega el estilo de línea de texto (solo en idiomas procedentes del latín).
La [API de Read 3.2 en versión preliminar](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-1/operations/5d986960601faab4bf452005) genera como salida un objeto **appearance** que clasifica si cada línea de texto tiene estilo impreso o de escritura a mano, junto con una puntuación de confianza. Esta característica solo es compatible con los idiomas derivados del latín.

Comience con las [guías de inicio rápido del SDK de OCR de Read en Computer Vision](./quickstarts-sdk/client-library.md) y las [guías de inicio rápido de la API REST de Read](./QuickStarts/CSharp-hand-text.md) para empezar a integrar las funcionalidades de OCR en sus aplicaciones.

## <a name="supported-languages-for-print-text"></a>Idiomas compatibles con el texto impreso
[Read API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) admite la extracción de texto impreso en inglés, español, alemán, francés, italiano, portugués y neerlandés.

Para obtener una lista completa de los idiomas admitidos para OCR, consulte los [idiomas admitidos](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr).

### <a name="read-32-preview-adds-simplified-chinese-and-japanese"></a>Read 3.2 en versión preliminar incorpora chino simplificado y japonés.
La [API de Read 3.2 en versión preliminar pública](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-1/operations/5d986960601faab4bf452005) agrega compatibilidad con chino simplificado y japonés. Si para su escenario se requieren más idiomas, consulte la sección de la [API de OCR](#ocr-api). 

## <a name="supported-languages-for-handwritten-text"></a>Idiomas compatibles con el texto manuscrito
Actualmente, la operación de lectura admite la extracción de texto manuscrito solo en inglés.

## <a name="use-the-rest-api-and-sdk"></a>Uso de la API de REST y el SDK
La [API REST Read 3.x](./QuickStarts/CSharp-hand-text.md) es la opción preferida para la mayoría de los clientes debido a su facilidad de integración y su inmediata productividad. Azure y el servicio Computer Vision controlan las necesidades de escalado, rendimiento, seguridad de los datos y cumplimiento, lo que le permite centrarse en satisfacer las necesidades de los clientes.

## <a name="deploy-on-premise-with-docker-containers"></a>Implementación local con contenedores de Docker
La [versión preliminar de contenedor de Docker de Read](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers) le permite implementar las nuevas funcionalidades de OCR en su entorno local. Los contenedores son excelentes para requisitos específicos de control de datos y seguridad.

## <a name="example-outputs"></a>Salidas de ejemplo

### <a name="text-from-images"></a>Texto en imágenes

En el siguiente resultado de Read API se muestra el texto extraído de una imagen que contiene texto con varios ángulos, colores y fuentes.

![Imagen de varias palabras en diferentes colores y ángulos, con texto extraído](./Images/text-from-images-example.png)

### <a name="text-from-documents"></a>Texto en documentos

Read API también puede tomar documentos PDF como entrada.

![Un documento de factura, con texto extraído](./Images/text-from-pdf-example.png)

### <a name="handwritten-text"></a>Texto manuscrito

La operación de lectura extrae texto manuscrito de imágenes (actualmente solo en inglés).

![Imagen de una nota escrita a mano, con texto extraído](./Images/handwritten-example.png)

### <a name="printed-text"></a>Texto impreso

La operación de lectura puede extraer texto impreso en varios idiomas distintos.

![Imagen de un libro de texto español, con texto extraído](./Images/supported-languages-example.png)

### <a name="mixed-language-documents"></a>Documentos en varios idiomas

Read API admite imágenes y documentos que contienen varios idiomas, conocidos comúnmente como "documentos con una mezcla de idiomas". Clasifica cada línea de texto que hay en un documento según el idioma detectado antes de extraer el contenido de texto.

![Imagen de frases en varios idiomas, con texto extraído](./Images/mixed-language-example.png)

## <a name="ocr-api"></a>API de OCR

La [API de OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f20d) usa un modelo de reconocimiento anterior, solo admite imágenes y se ejecuta de forma sincrónica, así que devuelve el texto detectado inmediatamente. Consulte los [idiomas admitidos por OCR](https://docs.microsoft.com/azure/cognitive-services/computer-vision/language-support#optical-character-recognition-ocr) y Read API.

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de datos

Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan los servicios Read/OCR deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la página de Cognitive Services en [Microsoft Trust Center](https://www.microsoft.com/trust-center/product-overview).

> [!NOTE]
> Las operaciones de reconocimiento de texto de Computer Vision 2.0 pronto estarán en desuso en favor de la nueva Read API de la que se habla en este artículo. Los clientes existentes deben [realizar la transición a operaciones de lectura](upgrade-api-versions.md).

## <a name="next-steps"></a>Pasos siguientes

- Comience con las [guías de inicio rápido del SDK de Read de Computer Vision](./quickstarts-sdk/client-library.md) en C#, Java, JavaScript o Python.
- Use los [inicios rápido de la API REST de Read](./QuickStarts/CSharp-hand-text.md) para C#, Java, JavaScript o Python para aprender a utilizar las API REST.
- Obtenga información sobre la [API REST de Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005).
- Obtenga información sobre la [API REST de Read 3.2 en versión preliminar pública](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-1/operations/5d986960601faab4bf452005) con compatibilidad agregada con chino simplificado y japonés.
