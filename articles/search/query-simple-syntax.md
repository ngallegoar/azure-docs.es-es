---
title: Sintaxis de consulta simplificada
titleSuffix: Azure Cognitive Search
description: Referencia de la sintaxis de consulta simple que se usa para las consultas de búsqueda de texto completo en Azure Cognitive Search.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/10/2020
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: fc1eb1836badc3ced688750bbc7c7a164773d022
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152676"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Sintaxis de consulta simple en Azure Cognitive Search

Azure Cognitive Search implementa dos lenguajes de consulta basados en Lucene: [Analizador de consultas simple](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) y [Analizador de consultas de Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). En Azure Cognitive Search, la sintaxis de consulta simple excluye las opciones de búsqueda aproximada y de desecho.  

> [!NOTE]
> La sintaxis de consulta simple se usa para las expresiones de consulta que se pasan en el parámetro **search** de la API [Buscar documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents), no se debe confundir con la [sintaxis de OData](query-odata-filter-orderby-syntax.md) que se usa para el parámetro [$filter](search-filters.md) de esa API. Estas distintas sintaxis tienen sus propias reglas para construir consultas, cadenas de escape, etc.
>
> Azure Cognitive Search proporciona una [sintaxis de consulta completa de Lucene](query-lucene-syntax.md) alternativa para consultas más complejas en el parámetro **search**. Para más información sobre la arquitectura de análisis de consulta y las ventajas de cada sintaxis, consulte [Cómo funciona la búsqueda de texto completo en Azure Cognitive Search](search-lucene-query-architecture.md).

## <a name="how-to-invoke-simple-parsing"></a>Invocación del análisis simple

La sintaxis simple es la opción predeterminada. La invocación solo es necesaria si se está restableciendo la sintaxis de completa a simple. Para establecer la sintaxis explícitamente, use el parámetro de búsqueda `queryType`. Los valores válidos incluyen `simple|full`, siendo `simple` el predeterminado, y `full` en Lucene. 

## <a name="query-behavior-anomalies"></a>Anomalías del comportamiento de las consultas

Cualquier texto con uno o varios términos se considera un punto inicial válido para la ejecución de consultas. Azure Cognitive Search encontrará coincidencias en los documentos que contengan cualquiera de los términos o todos ellos, incluidas las variaciones encontradas durante el análisis del texto. 

Tan sencillo como suena: hay un aspecto de la ejecución de consultas en Azure Cognitive Search que *podría* producir resultados inesperados, aumentando (en lugar de disminuir) los resultados de búsqueda a medida que se agregan más términos y operadores a la cadena de entrada. Que se produzca o no esta expansión realmente depende de la inclusión de un operador NOT, combinado con una configuración del parámetro `searchMode`, que determina cómo se interpreta NOT en términos de comportamientos de AND u OR. Dado el valor predeterminado, `searchMode=Any`, y un operador NOT, la operación se calcula como una acción OR, tal que `"New York" NOT Seattle` devuelve todas las ciudades que no son Seattle.  

Por lo general, es más probable ver estos comportamientos en los patrones de interacción del usuario para las aplicaciones que buscar en el contenido, donde es más probable que los usuarios incluyan un operador en una consulta, a diferencia de los sitios de comercio electrónico, que tienen estructuras de navegación más integradas. Para más información, vea [NOT operator](#not-operator) (Operador NOT). 

## <a name="boolean-operators-and-or-not"></a>Operadores booleanos (AND, OR, NOT) 

Puede insertar operadores en una cadena de consulta para crear un conjunto enriquecido de criterios en los que se encuentran los documentos coincidentes. 

### <a name="and-operator-"></a>Operador AND `+`

El operador AND es un signo más. Por ejemplo, `wifi+luxury` buscará documentos que contengan tanto `wifi` como `luxury`.

### <a name="or-operator-"></a>Operador OR `|`

El operador OR es una barra vertical o el carácter de barra vertical. Por ejemplo, `wifi | luxury` buscará documentos que contengan `wifi`, `luxury` o ambos.

<a name="not-operator"></a>

### <a name="not-operator--"></a>Operador NOT `-`

El operador NOT es un signo menos. Por ejemplo, `wifi –luxury` buscará documentos que tengan el término `wifi` y/o no tengan `luxury` (y/o se controle mediante `searchMode`).

> [!NOTE]  
>  La opción `searchMode` controla si se aplica AND u OR a un término con el operador NOT con los otros términos de la consulta en ausencia de un operador `+` o `|`. Recuerde que `searchMode` se puede establecer en `any` (opción predeterminada) o `all`. Si usa `any`, aumentará la recuperación de consultas incluyendo más resultados y, de forma predeterminada, `-` se interpretará como "OR NOT". Por ejemplo, `wifi -luxury` encontrará coincidencias en los documentos que contengan el término `wifi` o en aquellos que no contengan el término `luxury`. Si usa `all`, aumentará la precisión de las consultas incluyendo menos resultados y, de forma predeterminada, - se interpretará como "OR NOT". Por ejemplo, `wifi -luxury` encontrará coincidencias en los documentos que contengan el término `wifi` y no contengan el término "luxury". Este es posiblemente un comportamiento más intuitivo para el operador `-`. Por lo tanto, debe considerar el uso de `searchMode=all` en lugar de `searchMode=any` si desea optimizar las búsquedas para precisión en lugar de para recuperación, *y* si los usuarios utilizan con frecuencia el operador `-` en las búsquedas.

## <a name="suffix-operator"></a>Operador de sufijo

El operador de sufijo es un asterisco `*`. Por ejemplo, `lux*` buscará documentos que tengan un término que empiece por `lux`, sin distinguir entre mayúsculas y minúsculas.  

## <a name="phrase-search-operator"></a>Operador de búsquedas de frases

El operador de frase encierra una frase entre comillas `" "`. Por ejemplo, mientras que `Roach Motel` (sin comillas) buscaría documentos que contuvieran `Roach` y/o `Motel` en cualquier lugar y en cualquier orden, `"Roach Motel"` (con comillas) solo encontrará coincidencias en documentos que contengan esa frase completa junta y en ese orden (el análisis de texto sigue aplicándose).

## <a name="precedence-operator"></a>Operador de precedencia

El operador de precedencia incluye la cadena entre paréntesis `( )`. Por ejemplo, `motel+(wifi | luxury)` buscará documentos que contengan el término motel y `wifi` o `luxury` (o ambos).  

## <a name="escaping-search-operators"></a>Escape de los operadores de búsqueda  

 Para poder usar los símbolos anteriores como parte real del texto de búsqueda, se deben escapar utilizando una barra diagonal inversa como prefijo. Por ejemplo, `luxury\+hotel` dará como resultado el término `luxury+hotel`. Con el fin de simplificar las cosas para los casos más típicos, existen dos excepciones a esta regla donde la secuencia de escape no es necesaria:  

- El operador NOT `-` necesita escaparse solamente si es el primer carácter después de un espacio en blanco, no si se encuentra en medio de un término. Por ejemplo, `wi-fi` es un solo término; mientras que los GUID (como `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`) se tratan como un token único.
- El operador de sufijo `*` necesita escaparse solamente si es el último carácter antes de un espacio en blanco, no si se encuentra en medio de un término. Por ejemplo, `wi*fi` se trata como un token único.

> [!NOTE]  
>  Aunque la operación de escape mantiene los tokens juntos, el análisis de texto puede dividirlos, dependiendo del modo de análisis. Consulte [Compatibilidad con idiomas &#40;API REST de Azure Cognitive Search&#41;](index-add-language-analyzers.md) para más información.  

## <a name="see-also"></a>Consulte también  

+ [Búsqueda de documentos &#40;API REST de Azure Cognitive Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 
+ [Sintaxis de consulta de Lucene](query-lucene-syntax.md)
+ [Sintaxis de expresión de OData](query-odata-filter-orderby-syntax.md) 
