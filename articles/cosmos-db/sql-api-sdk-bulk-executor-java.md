---
title: 'Azure Cosmos DB: API, SDK y recursos de Java para Bulk Executor'
description: Obtenga toda la información sobre la API y el SDK de Java para BulkExecutor, incluidas la fechas de lanzamiento, fechas de retirada y cambios realizados entre las versiones del SDK de Java para BulkExecutor de Azure Cosmos DB.
author: milismsft
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 11/21/2018
ms.author: adrianmi
ms.openlocfilehash: 0030f974a36dc80dc8c4112000aa5934126a2482
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82836451"
---
# <a name="java-bulk-executor-library-download-information"></a>Biblioteca BulkExecutor para Java: Información de descarga

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Fuente de cambios de .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Proveedor de recursos de REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor: .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor: Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**Descripción**|La biblioteca BulkExecutor permite a las aplicaciones cliente realizar operaciones en masa en las cuentas de Azure Cosmos DB. La biblioteca BulkExecutor proporciona los espacios de nombres BulkImport y BulkUpdate. El módulo BulkImport puede ingerir documentos en masa de forma optimizada, de tal forma que la capacidad de proceso aprovisionada para una colección se consuma en el máximo nivel posible. El módulo BulkUpdate puede actualizar en masa los datos existentes en los contenedores de Azure Cosmos como revisiones.|
|**Descarga del SDK**|[Maven](https://search.maven.org/#search%7Cga%7C1%7Cdocumentdb-bulkexecutor)|
|**Biblioteca BulkExecutor en GitHub**|[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started)|
| **Documentación de la API**| [Documentación de referencia de API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)|
|**Introducción**|[Introducción al SDK de Java para la biblioteca BulkExecutor](bulk-executor-java.md)|
|**Tiempo de ejecución mínimo admitido**|[Java Development Kit (JDK) 7+](/java/azure/jdk/?view=azure-java-stable)|

## <a name="release-notes"></a>Notas de la versión

### <a name="2100"></a><a name="2.10.0"/>2.10.0

* Corrección de DocumentAnalyzer.java para extraer correctamente los valores de clave de partición anidada de JSON.

### <a name="294"></a><a name="2.9.4"/>2.9.4

* Se agregó una funcionalidad en las operaciones BulkDelete para volver a intentarlo en determinados errores, así como para devolver al usuario una lista de errores que se pueden reintentar.

### <a name="293"></a><a name="2.9.3"/>2.9.3

* Actualización de la versión 2.4.7 del SDK de Cosmos.

### <a name="292"></a><a name="2.9.2"/>2.9.2

* Corrección de "mergeAll" para continuar en "id" y el valor de la clave de partición, de modo que las propiedades de documento con revisiones que se coloquen después de "id" y el valor de clave de partición se agreguen a la lista de elementos actualizados.

### <a name="291"></a><a name="2.9.1"/>2.9.1

* Actualización del grado de simultaneidad de inicio a 1 y adición de registros de depuración para minilote.


