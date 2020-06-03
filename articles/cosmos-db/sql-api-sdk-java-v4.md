---
title: Notas de la versión y recursos de la versión 4 del SDK de Java para la API SQL de Azure Cosmos DB
description: Obtenga toda la información sobre la versión 4 del SDK de Java de Azure Cosmos DB para el SDK y API SQL, incluidas la fechas de lanzamiento y de retirada, y los cambios realizados entre las versiones del SDK de Java asincrónico para SQL de Azure Cosmos DB.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 05/20/2020
ms.author: anfeldma
ms.openlocfilehash: b222a94ee754b24192261451d8ddc429886e705c
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725659"
---
# <a name="azure-cosmos-db-java-sdk-v4-for-core-sql-api-release-notes-and-resources"></a>Versión 4 del SDK de Java para la API SQL de Azure Cosmos DB: notas de la versión y recursos
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Fuente de cambios de .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Versión 4 del SDK de Java](sql-api-sdk-java-v4.md)
> * [Versión 2 del SDK de Java asincrónico](sql-api-sdk-async-java.md)
> * [Versión 2 del SDK de Java sincrónico](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Proveedor de recursos de REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor: Java](sql-api-sdk-bulk-executor-java.md)

La versión 4 del SDK de Java de Azure Cosmos DB para Core (SQL) combina una API asincrónica y una API de sincronización en un solo artefacto de Maven. El SDK v4 ofrece rendimiento mejorado, nuevas características de API y compatibilidad asincrónica basada en Project Reactor y la [biblioteca Netty](https://netty.io/). Los usuarios pueden esperar un rendimiento mejorado con la versión 4 del SDK de Java de Azure Cosmos DB en comparación con la [versión 2 del SDK de Java asincrónico de Azure Cosmos DB](sql-api-sdk-async-java.md) y la [versión 2 del SDK de Java sincrónico de Azure Cosmos DB](sql-api-sdk-java.md).

> [!IMPORTANT]  
> Estas son las notas de la versión solo para la versión 4 del SDK de Java para Azure Cosmos DB. Si en la actualidad usa una versión anterior a la v4, vea la guía [Migración a la versión 4 del SDK de Java de Azure Cosmos DB](migrate-java-v4-sdk.md) a fin de obtener ayuda para actualizar a v4.
>
> Estos son tres pasos para empezar a trabajar rápidamente.
> 1. Instale el [runtime de Java mínimo admitido, JDK 8](/java/azure/jdk/?view=azure-java-stable) para poder usar el SDK.
> 2. Consulte la [Guía de inicio rápido para la versión 4 del SDK de Java de Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java), en la que se proporciona acceso al artefacto de Maven y se describen solicitudes básicas de Azure Cosmos DB.
> 3. Lea las [sugerencias de rendimiento](performance-tips-java-sdk-v4-sql.md) y las guías de [solución de problemas](troubleshoot-java-sdk-v4-sql.md) de la versión 4 del SDK de Java de Azure Cosmos DB para optimizar el SDK de la aplicación.
>
> Los [talleres y laboratorios de Azure Cosmos DB](https://aka.ms/cosmosworkshop) son otro fantástico recurso para aprender a usar la versión 4 del SDK de Java de Azure Cosmos DB.
>

| |  |
|---|---|
| **Descarga del SDK** | [Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos) |
|**Documentación de la API** | [Documentación de referencia de API](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-cosmos/4.0.1-beta.3/index.html) |
|**Contribuya al SDK** | [Repositorio central del SDK de Azure para Java en GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos) | 
|**Introducción** | [Inicio rápido: Creación de una aplicación Java para administrar los datos de la API SQL de Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java) · [Repositorio de GitHub con código de inicio rápido](https://github.com/Azure-Samples/azure-cosmos-java-getting-started) | 
|**Ejemplos de código básicos** | [Azure Cosmos DB: ejemplos de Java para la API SQL](sql-api-java-sdk-samples.md) · [Repositorio de GitHub con código de ejemplo](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples)|
|**Aplicación de consola con fuente de cambios**| [Fuente de cambios: ejemplo de la versión 4 del SDK de Java](create-sql-api-java-changefeed.md) · [Repositorio de GitHub con código de ejemplo](https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example)| 
|**Ejemplo de aplicación web**| [Compilación de una aplicación web con la versión 4 del SDK de Java](sql-api-java-application.md) · [Repositorio de GitHub con código de ejemplo](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-todo-app)|
| **Consejos de rendimiento**| [Sugerencias de rendimiento para la versión 4 del SDK de Java](performance-tips-java-sdk-v4-sql.md)| 
| **Solución de problemas** | [Solución de problemas de la versión 4 del SDK de Java](troubleshoot-java-sdk-v4-sql.md) |
| **Migración a v4 desde un SDK anterior** | [Migración a la versión 4 del SDK de Java](migrate-java-v4-sdk.md) |
| **Tiempo de ejecución mínimo admitido**|[JDK 8](/java/azure/jdk/?view=azure-java-stable) | 
| **Talleres y laboratorios de Azure Cosmos DB** |[Página principal de talleres de Cosmos DB](https://aka.ms/cosmosworkshop)

