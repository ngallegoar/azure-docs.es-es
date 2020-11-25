---
title: Ejemplos de .NET
titleSuffix: Azure Cognitive Search
description: Busque ejemplos de código de C# de demostración de Azure Cognitive Search que usan las bibliotecas cliente de .NET.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/17/2020
ms.openlocfilehash: d068365cc8197a579c0b043d3fff2da3d54eb803
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686589"
---
# <a name="net-c-code-samples-for-azure-cognitive-search"></a>Ejemplos de código de .NET (C#) de Azure Cognitive Search

Obtenga información sobre los ejemplos de código de C# que muestran las características y la funcionalidad de Azure Cognitive Search. Los repositorios principales son los siguientes:

| Repositorio | Descripción |
|------------|-------------|
| [azure-sdk-for-net/sdk/search/Azure.Search.Documents/samples/](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/search/Azure.Search.Documents/samples) | Ejemplos generados por el equipo del SDK de Azure incluidos en la biblioteca cliente Azure.Search.Documents del SDK. También puede revisar [pruebas unitarias](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/search/Azure.Search.Documents/tests) de la biblioteca cliente para ver cómo se denominan las distintas API. |
| [Azure-Samples/azure-search-dotnet-samples](https://github.com/Azure-Samples/azure-search-dotnet-samples) | Ejemplos que acompañan a los artículos de procedimientos de la documentación, incluido [cómo usar la biblioteca cliente de .NET](search-howto-dotnet-sdk.md).|
| [Azure-Samples/search-dotnet-getting-started](https://github.com/Azure-Samples/search-dotnet-getting-started) | Ejemplos que acompañan a guías de inicio rápido y tutoriales en la documentación.|

> [!Tip]
> Pruebe el [Explorador de ejemplos](/samples/browse/?languages=csharp&products=azure-cognitive-search) para buscar ejemplos de código de Microsoft en Github, filtrados por producto, servicio y lenguaje.

## <a name="net-sdk-samples"></a>Ejemplos de SDK de .NET

El SDK de Azure para .NET incluye numerosos ejemplos y un [archivo Léame de ejemplo](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/README.md) que describe a cada uno de ellos. Esta lista se incluye a continuación para su comodidad.

| Ejemplos | Descripción |
|---------|-------------|
| ["Hello world", sincrónicamente](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample01a_HelloWorld.md) | Muestra cómo crear un cliente, autenticarse y controlar errores mediante métodos sincrónicos.|
| ["Hello world", asincrónicamente](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample01b_HelloWorldAsync.md) | Muestra cómo crear un cliente, autenticarse y controlar errores mediante métodos asincrónicos.  |
| [Operaciones de nivel de servicio](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample02_Service.md) | Muestra cómo crear índices, indizadores, orígenes de datos, conjuntos de aptitudes y asignaciones de sinónimos. En este ejemplo también se muestra cómo obtener las estadísticas de servicio y cómo consultar un índice.  |
| [Operaciones de índice](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample03_Index.md) | Muestra cómo realizar una acción en un índice existente; en este caso, obtener un recuento de los documentos almacenados en el índice.  |
| [FieldBuilderIgnore](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample04_FieldBuilderIgnore.md) | Muestra una técnica para trabajar con tipos de datos no admitidos.  |
| [Indexación de documentos (modelo de inserción)](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample05_IndexingDocuments.md) | Indexación de modelos de tipo "inserción", en el que se envía una carga JSON a un índice en un servicio.   |
| [Ejemplo de clave de cifrado](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample06_EncryptedIndex.md) | Muestra el uso de una clave de cifrado administrada por el cliente para agregar una capa adicional de protección para el contenido confidencial.  |

## <a name="documentation-samples"></a>Documentación y ejemplos

Los ejemplos siguientes tienen un artículo asociado en [Documentación de Azure Cognitive Search](https://docs.microsoft.com/azure/search/).

| Ejemplos | Descripción |
|---------|-------------|
| [quickstart](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart) | Código fuente de [Inicio rápido: Creación de un índice de búsqueda](search-get-started-dotnet.md).  |
| [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)  | Código fuente de [Uso de la biblioteca cliente de .NET](search-howto-dotnet-sdk.md). |
| [DotNetHowToSynonyms](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms)  | Las listas de sinónimos se usan para la expansión de consultas, lo que proporciona términos coincidentes que son ajenos a un índice. Este ejemplo se incluye en [Ejemplo: Incorporación de sinónimos en C#](search-synonyms-tutorial-sdk.md). |
| [DotNetToIndexers](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToIndexers) | Código fuente que subyace a los fragmentos de código relacionados con el indizador en varios artículos. En este ejemplo se muestra cómo configurar un indizador que tiene una programación, asignaciones de campos y parámetros.  |
| [DotNetHowToEncryptionUsingCMK](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToEncryptionUsingCMK)  | Código fuente de [Configuración de claves administradas por el cliente para el cifrado de datos](search-security-manage-encryption-keys.md). |
| [Creación de la primera aplicación en C#](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11) |  Código fuente de [Tutorial: Creación de la primera aplicación de búsqueda](tutorial-csharp-create-first-app.md). Aunque la mayoría de los ejemplos son aplicaciones de consola, en este ejemplo de MVC se usa una página web para el índice de ejemplo "Hotels", en el que se muestra la búsqueda básica, la paginación, la función autocompletar y las consultas, las facetas y los filtros sugeridos. |
| [multiple-data-sources](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/multiple-data-sources)  | Código fuente de [Tutorial: Indexación de varios orígenes de datos](tutorial-multiple-data-sources.md). |
|  [optimize-data-indexing](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/optimize-data-indexing) | Código fuente de [Tutorial: Optimización de la indexación mediante la API de inserción](tutorial-optimize-indexing-push-api.md).  |
| [tutorial-ai-enrichment](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/tutorial-ai-enrichment)  | Código fuente de [Tutorial: Contenido de blobs de Azure con capacidad de búsqueda y generado mediante inteligencia artificial con el SDK de .NET](cognitive-search-tutorial-blob-dotnet.md).  |

## <a name="standalone-samples-and-solutions"></a>Ejemplos y soluciones independientes

| Ejemplos | Descripción |
|---------|-------------|
| [azure-search-power-skills](https://github.com/Azure-Samples/azure-search-power-skills)  | Código fuente de aptitudes personalizadas que se pueden consumir y que puede incorporar en sus soluciones ganadas.  |
| [Acelerador de soluciones de minería de conocimiento](https://docs.microsoft.com/samples/azure-samples/azure-search-knowledge-mining/azure-search-knowledge-mining/) | Incluye plantillas, archivos complementarios e informes analíticos para ayudarle a crear el prototipo de una solución de minería de conocimiento integral.  |
| [Repositorio de aplicaciones de búsqueda de COVID-19](https://github.com/liamca/covid19search) | Repositorio de código fuente para la [aplicación de búsqueda de COVID-19](https://covid19search.azurewebsites.net/) basada en Cognitive Search. |
| [JFK](https://github.com/Microsoft/AzureSearch_JFK_Files) | Más información sobre la [solución JFK](https://www.microsoft.com/ai/ai-lab-jfk-files) |