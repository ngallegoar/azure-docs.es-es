---
title: Cifrado de datos transparente (T-SQL)
description: Cifrado de datos transparente (TDE) en Azure Synapse Analytics (T-SQL)
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/30/2019
ms.author: jrasnick
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: ae751cc5b8e3ab67f3e65757724d0ebae1c45e02
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80745254"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Introducción al cifrado de datos transparente (TDE)

> [!div class="op_single_selector"]
>
> * [Información general sobre seguridad](sql-data-warehouse-overview-manage-security.md)
> * [Autenticación](sql-data-warehouse-authentication.md)
> * [Cifrado (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Cifrado (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permissions"></a>Permisos necesarios

Para habilitar el Cifrado de datos transparente (TDE), debe ser un administrador o un miembro del rol dbmanager.

## <a name="enabling-encryption"></a>Habilitar el cifrado

Siga estos pasos para habilitar TDE:

1. Conéctese a la base de datos *maestra* en el servidor que hospeda la base de datos mediante un inicio de sesión que es un administrador o un miembro del rol **dbmanager** en la base de datos maestra.
2. Ejecute la siguiente instrucción para cifrar la base de datos.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Deshabilitar el cifrado

Siga estos pasos para deshabilitar TDE:

1. Conéctese a la base de datos *maestra* mediante un inicio de sesión que es un administrador o un miembro del rol **dbmanager** en la base de datos maestra.
2. Ejecute la siguiente instrucción para cifrar la base de datos.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Se debe reanudar una instancia del grupo de SQL en pausa antes de hacer cambios en la configuración de TDE.

## <a name="verifying-encryption"></a>Comprobación del cifrado

Para comprobar el estado del cifrado, siga estos pasos:

1. Conéctese a la base de datos *maestra* o de instancia mediante un inicio de sesión que es un administrador o un miembro del rol **dbmanager** en la base de datos maestra.
2. Ejecute la siguiente instrucción para cifrar la base de datos.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Un resultado de ```1``` indica una base de datos cifrada, ```0``` indica una base de datos no cifrada.

## <a name="encryption-dmvs"></a>DMV de cifrado

* [sys.databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sys.dm_pdw_nodes_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
