---
title: Plantillas de Azure Resource Manager para Azure Cosmos DB
description: Use las plantillas de Azure Resource Manager para crear y configurar Azure Cosmos DB.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: thvankra
ms.openlocfilehash: 7b08ca98f25b079d831033b9393effd4ee4b65e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73961856"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Plantillas de Azure Resource Manager para Azure Cosmos DB

En las tablas siguientes se incluyen vínculos a plantillas de Azure Resource Manager para Azure Cosmos DB:

## <a name="sql-core-api"></a>SQL API (Core)

|**Plantilla**|**Descripción**|
|---|---|
|[Crear una cuenta, una base de datos y un contenedor de Azure Cosmos](manage-sql-with-resource-manager.md#create-resource) | Esta plantilla crea una cuenta de API de SQL (Core) en dos regiones con dos contenedores con rendimiento de base de datos compartido y un contenedor con rendimiento dedicado. El rendimiento se puede actualizar mediante el reenvío de la plantilla con el valor de la propiedad de rendimiento actualizado. |
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con un procedimiento almacenado, un desencadenador y una UDF](manage-sql-with-resource-manager.md#create-sproc) | Esta plantilla crea una cuenta de API de SQL (Core) en dos regiones con un procedimiento almacenado, un desencadenador y una UDF para un contenedor. |

## <a name="mongodb-api"></a>MongoDB API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta, una base de datos y una colección de Azure Cosmos](manage-mongodb-with-resource-manager.md#create-resource) | Con esta plantilla se crea una cuenta con la API de Azure Cosmos DB para MongoDB en dos regiones con la arquitectura multimaestro habilitada. La cuenta de Azure Cosmos tendrá dos contenedores que compartirán el rendimiento de nivel de base de datos. |

## <a name="cassandra-api"></a>Cassandra API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta, un espacio de claves y una tabla de Azure Cosmos](manage-cassandra-with-resource-manager.md#create-resource) | Con esta plantilla se crea una cuenta de Cassandra API en dos regiones con la arquitectura multimaestro habilitada. La cuenta de Azure Cosmos tendrá dos tablas que compartirán el rendimiento de nivel de espacio de claves. |

## <a name="gremlin-api"></a>API de Gremlin

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta, una base de datos y un grafo de Azure Cosmos](manage-gremlin-with-resource-manager.md#create-resource) | Con esta plantilla se crea una cuenta de API de Gremlin en dos regiones con la arquitectura multimaestro habilitada. La cuenta de Azure Cosmos tendrá dos grafos que compartirán el rendimiento de nivel de base de datos. |

## <a name="table-api"></a>Table API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta de Azure Cosmos, tabla](manage-table-with-resource-manager.md#create-resource) | Con esta plantilla se crea una cuenta de Table API en dos regiones con la arquitectura multimaestro habilitada. La cuenta de Azure Cosmos tendrá una sola tabla. |

> [!TIP]
> Para habilitar rendimiento compartido cuando se usa Table API, habilite el rendimiento de nivel de cuenta en Azure Portal.

Consulte la página de [referencia de Azure Resource Manager para Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) de la documentación de referencia.
