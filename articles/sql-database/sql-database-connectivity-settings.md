---
title: Configuración de conectividad para Azure SQL Database y Data Warehouse
description: En este artículo se describe la elección de la versión de TLS y el proxy en comparación con la configuración de redireccionamiento de Azure SQL
services: sql-database
ms.service: sql-database
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 03/09/2020
ms.openlocfilehash: d18fdee85bd0fbabe68fe9890c4a2dc74366041d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366094"
---
# <a name="azure-sql-connectivity-settings"></a>Configuración de la conectividad de Azure SQL
> [!NOTE]
> Este artículo se aplica a Azure SQL Server y tanto a las bases de datos de SQL Database como a SQL Data Warehouse que se crean en el servidor de Azure SQL. Para simplificar, SQL Database se utiliza cuando se hace referencia tanto a SQL Database como a SQL Data Warehouse.

> [!IMPORTANT]
> Este artículo *no* es válido para las **instancias administradas de Azure SQL Database**.

Este artículo presenta los valores que controlan la conectividad con Azure SQL Database en el nivel del servidor. Estos valores se aplican a **todas** las bases de datos de SQL Database y SQL Data Warehouse asociadas al servidor.

> [!NOTE]
> Una vez que se aplican estos valores, **surten efecto inmediato** y pueden producir la pérdida de conexión de los clientes si no cumplen los requisitos de cada valor.

Se puede acceder a los valores de conectividad desde la hoja **Firewalls y redes virtuales** como se indica en la siguiente captura de pantalla:

 ![Captura de pantalla de los valores de conectividad][1]


## <a name="deny-public-network-access"></a>Denegación del acceso a una red pública
En Azure Portal, si el valor **Deny public network access** (Denegar acceso a red pública) está establecido en **Sí**, solo se permiten las conexiones mediante puntos de conexión privados. Si el valor se establece en **No**, los clientes se podrán conectar mediante el punto de conexión privado o el público.

Después de establecer **Deny public network access** (Denegar acceso a red pública) en **Sí**, los intentos de conexión de clientes a través del punto de conexión público producirán el siguiente error:

```output
Error 47073
An instance-specific error occurred while establishing a connection to SQL Server. The public network interface on this server is not accessible. To connect to this server, use the Private Endpoint from inside your virtual network.
```

## <a name="change-public-network-access-via-powershell"></a>Cambio del acceso a la red pública mediante PowerShell
[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos. El script siguiente requiere el [módulo de Azure PowerShell](/powershell/azure/install-az-ps).

El siguiente script de PowerShell le muestra cómo `Get` y `Set` la propiedad **Public Network Access** (Acceso a la red pública) en el nivel del servidor lógico:

```powershell
#Get the Public Network Access property
(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).PublicNetworkAccess

# Update Public Network Access to Disabled
$SecureString = ConvertTo-SecureString "password" -AsPlainText -Force

Set-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group -SqlAdministratorPassword $SecureString -PublicNetworkAccess "Enabled" 
```

## <a name="change-public-network-access-via-cli"></a>Cambio del acceso a la red pública mediante la CLI
> [!IMPORTANT]
> Todos los scripts de esta sección requieren la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="azure-cli-in-a-bash-shell"></a>CLI de Azure en un shell de Bash
El siguiente script de la CLI muestra cómo cambiar el **acceso a la red pública** en un shell de Bash:

```azurecli-interactive

# Get current setting for Public Network Access
az sql server show -n sql-server-name -g sql-server-group --query "publicNetworkAccess"

# Update setting for Public Network Access
az sql server update -n sql-server-name -g sql-server-group --set publicNetworkAccess="Disabled"

```


## <a name="connection-policy"></a>Directiva de conexión
La [directiva de conexión](sql-database-connectivity-architecture.md#connection-policy) determina cómo se conectan los clientes a Azure SQL Server. 

## <a name="change-connection-policy-via-powershell"></a>Cambio de la directiva de conexión mediante PowerShell
[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos. El script siguiente requiere el [módulo de Azure PowerShell](/powershell/azure/install-az-ps).

El siguiente script muestra cómo cambiar la directiva de conexión mediante PowerShell:

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="change-connection-policy-via-azure-cli"></a>Cambio de la directiva de conexión mediante la CLI de Azure
> [!IMPORTANT]
> Todos los scripts de esta sección requieren la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="azure-cli-in-a-bash-shell"></a>CLI de Azure en un shell de Bash
El siguiente script de CLI muestra cómo cambiar la directiva de conexión en un shell de Bash: 

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
ids="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $ids

# Update connection policy
az resource update --ids $ids --set properties.connectionType=Proxy
```

### <a name="azure-cli-from-a-windows-command-prompt"></a>CLI de Azure desde un símbolo del sistema de Windows
En el siguiente script de la CLI se muestra cómo cambiar la directiva de conexión desde un símbolo del sistema de Windows (con la CLI de Azure instalada).

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Pasos siguientes
- Para obtener información general sobre cómo funciona la conectividad en Azure SQL Database, consulte [Arquitectura de conectividad de Azure SQL](sql-database-connectivity-architecture.md)
- Para información sobre cómo cambiar la directiva de conexión de Azure SQL Database para un servidor de Azure SQL Database, consulte [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/manage-connectivity-settings.png
