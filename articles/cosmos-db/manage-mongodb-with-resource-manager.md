---
title: Plantillas de Resource Manager para la API de Azure Cosmos DB para MongoDB
description: Use las plantillas de Azure Resource Manager para crear y configurar la API de Azure Cosmos DB para MongoDB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mjbrown
ms.openlocfilehash: 1fe2c96e3c469e41e12d64f27a6a3b3d16a1174d
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684777"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Administrar recursos de la API de MongoDB de Azure Cosmos DB con plantillas de Azure Resource Manager

En este artículo, aprenderá a usar plantillas de Azure Resource Manager para ayudarle a implementar y administrar las cuentas de Azure Cosmos DB para la API de MongoDB, bases de datos y colecciones.

Este artículo solo tiene ejemplos de cuentas de API de Azure Cosmos DB para MongoDB; para buscar ejemplos de otras cuentas de tipo de API, consulte los artículos sobre el uso de plantillas de Azure Resource Manager con la API de Azure Cosmos DB para [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md) y [Table](manage-table-with-resource-manager.md).

> [!IMPORTANT]
>
> * Los nombres de cuenta están limitados a 44 caracteres, todo en minúsculas.
> * Para cambiar los valores de rendimiento, vuelva a implementar la plantilla con las RU/s actualizadas.
> * Cuando agrega o elimina ubicaciones en una cuenta de Azure Cosmos, no puede modificar otras propiedades simultáneamente. Estas operaciones se deben realizar por separado.

Para crear cualquiera de los siguientes recursos de Azure Cosmos DB, copie la siguiente plantilla de ejemplo en un archivo JSON nuevo. También puede crear un archivo JSON de parámetros para usar al implementar varias instancias del mismo recurso con distintos nombres y valores. Hay muchas maneras de implementar plantillas de Azure Resource Manager, como, [Azure Portal](../azure-resource-manager/templates/deploy-portal.md), la [CLI de Azure](../azure-resource-manager/templates/deploy-cli.md), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) y [GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md).

<a id="create-autoscale"></a>

## <a name="azure-cosmos-account-for-mongodb-with-autoscale-provisioned-throughput"></a>Cuenta de Azure Cosmos para MongoDB con rendimiento aprovisionado de escalabilidad automática

Esta plantilla creará una cuenta de Azure Cosmos para la API de MongoDB (3.2 o 3.6) con dos colecciones que compartirán el rendimiento de escalabilidad automática en el nivel de base de datos. Esta plantilla también está disponible para implementar con un solo clic desde la galería de plantillas de inicio rápido de Azure.

[![Implementación en Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-mongodb-autoscale%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-mongodb-autoscale/azuredeploy.json":::

<a id="create-manual"></a>

## <a name="azure-cosmos-account-for-mongodb-with-standard-provisioned-throughput"></a>Cuenta de Azure Cosmos para MongoDB con rendimiento aprovisionado estándar

Esta plantilla creará una cuenta de Azure Cosmos para la API de MongoDB (3.2 o 3.6) con dos colecciones que compartirán el rendimiento estándar (manual) de 400 RU/s en el nivel de base de datos. Esta plantilla también está disponible para implementar con un solo clic desde la galería de plantillas de inicio rápido de Azure.

[![Implementación en Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-mongodb%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json":::

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

* [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
* [Esquema del proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
* [Plantillas de inicio rápido de Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
* [Solución de errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
