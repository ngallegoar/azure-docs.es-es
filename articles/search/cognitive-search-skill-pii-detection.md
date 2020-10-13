---
title: Aptitud cognitiva para la detección de información de identificación personal (versión preliminar)
titleSuffix: Azure Cognitive Search
description: Extraiga y enmascare información personal de un texto en una canalización de enriquecimiento en Azure Cognitive Search. Esta aptitud está actualmente en versión preliminar pública.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: acacf617d3f1d9ab891d08b32fc2dfb14deb64a4
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91540530"
---
# <a name="pii-detection-cognitive-skill"></a>Aptitud cognitiva para la detección de información de identificación personal

> [!IMPORTANT] 
> Esta aptitud está actualmente en versión preliminar pública. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Actualmente no hay compatibilidad con el portal ni con el SDK de .NET.

La aptitud para la **detección de información de identificación personal** extrae información personal de un texto de entrada y le ofrece la opción de enmascararla. Esta aptitud utiliza los modelos de aprendizaje automático proporcionados por [Text Analytics](../cognitive-services/text-analytics/overview.md) en Cognitive Services.

> [!NOTE]
> A medida que expanda el ámbito aumentando la frecuencia de procesamiento, agregando más documentos o agregando más algoritmos de IA, tendrá que [asociar un recurso facturable de Cognitive Services](cognitive-search-attach-cognitive-services.md). Los cargos se acumulan cuando se llama a las API de Cognitive Services y por la extracción de imágenes como parte de la fase de descifrado de documentos de Azure Cognitive Search. No hay ningún cargo por la extracción de texto de documentos.
>
> La ejecución de aptitudes integradas se cobra según los [precios de pago por uso de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) existentes. Los precios de la extracción de imágenes se describen en la [página de precios de Búsqueda cognitiva de Azure](https://azure.microsoft.com/pricing/details/search/).

## <a name="odatatype"></a>@odata.type

Microsoft.Skills.Text.PIIDetectionSkill

## <a name="data-limits"></a>Límites de datos

El tamaño máximo de un registro debe tener menos de 50 000 caracteres según la medición de [`String.Length`](/dotnet/api/system.string.length). Si necesita dividir los datos antes de enviarlos a la aptitud, puede usar la [aptitud División de texto](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parámetros de la aptitud

Los parámetros distinguen mayúsculas de minúsculas y son opcionales.

| Nombre de parámetro     | Descripción |
|--------------------|-------------|
| `defaultLanguageCode` |    Código de idioma del texto de entrada. Por el momento, solo se admite `en`. |
| `minimumPrecision` | Un valor entre 0,0 y 1,0. Si la puntuación de confianza (en la salida `piiEntities`) es inferior al valor `minimumPrecision` establecido, la entidad no se devuelve ni se enmascara. El valor predeterminado es 0,0. |
| `maskingMode` | Un parámetro que proporciona varias formas de enmascarar la información personal detectada en el texto especificado. Se admiten las siguientes opciones: <ul><li>`none` (predeterminado): no se produce enmascaramiento y no se devolverá la salida `maskedText`. </li><li> `redact`: quita las entidades detectadas del texto especificado y no reemplaza los valores eliminados. En este caso, el desplazamiento de la salida `piiEntities` se relacionará con el texto original, no con el texto enmascarado. </li><li> `replace`: reemplaza las entidades detectadas por el carácter que se especifica en el parámetro `maskingCharacter`. El carácter se repetirá hasta completar la longitud de la entidad detectada, con el fin de que los desplazamientos se correspondan correctamente tanto con el texto introducido como la salida `maskedText`.</li></ul> |
| `maskingCharacter` | El carácter utilizado para enmascarar el texto si el parámetro `maskingMode` está establecido en `replace`. Se admiten las siguientes opciones: `*` (predeterminada), `#` y `X`. Este parámetro solo puede ser `null` si `maskingMode` no está establecido en `replace`. |

## <a name="skill-inputs"></a>Entradas de la aptitud

| Nombre de entrada      | Descripción                   |
|---------------|-------------------------------|
| `languageCode`    | Opcional. El valor predeterminado es `en`.  |
| `text`          | Texto que se analizará.          |

## <a name="skill-outputs"></a>Salidas de la aptitud

| Nombre de salida      | Descripción                   |
|---------------|-------------------------------|
| `piiEntities` | Una matriz de tipos complejos, que contiene los siguientes campos: <ul><li>text (la información de identificación personal real que se ha extraído)</li> <li>type</li><li>subType</li><li>score (cuanto más alto sea el valor, más probable será que la entidad sea real)</li><li>offset (en el texto introducido)</li><li>length</li></ul> </br> [Aquí se pueden encontrar los valores posibles de type y subType.](../cognitive-services/text-analytics/named-entity-types.md?tabs=personal) |
| `maskedText` | Si `maskingMode` se establece en un valor distinto de `none`, esta salida será el resultado de la cadena del enmascaramiento realizado en el texto introducido, como se describe en el `maskingMode`seleccionado.  Si `maskingMode` se establece en `none`, esta salida no estará presente. |

## <a name="sample-definition"></a>Definición de ejemplo

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.PIIDetectionSkill",
    "defaultLanguageCode": "en",
    "minimumPrecision": 0.5,
    "maskingMode": "replace",
    "maskingCharacter": "*",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "piiEntities"
      },
      {
        "name": "maskedText"
      }
    ]
  }
```

## <a name="sample-input"></a>Entrada de ejemplo

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Microsoft employee with ssn 859-98-0987 is using our awesome API's."
           }
      }
    ]
}
```

## <a name="sample-output"></a>Salida de ejemplo

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "piiEntities":[ 
           { 
              "text":"859-98-0987",
              "type":"U.S. Social Security Number (SSN)",
              "subtype":"",
              "offset":28,
              "length":11,
              "score":0.65
           }
        ],
        "maskedText": "Microsoft employee with ssn *********** is using our awesome API's."
      }
    }
  ]
}
```

Los desplazamientos devueltos para las entidades en la salida de esta aptitud se devuelven directamente desde la [API de Text Analytics](../cognitive-services/text-analytics/overview.md), lo que significa que si los usa para indexar en la cadena original, debe usar la clase [StringInfo](/dotnet/api/system.globalization.stringinfo) en .NET para extraer el contenido correcto.  [Se pueden encontrar más detalles aquí](../cognitive-services/text-analytics/concepts/text-offsets.md).

## <a name="errors-and-warnings"></a>Errores y advertencias

Si el código de idioma del documento no se admite, se devuelve una advertencia y no se extrae ninguna entidad.
Si el texto está vacío, se devuelve una advertencia.
Si el texto tiene más de 50 000 caracteres, solo se analizarán los primeros 50 000 caracteres y se emitirá una advertencia.

Si la aptitud devuelve una advertencia, el valor `maskedText` de salida puede estar vacío, lo que podría afectar a las aptitudes de nivel inferior que esperasen la salida. Por esta razón, asegúrese de investigar todas las advertencias relacionadas con una salida que falta al escribir la definición del conjunto de aptitudes.

## <a name="see-also"></a>Consulte también

+ [Aptitudes integradas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)