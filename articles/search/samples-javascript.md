---
title: Ejemplos de JavaScript
titleSuffix: Azure Cognitive Search
description: Busque ejemplos de código de JavaScript de demostración de Azure Cognitive Search que usan el SDK de .NET de Azure para JavaScript.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/17/2020
ms.openlocfilehash: 234c70fffb6f353c670d23624cc446fdaf6bd886
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96498972"
---
# <a name="javascript-code-samples-for-azure-cognitive-search"></a>Ejemplos de código de JavaScript para Azure Cognitive Search

Obtenga información sobre los ejemplos de código de JavaScript que muestran las características y la funcionalidad de Azure Cognitive Search. Los repositorios principales son los siguientes:

| Repositorio | Descripción |
|------------|-------------|
| [azure-sdk-for-js/tree/master/sdk/search/search-documents](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents) | Ejemplos generados por el equipo del SDK de Azure incluidos en la biblioteca cliente Azure.Search.Documents del SDK. También puede revisar [pruebas unitarias](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/test) de la biblioteca cliente para ver cómo se denominan las distintas API. |
| [Azure-Samples/azure-search-javascript-samples](https://github.com/Azure-Samples/azure-search-javascript-samples) | Ejemplos de código que acompañan a artículos paso a paso, incluido [Inicio rápido: Creación de un índice de búsqueda en JavaScript](search-get-started-javascript.md).|

> [!Tip]
> Pruebe el [Explorador de ejemplos](/samples/browse/?languages=csharp&products=azure-cognitive-search) para buscar ejemplos de código de Microsoft en Github, filtrados por producto, servicio y lenguaje.

## <a name="javascript-sdk-samples"></a>Ejemplos del SDK de JavaScript

El SDK de Azure para Java incluye numerosos ejemplos y una [página de introducción](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/README.md#getting-started) que abarca la instalación del paquete, la configuración del cliente y la solución de problemas. La página también describe las siguientes categorías de ejemplos, que se enumeran aquí para su comodidad.

| Ejemplos | Descripción |
|---------|-------------|
| [índices](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/indexes) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar [índices de búsqueda](search-what-is-an-index.md). Esta categoría de ejemplos también incluye un ejemplo de estadísticas de servicio. |
| [dataSourceConnections (para indizadores)](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/dataSourceConnections) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar orígenes de datos de indizadores, necesarios para la indización basada en indizadores de [orígenes de datos de Azure compatibles](search-indexer-overview.md#supported-data-sources). |
| [Indizadores](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/indexers) |  Muestra cómo crear, actualizar, obtener, restablecer y eliminar [indizadores](search-indexer-overview.md).|
| [skillSet](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/skillSets) |   Muestra cómo crear, actualizar, obtener, enumerar y eliminar [conjuntos de aptitudes](cognitive-search-working-with-skillsets.md) que son indizadores adjuntos, y que realizan el enriquecimiento basado en IA durante la indexación. |
| [synonymMaps](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/synonymMaps) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar [asignaciones de sinónimos](search-synonyms.md).  |
| [Consultas](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/search/search-documents/samples/javascript/src/readonlyQuery.js) | Muestra la ejecución de consultas en un índice público de solo lectura hospedado por Microsoft.  |

## <a name="typescript-samples"></a>Ejemplos de TypeScript

El SDK también proporciona ejemplos de TypeScript, que se enumeran aquí para mayor comodidad.

| Ejemplos | Descripción |
|---------|-------------|
| [índices](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/indexes) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar [índices de búsqueda](search-what-is-an-index.md). Esta categoría de ejemplos también incluye un ejemplo de estadísticas de servicio. |
| [dataSourceConnections (para indizadores)](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/dataSourceConnections) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar orígenes de datos de indizadores, necesarios para la indización basada en indizadores de [orígenes de datos de Azure compatibles](search-indexer-overview.md#supported-data-sources). |
| [Indizadores](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/indexers) |  Muestra cómo crear, actualizar, obtener, restablecer y eliminar [indizadores](search-indexer-overview.md).|
| [skillSet](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/skillSets) |   Muestra cómo crear, actualizar, obtener, enumerar y eliminar [conjuntos de aptitudes](cognitive-search-working-with-skillsets.md) que son indizadores adjuntos, y que realizan el enriquecimiento basado en IA durante la indexación. |
| [synonymMaps](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/synonymMaps) | Muestra cómo crear, actualizar, obtener, enumerar y eliminar [asignaciones de sinónimos](search-synonyms.md).  |
| [Consultas](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/search/search-documents/samples/typescript/src/readonlyQuery.ts) | Muestra la ejecución de consultas en un índice público de solo lectura hospedado por Microsoft.  |

## <a name="documentation-samples"></a>Documentación y ejemplos

Los ejemplos siguientes tienen un artículo asociado en [Documentación de Azure Cognitive Search](./index.yml).

| Ejemplos | Descripción | 
|---------|-------------|
| [quickstart](https://github.com/Azure-Samples/azure-search-javascript-samples/tree/master/quickstart/v11) | Código fuente de [Inicio rápido: Creación de un índice de búsqueda en JavaScript](search-get-started-javascript.md).  |

## <a name="standalone-samples"></a>Ejemplos independientes

| Ejemplos | Descripción |
|---------|-------------|
| [azure-search-react-template](https://github.com/dereklegenzoff/azure-search-react-template) | Plantilla d React para Azure Cognitive Search (github.com) |