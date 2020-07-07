---
title: Aptitud cognitiva de extracción de documentos (versión preliminar)
titleSuffix: Azure Cognitive Search
description: Extrae contenido de un archivo dentro de la canalización de enriquecimiento. Esta aptitud está actualmente en versión preliminar pública.
manager: nitinme
author: careyjmac
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.author: chalton
ms.openlocfilehash: f209be383e445e3b0c011e0bfb4266a191a8d931
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85080875"
---
# <a name="document-extraction-cognitive-skill"></a>Aptitud cognitiva de extracción de documentos

> [!IMPORTANT] 
> Esta aptitud está actualmente en versión preliminar pública. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Actualmente no hay compatibilidad con el portal ni con el SDK de .NET.

La aptitud de **extracción de documentos** extrae el contenido de un archivo dentro de la canalización de enriquecimiento. Esto le permite aprovechar el paso de extracción de documentos que normalmente se produce antes de la ejecución del conjunto de aptitudes con archivos que se pueden haber generado otras aptitudes.

> [!NOTE]
> A medida que expanda el ámbito aumentando la frecuencia de procesamiento, agregando más documentos o agregando más algoritmos de IA, tendrá que [asociar un recurso facturable de Cognitive Services](cognitive-search-attach-cognitive-services.md). Los cargos se acumulan cuando se llama a las API de Cognitive Services y para la extracción de imágenes como parte de la fase de descifrado de documentos en la indexación. No hay ningún cargo por la extracción de texto de documentos.
>
> La ejecución de aptitudes integradas se cobra según los [precios de pago por uso de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) existentes. Los precios de la extracción de imágenes se describen en la [página de precios](https://azure.microsoft.com/pricing/details/search/).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.DocumentExtractionSkill

## <a name="skill-parameters"></a>Parámetros de la aptitud

Los parámetros distinguen mayúsculas de minúsculas.

| Entradas            | Valores permitidos | Descripción |
|-----------------|----------------|-------------|
| `parsingMode`   | `default` <br/> `text` <br/> `json`  | Se establece en `default` para la extracción de documentos desde archivos que no son de texto puro o JSON. Se establece en `text` para mejorar el rendimiento en los archivos de texto sin formato. Se establece en `json` para extraer contenido estructurado de los archivos JSON. Si `parsingMode` no se define explícitamente, se establecerá en `default`. |
| `dataToExtract` | `contentAndMetadata` <br/> `allMetadata` | Se establece en `contentAndMetadata` para extraer todos los metadatos y el contenido textual de cada archivo. Se establece en `allMetadata` para extraer solo los [metadatos específicos del tipo de contenido](search-howto-indexing-azure-blob-storage.md#ContentSpecificMetadata) (por ejemplo, los metadatos únicos de los archivos .png). Si `dataToExtract` no se define explícitamente, se establecerá en `contentAndMetadata`. |
| `configuration` | Véase a continuación. | Diccionario de parámetros opcionales que ajustan el modo en que se realiza la extracción de documentos. Consulta la tabla siguiente para obtener descripciones de las propiedades de configuración admitidas. |

| Parámetro de configuración   | Valores permitidos | Descripción |
|-------------------------|----------------|-------------|
| `imageAction`           | `none`<br/> `generateNormalizedImages`<br/> `generateNormalizedImagePerPage` | Se establece en `none` para ignorar las imágenes insertadas o los archivos de imagen del conjunto de datos. Este es el valor predeterminado. <br/>Para el [análisis de imágenes mediante aptitudes cognitivas](cognitive-search-concept-image-scenarios.md), se establece en `generateNormalizedImages` para que la aptitud cree una matriz de imágenes normalizadas como parte del descifrado de documentos. Esta acción requiere que `parsingMode` se establezca en `default` y `dataToExtract` se establezca en `contentAndMetadata`. Una imagen normalizada hace referencia a un procesamiento adicional que crea una imagen de salida uniforme, dimensionada y rotada para facilitar una representación consistente cuando se incluyen imágenes en resultados de búsqueda visuales (por ejemplo, fotografías del mismo tamaño en un control de gráficos, tal como se ve en la [demostración JFK](https://github.com/Microsoft/AzureSearch_JFK_Files)). Esta información se genera para cada imagen cuando se usa esta opción.  <br/>Si se establece en `generateNormalizedImagePerPage`, los archivos PDF se tratarán de manera diferente. En lugar de extraer las imágenes insertadas, cada página se representará como una imagen y se normalizará en consecuencia.  Los tipos de archivo que no son PDF se tratarán igual que si se hubiera establecido `generateNormalizedImages`.
| `normalizedImageMaxWidth` | Cualquier entero comprendido entre 50 y 10 000 | El ancho máximo (en píxeles) para las imágenes normalizadas generadas. El valor predeterminado es 2000. | 
| `normalizedImageMaxHeight` | Cualquier entero comprendido entre 50 y 10 000 | La altura máxima (en píxeles) para las imágenes normalizadas generadas. El valor predeterminado es 2000. |

> [!NOTE]
> El valor predeterminado es de 2000 píxeles para el ancho máximo de las imágenes normalizadas, y la altura se basa en los tamaños máximos admitidos por la [habilidad de OCR](cognitive-search-skill-ocr.md) y la [habilidad de análisis de imágenes](cognitive-search-skill-image-analysis.md). La [aptitud de OCR](cognitive-search-skill-ocr.md) admite un ancho y un alto máximos de 4200 para los idiomas distintos del inglés y 10 000 para el inglés.  Si aumenta los límites máximos, el procesamiento podría generar un error en imágenes de mayor tamaño en función de la definición del conjunto de aptitudes y del idioma de los documentos. 
## <a name="skill-inputs"></a>Entradas de la aptitud

| Nombre de entrada     | Descripción |
|--------------------|-------------|
| `file_data` | Archivo del que se debe extraer el contenido. |

La entrada "file_data" debe ser un objeto definido de la siguiente manera:

```json
{
  "$type": "file",
  "data": "BASE64 encoded string of the file"
}
```

Este objeto de referencia de archivo se puede generar mediante una de estas tres acciones:

 - Establecer el parámetro `allowSkillsetToReadFileData` en la definición del indexador en "true".  Así se creará la ruta de acceso `/document/file_data` que es un objeto que representa los datos del archivo original descargados del origen de datos del blob. Este parámetro solo se aplica a los datos de Blob Storage.

 - Establecer el parámetro `imageAction` en la definición del indexador en un valor distinto de `none`.  Así se crea una matriz de imágenes que sigue la convención necesaria para la entrada en esta aptitud si se pasa individualmente (es decir, `/document/normalized_images/*`).

 - En caso de tener una aptitud personalizada se devuelve un objeto JSON definido exactamente como se ha indicado anteriormente.  El parámetro `$type` debe establecerse en `file` exactamente y el parámetro `data` debe contener los datos de la matriz de bytes codificada en base 64 del contenido del archivo.

## <a name="skill-outputs"></a>Salidas de la aptitud

| Nombre de salida    | Descripción |
|--------------|-------------|
| `content` | Contenido textual del documento. |
| `normalized_images`   | Si `imageAction` se establece en un valor distinto de `none`, el nuevo campo *normalized_images* contendrá una matriz de imágenes. Consulte [la documentación de extracción de imágenes](cognitive-search-concept-image-scenarios.md) para obtener más detalles sobre el formato de salida de cada imagen. |

##  <a name="sample-definition"></a>Definición de ejemplo

```json
 {
    "@odata.type": "#Microsoft.Skills.Util.DocumentExtractionSkill",
    "parsingMode": "default",
    "dataToExtract": "contentAndMetadata",
    "configuration": {
        "imageAction": "generateNormalizedImages",
        "normalizedImageMaxWidth": 2000,
        "normalizedImageMaxHeight": 2000
    },
    "context": "/document",
    "inputs": [
      {
        "name": "file_data",
        "source": "/document/file_data"
      }
    ],
    "outputs": [
      {
        "name": "content",
        "targetName": "content"
      },
      {
        "name": "normalized_images",
        "targetName": "normalized_images"
      }
    ]
  }
```

##  <a name="sample-input"></a>Entrada de ejemplo

```json
{
  "values": [
    {
      "recordId": "1",
      "data":
      {
        "file_data": {
          "$type": "file",
          "data": "aGVsbG8="
        }
      }
    }
  ]
}
```


##  <a name="sample-output"></a>Salida de ejemplo

```json
{
  "values": [
    {
      "recordId": "1",
      "data": {
        "content": "hello",
        "normalized_images": []
      }
    }
  ]
}
```

## <a name="see-also"></a>Consulte también

+ [Aptitudes integradas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Procesamiento y extracción de información de imágenes en escenarios de búsqueda cognitiva](cognitive-search-concept-image-scenarios.md)