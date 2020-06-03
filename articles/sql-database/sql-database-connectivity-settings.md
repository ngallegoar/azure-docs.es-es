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
ms.openlocfilehash: ef846b23bf6c9da2b7dd64927eac7cc1a1704dae
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684941"
---
# <a name="azure-sql-connectivity-settings"></a>Configuración de la conectividad de Azure SQL
> [!NOTE]
> Este artículo se aplica al servidor lógico SQL en Azure usado para bases de datos tanto de Azure SQL Database como de SQL Data Warehouse que se crean en el servidor lógico. Para simplificar, SQL Database se utiliza cuando se hace referencia tanto a SQL Database como a SQL Data Warehouse.

> [!IMPORTANT]
> Este artículo *no* es válido para las **instancias administradas de Azure SQL Database**.

Este artículo presenta los valores que controlan la conectividad con Azure SQL Database en el nivel del servidor. Estos valores se aplican a **todas** las bases de datos de SQL Database y SQL Data Warehouse asociadas al servidor.

> [!NOTE]
> Una vez que se aplican estos valores, **surten efecto inmediato** y pueden producir la pérdida de conexión de los clientes si no cumplen los requisitos de cada valor.

Se puede acceder a la configuración de conectividad desde la pantalla **Firewalls y redes virtuales**, como se indica en la siguiente captura de pantalla:

 ![Captura de pantalla de los valores de conectividad][1]


## <a name="deny-public-network-access"></a>Denegación del acceso a una red pública

Los clientes pueden conectarse a SQL Database mediante puntos de conexión públicos (reglas de firewall basadas en IP, reglas de firewall basadas en redes virtuales) o puntos de conexión privados (mediante Private Link), tal como se describe en la [información general del acceso a redes](sql-database-networkaccess-overview.md). 

Cuando la opción **Deny public network access** (Denegar acceso a la red pública) está establecida en **Sí**, solo se permiten conexiones a través de puntos de conexión privados, y todas las conexiones a través de puntos de conexión públicos se deniegan con un mensaje de error similar al siguiente:  

```output
Error 47073
An instance-specific error occurred while establishing a connection to SQL Server. 
The public network interface on this server is not accessible. 
To connect to this server, use the Private Endpoint from inside your virtual network.
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

## <a name="minimal-tls-version"></a>Versión mínima de TLS 

La opción de versión mínima de [Seguridad de la capa de transporte (TLS)](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server) permite a los clientes controlar la versión de TLS utilizada por sus instancias de Azure SQL Database.

Actualmente, se admiten TLS 1.0, 1.1 y 1.2. La opción de versión mínima de TLS garantiza que se admitan las versiones posteriores más recientes de TLS. Por ejemplo, elegir una versión de TLS superior a 1.1. significa que solo se aceptan conexiones con TLS 1.1 y 1.2, y se rechaza TLS 1.0. Después de realizar las pruebas para confirmar que las aplicaciones son compatibles, se recomienda establecer la versión mínima de TLS en 1.2, ya que incluye correcciones de vulnerabilidades que se han encontrado en versiones anteriores, y es la versión más alta de TLS admitida en Azure SQL Database.

En el caso de los clientes con aplicaciones que se basan en versiones anteriores de TLS, se recomienda establecer la versión mínima de TLS según los requisitos de las aplicaciones. En el caso de los clientes que se basan en aplicaciones para conectarse mediante una conexión sin cifrar, se recomienda no establecer ninguna versión mínima de TLS. 

Para obtener más información, consulte [Consideraciones de TLS para la conectividad de SQL Database](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

Después de establecer la versión mínima de TLS, se producirá un error en los intentos de inicio de sesión de los clientes que usen una versión de TLS inferior a la versión mínima de TLS del servidor, con el siguiente error:

```output
Error 47072
Login failed with invalid TLS version
```

## <a name="set-minimal-tls-version-via-powershell"></a>Establecimiento de la versión mínima de TLS mediante PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos. El script siguiente requiere el [módulo de Azure PowerShell](/powershell/azure/install-az-ps).

El siguiente script de PowerShell le muestra cómo `Get` y `Set` la propiedad **Minimal TLS Version** (Versión mínima de TLS) en el nivel del servidor lógico:

```powershell
#Get the Public Network Access property
(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).PublicNetworkAccess

# Update Public Network Access to Disabled
$SecureString = ConvertTo-SecureString "password" -AsPlainText -Force

Set-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group -SqlAdministratorPassword $SecureString  -MinimalTlsVersion "1.2"
```

## <a name="set-minimal-tls-version-via-azure-cli"></a>Establecimiento de la versión mínima de TLS mediante la CLI de Azure

> [!IMPORTANT]
> Todos los scripts de esta sección requieren la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="azure-cli-in-a-bash-shell"></a>CLI de Azure en un shell de Bash

El siguiente script de la CLI muestra cómo cambiar la opción **Minimal TLS Version** (Versión mínima de TLS) en un shell de Bash:

```azurecli-interactive
# Get current setting for Minimal TLS Version
az sql server show -n sql-server-name -g sql-server-group --query "minimalTlsVersion"

# Update setting for Minimal TLS Version
az sql server update -n sql-server-name -g sql-server-group --set minimalTlsVersion="1.2"
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
