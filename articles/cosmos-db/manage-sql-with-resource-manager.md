---
title: Creación y administración de Azure Cosmos DB con plantillas de Resource Manager
description: Use las plantillas de Azure Resource Manager para crear y configurar la API de Azure Cosmos DB para SQL (Core)
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: 72a87c3b23e0eed6cfbf1614388702443f4e99d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79227276"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-with-azure-resource-manager-templates"></a>Administración de recursos de SQL (Core) API de Azure Cosmos DB con plantillas de Azure Resource Manager

En este artículo, aprenderá a usar plantillas de Azure Resource Manager para ayudarle a automatizar la administración de las cuentas, bases de datos y contenedores de Azure Cosmos DB.

En este artículo solo se muestran ejemplos de plantillas de Azure Resource Manager para cuentas de SQL API. También puede encontrar ejemplos de plantillas para [Cassandra API](manage-cassandra-with-resource-manager.md), [Gremlin API](manage-gremlin-with-resource-manager.md), [MongoDB API](manage-mongodb-with-resource-manager.md) y [Table API](manage-table-with-resource-manager.md).

<a id="create-resource"></a>

## <a name="create-an-azure-cosmos-account-database-and-container"></a>Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos

La siguiente plantilla de Azure Resource Manager crea una cuenta de Azure Cosmos con:

* Dos contenedores que comparten un rendimiento de 400 unidades de solicitud por segundo (RU/s) en el nivel de base de datos.
* Un contenedor con un rendimiento dedicado de 400 RU/s.

Para crear los recursos de Azure Cosmos DB, copie la siguiente plantilla de ejemplo e impleméntela como se describe mediante [PowerShell](#deploy-via-powershell) o la [CLI de Azure](#deploy-via-azure-cli).

* Opcionalmente, puede visitar la [galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) e implementar la plantilla desde Azure Portal.
* También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

> [!IMPORTANT]
>
> * Cuando agrega o elimina ubicaciones en una cuenta de Azure Cosmos, no puede modificar otras propiedades simultáneamente. Estas operaciones se deben realizar por separado.
> * Los nombres de cuenta están limitados a 44 caracteres, todo en minúsculas.
> * Para cambiar los valores de rendimiento, vuelva a enviar la plantilla con las RU/s actualizadas.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json":::

> [!NOTE]
> Para crear un contenedor con una clave de partición de gran tamaño, modifique la plantilla anterior para incluir la propiedad `"version":2` en el objeto `partitionKey`.

### <a name="deploy-via-powershell"></a>Implementación mediante PowerShell

Para usar PowerShell para implementar la plantilla de Azure Resource Manager:

1. **Copie** el script.
2. Seleccione **Probar ahora** para abrir Azure Cloud Shell.
3. Haga clic con el botón derecho en la ventana de Azure Cloud Shell y seleccione **Pegar**.

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$sharedThroughput = Read-Host -Prompt "Enter the shared database throughput (i.e. 400)"
$sharedContainer1Name = Read-Host -Prompt "Enter the first shared container name"
$sharedContainer2Name = Read-Host -Prompt "Enter the second shared container name"
$dedicatedContainer1Name = Read-Host -Prompt "Enter the dedicated container name"
$dedicatedThroughput = Read-Host -Prompt "Enter the dedicated container throughput (i.e. 400)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -sharedThroughput $ $sharedThroughput `
    -sharedContainer1Name $sharedContainer1Name `
    -sharedContainer2Name $sharedContainer2Name `
    -dedicatedContainer1Name $dedicatedContainer1Name `
    -dedicatedThroughput $dedicatedThroughput

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Puede optar por implementar la plantilla con una versión instalada localmente de PowerShell en lugar de Azure Cloud Shell. Necesitará [instalar el módulo de Azure PowerShell](/powershell/azure/install-az-ps). Ejecute `Get-Module -ListAvailable Az` para encontrar la versión necesaria.

### <a name="deploy-via-azure-cli"></a>Implementación mediante la CLI de Azure

Para usar la CLI de Azure para implementar la plantilla de Azure Resource Manager:

1. **Copie** el script.
2. Seleccione **Probar ahora** para abrir Azure Cloud Shell.
3. Haga clic con el botón derecho en la ventana de Azure Cloud Shell y seleccione **Pegar**.

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the shared database throughput: sharedThroughput
read -p 'Enter the first shared container name: ' sharedContainer1Name
read -p 'Enter the second shared container name: ' sharedContainer2Name
read -p 'Enter the dedicated container name: ' dedicatedContainer1Name
read -p 'Enter the dedicated container throughput: dedicatedThroughput

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName \
   primaryRegion=$primaryRegion \
   secondaryRegion=$secondaryRegion \
   databaseName=$databaseName \
   sharedThroughput=$sharedThroughput \
   sharedContainer1Name=$sharedContainer1Name \
   sharedContainer2Name=$sharedContainer2Name \
   dedicatedContainer1Name=$dedicatedContainer1Name \
   dedicatedThroughput=$dedicatedThroughput

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

El comando `az cosmosdb show` muestra la cuenta de Azure Cosmos recién creada después de que se haya aprovisionado. Puede optar por implementar la plantilla con una versión instalada localmente de la CLI de Azure en lugar de Azure Cloud Shell. Para más información, consulte el artículo [Interfaz de la línea de comandos (CLI) de Azure](/cli/azure/).

<a id="create-sproc"></a>

## <a name="create-an-azure-cosmos-db-container-with-server-side-functionality"></a>Creación de un contenedor de Azure Cosmos DB con funcionalidad del lado servidor

Puede usar una plantilla de Azure Resource Manager para crear un contenedor de Azure Cosmos DB con un procedimiento almacenado, un desencadenador y una función definida por el usuario.

Copie la plantilla de ejemplo siguiente e impleméntela como se describe, mediante [PowerShell](#deploy-with-powershell) o la [CLI de Azure](#deploy-with-azure-cli).

* Opcionalmente, puede visitar la [galería de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-sprocs/) e implementar la plantilla desde Azure Portal.
* También puede descargar la plantilla en el equipo local o crear una nueva plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-container-sprocs/azuredeploy.json":::

### <a name="deploy-with-powershell"></a>Implementación con PowerShell

Para usar PowerShell para implementar la plantilla de Azure Resource Manager:

1. **Copie** el script.
1. Seleccione **Probar ahora** para abrir Azure Cloud Shell.
1. Haga clic con el botón derecho en la ventana de Azure Cloud Shell y seleccione **Pegar**.

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -containerName $containerName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Puede optar por implementar la plantilla con una versión instalada localmente de PowerShell en lugar de Azure Cloud Shell. Necesitará [instalar el módulo de Azure PowerShell](/powershell/azure/install-az-ps). Ejecute `Get-Module -ListAvailable Az` para encontrar la versión necesaria.

### <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

Para usar la CLI de Azure para implementar la plantilla de Azure Resource Manager:

1. **Copie** el script.
2. Seleccione **Probar ahora** para abrir Azure Cloud Shell.
3. Haga clic con el botón derecho en la ventana de Azure Cloud Shell y seleccione **Pegar**.

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   containerName=$containerName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

## <a name="next-steps"></a>Pasos siguientes

Estos son algunos recursos adicionales:

* [Documentación de Azure Resource Manager](/azure/azure-resource-manager/)
* [Esquema del proveedor de recursos de Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
* [Plantillas de inicio rápido de Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Documentdb&pageNumber=1&sort=Popular)
* [Solución de errores comunes de implementación de Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
