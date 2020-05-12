---
title: Plantillas de Resource Manager para Cassandra API de Azure Cosmos DB
description: Use las plantillas de Azure Resource Manager para crear y configurar Cassandra API de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: mjbrown
ms.openlocfilehash: f16dec74b15f4945b54fe1423835fd8f5c8d96f1
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791279"
---
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>Administrar recursos de Cassandra API de Azure Cosmos DB con plantillas de Azure Resource Manager

En este artículo, aprenderá a usar plantillas de Azure Resource Manager para ayudarle a implementar y administrar las cuentas, espacios de claves y tablas de Azure Cosmos DB.

Este artículo solo contiene ejemplos de cuentas de Cassandra API. Para buscar ejemplos de otras cuentas de tipo API, consulte los artículos sobre el uso de plantillas de Azure Resource Manager con la API de Azure Cosmos DB para [SQL](manage-sql-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) y [Table](manage-table-with-resource-manager.md).

> [!IMPORTANT]
>
> * Los nombres de cuenta están limitados a 44 caracteres, todo en minúsculas.
> * Para cambiar los valores de rendimiento, vuelva a implementar la plantilla con las RU/s actualizadas.
> * Cuando agrega o elimina ubicaciones en una cuenta de Azure Cosmos, no puede modificar otras propiedades simultáneamente. Estas operaciones se deben realizar por separado.

Para crear cualquiera de los siguientes recursos de Azure Cosmos DB, copie la siguiente plantilla de ejemplo en un archivo JSON nuevo. También puede crear un archivo JSON de parámetros para usar al implementar varias instancias del mismo recurso con distintos nombres y valores. Hay muchas maneras de implementar plantillas de Azure Resource Manager, como, [Azure Portal](../azure-resource-manager/templates/deploy-portal.md), la [CLI de Azure](../azure-resource-manager/templates/deploy-cli.md), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) y [GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md).

<a id="create-autoscale"></a>

## <a name="azure-cosmos-account-for-cassandra-with-autoscale-provisioned-throughput"></a>Cuenta de Azure Cosmos para Cassandra con rendimiento aprovisionado de escalabilidad automática

Esta plantilla crea una cuenta de Azure Cosmos en dos regiones con opciones para la coherencia y la conmutación por error, con un espacio de claves y una tabla configurados para el rendimiento de escalabilidad automática. Esta plantilla también está disponible para implementar con un solo clic desde la galería de plantillas de inicio rápido de Azure.

[![Implementación en Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-cassandra-autosscale%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-cassandra-autoscale/azuredeploy.json":::

<a id="create-manual"></a>

## <a name="azure-cosmos-account-for-cassandra-with-standard-manual-provisioned-throughput"></a>Cuenta de Azure Cosmos para Cassandra con rendimiento aprovisionado estándar (manual)

Esta plantilla crea una cuenta de Azure Cosmos en dos regiones con opciones para la coherencia y la conmutación por error, con un espacio de claves y una tabla configurados para el rendimiento estándar. Esta plantilla también está disponible para implementar con un solo clic desde la galería de plantillas de inicio rápido de Azure.

[![Implementación en Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-cassandra%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-cassandra/azuredeploy.json":::

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

* [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
* [Esquema del proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
* [Plantillas de inicio rápido de Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
* [Solución de errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
