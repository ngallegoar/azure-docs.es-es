---
title: 'Inicio rápido: Creación de un área de trabajo de Azure Databricks con PowerShell'
description: En este inicio rápido se muestra cómo usar PowerShell para crear un área de trabajo de Azure Databricks.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.date: 06/02/2020
ms.custom: mvc
ms.openlocfilehash: 445832119a113f92dc1da51eb5b5e77dff176fa3
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2020
ms.locfileid: "84485693"
---
# <a name="quickstart-create-an-azure-databricks-workspace-using-powershell"></a>Inicio rápido: Creación de un área de trabajo de Azure Databricks con PowerShell

En esta guía de inicio rápido se describe cómo usar PowerShell para crear un área de trabajo de Azure Databricks. Puede usar PowerShell para crear y administrar recursos de Azure de forma interactiva o en scripts.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

Si decide usar PowerShell de forma local, para este artículo es preciso que instale el módulo Az PowerShell y que se conecte a su cuenta de Azure con el cmdlet [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount). Para más información sobre cómo instalar el módulo Az PowerShell, consulte [Instalación de Azure PowerShell](/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Mientras el módulo Az.Databricks de PowerShell se encuentre en versión preliminar, debe instalarlo por separado desde el módulo Az de PowerShell con el siguiente comando: `Install-Module -Name Az.Databricks -AllowPrerelease`.
> Una vez que el módulo Az.Databricks de PowerShell esté disponible con carácter general, formará parte de las futuras versiones del módulo Az de PowerShell y estará disponible de forma nativa en Azure Cloud Shell.

Si esta es la primera vez que usa Azure Databricks, debe registrar el proveedor de recursos **Microsoft.Databricks**.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Databricks
```

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Si tiene varias suscripciones a Azure, elija la suscripción adecuada en la que se debe facturar el recurso. Seleccione un identificador de suscripción específico con el cmdlet [Set-AzContext](/powershell/module/az.accounts/set-azcontext).

```azurepowershell-interactive
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) con el cmdlet [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran recursos de Azure como un grupo.

En el ejemplo siguiente, se crea un grupo de recursos denominado **myresourcegroup** en la región **Oeste de EE. UU. 2**.

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroup -Location westus2
```

## <a name="create-an-azure-databricks-workspace"></a>Creación de un área de trabajo de Azure Databricks

En esta sección, se crea un área de trabajo de Azure Databricks mediante PowerShell.

```azurepowershell-interactive
New-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup -Location westus2 -ManagedResourceGroupName databricks-group -Sku standard
```

Proporcione los valores siguientes:

|       **Propiedad**       |                                                                                **Descripción**                                                                                 |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Nombre                     | Proporcione un nombre para el área de trabajo de Databricks                                                                                                                                   |
| ResourceGroupName        | Especifique un nombre de grupo de recursos existente.                                                                                                                                        |
| Location                 | Seleccione **Oeste de EE. UU. 2**. Para otras regiones disponibles, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).                                     |
| ManagedResourceGroupName | Especifique si desea crear un nuevo grupo de recursos administrados o utilizar uno existente.                                                                                        |
| SKU                      | Elija entre **Estándar**, **Premium** o **Evaluación gratuita**. Para más información sobre estos niveles, consulte [Precios de Databricks](https://azure.microsoft.com/pricing/details/databricks/). |

Se tarda unos minutos en crear el área de trabajo. Una vez finalizado este proceso, la cuenta de usuario se agrega automáticamente como usuario administrador al área de trabajo.

Cuando se produce un error en una implementación del área de trabajo, esta todavía se crea en un estado de error. Elimine el área de trabajo con errores y cree una nueva que resuelva los errores de implementación. Al eliminar el área de trabajo con errores, también se eliminan el grupo de recursos administrados y los recursos implementados correctamente.

## <a name="determine-the-provisioning-state-of-a-databricks-workspace"></a>Determinación del estado de aprovisionamiento de un área de trabajo de Databricks

Para determinar si un área de trabajo de Databricks se aprovisionó correctamente, puede usar el cmdlet `Get-AzDatabricksWorkspace`.

```azurepowershell-interactive
Get-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup |
  Select-Object -Property Name, SkuName, Location, ProvisioningState
```

```Output
Name            SkuName   Location  ProvisioningState
----            -------   --------  -----------------
mydatabricksws  standard  westus2   Succeeded
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no se necesitan los recursos que se han creado en este inicio rápido para otro inicio rápido o tutorial, puede eliminarlos con el siguiente comando.

> [!CAUTION]
> En el ejemplo siguiente se elimina el grupo de recursos especificado y todos los recursos que contiene.
> Si los recursos que están fuera del ámbito de este inicio rápido existen en el grupo de recursos especificado, también se eliminarán.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myresourcegroup
```

Para eliminar solo el servidor creado en este inicio rápido sin eliminar el grupo de recursos, use el cmdlet `Remove-AzDatabricksWorkspace`.

```azurepowershell-interactive
Remove-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de un clúster de Spark en Databricks](quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks)
