---
title: 'Tutorial: Adición de una base de datos única a un grupo de conmutación por error'
description: Adición de una base de datos única de Azure SQL Database a un grupo de conmutación por error mediante Azure Portal, PowerShell o la CLI de Azure
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: c5ce6a1c2f231d372a2a8113eb9043a236090388
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061689"
---
# <a name="tutorial-add-an-azure-sql-database-single-database-to-a-failover-group"></a>Tutorial: Adición de una base de datos única de Azure SQL Database a un grupo de conmutación por error

Un [grupo de conmutación por error](sql-database-auto-failover-group.md) es una capa de abstracción declarativa que permite agrupar varias bases de datos con replicación geográfica. Aprenda a configurar un grupo de conmutación por error para una base de datos única de Azure SQL Database y pruebe la conmutación por error mediante Azure Portal, PowerShell o la CLI de Azure.  En este tutorial, aprenderá a:

> [!div class="checklist"]
> - Crear una base de datos única de Azure SQL Database.
> - Crear un grupo de conmutación por error para una base de datos única entre dos servidores lógicos de SQL.
> - Probar la conmutación por error.

## <a name="prerequisites"></a>Prerrequisitos

# <a name="portal"></a>[Portal](#tab/azure-portal)
Para completar este tutorial, asegúrese de disponer de los siguientes elementos: 

- Suscripción a Azure. [Cree una cuenta gratuita](https://azure.microsoft.com/free/) si aún no tiene una.


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Para completar el tutorial, asegúrese de que cuenta con los elementos siguientes:

- Suscripción a Azure. [Cree una cuenta gratuita](https://azure.microsoft.com/free/) si aún no tiene una.
- [Azure PowerShell](/powershell/azureps-cmdlets-docs)


# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
Para completar el tutorial, asegúrese de que cuenta con los elementos siguientes:

- Suscripción a Azure. [Cree una cuenta gratuita](https://azure.microsoft.com/free/) si aún no tiene una.
- La versión más reciente de la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest). 

---

## <a name="1---create-a-single-database"></a>1\. Creación de una base de datos única 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---create-the-failover-group"></a>2\. Creación de un grupo de conmutación por error 
En este paso, va a crear un [grupo de conmutación por error](sql-database-auto-failover-group.md) entre un servidor de Azure SQL Server existente y uno nuevo en otra región. A continuación, agregue la base de datos de ejemplo al grupo de conmutación por error. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Cree el grupo de conmutación por error y agregue la base de datos única mediante Azure Portal. 

1. Seleccione **Azure SQL** en el menú izquierdo de [Azure Portal](https://portal.azure.com). Si **Azure SQL** no está en la lista, seleccione **Todos los servicios** y escriba Azure SQL en el cuadro de búsqueda. (Opcional) Seleccione la estrella junto a **Azure SQL** para marcarlo como favorito y agréguelo como un elemento en el panel de navegación izquierdo. 
1. Seleccione la base de datos única creada en la sección 1, `mySampleDatabase`. 
1. Los grupos de conmutación por error se pueden configurar en el nivel de servidor. Seleccione el nombre del servidor en **Nombre del servidor** para abrir la configuración del servidor.

   ![Abrir servidor para una base de datos única](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. Seleccione **Grupos de conmutación por error** en el panel **Configuración** y, después, seleccione **Agregar grupo** para crear un nuevo grupo de conmutación por error. 

    ![Adición de un grupo de conmutación por error](media/sql-database-single-database-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. En la página **Grupo de conmutación por error**, escriba o seleccione los valores siguientes y, después, seleccione **Crear**:
    - **Nombre del grupo de conmutación por error** Escriba un nombre del grupo de conmutación por error único, como `failovergrouptutorial`. 
    - **Servidor secundario**: Seleccione la opción para *configurar los valores obligatorios* y, a continuación, elija **Crear un nuevo servidor**. Como alternativa, puede elegir un servidor ya existente como servidor secundario. Después de escribir los valores siguientes, elija **Seleccionar**. 
        - **Nombre del servidor**: Escriba un nombre único para el servidor secundario, como `mysqlsecondary`. 
        - **Inicio de sesión del administrador del servidor**: Escriba `azureuser`
        - **Contraseña**: Escriba una contraseña compleja que cumpla los requisitos de contraseña.
        - **Ubicación**: Elija una ubicación en la lista desplegable, como `East US`. Esta ubicación no puede ser la misma que la del servidor principal.

    > [!NOTE]
    > La configuración del firewall y de inicio de sesión del servidor debe coincidir con la del servidor principal. 
    
      ![Creación de un servidor secundario para el grupo de conmutación por error](media/sql-database-single-database-failover-group-tutorial/create-secondary-failover-server.png)

   - **Bases de datos en el grupo** Cuando se selecciona un servidor secundario, esta opción se desbloquea. Selecciónelo para **seleccionar las bases de datos que quiera agregar.** y después elija la base de datos que ha creado en la sección 1. Al agregar la base de datos al grupo de conmutación por error, se iniciará automáticamente el proceso de replicación geográfica. 
        
    ![Adición de la base de datos SQL a un grupo de conmutación por error](media/sql-database-single-database-failover-group-tutorial/add-sqldb-to-failover-group.png)
        

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Cree el grupo de conmutación por error y agregue la base de datos única mediante PowerShell. 

   > [!NOTE]
   > La configuración del firewall y de inicio de sesión del servidor debe coincidir con la del servidor principal. 

   ```powershell-interactive
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "West US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $databaseName = "mySampleDatabase"
   $drLocation = "East US"
   $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # The ip address range that you want to allow to access your server 
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "DR Server name is" $drServerName 
   Write-host "Failover group name is" $failoverGroupName
   
   # Create a secondary server in the failover region
   Write-host "Creating a secondary logical server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for secondary logical server..."
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   $serverFirewallRule   
   
   # Create a failover group between the servers
   $failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
   New-AzSqlDatabaseFailoverGroup `
      –ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -PartnerServerName $drServerName  `
      –FailoverGroupName $failoverGroupName `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $failovergroup
   
   # Add the database to the failover group
   Write-host "Adding the database to the failover group..." 
   Get-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName | `
   Add-AzSqlDatabaseToFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Successfully added the database to the failover group..." 
   ```

En esta parte del tutorial se usan los siguientes cmdlets de PowerShell:

| Get-Help | Notas |
|---|---|
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Crea un servidor de SQL Database que hospeda bases de datos únicas y grupos elásticos. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Crea una regla de firewall para un servidor lógico. | 
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Crea una nueva base de datos única de Azure SQL Database | 
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Crea un grupo de conmutación por error. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Obtiene una o más bases de datos SQL. |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Agrega una o varias bases de datos de Azure SQL a un grupo de conmutación por error. |

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
Cree el grupo de conmutación por error y agregue la base de datos única mediante la CLI de AZ. 

   > [!NOTE]
   > La configuración del firewall y de inicio de sesión del servidor debe coincidir con la del servidor principal. 

   ```azurecli-interactive
   #!/bin/bash
   # set variables
   $failoverLocation = "West US"
   $failoverServer = "failoverServer-$randomIdentifier"
   $failoverGroup = "failoverGroup-$randomIdentifier"

   echo "Creating a secondary logical server in the DR region..."
   az sql server create --name $failoverServer --resource-group $resourceGroup --location $failoverLocation --admin-user $login --admin-password $password
   
   echo "Creating a failover group between the two servers..."
   az sql failover-group create --name $failoverGroup --partner-server $failoverServer --resource-group $resourceGroup --server $server --add-db $database --failover-policy Automatic
   ```

En esta parte del tutorial se usan los siguientes cmdlets de la CLI de AZ:

| Get-Help | Notas |
|---|---|
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Crea un servidor de SQL Database que hospeda bases de datos únicas y grupos elásticos. |
| [az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule) | Crea las reglas de firewall de un servidor. | 
| [az sql failover-group create](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Crea un grupo de conmutación por error. | 

---

## <a name="3---test-failover"></a>3\. Prueba de la conmutación por error 
En este paso, se producirá un error en el grupo de conmutación por error en el servidor secundario y, a continuación, se realizará la conmutación por recuperación mediante Azure Portal. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Pruebe la conmutación por error mediante Azure Portal. 

1. Seleccione **Azure SQL** en el menú izquierdo de [Azure Portal](https://portal.azure.com). Si **Azure SQL** no está en la lista, seleccione **Todos los servicios** y escriba Azure SQL en el cuadro de búsqueda. (Opcional) Seleccione la estrella junto a **Azure SQL** para marcarlo como favorito y agréguelo como un elemento en el panel de navegación izquierdo. 
1. Seleccione la base de datos única creada en la sección 2, `mySampleDatbase`. 
1. Seleccione el nombre del servidor en **Nombre del servidor** para abrir la configuración del servidor.

   ![Abrir servidor para una base de datos única](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. Seleccione **Grupos de conmutación por error** en el panel **Configuración** y, a continuación, elija el grupo de conmutación por error que ha creado en la sección 2. 
  
   ![Seleccione el grupo de conmutación por error en el portal.](media/sql-database-single-database-failover-group-tutorial/select-failover-group.png)

1. Revise cuál es el servidor principal y cuál el secundario. 
1. Seleccione **Conmutación por error** en el panel de tareas para conmutar por error el grupo correspondiente que contiene la base de datos de ejemplo 
1. Seleccione **Sí** en la advertencia que le notifica que las sesiones de TDS se desconectarán. 

   ![Conmutación por error del grupo de conmutación por error que contiene la base de datos SQL](media/sql-database-single-database-failover-group-tutorial/failover-sql-db.png)

1. Revise qué servidor es ahora el principal y cuál el secundario. Si la conmutación por error se realiza correctamente, los dos servidores deben tener los roles intercambiados. 
1. Vuelva a seleccionar **Conmutación por error** para devolver los servidores a sus roles originales. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Pruebe la conmutación por error mediante PowerShell. 


Compruebe el rol de la réplica secundaria: 

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...." 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```

Conmute por error el servidor secundario: 

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully to" $drServerName 
   ```

Revierta el grupo de conmutación por error al servidor principal:

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully back to" $serverName
   ```

En esta parte del tutorial se usan los siguientes cmdlets de PowerShell:

| Get-Help | Notas |
|---|---|
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Obtiene o enumera grupos de conmutación por error de Azure SQL Database. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Ejecuta una conmutación por error de un grupo de conmutación por error de Azure SQL Database. |



# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
Pruebe la conmutación por error mediante la CLI de Azure. 

Compruebe cuál es el servidor secundario:

   
   ```azurecli-interactive
   echo "Verifying which server is in the secondary role..."
   az sql failover-group list --server $server --resource-group $resourceGroup
   ```

Conmute por error el servidor secundario: 

   ```azurecli-interactive
   echo "Failing over group to the secondary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $failoverServer
   echo "Successfully failed failover group over to" $failoverServer
   ```

Revierta el grupo de conmutación por error al servidor principal:

   ```azurecli-interactive
   echo "Failing over group back to the primary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $server
   echo "Successfully failed failover group back to" $server
   ```

En esta parte del tutorial se usan los siguientes cmdlets de la CLI de AZ:

| Get-Help | Notas |
|---|---|
| [az sql failover-group list](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Enumera los grupos de conmutación por error de un servidor. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Establece el servidor principal del grupo de conmutación por error mediante la conmutación por error de todas las bases de datos desde el servidor principal actual. | 

---

## <a name="clean-up-resources"></a>Limpieza de recursos 
Limpie los recursos mediante la eliminación del grupo de recursos. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Elimine el grupo de recursos mediante Azure Portal. 

1. Vaya a su grupo de recursos en [Azure Portal](https://portal.azure.com).
1. Seleccione **Eliminar grupo de recursos** para eliminar todos los recursos del grupo, así como el propio grupo de recursos. 
1. En la nueva ventana, escriba el nombre del grupo de recursos, `myResourceGroup`, y luego seleccione **Eliminar** para eliminar el grupo de recursos.  

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Elimine el grupo de recursos mediante Azure PowerShell. 

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"

   # Remove the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

En esta parte del tutorial se usan los siguientes cmdlets de PowerShell:

| Get-Help | Notas |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos. | 

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Elimine el grupo de recursos mediante la CLI de Azure. 


   ```azurecli-interactive
   echo "Cleaning up resources by removing the resource group..."
   az group delete --name $resourceGroup
   echo "Successfully removed resource group" $resourceGroup
   ```

En esta parte del tutorial se usan los siguientes cmdlets de la CLI de AZ:

| Get-Help | Notas |
|---|---|
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

---


> [!IMPORTANT]
> Si quiere mantener el grupo de recursos, pero eliminar la base de datos secundaria, quítela del grupo de conmutación por error antes de eliminarla. Eliminar una base de datos secundaria antes de quitarla del grupo de conmutación por error puede provocar un comportamiento impredecible. 


## <a name="full-scripts"></a>Scripts completos

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!code-powershell-interactive[main](../../powershell_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-ps.ps1 "Add single database to a failover group")]

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Crea un servidor de SQL Database que hospeda bases de datos únicas y grupos elásticos. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Crea una regla de firewall para un servidor lógico. | 
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Crea una nueva base de datos única de Azure SQL Database | 
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Crea un grupo de conmutación por error. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Obtiene una o más bases de datos SQL. |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Agrega una o varias bases de datos de Azure SQL a un grupo de conmutación por error. |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Obtiene o enumera grupos de conmutación por error de Azure SQL Database. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Ejecuta una conmutación por error de un grupo de conmutación por error de Azure SQL Database. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos. | 

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

[!code-azurecli-interactive[main](../../cli_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-cli.sh "Add single database to a failover group")]

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az account set](/cli/azure/account?view=azure-cli-latest#az-account-set) | Establece una suscripción como la suscripción activa actual. | 
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Crea un servidor de SQL Database que hospeda bases de datos únicas y grupos elásticos. |
| [az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule) | Crea las reglas de firewall de un servidor. | 
| [az sql db create](/cli/azure/sql/db?view=azure-cli-latest) | Crea una base de datos. | 
| [az sql failover-group create](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Crea un grupo de conmutación por error. | 
| [az sql failover-group list](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Enumera los grupos de conmutación por error de un servidor. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Establece el servidor principal del grupo de conmutación por error mediante la conmutación por error de todas las bases de datos desde el servidor principal actual. | 
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

# <a name="portal"></a>[Portal](#tab/azure-portal)
No hay scripts disponibles para Azure Portal. 
 
---

Puede encontrar otros scripts de Azure SQL Database aquí: [Azure PowerShell](sql-database-powershell-samples.md) y [CLI de Azure](sql-database-cli-samples.md). 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha agregado una base de datos única de Azure SQL Database a un grupo de conmutación por error y ha probado la conmutación por error. Ha aprendido a: 

> [!div class="checklist"]
> - Crear una base de datos única de Azure SQL Database. 
> - Crear un [grupo de conmutación por error](sql-database-auto-failover-group.md) para una base de datos única entre dos servidores lógicos de SQL.
> - Probar la conmutación por error.

Avance al siguiente tutorial sobre cómo agregar un grupo elástico a un grupo de conmutación por error. 

> [!div class="nextstepaction"]
> [Tutorial: Adición de un grupo elástico de Azure SQL Database a un grupo de conmutación por error](sql-database-elastic-pool-failover-group-tutorial.md)
