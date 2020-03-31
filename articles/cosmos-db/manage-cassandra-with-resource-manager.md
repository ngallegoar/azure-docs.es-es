---
title: Plantillas de Resource Manager para Cassandra API de Azure Cosmos DB
description: Use las plantillas de Azure Resource Manager para crear y configurar Cassandra API de Azure Cosmos DB.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: c4dc97453fe50865db74f8918ef3dffdb4013b4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79227292"
---
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>Administrar recursos de Cassandra API de Azure Cosmos DB con plantillas de Azure Resource Manager

En este artículo se explica cómo realizar diferentes operaciones para automatizar la administración de las cuentas, las bases de datos y los contenedores de Azure Cosmos DB con plantillas de Azure Resource Manager. Este artículo solo tiene ejemplos de cuentas de API de SQL; para buscar ejemplos de otras cuentas de tipo de API, consulte los artículos sobre el uso de plantillas de Azure Resource Manager con la API de Azure Cosmos DB para [SQL](manage-sql-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) y [Table](manage-table-with-resource-manager.md).

## <a name="create-azure-cosmos-account-keyspace-and-table"></a>Crear una cuenta, un espacio de claves y una tabla de Azure Cosmos <a id="create-resource"></a>

Cree recursos de Azure Cosmos DB con una plantilla de Azure Resource Manager. Esta plantilla creará una cuenta de Azure Cosmos para Cassandra API con dos tablas que compartirán el rendimiento de 400 RU/s en el nivel de espacio de claves. Copie la plantilla y realice la implementación tal y como se muestra a continuación o visite [Azure Quickstart Gallery](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra/) (Galería de inicio rápido de Azure) y realice la implementación desde Azure Portal. También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

> [!NOTE]
> Los nombres de cuenta deben estar en minúsculas y tener un máximo de 44 caracteres.
> Para actualizar las RU/s, vuelva a enviar la plantilla con los valores de propiedad de rendimiento actualizados.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-cassandra/azuredeploy.json":::

## <a name="deploy-with-the-azure-cli"></a>Implementación con la CLI de Azure

Para implementar la plantilla de Azure Resource Manager mediante la CLI de Azure, **copie** el script y seleccione **Probar** para abrir Azure Cloud Shell. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the keyset name: ' keysetName
read -p 'Enter the first table name: ' table1Name
read -p 'Enter the second table name: ' table2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion keysetName=$keysetName \
   table1Name=$table1Name table2Name=$table2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El comando `az cosmosdb show` muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Si decide usar una versión instalada localmente de la CLI de Azure en lugar de usar Cloud Shell, consulte el artículo [CLI de Azure](/cli/azure/).


## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

- [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema del proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Plantillas de inicio rápido de Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solución de errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
