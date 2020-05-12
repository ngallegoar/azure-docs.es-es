---
title: Plantillas de Azure Resource Manager para Azure Cosmos DB
description: Use las plantillas de Azure Resource Manager para crear y configurar Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: mjbrown
ms.openlocfilehash: 501065875cafc035d491e606c016f3ad4dbfc5d4
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791636"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Plantillas de Azure Resource Manager para Azure Cosmos DB

En las tablas siguientes se incluyen vínculos a plantillas de Azure Resource Manager para Azure Cosmos DB:

## <a name="core-sql-api"></a>API Core (SQL)

|**Plantilla**|**Descripción**|
|---|---|
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con rendimiento de escalabilidad automática](manage-sql-with-resource-manager.md#create-autoscale) | Esta plantilla crea una cuenta de API Core (SQL) en dos regiones, una base de datos y un contenedor con rendimiento de escalabilidad automática. |
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con rendimiento estándar (manual)](manage-sql-with-resource-manager.md#create-manual) | Esta plantilla crea una cuenta de API Core (SQL) en dos regiones, una base de datos y un contenedor con rendimiento estándar. |
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con un procedimiento almacenado, un desencadenador y una UDF](manage-sql-with-resource-manager.md#create-sproc) | Esta plantilla crea una cuenta de API Core (SQL) en dos regiones con un procedimiento almacenado, un desencadenador y una UDF para un contenedor. |
|[Creación de un punto de conexión privado para la cuenta de Azure Cosmos](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Esta plantilla crea un punto de conexión privado para una cuenta existente de la API Core (SQL) de Azure Cosmos en una red virtual existente. |
|[Creación de una cuenta de Azure Cosmos gratuita](manage-sql-with-resource-manager.md#free-tier) |  Esta plantilla crea una cuenta de API Core (SQL) de Azure Cosmos DB gratuita. |

## <a name="mongodb-api"></a>MongoDB API

|**Plantilla**|**Descripción**|
|---| ---|
|[Creación de una cuenta, una base de datos y una colección de Azure Cosmos con rendimiento de escalabilidad automática](manage-mongodb-with-resource-manager.md#create-autoscale) | Esta plantilla crea una cuenta mediante la API de Azure Cosmos DB para MongoDB en dos regiones con dos contenedores que comparten el rendimiento de escalabilidad automática de nivel de base de datos. |
|[Creación de una cuenta, una base de datos y una colección de Azure Cosmos con rendimiento estándar (manual)](manage-mongodb-with-resource-manager.md#create-manual) | Esta plantilla crea una cuenta mediante la API de Azure Cosmos DB para MongoDB en dos regiones con dos contenedores que comparten el rendimiento estándar de nivel de base de datos. |

## <a name="cassandra-api"></a>Cassandra API

|**Plantilla**|**Descripción**|
|---| ---|
|[Creación de una cuenta, un espacio de claves y una tabla de Azure Cosmos con rendimiento de escalabilidad automática](manage-cassandra-with-resource-manager.md#create-autoscale) | Esta plantilla crea una cuenta de Cassandra API en dos regiones, un espacio de claves y una tabla con rendimiento de escalabilidad automática. |
|[Creación de una cuenta, un espacio de claves y una tabla de Azure Cosmos con rendimiento estándar (manual)](manage-cassandra-with-resource-manager.md#create-manual) | Esta plantilla crea una cuenta de Cassandra API en dos regiones, un espacio de claves y una tabla con rendimiento manual. |

## <a name="gremlin-api"></a>API de Gremlin

|**Plantilla**|**Descripción**|
|---| ---|
|[Creación de una cuenta, una base de datos y un grafo de Azure Cosmos con rendimiento de escalabilidad automática](manage-gremlin-with-resource-manager.md#create-autoscale) | Esta plantilla crea una cuenta de API de Gremlin en dos regiones, una base de datos y un grafo con rendimiento de escalabilidad automática. |
|[Creación de una cuenta, una base de datos y un grafo de Azure Cosmos con rendimiento estándar (manual)](manage-gremlin-with-resource-manager.md#create-manual) | Esta plantilla crea una cuenta de API de Gremlin en dos regiones, una base de datos y un grafo con rendimiento estándar. |

## <a name="table-api"></a>Table API

|**Plantilla**|**Descripción**|
|---| ---|
|[Creación de una cuenta y una tabla de Azure Cosmos con rendimiento de escalabilidad automática](manage-table-with-resource-manager.md#create-autoscale) | Esta plantilla crea una cuenta de Table API en dos regiones y una única tabla con rendimiento de escalabilidad automática. |
|[Creación de una cuenta y una tabla de Azure Cosmos con rendimiento estándar (manual)](manage-table-with-resource-manager.md#create-manual) | Esta plantilla crea una cuenta de Table API en dos regiones y una única tabla con rendimiento estándar. |

Consulte la página de [referencia de Azure Resource Manager para Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) de la documentación de referencia.
