---
title: Referencia sobre la API de administración de la instancia administrada
description: Obtenga información acerca de la creación y administración de Instancias administradas de Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: ca7e123a3a0a1707eb979f25cabfc5913c5bfcb8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84032936"
---
# <a name="managed-api-reference-for-azure-sql-managed-instances"></a>Referencia sobre la API de administración de Instancias administradas de Azure SQL
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Puede crear y administrar Instancias administradas de Azure SQL mediante Azure Portal, PowerShell, la CLI de Azure, la API de REST y Transact-SQL. En este artículo, puede encontrar información general sobre las funciones y la API que puede usar para crear y configurar Instancia administrada.

## <a name="azure-portal-create-a-managed-instance"></a>Azure Portal: Creación de una instancia administrada

Para consultar un artículo de inicio rápido que muestra cómo crear una Instancia administrada de SQL Database, consulte [Inicio rápido: Creación de una Instancia administrada de SQL](instance-create-quickstart.md).

## <a name="powershell-create-and-manage-managed-instances"></a>PowerShell: Creación y administración de instancias administradas

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos.

Para crear y administrar instancias administradas con Azure PowerShell, use los siguientes cmdlets de PowerShell. Si necesita instalar o actualizar PowerShell, consulte [Install and configure Azure PowerShell](/powershell/azure/install-az-ps) (Instalación y configuración de Azure PowerShell).

> [!TIP]
> Para los scripts de ejemplo de PowerShell, consulte [Quick-start script: Creación de una Instancia administrada de SQL mediante una biblioteca de PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | Descripción |
| --- | --- |
|[New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)|Crea una Instancia administrada de Azure SQL. |
|[Get-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstance)|Devuelve información sobre Instancia administrada de Azure SQL Database|
|[Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)|Establece las propiedades para una Instancia administrada de Azure SQL.|
|[Remove-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance)|Elimina una Instancia administrada de Azure SQL Database|
|[New-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstancedatabase)|Crea una base de datos de Instancia administrada de Azure SQL.|
|[Get-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabase)|Devuelve información sobre base de datos de Instancia administrada de Azure SQL Database|
|[Remove-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabase)|Elimina una base de datos de Instancia administrada de Azure SQL Database|
|[Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)|Restaura una base de datos de Instancia administrada de Azure SQL Database|

## <a name="azure-cli-create-and-manage-managed-instances"></a>CLI de Azure: Creación y administración de instancias administradas

Para crear y administrar instancias administradas con la [CLI de Azure](/cli/azure), use los siguientes comandos de [Instancia administrada de la CLI de Azure SQL](/cli/azure/sql/mi). Use [Cloud Shell](/azure/cloud-shell/overview) para ejecutar la CLI en el explorador o [instálela](/cli/azure/install-azure-cli) en Windows, Linux o macOS.

> [!TIP]
> Para consultar un artículo de inicio rápido acerca de la CLI de Azure, consulte [Working with SQL Managed Instance using Azure CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) (Uso de Instancia administrada de Azure SQL Database mediante la CLI de Azure).

| Cmdlet | Descripción |
| --- | --- |
|[az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Crea una instancia administrada|
|[az sql mi list](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Enumera las instancias administradas disponibles|
|[az sql mi show](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Obtiene los detalles de una instancia administrada|
|[az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Actualiza una instancia administrada|
|[az sql mi delete](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Elimina una instancia administrada|
|[az sql midb create](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Crea una base de datos administrada|
|[az sql midb list](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Enumera las bases de datos administradas disponibles|
|[az sql midb restore](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Restaura una base de datos administrada|
|[az sql midb delete](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Elimina una base de datos administrada|

## <a name="transact-sql-create-and-manage-instance-databases"></a>Transact-SQL: Creación y administración de bases de datos de instancia

Para crear y administrar una base de datos de instancia después de crear Instancia administrada, use los siguientes comandos de T-SQL. Puede emitir estos comandos mediante Azure Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio) y [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is). [Visual Studio Code](https://code.visualstudio.com/docs), o cualquier otro programa que se pueda conectar a un servidor y pasar comandos de Transact-SQL.

> [!TIP]
> Para consultar artículos de inicio rápido que describan la configuración y conexión a una Instancia administrada de SQL mediante SQL Server Management Studio en Microsoft Windows, consulte [Inicio rápido: Configuración de una máquina virtual de Azure para una instancia administrada de Azure SQL](connect-vm-instance-configure.md) e [Inicio rápido: Configuración de una conexión de punto a sitio a una instancia administrada de Azure SQL desde el entorno local](point-to-site-p2s-configure.md).
> [!IMPORTANT]
> No se puede crear ni eliminar una instancia administrada mediante Transact-SQL.

| Get-Help | Descripción |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Crea una nueva base de datos de instancia administrada. Debe estar conectado a la base de datos maestra para crear una base de datos.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Modifica una base de datos de Instancia administrada de Azure SQL Database.|

## <a name="rest-api-create-and-manage-managed-instances"></a>API REST: Creación y administración de instancias administradas

Para crear y administrar instancias administradas, use estas solicitudes de la API de REST.

| Get-Help | Descripción |
| --- | --- |
|[Managed Instances - Create Or Update](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Crea o actualiza una instancia administrada.|
|[Managed Instances - Delete](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Elimina una instancia administrada.|
|[Managed Instances - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Obtiene una instancia administrada.|
|[Managed Instances - List](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Devuelve una lista de instancias administradas en una suscripción.|
|[Managed Instances - List By Resource Group](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Devuelve una lista de instancias administradas en un grupo de recursos.|
|[Managed Instances - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Actualiza una instancia administrada.|

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo migrar una base de datos de SQL Server a Azure, vea [Migración a Azure SQL Database](../database/migrate-to-database-from-sql-server.md).
- Para obtener información sobre las características admitidas, consulte el [artículo que trata sobre dicho tema](../database/features-comparison.md).
