---
title: 'Problemas conocidos: Migración de Oracle a Azure Database for PostgreSQL'
titleSuffix: Azure Database Migration Service
description: Obtenga información sobre problemas conocidos y limitaciones de migración con las migraciones en línea de Oracle a un servidor único de Azure Database for PostgreSQL con Azure Database Migration Service.
services: database-migration
author: arunkumarthiags
ms.author: arthiaga
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: troubleshooting
ms.date: 05/20/2020
ms.openlocfilehash: 2316e0314c9d8f36aee123d4a516f382f58e83e6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91291663"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-from-oracle-to-azure-db-for-postgresql-single-server"></a>Problemas conocidos y limitaciones de migración con las migraciones en línea de Oracle a Azure DB for PostgreSQL: servidor único

Los problemas conocidos y las limitaciones relacionadas con las migraciones en línea de Oracle a Azure Database for PostgreSQL (servidor único) se describen en las siguientes secciones.

## <a name="oracle-versions-supported-as-a-source-database"></a>Versiones de Oracle admitidas como base de datos de origen

Azure Database Migration Service admite la conexión a:

- Oracle, versión 10g, 11g y 12c.
- Oracle Enterprise, Standard, Express y Personal Edition.

Azure Database Migration Service no admite la conexión a bases de datos de contenedor de varios inquilinos (CDB).

## <a name="postgresql-versions-supported-as-a-target-database"></a>Versiones de PostgreSQL admitidas como base de datos de destino

Azure Database Migration Service admite la migración a las versiones 9.5, 9.6, 10 y 11 de Azure Database for PostgreSQL (servidor único). Consulte el artículo [Versiones de base de datos de PostgreSQL admitidas](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions) para la información actualizada sobre la compatibilidad de versiones en Azure Database for PostgreSQL (servidor único).

## <a name="datatype-limitations"></a>Limitaciones del tipo de datos

**No** se migrarán los siguientes tipos de datos:

- BFILE
- ROWID
- REF
- UROWID
- ANYDATA
- SDO_GEOMETRY
- Tablas anidadas
- Tipos de datos definidos por el usuario
- Notas
- Columnas virtuales
- Vistas materializadas basadas en la columna ROWID

Además, las columnas BLOB/CLOB vacías se asignan a NULL en el destino.

## <a name="lob-limitations"></a>Limitaciones de LOB

- Cuando está habilitado el modo de aplicación de línea de negocio (LOB) de tamaño limitado, los LOB vacíos en el origen de Oracle se replican como valores NULL.
- No se admiten nombres de objeto largos (más de 30 bytes).
- Los datos de las columnas LONG y LONG RAW no pueden superar los 64 k. Se truncarán los datos de más de 64 k.
- Solo en Oracle 12, no se admiten cambios en las columnas LOB (migradas).
- No se admiten las actualizaciones en las columnas XMLTYPE ni LOB (migradas).

## <a name="known-issues-and-limitations"></a>Limitaciones y problemas conocidos

- El usuario debe tener privilegios de DBA en el servidor de Oracle.
- Los cambios de datos resultantes de las operaciones de partición y subpartición (ADD, DROP, EXCHANGE y TRUNCATE) no se migran y pueden producir los siguientes errores:
  - En el caso de las operaciones ADD, las actualizaciones y eliminaciones en los datos agregados pueden devolver una advertencia "0 rows affected" (0 filas afectadas).
  - En el caso de las operaciones DROP y TRUNCATE, las inserciones nuevas pueden dar lugar a errores de "duplicates" (duplicados).
  - En el caso de las operaciones EXCHANGE, pueden producirse errores "0 rows affected" (0 filas afectadas) y "duplicates" (duplicados).
- Las tablas cuyos nombres contienen apóstrofos no se pueden replicar.
