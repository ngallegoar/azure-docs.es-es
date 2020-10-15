---
title: 'Problemas conocidos: Migraciones en línea a SQL Database'
titleSuffix: Azure Database Migration Service
description: Obtenga información sobre limitaciones y problemas conocidos de migración con las migraciones en línea a Azure SQL Database al usar Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: troubleshooting
ms.date: 02/20/2020
ms.openlocfilehash: 6648a20e03facad4b791cacba8513f9f1aa7d2f0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91291918"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-database"></a>Problemas conocidos y limitaciones de migración con las migraciones en línea a Azure SQL Database

A continuación se describen problemas conocidos y limitaciones relacionadas con las migraciones en línea de SQL Server a Azure SQL Database.

> [!IMPORTANT]
> Con las migraciones en línea de SQL Server a Azure SQL Database, no se admite la migración de los tipos de datos SQL_variant.

### <a name="migration-of-temporal-tables-not-supported"></a>Migración de tablas temporales no compatibles

**Síntoma**

Si la base de datos de origen consta de una o de varias tablas temporales, se produce un error en la migración de la base de datos durante la operación de "carga completa de datos" y verá el mensaje siguiente:

```
{ "resourceId":"/subscriptions/<subscription id>/resourceGroups/migrateready/providers/Microsoft.DataMigration/services/<DMS Service name>", "errorType":"Database migration error", "errorEvents":"["Capture functionalities could not be set. RetCode: SQL_ERROR SqlState: 42000 NativeError: 13570 Message: [Microsoft][SQL Server Native Client 11.0][SQL Server]The use of replication is not supported with system-versioned temporal table '[Application. Cities]' Line: 1 Column: -1 "]" }
```

 ![Ejemplo de errores de tablas temporales](media/known-issues-azure-sql-online/dms-temporal-tables-errors.png)

**Solución alternativa**

Para ello, siga los pasos que se describen a continuación.

1. Busque las tablas temporales en el esquema de origen mediante la siguiente consulta.

     ``` 
     select name,temporal_type,temporal_type_desc,* from sys.tables where temporal_type <>0
     ```

2. Excluya estas tablas de la hoja **Configurar los valores de la migración**, en la que especifica las tablas para la migración.

3. Vuelva a ejecutar la actividad de migración.

**Recursos**

Para más información, consulte el artículo [Tablas temporales](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables?view=sql-server-2017).

### <a name="migration-of-tables-includes-one-or-more-columns-with-the-hierarchyid-data-type"></a>La migración de las tablas incluye una o varias columnas con el tipo de datos hierarchyid

**Síntoma**

Es posible que vea una excepción SQL que indique que "ntext no es compatible con hierarchyid" durante la operación de "carga completa de datos":

![ejemplo de errores de hierarchyid](media/known-issues-azure-sql-online/dms-hierarchyid-errors.png)

**Solución alternativa**

Para ello, siga los pasos que se describen a continuación.

1. Busque las tablas de usuario que incluyan columnas con el tipo de datos hierarchyid mediante la siguiente consulta.

      ``` 
      select object_name(object_id) 'Table name' from sys.columns where system_type_id =240 and object_id in (select object_id from sys.objects where type='U')
      ```

2. Excluya estas tablas de la hoja **Configurar los valores de la migración**, en la que especifica las tablas para la migración.

3. Vuelva a ejecutar la actividad de migración.

### <a name="migration-failures-with-various-integrity-violations-with-active-triggers-in-the-schema-during-full-data-load-or-incremental-data-sync"></a>Errores de migración con varias infracciones de integridad con desencadenadores activos en el esquema durante la "carga completa de datos" o la "sincronización de datos incrementales"

**Solución alternativa**

Para ello, siga los pasos que se describen a continuación.

1. Busque los desencadenadores que están actualmente activos en la base de datos de origen mediante la siguiente consulta:

     ```
     select * from sys.triggers where is_disabled =0
     ```

2. Deshabilite los desencadenadores en la base de datos de origen mediante los pasos proporcionados en el artículo [DISABLE TRIGGER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017).

3. Vuelva a ejecutar la actividad de migración.

### <a name="support-for-lob-data-types"></a>Compatibilidad con tipos de datos de LOB

**Síntoma**

Si la longitud de columna de objetos grandes (LOB) es mayor que 32 KB, es posible que se trunquen los datos en el destino. Puede comprobar la longitud de columna de LOB mediante la siguiente consulta:

``` 
SELECT max(DATALENGTH(ColumnName)) as LEN from TableName
```

**Solución alternativa**

Si tiene una columna de LOB que es mayor que 32 KB, póngase en contacto con el equipo de ingeniería escribiendo un correo electrónico a [Ask Azure Database Migrations](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="issues-with-timestamp-columns"></a>Problemas con las columnas de marca de tiempo

**Síntoma**

Azure Database Migration Service no migra el valor de marca de tiempo de origen; en su lugar, Azure Database Migration Service genera un nuevo valor de marca de tiempo en la tabla de destino.

**Solución alternativa**

Si necesita Azure Database Migration Service para migrar el valor de marca de tiempo exacto almacenado en la tabla de origen, póngase en contacto con el equipo de ingeniería al correo electrónico [Ask Azure Database Migrations](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="data-migration-errors-dont-provide-additional-details-on-the-database-detailed-status-blade"></a>Los errores de migración de datos no proporcionan detalles adicionales en la hoja de estado detallado de la base de datos

**Síntoma**

Cuando vea los errores de migración en la vista de estado de detalles de las bases de datos, si selecciona el vínculo **Errores de migración de datos** en la cinta de opciones superior, es posible que no se proporcionen detalles adicionales específicos de los errores de migración.

![ejemplo no detallado de errores de migración de datos](media/known-issues-azure-sql-online/dms-data-migration-errors-no-details.png)

**Solución alternativa**

Para obtener detalles del error específico, siga los pasos siguientes.

1. Cierre la hoja de estado detallado de la base de datos para mostrar la pantalla de la actividad de migración.

     ![pantalla de la actividad de migración](media/known-issues-azure-sql-online/dms-migration-activity-screen.png)

2. Seleccione **Ver detalles del error** para ver mensajes de error específicos que le ayudarán a solucionar los errores de migración.

### <a name="geography-datatype-not-supported-in-sqldb-online-migration"></a>El tipo de datos Geography no es compatible con la migración en línea de SQLDB

**Síntoma**

No se puede realizar la migración de síntomas con un mensaje de error que contiene el texto siguiente:

```output
"** encountered a fatal error", "errorEvents":<Table>.<Column> is of type 'GEOGRAPHY', which is not supported by 'Full Load' under 'Full LOB' support mode.
```

**Solución alternativa**

Aunque Azure Database Migration Service admite el tipo de datos Geography para migraciones sin conexión a Azure SQL Database, en el caso de las migraciones en línea, no se admite dicho tipo de datos. Pruebe métodos alternativos para cambiar el tipo de datos en el origen a un tipo compatible antes de intentar usar Azure Database Migration Service para una migración en línea de esta base de datos.

### <a name="supported-editions"></a>Ediciones compatibles

**Síntoma**

No se puede realizar la migración de síntomas con un mensaje de error que contiene el texto siguiente:

```output
Migration settings validation error: The edition of the server [Business Intelligence Edition (64-bit)] does not match the supported edition(s) [Enterprise,Standard,Developer].
```

**Solución alternativa**

La compatibilidad de las migraciones en línea a Azure SQL Database con Azure Database Migration Service solo se extiende a las ediciones Enterprise, Standard y Developer. Asegúrese de usar una edición admitida antes de iniciar el proceso de migración.
