---
title: Ejemplos de Python
titleSuffix: Azure Cognitive Search
description: Busque ejemplos de código de Python de demostración de Azure Cognitive Search que usan el SDK de .NET de Azure para Python o REST.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/17/2020
ms.openlocfilehash: 244408cc512a51683ecb3c062113170b1c7f4781
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499567"
---
# <a name="python-code-samples-for-azure-cognitive-search"></a>Ejemplos de código de Python de Azure Cognitive Search

Obtenga información sobre los ejemplos de código de Python que muestran las características y la funcionalidad de Azure Cognitive Search. Los repositorios principales son los siguientes:

| Repositorio | Descripción |
|------------|-------------|
| [azure-sdk-for-python/tree/master/sdk/search/azure-search-documents/samples/](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/search/azure-search-documents/samples) | Ejemplos generados por el equipo del SDK de Azure incluidos en la biblioteca cliente Azure.Search.Documents del SDK. También puede revisar [pruebas unitarias](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/search/azure-search-documents/tests) de la biblioteca cliente para ver cómo se denominan las distintas API. |
| [Azure-Samples/azure-search-python-samples](https://github.com/Azure-Samples/azure-search-python-samples) | Ejemplos de código que acompañan a artículos paso a paso, incluido [Inicio rápido: Creación de un índice de Azure Cognitive Search en Python](search-get-started-python.md).|

> [!Tip]
> Pruebe el [Explorador de ejemplos](/samples/browse/?languages=csharp&products=azure-cognitive-search) para buscar ejemplos de código de Microsoft en Github, filtrados por producto, servicio y lenguaje.

## <a name="python-sdk-samples"></a>Ejemplos de SDK de Python

El SDK de Azure para Python incluye numerosos ejemplos y una [página de introducción](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/search/azure-search-documents/samples) que incluye los requisitos previos y la instalación del paquete. La página también contiene vínculos a los ejemplos siguientes, que se enumeran aquí para su comodidad.

| Ejemplos | Descripción |
|---------|-------------|
| [Autenticar](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_authentication.py) | Muestra cómo configurar un cliente y autenticarse en el servicio. | 
| [Operaciones de creación, lectura, actualización y eliminación de índices](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_index_crud_operations.py) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar [índices de búsqueda](search-what-is-an-index.md). |
| [Operaciones de creación, lectura, actualización y eliminación de indizadores](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_indexers_operations.py) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar [indizadores](search-indexer-overview.md). |
| [Orígenes de datos de indizadores de búsqueda](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_indexer_datasource_skillset.py) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar orígenes de datos de indizadores, necesarios para la indización basada en indizadores de [orígenes de datos de Azure compatibles](search-indexer-overview.md#supported-data-sources). |
| [Sinónimos](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_synonym_map_operations.py) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar [asignaciones de sinónimos](search-synonyms.md).  |
| [Carga de documentos](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_crud_operations.py) | Muestra cómo cargar o combinar documentos en un índice en una operación de [importación de datos](search-what-is-data-import.md). |
| [Consulta sencilla](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_simple_query.py) | Muestra cómo configurar una [consulta básica](search-query-overview.md). |
| [Consulta con filtro](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_filter_query.py) | Muestra la configuración de una [expresión de filtro](search-filters.md). |
| [Consulta de facetas](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_facet_query.py) | Muestra el trabajo con [facetas](search-filters-facets.md). |

## <a name="documentation-samples"></a>Documentación y ejemplos

Los ejemplos siguientes tienen un artículo asociado en [Documentación de Azure Cognitive Search](./index.yml).

| Ejemplos | Descripción | 
|---------|-------------|
| [quickstart](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Quickstart) | Código fuente de [Inicio rápido: Creación de un índice de Azure Cognitive Search en Python](search-get-started-python.md).  |
| [tutorial-ai-enrichment](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Tutorial-AI-Enrichment)  | Código fuente de [Tutorial: Uso de Python y AI para generar contenido que permita búsquedas desde blobs de Azure](cognitive-search-tutorial-blob-python.md).  |
| [AzureML-Custom-Skill](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/AzureML-Custom-Skill)  | Código fuente de [Ejemplo: Creación de una aptitud personalizada con Python](cognitive-search-custom-skill-python.md).  |