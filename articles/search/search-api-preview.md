---
title: Características en versión preliminar de la API REST
titleSuffix: Azure Cognitive Search
description: La API REST del servicio Azure Cognitive Search, versión 2019-05-06-Preview, incluye características experimentales, como almacén de conocimiento y almacenamiento en caché de indexadores para enriquecimiento incremental.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: db941152186127302680b5e659e43cd2d82a8908
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162283"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Características en versión preliminar de Azure Cognitive Search

En este artículo se enumeran las características que se encuentran actualmente en versión preliminar. Se han eliminado de la lista las que pasan de versión preliminar a disponibilidad general. Puede marcar [Actualizaciones del servicio](https://azure.microsoft.com/updates/?product=search) o [Novedades](whats-new.md) para ver los anuncios relativos a la disponibilidad general.

Aunque algunas características en versión preliminar pueden estar disponibles en el portal y en el SDK de .NET, la API REST siempre tiene características en versión preliminar.

+ En el caso de las operaciones de búsqueda, la versión preliminar actual es [ **`2019-05-06-Preview`** ](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview).
+ En el caso de las operaciones de administración, la versión preliminar actual es [ **`2019-10-01-Preview`** ](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview).

> [!IMPORTANT]
> La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="ai-enrichment-features"></a>Características de enriquecimiento con IA

Explore las últimas mejoras del enriquecimiento con IA mediante la [API de búsqueda en versión preliminar](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview).

|||
|-|-|
| [Aptitud de búsqueda de entidades personalizadas (versión preliminar)](cognitive-search-skill-custom-entity-lookup.md ) | Aptitud cognitiva que busca texto de una lista de palabras y frases personalizada definida por el usuario. Con esta lista, se etiquetan todos los documentos con entidades coincidentes. La aptitud también admite un grado de coincidencia aproximada que se puede aplicar para buscar coincidencias similares pero no exactas. | 
| [Aptitud para la detección de información de identificación personal (versión preliminar)](cognitive-search-skill-pii-detection.md) | Aptitud cognitiva utilizada durante la indexación que extrae información de identificación personal de un texto de entrada y le ofrece la opción de enmascararla a partir de ese texto de varias maneras.| 
| [Enriquecimiento incremental (versión preliminar)](cognitive-search-incremental-indexing-conceptual.md) | Agrega almacenamiento en caché a una canalización de enriquecimiento, lo que le permite reutilizar los resultados existentes si una modificación de destino, como una actualización de conjunto de aptitudes u otro objeto, no cambia el contenido. El almacenamiento en caché solo se aplica a los documentos enriquecidos generados por un conjunto de aptitudes.| 
| [Almacén de conocimiento (versión preliminar)](knowledge-store-concept-intro.md) | Nuevo destino de una canalización de enriquecimiento basada en inteligencia artificial. La estructura de datos física existe en Azure Blob Storage y Azure Table Storage, y se crea y rellena al ejecutar un indexador que tiene un conjunto de aptitudes cognitivas conectado. La definición de un almacén de conocimiento se especifica en la definición de un conjunto de aptitudes. En la definición del almacén de conocimiento, controla las estructuras físicas de los datos a través de los elementos de *proyección* que determinan cómo se forman los datos, si estos se almacenan en Table Storage o Blob Storage, y si hay varias vistas.| 

## <a name="indexing-and-query-features"></a>Características de indexación y consulta

Las características en versión preliminar del indexador están disponibles en la API de búsqueda en versión preliminar. 

|||
|-|-|
| [Indizador de Cosmos DB](search-howto-index-cosmosdb.md) | Es compatible con las versiones preliminares del tipo de MongoDB API, Gremlin API y Cassandra API. | 
|  [Indizador de Azure Data Lake Storage Gen2 (versión preliminar)](search-howto-index-azure-data-lake-storage.md) | Indiza contenido y metadatos de Azure Data Lake Storage Gen2.| 
| [Parámetro de consulta moreLikeThis (versión preliminar)](search-more-like-this.md) | Busca documentos que sean pertinentes para un documento específico. Esta característica ha aparecido en versiones anteriores. | 

## <a name="management-features"></a>Características de administración

|||
|-|-|
| [Compatibilidad con el punto de conexión privado](service-create-private-endpoint.md) | Puede crear una red virtual con un cliente seguro (como una máquina virtual) y, a continuación, crear un servicio de búsqueda que use un punto de conexión privado. |
| Restricción de acceso IP | Con [`api-version=2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) de la API REST de administración, puede crear un servicio con restricciones sobre las direcciones IP a las que se permite el acceso. |

## <a name="earlier-preview-features"></a>Características de las versiones preliminares anteriores

Las características anunciadas en versiones preliminares anteriores siguen estando en versión preliminar pública si no han pasado a disponibilidad general. Si está llamando a una API con una versión anterior de la API en versión preliminar, puede continuar usando esa versión o cambiar a `2019-05-06-Preview` sin que se produzcan cambios en el comportamiento esperado.

## <a name="how-to-call-a-preview-api"></a>Cómo llamar a una API en versión preliminar

Las vistas previas anteriores siguen funcionando, pero quedarán obsoletas con el paso del tiempo. Si el código llama a `api-version=2016-09-01-Preview` o `api-version=2017-11-11-Preview`, dichas llamadas siguen siendo válidas. Sin embargo, solo la versión preliminar más reciente se actualiza con mejoras. 

En la sintaxis de ejemplo siguiente se ilustra una llamada a la versión de API de versión preliminar.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

El servicio Azure Cognitive Search está disponible en varias versiones. Para obtener más información, consulte [Versiones de API](search-api-versions.md).

## <a name="next-steps"></a>Pasos siguientes

Revise la documentación de referencia de la API de REST del servicio Search. Si tiene algún problema, puede solicitar ayuda en [StackOverflow](https://stackoverflow.com/) o [ponerse en contacto con el servicio de soporte técnico](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referencia de la API de REST del servicio Search](https://docs.microsoft.com/rest/api/searchservice/)