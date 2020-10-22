---
title: Búsqueda en blobs de texto sin formato
titleSuffix: Azure Cognitive Search
description: Configure un indexador de búsqueda para extraer texto sin formato de blobs de Azure para la búsqueda de texto completo en Azure Cognitive Search.
manager: nitinme
author: mgottein
ms.author: magottei
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/25/2020
ms.openlocfilehash: 417bdacc3ce8b619d5ec9618e6060ac071882471
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91533932"
---
# <a name="how-to-index-plain-text-blobs-in-azure-cognitive-search"></a>Indexación de blobs de texto sin formato en Azure Cognitive Search

Si usa un [indexador de blobs](search-howto-indexing-azure-blob-storage.md) para extraer texto que permite búsquedas para la búsqueda de texto completo, se pueden invocar varios modos de análisis para obtener mejores resultados de la indexación. De forma predeterminada, el indexador analiza los blobs de texto delimitados como un único fragmento de texto. Sin embargo, si todos los blobs contienen texto sin formato con la misma codificación, se puede mejorar notablemente el rendimiento de la indexación mediante el uso del **modo de análisis de texto**.

## <a name="set-up-plain-text-indexing"></a>Configuración de la indexación del texto sin formato

Para indexar blobs de texto sin formato, cree o actualice una definición de indexador con la propiedad de configuración `parsingMode` a `text`en una solicitud [Crear indexador](/rest/api/searchservice/create-indexer):

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }
```

De forma predeterminada, se da por hecha la codificación `UTF-8`. Para especificar otra, use la propiedad de configuración `encoding`: 

```http
    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }
```

## <a name="request-example"></a>Ejemplo de solicitud

Los modos de análisis se especifican en la definición del indexador.

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-plaintext-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }
```

## <a name="help-us-make-azure-cognitive-search-better"></a>Ayúdenos a mejorar Azure Cognitive Search

Si tiene solicitudes o ideas para mejorar las características, comuníquelas en [UserVoice](https://feedback.azure.com/forums/263029-azure-search/). Si necesita ayuda para usar la característica existente, publique su pregunta en [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870).

## <a name="next-steps"></a>Pasos siguientes

* [Indexadores de Azure Cognitive Search](search-indexer-overview.md)
* [Configuración de un indexador de blobs](search-howto-indexing-azure-blob-storage.md)
* [Introducción a la indexación de blobs](search-blob-storage-integration.md)