---
title: 'Instancia administrada: restauración a un momento dado (PITR)'
description: Restaure una base de datos SQL de una instancia administrada a un momento dado anterior.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, mathoma
ms.date: 08/25/2019
ms.openlocfilehash: 27f465e6864d0ff639e825c8a816d86648bd8853
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79232496"
---
# <a name="restore-a-sql-database-in-a-managed-instance-to-a-previous-point-in-time"></a>Restauración de una base de datos SQL de una instancia administrada a un momento dado anterior

Use la restauración a un momento dado para crear una base de datos como una copia de otra base de datos de algún momento del pasado. En este artículo se describe cómo realizar una restauración a un momento dado de una base de datos de una instancia administrada de Azure SQL Database.

La restauración a un momento dado es útil en escenarios de recuperación, como incidentes causados por errores, datos cargados incorrectamente o eliminación de datos cruciales. También se puede usar simplemente para realizar pruebas o auditorías. Los archivos de copia de seguridad se conservan entre 7 y 35 días, en función de la configuración de la base de datos.

La restauración a un momento dado puede restaurar una base de datos:

- de una base de datos existente
- de una base de datos eliminada.
- en la misma instancia administrada o en otra. 

## <a name="limitations"></a>Limitaciones

La restauración a un momento dado en una instancia administrada tiene las siguientes limitaciones:

- Al restaurar de una instancia administrada a otra, ambas instancias deben estar en la misma suscripción y región. Actualmente no se admite la restauración entre regiones y suscripciones.
- No es posible la restauración a un momento dado de una instancia administrada completa. En este artículo solo se explica lo que es posible: la restauración a un momento dado de una base de datos hospedada en una instancia administrada.

> [!WARNING]
> Tenga en cuenta el tamaño de almacenamiento de la instancia administrada. En función del tamaño de los datos que se van a restaurar, podría quedarse sin almacenamiento de instancia. Si no hay suficiente espacio para los datos restaurados, use una estrategia diferente.

En la tabla siguiente se muestran escenarios de restauración a un momento dado para las instancias administradas:

|           |Restauración de una base de datos existente en la misma instancia administrada| Restauración de una base de datos existente en otra instancia administrada|Restauración de una base de datos eliminada en la misma instancia administrada|Restauración de una base de datos eliminada en otra instancia administrada|
|:----------|:----------|:----------|:----------|:----------|
|**Azure Portal**| Sí|No |Sí|No|
|**CLI de Azure**|Sí |Sí |No|No|
|**PowerShell**| Sí|Sí |Sí|Sí|

## <a name="restore-an-existing-database"></a>Restauración de una base de datos existente

Restaure una base de datos existente en la misma instancia mediante Azure Portal, PowerShell o la CLI de Azure. Para restaurar una base de datos en otra instancia, use PowerShell o la CLI de Azure de modo que pueda especificar las propiedades de la instancia administrada y del grupo de recursos de destino. Si no se especifican estos parámetros, la base de datos se restaurará de forma predeterminada en la instancia existente. Azure Portal no admite actualmente la restauración en otra instancia.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
2. Vaya a la instancia administrada y seleccione la base de datos que quiera restaurar.
3. Seleccione **Restaurar** en la página de la base de datos:

    ![Restauración de una base de datos mediante Azure Portal](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

4. En la página **Restaurar**, seleccione el punto relativo a fecha y hora al que quiere restaurar la base de datos.
5. Seleccione **Confirmar** para restaurar la base de datos. Esta acción inicia el proceso de restauración, que crea una base de datos y la rellena con datos de la base de datos original en el momento especificado. Para más información sobre el tiempo de recuperación, consulte [Tiempo de recuperación](sql-database-recovery-using-backups.md#recovery-time).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Si aún no tiene instalado Azure PowerShell, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Para restaurar la base de datos mediante PowerShell, especifique los valores de los parámetros en el siguiente comando. A continuación, ejecute el comando:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"

Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
```

Para restaurar la base de datos en otra instancia administrada, especifique también los nombres del grupo de recursos y de la instancia administrada de destino:  

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

Para obtener más información, consulte [Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase).

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Si no ha instalado aún la CLI de Azure, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).

Para restaurar la base de datos mediante la CLI de Azure, especifique los valores de los parámetros en el siguiente comando. A continuación, ejecute el comando:

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```

Para restaurar la base de datos en otra instancia administrada, especifique también los nombres del grupo de recursos y de la instancia administrada de destino:  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Para una explicación detallada de los parámetros disponibles, consulte la [documentación de la CLI de para restaurar una base de datos de una instancia administrada](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore).

---

## <a name="restore-a-deleted-database"></a>Restaurar una base de datos eliminada

La restauración de una base de datos eliminada solo se puede realizar mediante PowerShell o Azure Portal. Para restaurar una base de datos eliminada en la misma instancia, use Azure Portal o PowerShell. Para restaurar una base de datos eliminada en otra instancia, use PowerShell. 

### <a name="portal"></a>Portal 


Para recuperar una base de datos administrada mediante Azure Portal, abra la página de información general de instancia administrada y seleccione **Bases de datos eliminadas**. Seleccione la base de datos eliminada que quiere restaurar y escriba el nombre de la nueva base de datos que se creará con los datos restaurados de la copia de seguridad.

  ![Captura de pantalla de restaurar base de datos de instancia de Azure SQL eliminada](./media/sql-database-recovery-using-backups/restore-deleted-sql-managed-instance-annotated.png)

### <a name="powershell"></a>PowerShell

Para restaurar una base de datos en la misma instancia, actualice los valores de los parámetros y, luego, ejecute el siguiente comando de PowerShell: 

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$deletedDatabaseName = "<Source database name>"
$targetDatabaseName = "<target database name>"

$deletedDatabase = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
-InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName
```

Para restaurar la base de datos en otra instancia administrada, especifique también los nombres del grupo de recursos y de la instancia administrada de destino:

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName `
   -TargetResourceGroupName $targetResourceGroupName `
   -TargetInstanceName $targetInstanceName 
```

## <a name="overwrite-an-existing-database"></a>Sobrescritura de una base de datos existente

Para sobrescribir una base de datos existente, debe:

1. Quitar la base de datos existente que quiere sobrescribir
2. Cambiar el nombre de la base de datos restaurada a un momento dado por el nombre de la base de datos que se quitó

### <a name="drop-the-original-database"></a>Quitar la base de datos original.

Puede quitar la base de datos mediante Azure Portal, PowerShell o la CLI de Azure.

Otras formas de quitarla es conectarse directamente a la instancia administrada, iniciar SQL Server Management Studio (SSMS) y ejecutar luego el siguiente comando de Transact-SQL (T-SQL):

```sql
DROP DATABASE WorldWideImporters;
```

Use uno de los métodos siguientes para conectarse a su base de datos de la instancia administrada:

- [SSMS/Azure Data Studio mediante una máquina virtual de Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [De punto a sitio](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Punto de conexión público](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portal"></a>[Portal](#tab/azure-portal)

En Azure Portal, seleccione la base de datos de la instancia administrada y elija **Eliminar**.

   ![Eliminación de una base de datos mediante Azure Portal](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Use el siguiente comando de PowerShell para quitar una base de datos existente de una instancia administrada:

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Use el siguiente comando de la CLI de Azure para quitar una base de datos existente de una instancia administrada:

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---

### <a name="alter-the-new-database-name-to-match-the-original-database-name"></a>Modificación del nombre de la nueva base de datos para que coincida con el nombre de base de datos original

Conéctese directamente a la instancia administrada e inicie SQL Server Management Studio. Luego, ejecute la siguiente consulta de Transact-SQL (T-SQL). La consulta cambiará el nombre de la base de datos restaurada por el de la base de datos quitada que pretende sobrescribir.

```sql
ALTER DATABASE WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```

Use uno de los métodos siguientes para conectarse a su base de datos de la instancia administrada:

- [Máquina virtual de Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [De punto a sitio](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Punto de conexión público](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Pasos siguientes

Aprenda sobre las [copias de seguridad automatizadas](sql-database-automated-backups.md).
