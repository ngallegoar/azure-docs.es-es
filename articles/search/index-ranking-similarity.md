---
title: Algoritmo de similitud de clasificación
titleSuffix: Azure Cognitive Search
description: Cómo establecer el algoritmo de similitud para probar el nuevo algoritmo de similitud para la clasificación
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/13/2020
ms.openlocfilehash: c327440649300533c94c2a1956e3c45f433c9780
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409962"
---
# <a name="ranking-algorithm-in-azure-cognitive-search"></a>Algoritmo de clasificación de Azure Cognitive Search

> [!IMPORTANT]
> A partir del 15 de julio de 2020, los servicios de búsqueda recién creados utilizarán la función de clasificación BM25 que, en la mayoría de los casos, ha demostrado proporcionar clasificaciones de búsqueda que se ajustan mejor a las expectativas de los usuarios que la clasificación predeterminada actual.  Más allá de la clasificación superior, BM25 también habilita las opciones de configuración para optimizar los resultados en función de factores como el tamaño del documento.  
>
> Con este cambio, lo más probable es que vea pequeños cambios en el orden de los resultados de la búsqueda.   Para aquellos que quieran probar el impacto de este cambio, hemos incluido en la API 2019-05-06-Preview la posibilidad de habilitar la puntuación de BM25 en los nuevos índices.  

En este artículo se describe cómo puede actualizar un servicio creado antes del 15 de julio de 2020 para usar el nuevo algoritmo de clasificación BM25.

Azure Cognitive Search usará la implementación oficial de Lucene del algoritmo BM25 de Okapi, *BM25Similarity*, que reemplazará a la implementación de *ClassicSimilarity* usada anteriormente. Al igual que el algoritmo ClassicSimilarity anterior, BM25Similarity es una función de recuperación de tipo TF-IDF que usa la frecuencia de términos (TF) y la frecuencia de documentos inversos (IDF) como variables para calcular las puntuaciones de relevancia de cada par documento-consulta, que posteriormente se usa para la clasificación. A pesar de que conceptualmente es similar al algoritmo Classic Similarity más antiguo, BM25 toma su raíz de la recuperación de información probabilística para mejorarla. BM25 también ofrece opciones de personalización avanzadas, como permitir que el usuario decida cómo escala la puntuación de relevancia con la frecuencia de los términos coincidentes.

## <a name="how-to-test-bm25-today"></a>Cómo probar BM25 hoy

Al crear un nuevo índice, puede establecer una propiedad "similarity". Tendrá que usar la versión *2019-05-06-Preview*, como se muestra a continuación.

```
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2019-05-06-Preview
```

```json  
{
    "name": "indexName",
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": true
        },
        {
            "name": "name",
            "type": "Edm.String",
            "searchable": true,
            "analyzer": "en.lucene"
        },
        ...
    ],
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity"
    }
}
```

En el caso de los servicios creados antes del 15 de julio de 2020: Si se omite la similitud o se establece en null, el índice usará el antiguo algoritmo de similitud clásico.

En el caso de los servicios creados después del 15 de julio de 2020: Si se omite la similitud o se establece en null, el índice usará el nuevo algoritmo de similitud BM25.

También puede establecer explícitamente el valor de similitud en uno de los dos valores siguientes: *"#Microsoft.Azure.Search.ClassicSimilarity"* o *"#Microsoft.Azure.Search.BM25Similarity"* .


## <a name="bm25-similarity-parameters"></a>Parámetros de similitud de BM25

La similitud de BM25 agrega dos parámetros personalizables por el usuario para controlar la puntuación de relevancia calculada:

### <a name="k1"></a>k1

El parámetro *k1* controla la función de escalado entre la frecuencia de cada uno de los términos coincidentes y la puntuación de relevancia final de un par documento-consulta.

Un valor de cero representa un "modelo binario", en el que la contribución de un único término coincidente es la misma para todos los documentos coincidentes, independientemente del número de veces que ese término aparezca en el texto, mientras que un valor k1 mayor permite que la puntuación siga aumentando a medida que se encuentren más instancias del mismo término en el documento. De forma predeterminada, Azure Cognitive Search usa un valor de 1,2 para el parámetro k1. El uso de un valor de k1 mayor puede ser importante en los casos en los que se espera que varios términos formen parte de una consulta de búsqueda. En estos casos, podemos favorecer documentos que coincidan con muchos de los diferentes términos de consulta buscados frente a los documentos que solo coinciden con uno varias veces. Por ejemplo, al consultar el índice de los documentos que contienen los términos "Apollo Spaceflight", podemos reducir la puntuación de un artículo sobre la mitología griega que contiene el término "Apollo" algunas decenas de veces sin menciones a "Spaceflight", en comparación con otro artículo que menciona explícitamente "Apollo" y "Spaceflight" solo unas pocas veces. 
 
### <a name="b"></a>b

El parámetro *b* controla cómo afecta la longitud de un documento a la puntuación de relevancia.

Un valor de 0,0 significa que la longitud del documento no influye en la puntuación, mientras que un valor de 1,0 significa que el impacto de la frecuencia de los términos en la puntuación de relevancia se normalizará según la longitud del documento. El valor predeterminado que se usa en Azure Cognitive Search para el parámetro b es 0,75. La normalización de la frecuencia de los términos según la longitud del documento es útil en los casos en los que queremos penalizar los documentos más largos. En algunos casos, es más probable que los documentos más largos (como una novela completa) contengan muchos términos irrelevantes, en comparación con documentos mucho más cortos.

### <a name="setting-k1-and-b-parameters"></a>Establecimiento de los parámetros k1 y b

Para personalizar los valores de b o k1, simplemente agréguelos como propiedades al objeto de similitud cuando use BM25:

```json
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity",
        "b" : 0.5,
        "k1" : 1.3
    }
```

El algoritmo de similitud solo se puede establecer en el momento de crear el índice. Esto significa que no se puede cambiar el algoritmo de similitud que se usa para los índices existentes. Los parámetros *"b"* y *"k1"* se pueden modificar al actualizar una definición de índice existente que usa BM25. El cambio de estos valores en un índice existente dejará el índice sin conexión durante al menos unos segundos, lo que provocará errores en las solicitudes de indexación y consulta. Por eso, deberá establecer el parámetro "allowIndexDowntime=true" en la cadena de consulta de la solicitud de actualización:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true
```


## <a name="see-also"></a>Consulte también  

 [REST de Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice/)   
 [Adición de perfiles de puntuación al índice](index-add-scoring-profiles.md)    
 [Creación de un índice &#40;API REST de Azure Cognitive Search&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)   
  [SDK de .NET de Azure Cognitive Search](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
