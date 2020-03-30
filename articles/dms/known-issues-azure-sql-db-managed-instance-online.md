---
title: Problemas conocidos y limitaciones con las migraciones en línea a la instancia administrada de Azure SQL Database
description: Información sobre problemas conocidos y limitaciones de migración asociadas a las migraciones en línea a la instancia administrada de Azure SQL Database.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 88e2b5894686ee93caecf33e04940803eb75f394
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77648672"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-database-managed-instance"></a>Problemas conocidos y limitaciones con las migraciones en línea a la instancia administrada de Azure SQL Database

A continuación se describen los problemas conocidos y las limitaciones relacionadas con las migraciones en línea de SQL Server a la instancia administrada de Azure SQL Database.

> [!IMPORTANT]
> Con las migraciones en línea de SQL Server a Azure SQL Database, no se admite la migración de los tipos de datos SQL_variant.

## <a name="backup-requirements"></a>Requisitos de la copia de seguridad

- **Copias de seguridad con suma de comprobación**

    Azure Database Migration Service usa el método de copia de seguridad y restauración para migrar las bases de datos locales a la instancia administrada de SQL Database. Actualmente, Azure Database Migration Service solo admite las copias de seguridad creadas mediante la suma de comprobación.

    [Habilitar o deshabilitar sumas de comprobación de copia de seguridad durante copia de seguridad o restauración (SQL Server)](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?view=sql-server-2017)

    > [!NOTE]
    > Si realiza copias de seguridad de base de datos con compresión, la suma de comprobación es un comportamiento predeterminado, a menos que se deshabilite explícitamente.

    Con las migraciones sin conexión, si elige **I will let Azure Database Migration Service…** (Permitiré a Azure Database Migration Service...), entonces Azure Database Migration Service realizará la copia de seguridad de base de datos con la opción de suma de comprobación habilitada.

- **Medio de copia de seguridad**

    Asegúrese de realizar todas las copias de seguridad en un medio de copia de seguridad independiente (archivos de copia de seguridad). Azure Database Migration Service no admite las copias de seguridad que se anexan a un único archivo de copia de seguridad. Realice copias de seguridad completas y regístrelas en archivos de copia de seguridad independientes.

## <a name="data-and-log-file-layout"></a>Diseño del archivo de registro y datos

- **Número de archivos de registro**

    Azure Database Migration Service no admite bases de datos con varios archivos de registro. Si tiene varios archivos de registro, puede reducirlos y reorganizarlos en un único archivo de registro de transacciones. Dado que no es posible controlar de forma remota los archivos de registro que no están vacíos, primero debe realizar una copia de seguridad del archivo de registro.

## <a name="sql-server-features"></a>características de SQL Server

- **FileStream/FileTables**

    En la actualidad, la instancia administrada de SQL Database no admite FileStream ni FileTables. En el caso de las cargas de trabajo que dependen de estas características, se recomienda que opte por los servidores SQL Server que se ejecutan en máquinas virtuales de Azure como destino de Azure.

- **Tablas en memoria**

    OLTP en memoria está disponible en los niveles Prémium y Crítico para la empresa para la instancia administrada de SQL Database. El nivel De uso general no admite OLTP en memoria.

## <a name="migration-resets"></a>Restablecimientos de migración

- **Implementaciones**

    La instancia administrada de SQL Database es un servicio PaaS con revisiones automáticas y actualizaciones de versiones. Durante la migración de la instancia administrada de SQL Database, las actualizaciones no críticas resultan de ayuda hasta 36 horas. Después (y para las actualizaciones críticas), si se interrumpe la migración, el proceso se restablece a un estado de restauración completa.

    Solo se puede llamar a la migración total una vez se restaura la copia de seguridad completa y se pone al día con todas las copias de seguridad de registros. Si las migraciones totales de producción se ven afectadas, póngase en contacto con el [alias de comentarios de Azure DMS](mailto:dmsfeedback@microsoft.com).
