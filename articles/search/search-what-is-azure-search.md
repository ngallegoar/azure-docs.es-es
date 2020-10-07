---
title: Introducción a Azure Cognitive Search
titleSuffix: Azure Cognitive Search
description: Azure Cognitive Search es un servicio de búsqueda en la nube completamente administrado de Microsoft. Obtenga información sobre los casos de uso, el flujo de trabajo de desarrollo, comparaciones con otros productos de búsqueda de Microsoft y cómo empezar a utilizar el servicio.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 09/22/2020
ms.custom: contperfq1
ms.openlocfilehash: 26a448ded06b32fef80fee06568655067a727620
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91320425"
---
# <a name="what-is-azure-cognitive-search"></a>¿Qué es Azure Cognitive Search?

Azure Cognitive Search ([anteriormente conocido como "Azure Search"](whats-new.md#new-service-name)) es un servicio de búsqueda en la nube que proporciona a los desarrolladores las API y herramientas necesarias para crear una experiencia de búsqueda de datos enriquecida en un contenido privado y heterogéneo en las aplicaciones web, para aplicaciones móviles y empresariales.

Cuando se crea un servicio de Cognitive Search, se obtiene un motor de búsqueda que realiza la indexación y la ejecución de consultas, el almacenamiento persistente de los índices que se crean y administran, y un lenguaje de consulta para crear consultas simples y complejas. Opcionalmente, los servicios de búsqueda se integran con otros servicios de Azure en forma de *indexadores* que automatizan la ingesta y recuperación de datos de orígenes de datos de Azure, y *conjuntos de aptitudes* que incorporan una IA consumible de Cognitive Services, como el análisis de imágenes y texto, o la IA personalizado que se crea en Azure Machine Learning o se encapsula dentro de Azure Functions.

![Arquitectura de Azure Cognitive Search](media/search-what-is-azure-search/azure-search-diagram.svg "Arquitectura de Azure Cognitive Search")

En cuanto a la arquitectura, un servicio de búsqueda se coloca entre los almacenes de datos externos que contienen los datos no indexados y una aplicación cliente que envía solicitudes de consulta a un índice de búsqueda y controla la respuesta.  Un esquema de índices determina la estructura del contenido en el que se pueden realizar búsquedas. 

Las dos cargas de trabajo principales de un servicio de búsqueda son la *indexación* y la *realización de consultas*.

+ La indexación lleva el texto al servicio de búsqueda y hace que se puedan realizar búsquedas en él. Internamente, el texto de entrada se procesa en tokens y se almacena en índices invertidos, con el fin de agilizar los exámenes. Durante la indexación, tiene la opción de agregar *aptitudes cognitivas*, ya sean las predefinidas de Microsoft u otras personalizadas que cree. Tanto las transformaciones como los análisis posteriores pueden dar lugar a una nueva información y estructuras que no existían previamente, lo que resulta muy útil para muchos escenarios de búsqueda y minería de conocimientos.

+ Una vez que un índice se rellena con datos en los que se pueden realizar búsquedas, la aplicación cliente envía solicitudes de consulta a un servicio de búsqueda y controla las respuestas. Toda la ejecución de consultas se realiza a través de un índice de búsqueda que crea, posee y almacena en su servicio. En la aplicación cliente, la experiencia de búsqueda se define mediante las API de Azure Cognitive Search y puede incluir las funciones de ajuste de relevancia, autocompletar, coincidencia de sinónimos, coincidencia aproximada, coincidencia de patrones, filtro y ordenación.

La funcionalidad se expone a través de [API de REST](/rest/api/searchservice/) o [SDK de .NET](search-howto-dotnet-sdk.md) sencillos que enmascaran la complejidad inherente de la recuperación de información. También puede usar Azure Portal para la administración de servicios y de contenido, con herramientas para la creación de prototipos y la consulta de índices y conjuntos de aptitudes. Como el servicio se ejecuta en la nube, Microsoft administra la infraestructura y la disponibilidad.

## <a name="when-to-use-cognitive-search"></a>Cuándo se usa Cognitive Search

Azure Cognitive Search es adecuado en los siguientes escenarios de aplicación:

+ Consolidación de tipos de contenido heterogéneos en un único índice de búsqueda privado definido por el usuario. Los índices de búsqueda se pueden rellenar con flujos de documentos JSON de cualquier origen. En el caso de los orígenes admitidos en Azure, use un *indexador* para automatizar la indexación. El control sobre el esquema de índices y la programación de actualización es una razón de mucho peso para usar Cognitive Search.

+ Implementación sencilla de las características relacionadas con la búsqueda. Las API de Search simplifican la creación de consultas, la navegación por facetas, los filtros (incluida la búsqueda geoespacial), la asignación de sinónimos, el autocompletado y el ajuste de la relevancia. Mediante el uso de las características integradas, puede satisfacer las expectativas del usuario final y ofrecer una experiencia de búsqueda similar a la de los motores de búsqueda web comerciales.

+ El contenido sin procesar es un texto grande no diferenciado, o los archivos de imagen o de aplicación almacenados en Azure Blob Storage o Cosmos DB. Puede aplicar [aptitudes cognitivas](cognitive-search-concept-intro.md) durante la indexación para identificar y extraer texto, crear una estructura o crear información, como texto traducido o entidades.

+ El contenido requiere un análisis de texto lingüístico o personalizado. Si tiene contenido que no está en inglés, Azure Cognitive Search admite tanto los analizadores de Lucene como los procesadores de lenguaje natural de Microsoft. También puede configurar los analizadores para obtener un procesamiento especializado de contenido sin procesar, como el filtrado de los caracteres diacríticos o el reconocimiento y la preservación de patrones en las cadenas.

Para más información sobre la funcionalidad específica, consulte [Características de Azure Cognitive Search](search-features-list.md)

## <a name="how-to-use-cognitive-search"></a>Modo de uso de Cognitive Search

### <a name="step-1-provision-service"></a>Paso 1: Servicio de aprovisionamiento

Puede [crear un servicio gratis](search-create-service-portal.md) compartido con otros suscriptores, o bien un [plan de pago](https://azure.microsoft.com/pricing/details/search/) que dedique recursos que solo use su servicio. Todas las guías de inicio rápido y los tutoriales se pueden realizar en el servicio gratis.

En el caso de los planes de pago, puede escalar un servicio en dos dimensiones para calibrar los recursos en función de los requisitos de producción:

+ Agregar réplicas para aumentar su capacidad de controlar cargas de consulta elevadas.
+ Agregar particiones para aumentar el almacenamiento y que pueda haber más documentos.

### <a name="step-2-create-an-index"></a>Paso 2: Creación de un índice

Defina un esquema de índice para su asignación y reflejo en la estructura de los documentos que desea buscar, igual que los campos de una base de datos. Un índice de búsqueda es una estructura de datos especializada que está optimizada para la ejecución rápida de consultas.

Es común [crear un esquema el esquema de índice en Azure Portal](search-what-is-an-index.md), o mediante programación mediante el [SDK de .NET](search-howto-dotnet-sdk.md) o la [API REST](/rest/api/searchservice/).

> [!TIP]
> Para empezar consulte el artículo [Inicio rápido: Creación de un índice de Azure Cognitive Search en Azure Portal](search-get-started-portal.md) para crear, cargar y consultar un índice en cuestión de minutos.

### <a name="step-3-load-data"></a>Paso 3: Cargar datos

Una vez definido un índice, estará listo para cargar contenido. Puede usar un modelo de inserción o de extracción.

El modelo de inserción "inserta" documentos JSON en un índice, para lo que usa API de un [SDK](search-howto-dotnet-sdk.md) o [REST](/rest/api/searchservice/addupdate-or-delete-documents). El conjunto de datos externo puede ser prácticamente cualquier origen de datos, siempre y cuando los documentos sean JSON.

El modelo de extracción "extrae" datos de orígenes en Azure y los envía a un índice de búsqueda. El modelo de extracción se implementa mediante [*indexadores*](/rest/api/searchservice/Indexer-operations) que agilizan y automatizan los distintos aspectos de la ingesta de datos, como conectarse, leer y serializar datos. Entre los orígenes de datos que se admiten se incluyen Azure Cosmos DB, Azure SQL y Azure Storage.

### <a name="step-4-send-queries-and-handle-responses"></a>Paso 4: Envío de consultas y control de respuestas

Tras completar un índice, puede [generar consultas de búsqueda](search-query-overview.md) para el punto de conexión de servicio mediante solicitudes HTTP sencillas con [API REST](/rest/api/searchservice/Search-Documents) o el [SDK de .NET](/dotnet/api/microsoft.azure.search.idocumentsoperations).

Consulte [Creación de la primera aplicación de búsqueda](tutorial-csharp-create-first-app.md) para crear y, posteriormente, ampliar una página web que recoja los datos que especifique el usuario y controle los resultados. También puede usar llamadas a [Postman para REST interactivo](search-get-started-postman.md) o el [Explorador de búsqueda](search-explorer.md) integrado en Azure Portal para consultar un índice existente.

## <a name="how-it-compares"></a>Comparación

Los clientes a menudo preguntan cuáles son las diferencias de Azure Cognitive Search con respecto a otras soluciones relacionadas de búsqueda. En la tabla siguiente se resumen las principales diferencias.

| En comparación con | Principales diferencias |
|-------------|-----------------|
|Bing | [Bing Web Search API](../cognitive-services/bing-web-search/index.yml) busca en los índices de Bing.com aquellos términos que coincidan con lo que envía. Los índices se compilan a partir de HTML, XML y otro tipo de contenido web en sitios públicos. [Bing Custom Search](/azure/cognitive-services/bing-custom-search/) tiene los mismos cimientos y ofrece la misma tecnología de rastreador (crawler) para tipos de contenido web y cuyo destino son los sitios web individuales.<br/><br/>Azure Cognitive Search busca en un índice que usted define y que está completado con datos y documentos de su propiedad, a menudo procedentes de diversas fuentes. Azure Cognitive Search tiene funcionalidades de rastreo para algunos orígenes de datos a través de [indexadores](search-indexer-overview.md), pero puede insertar cualquier documento JSON que se ajuste a su esquema de índice en un recurso sencillo y consolidado en el que se puedan realizar búsquedas. |
|Búsqueda de bases de datos | Muchas plataformas de base de datos incluyen una experiencia de búsqueda integrada. SQL Server tiene [búsqueda de texto completo](/sql/relational-databases/search/full-text-search). Cosmos DB y otras tecnologías similares tienen índices que se pueden consultar. Al evaluar productos que combinan búsqueda y almacenamiento, la elección puede ser complicada. Muchas soluciones usan: DBMS para el almacenamiento y Azure Cognitive Search para las características de búsqueda especializadas.<br/><br/>En comparación con la búsqueda de DBMS, Azure Cognitive Search almacena contenido de orígenes heterogéneos y ofrece características de procesamiento de texto especializadas como el procesamiento de texto en función del idioma (lematización o formas de las palabras) en [56 idiomas](/rest/api/searchservice/language-support). También admite la autocorrección de palabras mal escritas, [sinónimos](/rest/api/searchservice/synonym-map-operations), [sugerencias](/rest/api/searchservice/suggestions), [controles de puntuación](/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [facetas](./search-filters-facets.md) y [tokenización personalizada](/rest/api/searchservice/custom-analyzers-in-azure-search). El [motor de búsqueda de texto completo](search-lucene-query-architecture.md) en Azure Cognitive Search está compilado en Apache Lucene, un estándar del sector en cuanto a recuperación de información. Sin embargo, aunque Azure Cognitive Search conserva los datos en forma de índice invertido, no sustituye a un verdadero almacenamiento de datos y no se recomienda usarlo en esa capacidad. Para más información, consulte esta [entrada del foro](https://stackoverflow.com/questions/40101159/can-azure-search-be-used-as-a-primary-database-for-some-data). <br/><br/>El uso de recursos es otro punto de inflexión en esta categoría. Tanto la indexación como algunas operaciones conllevan a menudo muchos cálculos. La descarga de la búsqueda desde DBMS en una solución dedicada en la nube conserva los recursos del sistema para el procesamiento de transacciones. Además, mediante la externalización de la búsqueda se puede ajustar fácilmente la escala para que se adapte al volumen de consultas.|
|Solución de búsqueda dedicada | Suponiendo que se haya decidido por una búsqueda dedicada con la funcionalidad de todo el espectro, al final se realiza una comparación de categorías entre en soluciones locales y un servicio en la nube. Muchas tecnologías de búsqueda ofrecen control sobre la indexación y las canalizaciones de consultas, acceso a una sintaxis de filtrado y consultas más completa, control sobre la clasificación y la relevancia, y características para la búsqueda inteligente y autodirigida. <br/><br/>Un servicio en la nube es la opción adecuada si desea una solución llave en mano con costos generales y un mantenimiento mínimos, y una escala ajustable. <br/><br/>Dentro del paradigma de la nube, varios proveedores ofrecen características de línea base similares, con búsqueda de texto completo, búsqueda geográfica y capacidad de controlar cierto nivel de ambigüedad en entradas de búsqueda. Normalmente, es una [característica especializada](search-features-list.md) o la facilidad y sencillez general de las API, las herramientas y la administración, lo que determina la solución que mejor se ajusta a nuestras necesidades. |

Entre los proveedores de servicios en la nube, Azure Cognitive Search es el más potente para cargas de trabajo de búsqueda de texto completo en almacenes de contenido y bases de datos de Azure, y para aplicaciones que se basan principalmente en la búsqueda para navegar por el contenido y recuperar información. 

Las principales ventajas incluyen:

+ Integración de datos de Azure (rastreadores o crawlers) en el nivel de indexación
+ Azure Portal para administración central
+ Escalado, confiabilidad y disponibilidad internacional de Azure
+ Procesamiento mediante IA de datos sin procesar para mejorar las búsquedas, incluido el texto de imágenes, o bien la identificación de patrones en contenido no estructurado.
+ Análisis lingüístico y personalizado, con analizadores para la búsqueda de texto completo en 56 idiomas
+ [Características principales comunes a las aplicaciones centradas en la búsqueda](search-features-list.md): puntuación, uso de facetas, sugerencias, sinónimos, búsqueda geográfica y muchas más.

Entre nuestros clientes, los que son capaces de utilizar la gama más amplia de características de Azure Cognitive Search pueden usar catálogos en línea, programas de línea de negocio y aplicaciones de detección de documentos.

## <a name="watch-this-video"></a>Vea este vídeo

En este vídeo de 15 minutos, el administrador de programas Luis Cabrera presenta Azure Cognitive Search.

>[!VIDEO https://www.youtube.com/embed/kOJU0YZodVk?version=3]