---
title: Habilitación del cifrado de datos transparente para Stretch Database (T-SQL)
description: Habilitación del cifrado de datos transparente (TDE) para SQL Server Stretch Database en Azure TSQL
services: sql-server-stretch-database
documentationcenter: ''
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
author: blazem-msft
ms.author: blazem
ms.reviewer: jroth
manager: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 6f1f5f55348069dbfe11b4d5857d93f8ba8c9b19
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74033954"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Habilitación del cifrado de datos transparente (TDE) para Stretch Database en Azure (Transact-SQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

El cifrado de datos transparente (TDE) facilita la protección contra la amenaza de actividades malintencionadas, ya que la base de datos se cifra y descifra en tiempo real, se realizan copias de seguridad asociadas y archivos de registro de transacciones en reposo sin requerir cambios en la aplicación.

TDE cifra el almacenamiento de una base de datos completa mediante el uso de una clave simétrica denominada clave de cifrado de base de datos. La clave de cifrado de base de datos está protegida por un certificado de servidor integrado. El certificado de servidor integrado es único para cada servidor de Azure. Microsoft gira automáticamente estos certificados al menos cada 90 días. Para obtener una descripción general del TDE, vea [Cifrado de datos transparente (TDE)].

## <a name="enabling-encryption"></a>Habilitar el cifrado
Para habilitar TDE para una base de datos de Azure que almacena los datos migrados desde una base de datos SQL Server habilitada para Stretch, haga lo siguiente:

1. Conéctese a la base de datos *maestra* en el servidor que hospeda la base de datos mediante un inicio de sesión que es un administrador o un miembro del rol **dbmanager** en la base de datos maestra.
2. Ejecute la siguiente instrucción para cifrar la base de datos.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Deshabilitar el cifrado
Para deshabilitar TDE para una base de datos de Azure que almacena los datos migrados desde una base de datos SQL Server habilitada para Stretch, haga lo siguiente:

1. Conéctese a la base de datos *maestra* mediante un inicio de sesión que es un administrador o un miembro del rol **dbmanager** en la base de datos maestra.
2. Ejecute la siguiente instrucción para cifrar la base de datos.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Comprobación del cifrado
Para comprobar el estado de cifrado de una base de datos de Azure que almacena los datos migrados desde una base de datos de SQL Server habilitada para Stretch, haga lo siguiente:

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

<!--Anchors-->
[Cifrado de datos transparente (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
