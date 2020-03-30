---
title: Coincidencia de patrones y caracteres especiales
titleSuffix: Azure Cognitive Search
description: Use consultas de caracteres comodín y prefijos para que coincidan con términos completos o parciales en una solicitud de consulta de Azure Cognitive Search. Los patrones de coincidencia compleja que incluyen caracteres especiales pueden resolverse mediante la sintaxis de consulta completa y los analizadores personalizados.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: f78ba5b351a3da46d7b8b3780cf00772c4f3b2ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289318"
---
# <a name="match-on-patterns-and-special-characters-dashes"></a>Coincidencia de patrones y caracteres especiales (guiones)

En el caso de las consultas que incluyen caracteres especiales (`-, *, (, ), /, \, =`), o en relación con los patrones de consulta basados en términos parciales dentro de un período mayor, normalmente se necesitan pasos de configuración adicionales a fin de asegurarse de que el índice incluye el contenido esperado en el formato correcto. 

De forma predeterminada, un número de teléfono como `+1 (425) 703-6214` se acorta de la manera siguiente: `"1"`, `"425"`, `"703"`, `"6214"`. Como puede imaginarse, se producirá un error en la búsqueda de `"3-62"` (términos parciales que incluyen un guión), ya que ese contenido realmente no existe en el índice. 

Cuando necesite realizar búsquedas en cadenas parciales o caracteres especiales, puede invalidar el analizador predeterminado con un analizador personalizado que funcione con reglas de tokenización más sencillas y mantener los términos completos, lo que resulta necesario cuando las cadenas de consulta incluyen partes de un término o caracteres especiales. Si realiza un paso atrás, el enfoque tiene el siguiente aspecto:

+ Elija un analizador predefinido o defina un analizador personalizado que genere el resultado deseado.
+ Asigne el analizador al campo.
+ Compile el índice y la prueba.

En este artículo se explica cómo debe realizar estas tareas. El enfoque que se describe aquí resulta útil en otros escenarios, ya que las consultas de expresiones regulares y caracteres comodín también necesitan términos completos como base para la coincidencia de patrones. 

> [!TIP]
> La evaluación de analizadores es un proceso iterativo que requiere recompilaciones frecuentes de índices. Para simplificar este paso, puede usar Postman y las API REST para [crear un índice](https://docs.microsoft.com/rest/api/searchservice/create-index), [eliminar un índice](https://docs.microsoft.com/rest/api/searchservice/delete-index),[cargar documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)y [buscar documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents). Para la carga de documentos, el cuerpo de la solicitud debe contener un pequeño conjunto de datos representativo que quiera probar (por ejemplo, un campo con números de teléfono o códigos de producto). Con estas API en la misma colección de Postman, puede seguir estos pasos rápidamente.

## <a name="choosing-an-analyzer"></a>Elección de un analizador

Al elegir un analizador que produce tokens de términos completos, los siguientes analizadores son opciones comunes:

| Analizador | Comportamientos |
|----------|-----------|
| [keyword](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | El contenido de todo el campo se acorta para pasar a ser un solo término. |
| [whitespace](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | Solo se separa en espacios en blanco. Los términos que incluyen guiones u otros caracteres se tratan como un solo token. |
| [analizador personalizado](index-add-custom-analyzers.md) | (Recomendado) La creación de un analizador personalizado le permite especificar el filtro de tokens y el tokenizador. Los analizadores anteriores deben usarse tal cual. Un analizador personalizado le permite elegir los filtros de token y los tokenizadores que quiere usar. <br><br>Una combinación recomendada es el [tokenizador de palabras claves](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html) con un [filtro de tokens en minúsculas](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html). Por sí solo, el [analizador de palabras clave](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) predefinido no pasa a minúsculas ningún texto en mayúsculas, lo que puede provocar errores en las consultas. Un analizador personalizado le ofrece un mecanismo para agregar el filtro de tokens en minúsculas. |

Si usa una herramienta de prueba de API web como Postman, puede agregar la [llamada de REST del analizador de pruebas](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) para inspeccionar la salida con tokens. Si se da una situación en que tiene un índice y un campo que contiene guiones o términos parciales, puede probar varios analizadores en términos específicos para ver qué tokens se emiten.  

1. Compruebe el analizador estándar para ver cómo se acortan los términos de forma predeterminada.

   ```json
   {
   "text": "SVP10-NOR-00",
   "analyzer": "standard"
   }
    ```

1. Evalúe la respuesta para ver cómo se acorta el texto en el índice. Observe que cada término está en minúsculas y dividido.

    ```json
    {
        "tokens": [
            {
                "token": "svp10",
                "startOffset": 0,
                "endOffset": 5,
                "position": 0
            },
            {
                "token": "nor",
                "startOffset": 6,
                "endOffset": 9,
                "position": 1
            },
            {
                "token": "00",
                "startOffset": 10,
                "endOffset": 12,
                "position": 2
            }
        ]
    }
    ```
1. Modifique la solicitud para usar el analizador `whitespace` o `keyword`:

    ```json
    {
    "text": "SVP10-NOR-00",
    "analyzer": "keyword"
    }
    ```

1. Ahora la respuesta se compone de un solo token escrito en mayúsculas con guiones que se mantienen como parte de la cadena. Si necesita realizar una búsqueda en un patrón o un término parcial, el motor de consultas ahora cuenta con la base para encontrar una coincidencia.


    ```json
    {

        "tokens": [
            {
                "token": "SVP10-NOR-00",
                "startOffset": 0,
                "endOffset": 12,
                "position": 0
            }
        ]
    }
    ```
> [!Important]
> Tenga en cuenta que los analizadores de consultas a menudo usan términos en minúsculas en una expresión de búsqueda al compilar el árbol de consulta. Si usa un analizador que no introduce las entradas de texto en minúsculas y no obtiene los resultados esperados, este podría ser el motivo. La solución consiste en agregar un filtro de token de caracteres en minúsculas.

## <a name="analyzer-definitions"></a>Definiciones del analizador
 
Tanto si está evaluando analizadores como si avanza con una configuración concreta, tendrá que especificar el analizador en la definición del campo y, posiblemente, deberá configurar el analizador en sí si no usa ningún analizador integrado. Al intercambiar analizadores, normalmente debe volver a generar el índice (quitarlo, volverlo a crear y cargarlo de nuevo). 

### <a name="use-built-in-analyzers"></a>Uso de analizadores integrados

Los analizadores predefinidos o integrados se pueden especificar por su nombre en una propiedad `analyzer` de una definición de campo, sin necesidad de establecer configuración adicional en el índice. En el ejemplo siguiente se muestra cómo debería establecer el analizador `whitespace` en un campo.

```json
    {
      "name": "phoneNumber",
      "type": "Edm.String",
      "key": false,
      "retrievable": true,
      "searchable": true,
      "analyzer": "whitespace"
    }
```
Para obtener más información acerca de todos los analizadores integrados disponibles, consulte [Lista de analizadores predefinidos](https://docs.microsoft.com/azure/search/index-add-custom-analyzers#predefined-analyzers-reference). 

### <a name="use-custom-analyzers"></a>Uso de analizadores personalizados

Si usa un [analizador personalizado](index-add-custom-analyzers.md), debe definirlo en el índice con una combinación definida por el usuario de tokenizador, tokenfilter, con posibles valores de configuración. A continuación, haga referencia a él en una definición de campo, tal como lo haría con un analizador integrado.

Cuando el objetivo sea la tokenización de términos completos, le recomendamos que use un analizador personalizado que conste de un **tokenizador de palabras clave** y un **filtro de tokens en minúsculas**.

+ El tokenizador de palabras clave crea un solo token para todo el contenido de un campo.
+ El filtro de tokens en minúsculas transforma las letras mayúsculas en texto en minúsculas. Los analizadores de consultas suelen convertir en minúsculas cualquier entrada de texto en mayúsculas. El uso de minúsculas homogeneiza las entradas con los términos con tokens.

En el ejemplo siguiente se muestra un analizador personalizado que proporciona el tokenizador de palabras clave y un filtro de tokens en minúsculas.

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
],

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": []
```

> [!NOTE]
> El sistema conoce el tokenizador `keyword_v2` y el filtro de tokens `lowercase` y estos utilizan sus configuraciones predeterminadas, por lo que puede hacer referencia a ellos por su nombre sin tener que definirlos primero.

## <a name="tips-and-best-practices"></a>Sugerencias y prácticas recomendadas

### <a name="tune-query-performance"></a>Ajustar rendimiento de consulta

Si implementa la configuración recomendada que incluye el tokenizador keyword_v2 y el filtro de tokens en minúsculas, es posible que observe una disminución en el rendimiento de las consultas debido al procesamiento adicional de los filtros de tokens en los tokens existentes en el índice. 

En el ejemplo siguiente se agrega el elemento [EdgeNGramTokenFilter](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html) para crear coincidencias de prefijo de manera más rápida. Se generan tokens adicionales para combinaciones de 2 a 25 caracteres que incluyen los caracteres siguientes: (no solo MS, MSF, MSFT, MSFT/, MSFT/S, MSFT/SQ, MSFT/SQL). Como puede imaginar, la tokenización adicional da como resultado un índice mayor.

```json
{
"fields": [
  {
  "name": "accountNumber",
  "analyzer":"myCustomAnalyzer",
  "type": "Edm.String",
  "searchable": true,
  "filterable": true,
  "retrievable": true,
  "sortable": false,
  "facetable": false
  }
],

"analyzers": [
  {
  "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
  "name":"myCustomAnalyzer",
  "charFilters":[],
  "tokenizer":"keyword_v2",
  "tokenFilters":["lowercase", "my_edgeNGram"]
  }
],
"tokenizers":[],
"charFilters": [],
"tokenFilters": [
  {
  "@odata.type":"#Microsoft.Azure.Search.EdgeNGramTokenFilterV2",
  "name":"my_edgeNGram",
  "minGram": 2,
  "maxGram": 25,
  "side": "front"
  }
]
```

### <a name="use-different-analyzers-for-indexing-and-query-processing"></a>Uso de analizadores diferentes para las operaciones de indexación y procesamiento de consultas

La llamada a los analizadores se realiza durante la indexación y la ejecución de la consulta. Es habitual usar el mismo analizador para ambos, pero puede configurar analizadores personalizados para cada carga de trabajo. Las invalidaciones del analizador se especifican en la [definición del índice](https://docs.microsoft.com/rest/api/searchservice/create-index) en la sección `analyzers` y, a continuación, se hace referencia a ellos en campos específicos. 

Cuando el análisis personalizado solo es necesario durante la indexación, puede aplicar el analizador personalizado solo a la indexación y seguir usando el analizador estándar de Lucene (u otro) para las consultas.

Para determinar el análisis específico de roles, puede establecer propiedades en el campo para cada uno. Para ello, establezca `indexAnalyzer` y `searchAnalyzer` en lugar de la propiedad `analyzer` predeterminada.

```json
"name": "featureCode",
"indexAnalyzer":"my_customanalyzer",
"searchAnalyzer":"standard",
```

### <a name="duplicate-fields-for-different-scenarios"></a>Campos duplicados para diferentes escenarios

Hay otra opción en que se la asignación del analizador por campo para la optimización en diferentes escenarios. En concreto, puede definir "featureCode" y "featureCodeRegex" para admitir la búsqueda de texto completo normal en el primero y la coincidencia de patrones avanzada en el segundo.

```json
{
  "name": "featureCode",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": null
},
{
  "name": "featureCodeRegex",
  "type": "Edm.String",
  "retrievable": true,
  "searchable": true,
  "analyzer": "my_customanalyzer"
},
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se explica cómo contribuyen ambos analizadores a los problemas de consultas y a cómo solucionarlos. A continuación, examine más detenidamente el impacto del analizador en la indexación y el procesamiento de consultas. En concreto, considere la posibilidad de usar la API de análisis de texto para devolver la salida con tokens, de modo que pueda ver exactamente lo que crea un analizador para el índice.

+ [Analizadores de idiomas](search-language-support.md)
+ [Analizadores para el procesamiento de texto en Azure Cognitive Search](search-analyzers.md)
+ [API de análisis de texto (REST)](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)
+ [Cómo funciona la búsqueda de texto completo (arquitectura de consultas)](search-lucene-query-architecture.md)
