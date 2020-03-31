---
title: API de PowerShell y REST
description: Busque los principales cmdlets de PowerShell para el grupo de SQL de Azure Synapse Analytics, incluidos aquellos para pausar y reanudar una base de datos.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 5f22de08c4eabd3f9a3d6ee29cad3f0a9e8509e0
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351387"
---
# <a name="powershell--rest-apis-for-azure-synapse-analytics-sql-pool"></a>API de REST y PowerShell para el grupo de SQL de Azure Synapse Analytics

Muchas tareas de administración del grupo de SQL de Azure Synapse Analytics se pueden administrar mediante los cmdlets de Azure PowerShell o las API de REST.  A continuación, se muestran algunos ejemplos de cómo usar comandos de PowerShell para automatizar tareas comunes en el grupo de SQL.  Para ver algunos buenos ejemplos de REST, consulte el artículo [Administración de la potencia de proceso en Almacenamiento de datos SQL de Azure (REST)](sql-data-warehouse-manage-compute-rest-api.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="get-started-with-azure-powershell-cmdlets"></a>Introducción a los cmdlets de Azure PowerShell

1. Abra Windows PowerShell.
2. En el símbolo del sistema de PowerShell, ejecute estos comandos para iniciar sesión en Azure Resource Manager y seleccione su suscripción.
   
    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Select-AzSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-data-warehouse-example"></a>Ejemplo de pausa de almacenamiento de datos
Pausa una base de datos denominada "Database02" que está hospedada en un servidor cuyo nombre es "Server01".  El servidor está en un grupo de recursos de Azure denominado "ResourceGroup1."

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```

Este ejemplo, que es una variación, canaliza el objeto recuperado a [Suspend-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase).  El resultado es que se pausa la base de datos. El comando final muestra los resultados.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="start-data-warehouse-example"></a>Ejemplo de inicio de almacenamiento de datos

Reanuda el funcionamiento de una base de datos denominada "Database02" que está hospedada en un servidor cuyo nombre es "Server01". El servidor está en un grupo de recursos denominado "ResourceGroup1."

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Este ejemplo, que es una variación, recupera una base de datos denominada "Database02" desde un servidor cuyo nombre es "Server01" que está incluido en un grupo de recursos denominado "ResourceGroup1". Canaliza el objeto recuperado a [Resume-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase).

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Tenga en cuenta que si el servidor es foo.database.windows.net, debe usar "foo" como -ServerName en los cmdlets de Powershell.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Otros cmdlets de PowerShell admitidos
Estos cmdlets de PowerShell son compatibles con el almacenamiento de datos de Azure Synapse Analytics.

* [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase)
* [Get-AzSqlDeletedDatabaseBackup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup)
* [Get-AzSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserestorepoint)
* [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
* [Remove-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabase)
* [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)
* [Resume-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase)
* [Select-AzSubscription](https://msdn.microsoft.com/library/dn722499.aspx)
* [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
* [Suspend-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase)

## <a name="next-steps"></a>Pasos siguientes
Para obtener más ejemplos de PowerShell, consulte:

* [Creación de un almacenamiento de datos mediante PowerShell](create-data-warehouse-powershell.md)
* [Restauración de base de datos](sql-data-warehouse-restore-points.md)

Para conocer otras tareas que se pueden automatizar con PowerShell, vea [Cmdlets de Azure SQL Database](https://docs.microsoft.com/powershell/module/az.sql). No todos los cmdlets de Azure SQL Database son compatibles con el almacenamiento de datos de Azure Synapse Analytics.  Para ver una lista de todas las tareas que se pueden automatizar con REST, consulte [Operaciones para Azure SQL Database](/rest/api/sql/).
