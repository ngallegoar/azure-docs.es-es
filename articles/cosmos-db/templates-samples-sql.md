---
title: Plantillas de Azure Resource Manager para Azure Cosmos DB Core (API de SQL)
description: Use las plantillas de Azure Resource Manager para crear y configurar Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/14/2020
ms.author: mjbrown
ms.openlocfilehash: 3247f35fec8615b9c5e277b7453c340caaf56ffd
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283461"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Plantillas de Azure Resource Manager para Azure Cosmos DB

En este artículo solo se muestran ejemplos de plantillas de Azure Resource Manager para cuentas de Core (SQL) API. También puede encontrar ejemplos de plantillas para [Cassandra API](templates-samples-cassandra.md), [Gremlin API](templates-samples-gremlin.md), [MongoDB API](templates-samples-mongodb.md) y [Table API](templates-samples-table.md).

## <a name="core-sql-api"></a>API Core (SQL)

|**Plantilla**|**Descripción**|
|---|---|
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con rendimiento de escalabilidad automática](manage-with-templates.md#create-autoscale) | Esta plantilla crea una cuenta de API Core (SQL) en dos regiones, una base de datos y un contenedor con rendimiento de escalabilidad automática. |
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con un almacén analítico](manage-with-templates.md#create-analytical-store) | Esta plantilla crea una cuenta de Core (SQL) API en una región con un contenedor configurado con TTL analítico habilitado y una opción para usar el rendimiento manual o de escalado automático. |
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con rendimiento estándar (manual)](manage-with-templates.md#create-manual) | Esta plantilla crea una cuenta de API Core (SQL) en dos regiones, una base de datos y un contenedor con rendimiento estándar. |
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con un procedimiento almacenado, un desencadenador y una UDF](manage-with-templates.md#create-sproc) | Esta plantilla crea una cuenta de API Core (SQL) en dos regiones con un procedimiento almacenado, un desencadenador y una UDF para un contenedor. |
|[Creación de un punto de conexión privado para la cuenta de Azure Cosmos](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Esta plantilla crea un punto de conexión privado para una cuenta existente de la API Core (SQL) de Azure Cosmos en una red virtual existente. |
|[Creación de una cuenta de Azure Cosmos gratuita](manage-with-templates.md#free-tier) |  Esta plantilla crea una cuenta de API Core (SQL) de Azure Cosmos DB gratuita. |

Consulte la página de [referencia de Azure Resource Manager para Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) de la documentación de referencia.
