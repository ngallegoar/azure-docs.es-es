---
title: Plantillas de Resource Manager para la API de Azure Cosmos DB para MongoDB
description: Use las plantillas de Azure Resource Manager para crear y configurar la API de Azure Cosmos DB para MongoDB.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: 531f122679c463b11c84eba2fca9f30b09e0935f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063637"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Administrar recursos de la API de MongoDB de Azure Cosmos DB con plantillas de Azure Resource Manager

En este artículo se explica cómo realizar diferentes operaciones para automatizar la administración de las cuentas, las bases de datos y los contenedores de Azure Cosmos DB con plantillas de Azure Resource Manager. Este artículo solo tiene ejemplos de cuentas de API de Azure Cosmos DB para MongoDB; para buscar ejemplos de otras cuentas de tipo de API, consulte los artículos sobre el uso de plantillas de Azure Resource Manager con la API de Azure Cosmos DB para [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md) y [Table](manage-table-with-resource-manager.md).

## <a name="create-azure-cosmos-db-api-for-mongodb-account-database-and-collection"></a>Crear una API de Azure Cosmos DB para la cuenta, la base de datos y la colección de MongoDB <a id="create-resource"></a>

Cree recursos de Azure Cosmos DB con una plantilla de Azure Resource Manager. Esta plantilla creará una cuenta de Azure Cosmos para la API de MongoDB con dos colecciones que compartirán el rendimiento de 400 RU/s en el nivel de base de datos. Copie la plantilla y realice la implementación tal y como se muestra a continuación o visite [Azure Quickstart Gallery](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb/) (Galería de inicio rápido de Azure) y realice la implementación desde Azure Portal. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

> [!NOTE]
> Los nombres de cuenta deben estar en minúsculas y tener un máximo de 44 caracteres.
> Para actualizar las RU/s, vuelva a enviar la plantilla con los valores de propiedad de rendimiento actualizados.
>
> Actualmente solo se puede crear la versión 3.2 (es decir, cuentas que usen el punto de conexión con formato `*.documents.azure.com`) de las cuentas de la API de Azure Cosmos DB para MongoDB mediante PowerShell y la CLI. Para crear la versión 3.6 de las cuentas, use las plantillas de Resource Manager (a continuación) o Azure Portal en su lugar.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json":::

### <a name="deploy-via-the-azure-cli"></a>Implementación mediante la CLI de Azure

Para implementar la plantilla de Azure Resource Manager mediante la CLI de Azure, **copie** el script y seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first collection name: ' collection1Name
read -p 'Enter the second collection name: ' collection2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb/azuredeploy.json \
  --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion \
  databaseName=$databaseName collection1Name=$collection1Name collection2Name=$collection2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El comando `az cosmosdb show` muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión instalada localmente de la CLI de Azure en lugar de usar Cloud Shell, consulte el artículo [CLI de Azure](/cli/azure/).

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema del proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de inicio rápido de Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solución de errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
