---
title: Información general sobre similitud y puntuación
titleSuffix: Azure Cognitive Search
description: En este tema se explican los conceptos de similitud y puntuación, y lo que un desarrollador puede hacer para personalizar el resultado de la puntuación.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 9f9cc4c29b117c83595a36c4e28b1edb428c3cde
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82254065"
---
# <a name="similarity-and-scoring-in-azure-cognitive-search"></a>Similitud y puntuación en Azure Cognitive Search

La puntuación hace referencia al cálculo de una puntuación de búsqueda de todos los artículos devueltos en los resultados de la búsqueda de las consultas de búsqueda de texto completo. La puntuación es un indicador de la importancia de un elemento en el contexto de la operación de búsqueda actual. Cuanto mayor sea la puntuación, mayor importancia tendrá el elemento. En los resultados de búsqueda, los artículos están ordenados de mayor a menor, según las puntuaciones de búsqueda calculadas para cada uno. 

De forma predeterminada, se devuelven los 50 mejores en la respuesta, pero puede usar el parámetro **$top** para devolver un número mayor o menor de elementos (hasta 1000 en una sola respuesta) y **$skip** para obtener el siguiente conjunto de resultados.

La puntuación de búsqueda se calcula en función de las propiedades estadísticas de los datos y la consulta. Azure Cognitive Search busca documentos que coinciden con los términos de búsqueda (algunos o todos, dependiendo de [searchMode](https://docs.microsoft.com/rest/api/searchservice/search-documents#searchmodeany--all-optional)), lo cual favorece a los documentos que contienen muchas instancias del término de búsqueda. La puntuación de búsqueda aumenta todavía más si el término es poco frecuente en el índice de datos, pero común dentro del documento. La base de este enfoque de relevancia informática se denomina *TF-IDF* o frecuencia de documento inverso de la frecuencia del término.

Los valores de puntuación de búsqueda pueden repetirse a lo largo de un conjunto de resultados. Cuando varios resultados tienen la misma puntuación de búsqueda, el orden de estos elementos puntuados no se define y no es estable. Vuelva a ejecutar la consulta y podrá ver que los elementos cambian de posición, especialmente si usa el servicio gratuito o un servicio facturable con varias réplicas. Si dos elementos disponen de la misma puntuación, no hay ninguna garantía de cuál aparecerá en primer lugar.

Si desea romper el vínculo entre las puntuaciones repetidas, puede agregar una cláusula **$orderby** para ordenar primero por puntuación y, a continuación, ordenar por otro campo que se pueda ordenar (por ejemplo, `$orderby=search.score() desc,Rating desc`). Para obtener más información, consulte [$orderby](https://docs.microsoft.com/azure/search/search-query-odata-orderby).

> [!NOTE]
> `@search.score = 1.00` indica un conjunto de resultados sin puntuar o sin clasificar. La puntuación es uniforme en todos los resultados. Los resultados sin puntuar se producen cuando el formulario de consulta es una búsqueda aproximada, una consulta con caracteres comodín o regex, o una expresión **$filter**. 

## <a name="scoring-profiles"></a>Perfiles de puntuación

Puede personalizar la forma en que se clasifican los distintos campos mediante la definición de un *perfil de puntuación* personalizado. Los perfiles de puntuación proporcionan un mayor control sobre la clasificación de los elementos en los resultados de búsqueda. Por ejemplo, desea aumentar los elementos según su potencial de ingresos, promover elementos más recientes o quizás aumentar los elementos que han permanecido en inventario demasiado largo. 

Un perfil de puntuación es parte de la definición del índice que se compone de campos, funciones y parámetros ponderados. Para obtener más información sobre cómo definir uno, consulte [Perfiles de puntuación](index-add-scoring-profiles.md).

## <a name="scoring-statistics"></a>Estadísticas de puntuación

Para ofrecer escalabilidad, Azure Cognitive Search distribuye cada índice horizontalmente a través de un proceso de particionamiento, lo que significa que las partes de un índice están físicamente separadas.

De forma predeterminada, la puntuación de un documento se calcula en función de las propiedades estadísticas de los datos *de una partición*. Este enfoque no suele ser un problema para una gran corpus de datos y proporciona un mejor rendimiento que el cálculo de la puntuación basado en la información de todas las particiones. Dicho esto, el uso de esta optimización del rendimiento puede provocar que dos documentos muy similares (o incluso idénticos) terminen con puntuaciones de relevancia diferentes si acaban en diferentes particiones.

Si prefiere calcular la puntuación en función de las propiedades estadísticas de todas las particiones, para hacerlo puede agregar *scoringStatistics=global* como un [parámetro de consulta](https://docs.microsoft.com/rest/api/searchservice/search-documents) (o agregar *"scoringStatistics": "global"* como un parámetro del cuerpo de la [solicitud de consulta](https://docs.microsoft.com/rest/api/searchservice/search-documents)).

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?scoringStatistics=global
  Content-Type: application/json
  api-key: [admin key]  
```

> [!NOTE]
> Se requiere una clave de API de administración para el parámetro `scoringStatistics`.

## <a name="similarity-ranking-algorithms"></a>Algoritmo de clasificación de similitud

Azure Cognitive Search admite dos algoritmos de clasificación de similitud diferentes: un algoritmo de *similitud clásico* y la implementación oficial del algoritmo *Okapi BM25* (actualmente en versión preliminar). El algoritmo de similitud clásico es el predeterminado, pero los nuevos servicios creados a partir del 15 de julio usarán el nuevo algoritmo BM25. Será el único algoritmo disponible en los nuevos servicios.

Por ahora, puede especificar el algoritmo de clasificación de similitud que le gustaría usar. Para obtener más información, consulte [Algoritmo de clasificación](index-ranking-similarity.md).

## <a name="watch-this-video"></a>Vea este vídeo

En este vídeo de 16 minutos, el ingeniero de software Raouf Merouche explica el proceso de indexación, consulta y creación de perfiles de puntuación. Le ofrece una buena idea de lo que está ocurriendo entre bambalinas mientras los documentos se indexan y recuperan.

>[!VIDEO https://channel9.msdn.com/Shows/AI-Show/Similarity-and-Scoring-in-Azure-Cognitive-Search/player]

+ 2-3 minutos de cobertura de la indexación: procesamiento de texto y análisis léxico.
+ 3-4 minutos de cobertura de la indexación: índices invertidos.
+ 4-6 minutos de cobertura de las consultas: recuperación y clasificación.
+ 7 - 16 minutos de cobertura de los perfiles de puntuación.

## <a name="see-also"></a>Consulte también

 [Perfiles de puntuación](index-add-scoring-profiles.md) [Referencia de la API de REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [API de búsqueda de documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents)   
 [SDK de .NET de Azure Cognitive Search](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
