---
title: Ejemplos de script de la CLI de Azure
description: Ejemplos de script de la CLI de Azure para crear y administrar servidores, grupos elásticos, bases de datos y firewalls de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 02/03/2019
ms.openlocfilehash: 459a5ea69e11a8614c572f68fce039b6cabbb1ed
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061714"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Ejemplos de la CLI de Azure para Azure SQL Database

Azure SQL Database se puede configurar con la <a href="/cli/azure">CLI de Azure</a>.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este tema es preciso que ejecute la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

# <a name="single-database--elastic-pools"></a>[Base de datos única y grupos elásticos](#tab/single-database)

| | |
|---|---|
|**Creación de una base de datos única y un grupo elástico**||
| [Creación de una base de datos única y configuración de una regla de firewall](scripts/sql-database-create-and-configure-database-cli.md) | Crea una base de datos de Azure SQL y configura una regla de firewall de nivel de servidor. |
| [Creación de grupos elásticos y traslado de bases de datos agrupadas](scripts/sql-database-move-database-between-pools-cli.md) | Crea grupos elásticos de SQL, traslada las bases de datos agrupadas de Azure SQL Database y cambia los tamaños de proceso. |
|**Escalado de una base de datos única y un grupo elástico**||
| [Escalado de una base de datos única](scripts/sql-database-monitor-and-scale-database-cli.md) | Escala una base de datos de Azure SQL a un tamaño de proceso distinto después de consultar la información del tamaño de la base de datos. |
| [Escalado de un grupo elástico](scripts/sql-database-scale-pool-cli.md) | Escala un grupo elástico de SQL a un tamaño de proceso distinto. |
|**Configuración de la replicación geográfica y de la conmutación por error**||
| [Incorporación de una base de datos única a un grupo de conmutación por error](scripts/sql-database-add-single-db-to-failover-group-cli.md)| Crea una base de datos y un grupo de conmutación por error, agrega la base de datos al grupo de conmutación por error y, después, prueba la conmutación por error en el servidor secundario. |
| [Configuración de un grupo de conmutación por error para un grupo elástico](scripts/sql-database-add-elastic-pool-to-failover-group-cli.md) | Crea una base de datos, lo agrega a un grupo elástico, agrega el grupo elástico al grupo de conmutación por error y, después, prueba la conmutación por error en el servidor secundario. |
| [Configuración y conmutación por error de una base de datos única mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-database-cli.md)| Configura la replicación geográfica activa para una base de datos de Azure SQL y la conmuta por error a la réplica secundaria. |
| [Configuración y conmutación por error de una base de datos agrupada mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-pool-cli.md)| Configura la replicación geográfica activa para una base de datos de Azure SQL de un grupo elástico de SQL y, después, la conmuta por error a la réplica secundaria. |
| **Detección de amenazas y auditoría** |
| [Configuración de detección de amenazas y auditoría](scripts/sql-database-auditing-and-threat-detection-cli.md)| Configura las directivas de auditoría y detección de amenazas para una base de datos de Azure SQL. |
| **Copia de seguridad, restauración, copia e importación de una base de datos**||
| [Realización de una copia de seguridad de una base de datos](scripts/sql-database-backup-database-cli.md)| Realiza una copia de seguridad de una base de datos SQL de Azure en una copia de seguridad de Azure Storage. |
| [Restauración de una base de datos](scripts/sql-database-restore-database-cli.md)| Restaura una base de datos de Azure SQL desde una copia de seguridad con redundancia geográfica y restaura una base de datos de Azure SQL eliminada a la copia de seguridad más reciente. |
| [Copia de una base de datos en un nuevo servidor](scripts/sql-database-copy-database-to-new-server-cli.md) | Crea una copia de una base de datos de Azure SQL existente en un nuevo servidor SQL de Azure. |
| [Importación de una base de datos desde un archivo bacpac](scripts/sql-database-import-from-bacpac-cli.md)| Importa una base de datos a un servidor SQL Server de Azure desde un archivo *.bacpac*. |
|||

Obtenga más información sobre la [API de la CLI de Azure de la base de datos única](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases).

# <a name="managed-instance"></a>[Instancia administrada](#tab/managed-instance)

En la tabla siguiente se incluyen vínculos a ejemplos de script de la CLI de Azure para la instancia administrada de Azure SQL Database.

| | |
|---|---|
| **Creación de una instancia administrada**||
| [Creación de una instancia administrada](scripts/sql-database-create-configure-managed-instance-cli.md)| Crea una instancia administrada. |
| **Configuración del Cifrado de datos transparente (TDE)**||
| [Administración del Cifrado de datos transparente en una Instancia administrada mediante Azure Key Vault](scripts/transparent-data-encryption-byok-sql-managed-instance-cli.md)| Configura Cifrado de datos transparente (TDE) en una Instancia administrada de Azure SQL mediante Azure Key Vault con varios escenarios clave. |
|**Configuración de un grupo de conmutación por error**||
| [Configuración de un grupo de conmutación por error para una instancia administrada](scripts/sql-database-add-managed-instance-to-failover-group-cli.md) | Crea dos instancias administradas, las agrega a un grupo de conmutación por error y, a continuación, prueba la conmutación por error de la instancia administrada principal en la instancia administrada secundaria. |
|||

Para obtener ejemplos de Instancia administrada, consulte la información sobre [crear](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/), [actualizar](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/), [mover una base de datos](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/), [trabajar con](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) scripts.

Más información sobre la [API de la CLI de Azure de Instancia administrada](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances).

---
