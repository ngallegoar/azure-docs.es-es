---
title: Ejemplos de Java
titleSuffix: Azure Cognitive Search
description: Busque ejemplos de código de Java de demostración de Azure Cognitive Search que usan el SDK de .NET de Azure para Java.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/17/2020
ms.openlocfilehash: c1d21d88ed49cb14aa9f964791154f001a40fbe2
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499584"
---
# <a name="java-code-samples-for-azure-cognitive-search"></a>Ejemplos de código de Java de Azure Cognitive Search

Obtenga información sobre los ejemplos de código de Java que muestran las características y la funcionalidad de Azure Cognitive Search. Los repositorios principales son los siguientes:

| Repositorio | Descripción |
|------------|-------------|
| [azure-sdk-for-java/tree/master/sdk/search/azure-search-documents/src/samples](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/search/azure-search-documents/src/samples) | Ejemplos generados por el equipo del SDK de Azure incluidos en la biblioteca cliente Azure.Search.Documents del SDK. También puede revisar [pruebas unitarias](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/search/azure-search-documents/src/test) de la biblioteca cliente para ver cómo se denominan las distintas API. |
| [Azure-Samples/azure-search-java-samples](https://github.com/Azure-Samples/azure-search-java-samples) | Ejemplos de código que acompañan a artículos paso a paso. **Los ejemplos de este repositorio todavía no se han actualizado para usar el SDK de Azure para Java**. Actualmente, estos ejemplos llaman a las API REST en código Java.|

> [!Tip]
> Pruebe el [Explorador de ejemplos](/samples/browse/?languages=csharp&products=azure-cognitive-search) para buscar ejemplos de código de Microsoft en Github, filtrados por producto, servicio y lenguaje.

## <a name="java-sdk-samples"></a>Ejemplos de SDK de Java

El SDK de Azure para Java incluye numerosos ejemplos y una [página de introducción](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/search/azure-search-documents/src/samples) que abarca la instalación del paquete. La página también muestra una amplia gama de ejemplos. A continuación se enumeran algunas de las operaciones más comunes para su comodidad.

| Ejemplos | Descripción |
|---------|-------------|
| [Creación de índices de búsqueda](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/CreateIndexExample.java) | Muestra cómo crear [índices de búsqueda](search-what-is-an-index.md). |
| [Creación de sinónimos](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/SynonymMapsCreateExample.java) | Muestra cómo crear [asignaciones de sinónimos](search-synonyms.md).  |
| [Creación de indizadores de búsqueda](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/CreateIndexerExample.java) | Muestra cómo crear [indizadores](search-indexer-overview.md). |
| [Creación del origen de datos del indizador de búsqueda](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/DataSourceExample.java) | Muestra cómo crear orígenes de datos de indizadores, necesarios para la indización basada en indizadores de [orígenes de datos de Azure compatibles](search-indexer-overview.md#supported-data-sources). |
| [Creación de conjuntos de aptitudes](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/CreateSkillsetExample.java) |  Muestra cómo crear [conjuntos de aptitudes](cognitive-search-working-with-skillsets.md) que son indizadores adjuntos, y que realizan el enriquecimiento basado en IA durante la indexación. |
| [Carga de documentos](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/IndexContentManagementExample.java) | Muestra cómo cargar o combinar documentos en un índice en una operación de [importación de datos](search-what-is-data-import.md). |
| [Sintaxis de consulta](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/SearchAsyncWithFullyTypedDocumentsExample.java) | Muestra cómo configurar una [consulta básica](search-query-overview.md). |

## <a name="documentation-samples"></a>Documentación y ejemplos

Los ejemplos siguientes tienen un artículo asociado en [Documentación de Azure Cognitive Search](./index.yml).

| Ejemplos | Descripción | 
|---------|-------------|
| [quickstart](https://github.com/Azure-Samples/azure-search-java-samples/tree/master/quickstart) | Código fuente de [Inicio rápido: Creación de un índice de búsqueda en Java](search-get-started-java.md). Este ejemplo llama a las API REST. |
| [search-java-indexer-demo](https://github.com/Azure-Samples/azure-search-java-samples/tree/master/search-java-indexer-demo) | Muestra un indizador de Azure Cosmos DB en Java. Este ejemplo llama a las API REST. |