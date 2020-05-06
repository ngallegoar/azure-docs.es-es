---
title: Términos parciales, patrones y caracteres especiales
titleSuffix: Azure Cognitive Search
description: Use consultas de caracteres comodín, expresiones regulares y prefijos para que coincidan con términos completos o parciales en una solicitud de consulta de Azure Cognitive Search. Los patrones de coincidencia compleja que incluyen caracteres especiales pueden resolverse mediante la sintaxis de consulta completa y los analizadores personalizados.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 05ff56c904fc48a1041ad40f00110a8ff0fd01f1
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82592050"
---
# <a name="partial-term-search-and-patterns-with-special-characters-wildcard-regex-patterns"></a>Búsqueda de términos parciales y patrones con caracteres especiales (carácter comodín, expresión regular y patrones)

Una *búsqueda de términos parciales* hace referencia a las consultas que se componen de fragmentos de términos, en las que en lugar de un término completo, es posible que tenga solo el inicio, el centro o el final del término (a veces se denominan consultas de prefijo, infijo o sufijo). Un *patrón* podría ser una combinación de fragmentos, a menudo con caracteres especiales, como guiones o barras diagonales, que forman parte de la cadena de consulta. Los casos de uso comunes incluyen la consulta de partes de un número de teléfono, una dirección URL, códigos de productos o personas, o palabras compuestas.

La búsqueda de términos parciales o patrones puede ser problemática si el índice no tiene términos con el formato previsto. Durante la [fase de análisis de texto](search-lucene-query-architecture.md#stage-2-lexical-analysis) de la indexación, con el analizador estándar predeterminado, los caracteres especiales se descartan, las cadenas compuestas se dividen y se eliminan los espacios en blanco, lo que puede provocar un error en las consultas de patrones cuando no se encuentra ninguna coincidencia. Por ejemplo, un número de teléfono como `+1 (425) 703-6214` (tokenizado como `"1"`, `"425"`, `"703"`, `"6214"`) no se mostrará en una consulta `"3-62"` porque ese contenido no existe realmente en el índice. 

La solución consiste en invocar un analizador que conserva una cadena completa, incluidos espacios y caracteres especiales, si es necesario, para que pueda hacer coincidir términos parciales y patrones. La creación de un campo adicional para una cadena intacta, además de usar un analizador de preservación de contenido, es la base de la solución.

> [!TIP]
> ¿Está familiarizado con las API REST y Postman? [Descargue la colección de ejemplos de consultas](https://github.com/Azure-Samples/azure-search-postman-samples/) para consultar los términos parciales y caracteres especiales que se describen en este artículo.

## <a name="what-is-partial-search-in-azure-cognitive-search"></a>Definición de la búsqueda parcial en Azure Cognitive Search

En Azure Cognitive Search, la búsqueda parcial y el patrón están disponibles con estos formatos:

+ [Búsqueda de prefijo](query-simple-syntax.md#prefix-search), como `search=cap*`, coincidente con "Cap'n Jack's Waterfront Inn" o "Gacc Capital". Puede usar la sintaxis de consulta simple o la sintaxis de consulta completa de Lucene para la búsqueda de prefijos.

+ [Búsqueda con caracteres comodín](query-lucene-syntax.md#bkmk_wildcard) o [Expresiones regulares](query-lucene-syntax.md#bkmk_regex), que buscan un patrón o partes de una cadena insertada. Los caracteres comodín y las expresiones regulares requieren la sintaxis de Lucene completa. Las consultas de sufijo e índice se formulan como una expresión regular.

  Algunos ejemplos de búsqueda de términos parciales son los siguientes. En el caso de una consulta de sufijo, dado el término "alfanumérico", usaría una búsqueda con caracteres comodín (`search=/.*numeric.*/`) para buscar una coincidencia. Para un término parcial que incluye caracteres interiores, como un fragmento de dirección URL, puede que tenga que agregar caracteres de escape. En JSON, una barra diagonal `/` se escapa con una barra diagonal inversa `\`. Así, `search=/.*microsoft.com\/azure\/.*/` es la sintaxis del fragmento de dirección URL "microsoft.com/azure/".

Como hemos indicado, todo lo anterior requiere que el índice contenga cadenas en un formato favorable para la coincidencia de patrones, algo que el analizador estándar no proporciona. Al seguir los pasos de este artículo, puede asegurarse de que existe el contenido necesario para admitir estos escenarios.

## <a name="solving-partialpattern-search-problems"></a>Solución de problemas de búsqueda de términos parciales o patrones

Si necesita buscar en fragmentos, patrones o caracteres especiales, puede invalidar el analizador predeterminado con un analizador personalizado que funcione con reglas de tokenización más sencillas y conserve toda la cadena. Si realiza un paso atrás, el enfoque tiene el siguiente aspecto:

+ Defina un campo para almacenar una versión intacta de la cadena (suponiendo que desee analizar texto analizado y sin analizar).
+ Elija un analizador predefinido o defina un analizador personalizado que genere una cadena intacta no analizada.
+ Asignación del analizador personalizado al campo
+ Compile y pruebe el índice.

> [!TIP]
> La evaluación de analizadores es un proceso iterativo que requiere recompilaciones frecuentes de índices. Para simplificar este paso, puede usar Postman y las API REST para [crear un índice](https://docs.microsoft.com/rest/api/searchservice/create-index), [eliminar un índice](https://docs.microsoft.com/rest/api/searchservice/delete-index),[cargar documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)y [buscar documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents). Para la carga de documentos, el cuerpo de la solicitud debe contener un pequeño conjunto de datos representativo que quiera probar (por ejemplo, un campo con números de teléfono o códigos de producto). Con estas API en la misma colección de Postman, puede seguir estos pasos rápidamente.

## <a name="duplicate-fields-for-different-scenarios"></a>Campos duplicados para diferentes escenarios

Los analizadores se asignan por campo, lo que significa que puede crear campos en el índice para optimizar los distintos escenarios. En concreto, puede definir "featureCode" y "featureCodeRegex" para admitir la búsqueda de texto completo normal en el primero y la coincidencia de patrones avanzada en el segundo.

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
  "analyzer": "my_custom_analyzer"
},
```

## <a name="choose-an-analyzer"></a>Selección de un analizador

Al elegir un analizador que produce tokens de términos completos, los siguientes analizadores son opciones comunes:

| Analizador | Comportamientos |
|----------|-----------|
| [Analizadores de idiomas](index-add-language-analyzers.md) | Conserva los guiones en palabras compuestas o cadenas, mutaciones vocálicas y formas verbales. Si los patrones de consulta incluyen guiones, el uso de un analizador de idioma puede ser suficiente. |
| [keyword](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) | El contenido de todo el campo se acorta para pasar a ser un solo término. |
| [whitespace](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html) | Solo se separa en espacios en blanco. Los términos que incluyen guiones u otros caracteres se tratan como un solo token. |
| [analizador personalizado](index-add-custom-analyzers.md) | (Recomendado) La creación de un analizador personalizado le permite especificar el filtro de tokens y el tokenizador. Los analizadores anteriores deben usarse tal cual. Un analizador personalizado le permite elegir los filtros de token y los tokenizadores que quiere usar. <br><br>Una combinación recomendada es el [tokenizador de palabras claves](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html) con un [filtro de tokens en minúsculas](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html). Por sí solo, el [analizador de palabras clave](https://lucene.apache.org/core/6_6_1/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html) predefinido no pasa a minúsculas ningún texto en mayúsculas, lo que puede provocar errores en las consultas. Un analizador personalizado le ofrece un mecanismo para agregar el filtro de tokens en minúsculas. |

Si usa una herramienta de prueba de API web como Postman, puede agregar la [llamada de REST del analizador de pruebas](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) para inspeccionar la salida con tokens.

Debe tener un índice existente con el que trabajar. Si se da una situación en que tiene un índice y un campo que contiene guiones o términos parciales, puede probar varios analizadores en términos específicos para ver qué tokens se emiten.  

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
> Tenga en cuenta que los analizadores de consultas a menudo usan términos en minúsculas en una expresión de búsqueda al compilar el árbol de consulta. Si usa un analizador que no introduce las entradas de texto en minúsculas y no obtiene los resultados esperados, este podría ser el motivo. La solución consiste en agregar un filtro de tokens en minúsculas, tal como se describe en la sección "Uso de analizadores personalizados" más adelante.

## <a name="configure-an-analyzer"></a>Configuración de un analizador
 
Tanto si está evaluando analizadores como si avanza con una configuración concreta, tendrá que especificar el analizador en la definición del campo y, posiblemente, deberá configurar el analizador en sí si no usa ningún analizador integrado. Al intercambiar analizadores, normalmente debe volver a generar el índice (quitarlo, volverlo a crear y cargarlo de nuevo). 

### <a name="use-built-in-analyzers"></a>Uso de analizadores integrados

Los analizadores predefinidos o integrados se pueden especificar por su nombre en una propiedad `analyzer` de una definición de campo, sin necesidad de establecer configuración adicional en el índice. En el ejemplo siguiente se muestra cómo debería establecer el analizador `whitespace` en un campo. 

Para ver otros escenarios y obtener más información sobre otros analizadores integrados, consulte [Lista de analizadores predefinidos](https://docs.microsoft.com/azure/search/index-add-custom-analyzers#predefined-analyzers-reference). 

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

### <a name="use-custom-analyzers"></a>Uso de analizadores personalizados

Si usa un [analizador personalizado](index-add-custom-analyzers.md), debe definirlo en el índice con una combinación definida por el usuario de tokenizador, filtro de tokens y posibles valores de configuración. A continuación, haga referencia a él en una definición de campo, tal como lo haría con un analizador integrado.

Cuando el objetivo sea la tokenización de términos completos, le recomendamos que use un analizador personalizado que conste de un **tokenizador de palabras clave** y un **filtro de tokens en minúsculas**.

+ El tokenizador de palabras clave crea un solo token para todo el contenido de un campo.
+ El filtro de tokens en minúsculas transforma las letras mayúsculas en texto en minúsculas. Los analizadores de consultas suelen convertir en minúsculas cualquier entrada de texto en mayúsculas. El uso de minúsculas homogeneiza las entradas con los términos acortados.

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

## <a name="build-and-test"></a>Compilación y prueba

Una vez que haya definido un índice con analizadores y definiciones de campo que admitan su escenario, cargue documentos que tengan cadenas representativas para poder probar consultas de cadenas parciales. 

La lógica se explicó en las secciones anteriores. En esta sección se explican las API que debe llamar al probar la solución. Como se indicó anteriormente, si usa una herramienta de prueba web interactiva, como Postman, puede realizar estas tareas rápidamente.

+ [Eliminación de índice](https://docs.microsoft.com/rest/api/searchservice/delete-index) quita un índice existente con el mismo nombre para que pueda volver a crearlo.

+ [Creación de índice](https://docs.microsoft.com/rest/api/searchservice/create-index) crea la estructura de índice en el servicio de búsqueda, incluidas las definiciones y los campos, así como una especificación, del analizador.

+ [Carga de documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) importa documentos que tienen la misma estructura que el índice, así como contenido que se puede buscar. Después de este paso, el índice está listo para realizar consultas o pruebas.

+ El [analizador de pruebas](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) se presentó en [Selección de un analizador](#choose-an-analyzer). Pruebe algunas de las cadenas del índice con diversos analizadores para entender cómo se acortan los términos.

+ [Búsqueda de documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents) explica cómo construir una solicitud de consulta mediante una [sintaxis simple](query-simple-syntax.md) o la [sintaxis de Lucene completa](query-lucene-syntax.md) para caracteres comodín y expresiones regulares.

  En el caso de las consultas de términos parciales, como la consulta de "3-6214" para encontrar una coincidencia en "+ 1 (425) 703-6214", puede usar la sintaxis simple: `search=3-6214&queryType=simple`.

  En el caso de las consultas de infijos y de sufijos, como consultar "num" o "numéric" para encontrar una coincidencia en "alfanumérico", use la sintaxis completa de Lucene y una expresión regular: `search=/.*num.*/&queryType=full`

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

## <a name="next-steps"></a>Pasos siguientes

En este artículo se explica cómo contribuyen ambos analizadores a los problemas de consultas y a cómo solucionarlos. A continuación, examine más detenidamente el impacto del analizador en la indexación y el procesamiento de consultas. En concreto, considere la posibilidad de usar la API de análisis de texto para devolver la salida con tokens, de modo que pueda ver exactamente lo que crea un analizador para el índice.

+ [Analizadores de idiomas](search-language-support.md)
+ [Analizadores para el procesamiento de texto en Azure Cognitive Search](search-analyzers.md)
+ [API de análisis de texto (REST)](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)
+ [Cómo funciona la búsqueda de texto completo (arquitectura de consultas)](search-lucene-query-architecture.md)