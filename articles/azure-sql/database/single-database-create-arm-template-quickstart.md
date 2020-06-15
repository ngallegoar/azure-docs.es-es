---
title: 'Azure Resource Manager: Crear una base de datos única'
description: Cree una base de datos única en Azure SQL Database con una plantilla de Azure Resource Manager.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: subject-armqs sqldbrb=1
ms.devlang: ''
ms.topic: quickstart
author: mumian
ms.author: jgao
ms.reviewer: carlrab
ms.date: 06/28/2019
ms.openlocfilehash: 02e0947de3e7e5c6ce5110740127571ea393b168
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84343870"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Inicio rápido: Creación de una base de datos única en Azure SQL Database con una plantilla de Azure Resource Manager

La creación de una [base de datos única](single-database-overview.md) es la opción más rápida y sencilla para crear una base de datos en Azure SQL Database. En este inicio rápido se muestra cómo crear una base de datos única con una plantilla de Azure Resource Manager.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Si no tiene una suscripción a Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

Ninguno.

## <a name="create-a-single-database"></a>Crear una base de datos única

Una base de datos única tiene un conjunto definido de recursos de proceso, memoria, E/S y almacenamiento y se usa uno de los dos [modelos de compra](purchasing-models.md). Cuando se crea una base de datos única, también se define un [servidor](logical-servers.md) para administrarla y colocarla dentro de un [grupo de recursos de Azure](../../active-directory-b2c/overview.md) de una región determinada.

### <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-sql-logical-server/).

:::code language="json" source="~/quickstart-templates/101-sql-logical-server/azuredeploy.json" range="1-163" highlight="63-132":::

Estos recursos se definen en la plantilla:

- [**Microsoft.Sql/servers**](/azure/templates/microsoft.sql/servers)
- [**Microsoft.Sql/servers/firewallRules**](/azure/templates/microsoft.sql/servers/firewallrules)
- [**Microsoft.Sql/servers/securityAlertPolicies**](/azure/templates/microsoft.sql/servers/securityalertpolicies)
- [**Microsoft.Sql/servers/vulnerabilityAssessments**](/azure/templates/microsoft.sql/servers/vulnerabilityassessments)
- [**Microsoft.Sql/servers/connectionPolicies**](/azure/templates/microsoft.sql/servers/connectionpolicies)
- [**Microsoft.Storage/storageAccounts**](/azure/templates/microsoft.storage/storageaccounts)
- [**Microsoft.Storage/storageAccounts/providers/roleAssignments**](/azure/templates/microsoft.authorization/roleassignments)

Puede encontrar más ejemplos de plantillas de Azure SQL Database en [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

### <a name="deploy-the-template"></a>Implementación de la plantilla

Seleccione **Pruébelo** en el bloque de código de PowerShell siguiente para abrir Azure Cloud Shell.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sql-logical-server/azuredeploy.json" -administratorLogin $adminUser -administratorLoginPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

# <a name="the-azure-cli"></a>[La CLI de Azure](#tab/azure-cli)

```azurecli-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

az group create --location $location --name $resourceGroupName

az group deployment create -g $resourceGroupName --template-uri "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" `
    --parameters 'projectName=' + $projectName \
                 'administratorLogin=' + $adminUser \
                 'administratorLoginPassword=' + $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

* * *

## <a name="validate-the-deployment"></a>Validación de la implementación

Para consultar la base de datos, vea [Consulta de la base de datos](single-database-create-quickstart.md#query-the-database).

## <a name="clean-up-resources"></a>Limpieza de recursos

Mantenga este grupo de recursos, el servidor y la base de datos única si desea ir a los artículos de [Pasos siguientes](#next-steps). Los pasos siguientes muestran cómo conectarse a la base de datos y consultarla mediante distintos métodos.

Para eliminar el grupo de recursos:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

# <a name="the-azure-cli"></a>[La CLI de Azure](#tab/azure-cli)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

* * *

## <a name="next-steps"></a>Pasos siguientes

- Cree una regla de firewall de nivel de servidor para conectarse a la base de datos única desde herramientas locales o remotas. Para más información, consulte [Creación de una regla de firewall de nivel de servidor](firewall-create-server-level-portal-quickstart.md).
- Después de crear una regla de firewall de nivel de servidor, puede usar varias herramientas y lenguajes para [conectarse a la base de datos y realizar consultas en ella](connect-query-content-reference-guide.md).
  - [Conexión y consulta con SQL Server Management Studio](connect-query-ssms.md)
  - [Conexión y consulta de datos con Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Para crear una base de datos única mediante la CLI de Azure, consulte [Ejemplos de la CLI de Azure](az-cli-script-samples-content-guide.md).
- Para crear una base de datos única mediante Azure PowerShell, consulte [Ejemplos de Azure PowerShell](powershell-script-content-guide.md).
- Para aprender a crear plantillas de Administrador de recursos, consulte [Creación de la primera plantilla](../../azure-resource-manager/templates/template-tutorial-create-first-template.md).
 