---
title: Novedades de Azure Cognitive Search
description: Anuncios de características nuevas y mejoradas, como el cambio de nombre del servicio Azure Search por Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 06/30/2020
ms.openlocfilehash: 078892691bfaec62f71f9d601a42de3f80221149
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85958164"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Novedades de Azure Cognitive Search

Conozca las novedades del servicio. Marque esta página para mantenerse actualizado con el servicio.

## <a name="feature-announcements"></a>Anuncios de características

### <a name="june-2020"></a>Junio de 2020

+ [Knowledge Store](knowledge-store-concept-intro.md) ya está disponible con carácter general.

+ La [API REST de Search Service 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/) es la nueva versión estable de las API REST. Además del almacén de conocimiento, esta versión disponible con carácter general incluye mejoras en la relevancia y puntuación de las búsquedas.

+ El nuevo algoritmo de clasificación de relevancia ahora es [BM25](https://en.wikipedia.org/wiki/Okapi_BM25) para todos los servicios que se creen. En el caso de los servicios existentes, puede participar estableciendo la propiedad `similarity` en los campos de índice. Esta propiedad se encuentra disponible con carácter general.

+ El nuevo indexador `executionEnvironment` de se puede establecer explícitamente en `private`. Esta funcionalidad admite el acceso del indexador a datos externos a través de puntos de conexión privados y está disponible con carácter general.

+ [Azure Machine Learning (AML)](cognitive-search-aml-skill.md) es un nuevo tipo de aptitud para integrar un punto de conexión de inferencia desde Azure Machine Learning. La experiencia del portal admite la detección y la integración de un punto de conexión de Azure Machine Learning en un conjunto de aptitudes de Cognitive Search. La detección requiere que los servicios Cognitive Search y Azure Machine Learning se implementen en la misma suscripción. Esta aptitud se encuentra disponible con carácter general. Para empezar, realice [este tutorial](cognitive-search-tutorial-aml-custom-skill.md).

### <a name="may-2020-microsoft-build"></a>Mayo de 2020 (Microsoft Build)

+ La característica [Sesiones de depuración](cognitive-search-debug-session.md) está ahora en versión preliminar. Las sesiones de depuración proporcionan una interfaz basada en portal para investigar y resolver problemas con un conjunto de aptitudes. Las correcciones creadas en la sesión de depuración se pueden guardar en conjuntos de aptitudes de producción. Para empezar, realice [este tutorial](cognitive-search-tutorial-debug-sessions.md).

+ Proteja un punto de conexión de servicio de búsqueda desde la red pública de Internet mediante la [configuración de reglas de IP para lograr compatibilidad con el firewall de entrada](service-configure-firewall.md), o bien sacando provecho de [Azure Private Link para un punto de conexión de búsqueda privado](service-create-private-endpoint.md). Por lo general, ambas funcionalidades están disponibles.

+ Use una [identidad administrada por el sistema (versión preliminar)](search-howto-managed-identities-data-sources.md) para configurar una conexión a un origen de datos de Azure para la indexación. Se aplica a los [indexadores](search-indexer-overview.md) que ingieren contenido de orígenes de datos de Azure como Azure SQL Database, Azure Cosmos DB y Azure Storage.

+ Cambie la forma en que se calculan las puntuaciones de búsqueda, desde por partición a todas las particiones, mediante los parámetros [sessionId](index-similarity-and-scoring.md) y [scoringStatistics=global](index-similarity-and-scoring.md#scoring-statistics). Estos parámetros están disponibles con carácter general.

+ Agregue un parámetro de consulta [featuresMode (versión preliminar)](index-similarity-and-scoring.md#featuresMode-param) para expandir una puntuación de relevancia y mostrar más detalles: la puntuación de similitud por campo, la frecuencia de los términos por campo y el número de tokens únicos coincidentes por campo. Puede consumir estos puntos de datos en algoritmos de puntuación personalizados. Para obtener un ejemplo que muestra esta funcionalidad, consulte [Adición de aprendizaje automático (LearnToRank) a la relevancia de búsqueda](https://github.com/Azure-Samples/search-ranking-tutorial).

### <a name="march-2020"></a>Marzo de 2020

+ [Eliminación temporal de blobs nativos (versión preliminar)](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) significa que el indexador de Azure Blob Storage en Azure Cognitive Search reconocerá los blobs que se encuentran en un estado de eliminación temporal y quitará el documento de búsqueda correspondiente durante la indexación.

+ Ya está disponible con carácter general la nueva y estable [API REST de administración](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) (2020-03-13). 

### <a name="february-2020"></a>Febrero de 2020

+ La [detección de PII (versión preliminar)](cognitive-search-skill-pii-detection.md) es una aptitud cognitiva utilizada durante la indexación que extrae información de identificación personal de un texto de entrada y le ofrece la opción de enmascararla a partir de ese texto de varias maneras.

+ [Búsqueda de entidades personalizadas (versión preliminar)](cognitive-search-skill-custom-entity-lookup.md ) busca texto de una lista de palabras y frases personalizada definida por el usuario. Con esta lista, se etiquetan todos los documentos con entidades coincidentes. La aptitud también admite un grado de coincidencia aproximada que se puede aplicar para buscar coincidencias similares pero no exactas. 

### <a name="january-2020"></a>Enero de 2020

+ Las [claves de cifrado administradas por el cliente](search-security-manage-encryption-keys.md) ya están disponibles con carácter general. Si usa REST, puede acceder a la característica mediante `api-version=2019-05-06`, o cualquier versión posterior. Para el código administrado, el paquete correcto sigue siendo el [SDK de .NET, versión 8.0-preview](search-dotnet-sdk-migration-version-9.md) aunque la característica no se incluya en la versión preliminar. 

+ El acceso privado a un servicio de búsqueda está disponible mediante dos mecanismos que se encuentran en versión preliminar:

  + Puede restringir el acceso a direcciones IP específicas mediante la API REST de administración `api-version=2019-10-01-Preview` para crear el servicio. La API de vista previa tiene las propiedades nuevas **IpRule** y **NetworkRuleSet** en la [API CreateOrUpdate](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service). Esta característica en vista previa está disponible en regiones seleccionadas. Para más información, vea [Uso de la API REST de administración](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

### <a name="december-2019"></a>Diciembre de 2019

+ [Create Demo App (versión preliminar)](search-create-app-portal.md) (Crear aplicación de demostración [versión preliminar]) es un nuevo asistente del portal que genera un archivo HTML descargable con acceso de consulta (solo lectura) a un índice. El archivo incluye un script insertado que representa una aplicación web de estilo "localhost" operativa, enlazada a un índice en el servicio de búsqueda. Las páginas se pueden configurar en el asistente y pueden contener una barra de búsqueda, un área de resultados, una barra de navegación lateral y compatibilidad con consultas de escritura anticipada. Puede modificar el código HTML sin conexión para extender o personalizar el flujo de trabajo o la apariencia. Una aplicación de demostración no se extiende fácilmente para incluir las capas de seguridad y hospedaje que normalmente son necesarias en escenarios de producción. Debería considerarla una herramienta de validación y prueba, en lugar de un acceso directo a una aplicación cliente completa.

+ En [Creación de un punto de conexión privado para conexiones seguras (versión preliminar)](service-create-private-endpoint.md) se explica cómo configurar un vínculo privado para las conexiones seguras al servicio de búsqueda. Esta característica en vista previa está disponible a petición y usa [Azure Private Link](../private-link/private-link-overview.md) y [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) como parte de la solución.

### <a name="november-2019---ignite-conference"></a>Noviembre de 2019: Conferencia Ignite

+ [Enriquecimiento incremental (versión preliminar)](cognitive-search-incremental-indexing-conceptual.md) agrega almacenamiento en caché y disponibilidad de estados a una canalización de enriquecimiento para que pueda trabajar en pasos o fases específicos sin perder el contenido que ya se ha procesado. Anteriormente, cualquier cambio en una canalización de enriquecimiento requería una recompilación completa. Con el enriquecimiento incremental, se conserva la salida del análisis costoso, especialmente el de las imágenes.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ La [extracción de documentos (versión preliminar)](cognitive-search-skill-document-extraction.md) es una aptitud cognitiva que se usa durante la indexación y que permite extraer el contenido de un archivo de un conjunto de aptitudes. Antes, el descifrado de documentos solo tenía lugar antes de la ejecución del conjunto de aptitudes. Con la incorporación de esta aptitud, también puede realizar esta operación dentro de la ejecución del conjunto de aptitudes.

+ La [traducción de texto](cognitive-search-skill-text-translation.md) es una aptitud cognitiva usada durante la indexación que evalúa el texto y devuelve para cada registro el texto traducido al idioma de destino especificado.

+ Las [plantillas de Power BI](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) pueden impulsar las visualizaciones y el análisis de contenido enriquecido en un almacén de conocimiento en Power BI Desktop. Esta plantilla está diseñada para las proyecciones de tablas de Azure creadas mediante el [Asistente para importar datos](knowledge-store-create-portal.md).

+ [Azure Data Lake Storage Gen2 (versión preliminar)](search-howto-index-azure-data-lake-storage.md), [Gremlin API de Cosmos DB (versión preliminar)](search-howto-index-cosmosdb.md) y [Cassandra API de Cosmos DB (versión preliminar)](search-howto-index-cosmosdb.md) ahora se admiten en los indexadores. Para registrarse, utilice [este formulario](https://aka.ms/azure-cognitive-search/indexer-preview). Recibirá un correo de confirmación cuando se le haya aceptado en el programa de versión preliminar.

### <a name="july-2019"></a>Julio de 2019

+ Está disponible generalmente en la [nube de Azure Government](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search).

<a name="new-service-name"></a>

## <a name="new-service-name"></a>Nuevo nombre de servicio

Azure Search se llama ahora **Azure Cognitive Search** para reflejar el uso ampliado (aunque opcional) de las aptitudes cognitivas y el procesamiento de inteligencia artificial en las operaciones principales. Las versiones de API, los paquetes NuGet, los espacios de nombres y los puntos de conexión permanecen sin cambios. Las soluciones de búsqueda nuevas y las existentes no se ven afectadas por el cambio de nombre del servicio.

## <a name="service-updates"></a>Actualizaciones del servicio

Los [anuncios de actualización de servicios](https://azure.microsoft.com/updates/?product=search&status=all) de Azure Cognitive Search se pueden encontrar en el sitio web de Azure.