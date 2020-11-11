---
title: Uso de procedimientos almacenados
description: Sugerencias para implementar procedimientos almacenados mediante Synapse SQL en Azure Synapse Analytics para el desarrollo de soluciones.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 11/03/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 3940d762dbc249e0303ddf905acbeeed7f96aa4f
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93315551"
---
# <a name="stored-procedures-using-synapse-sql-in-azure-synapse-analytics"></a>Procedimientos almacenados mediante Synapse SQL en Azure Synapse Analytics

Los grupos de Synapse SQL aprovisionados y sin servidor permiten incluir lógica compleja de procesamiento de datos en los procedimientos almacenados de SQL. Los procedimientos almacenados son una excelente manera de encapsular el código SQL y almacenarlo cerca de los datos, en el almacenamiento de datos. Los procedimientos almacenados ayudan a los desarrolladores a encapsular el código en unidades administrables para modularizar sus soluciones. Y esto propicia una mayor reutilización del código. Cada procedimiento almacenado también puede aceptar parámetros para que sean todavía más flexibles.
En este artículo, encontrará algunas sugerencias para implementar procedimientos almacenados en un grupo de Synapse SQL para soluciones de desarrollo.

## <a name="what-to-expect"></a>Qué esperar

Synapse SQL admite muchas de las características de T-SQL que se usan en SQL Server. Más importante aún, hay características específicas de escalabilidad horizontal que puede utilizar para maximizar el rendimiento de la solución. En este artículo, obtendrá información sobre las características que puede implementar en los procedimientos almacenados.

> [!NOTE]
> En el cuerpo del procedimiento puede usar solo las características que se admiten en el área expuesta de Synapse SQL. Consulte [este artículo](overview-features.md) para identificar objetos y la instrucción que se puede usar en procedimientos almacenados. Los ejemplos de estos artículos usan características genéricas que están disponibles en el área de superficie dedicada y sin servidor. Consulte las [limitaciones adicionales en los grupos de Synapse SQL aprovisionados y sin servidor](#limitations) al final de este artículo.

Para mantener la escalabilidad y el rendimiento del grupo de SQL, también hay algunas características y funcionalidad que tienen diferencias de comportamiento y otras que no son compatibles.

## <a name="stored-procedures-in-synapse-sql"></a>Procedimientos almacenados en Synapse SQL

En el ejemplo siguiente, puede ver los procedimientos que quitan objetos externos si existen en la base de datos:

```sql
CREATE PROCEDURE drop_external_table_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_tables WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL TABLE ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
GO
CREATE PROCEDURE drop_external_file_format_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_file_formats WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL FILE FORMAT ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
GO
CREATE PROCEDURE drop_external_data_source_if_exists @name SYSNAME
AS BEGIN
    IF (0 <> (SELECT COUNT(*) FROM sys.external_data_sources WHERE name = @name))
    BEGIN
        DECLARE @drop_stmt NVARCHAR(200) = N'DROP EXTERNAL DATA SOURCE ' + @name; 
        EXEC sp_executesql @tsql = @drop_stmt;
    END
END
```

Estos procedimientos se pueden ejecutar mediante la instrucción `EXEC`, en la que puede especificar el nombre y los parámetros del procedimiento:

```sql
EXEC drop_external_table_if_exists 'mytest';
EXEC drop_external_file_format_if_exists 'mytest';
EXEC drop_external_data_source_if_exists 'mytest';
```

Synapse SQL proporciona una implementación optimizada y simplificada de procedimientos almacenados. La diferencia más importante en comparación con SQL Server es que el procedimiento almacenado no es código compilado previamente. En el almacenamiento de datos, el tiempo de compilación es pequeño en comparación con el tiempo necesario para ejecutar consultas en grandes volúmenes de datos. Es más importante asegurarse de que el código del procedimiento almacenado esté optimizado correctamente para consultas grandes. El objetivo es ahorrar horas, minutos y segundos, no milisegundos. Por lo tanto, resulta más útil pensar en los procedimientos almacenados como contenedores para la lógica SQL.

Cuando Synapse SQL ejecuta el procedimiento almacenado, las instrucciones SQL se analizan, traducen y optimizan en tiempo de ejecución. Durante este proceso, cada instrucción se convierte en consultas distribuidas. El código SQL que se ejecuta en los datos es diferente de la consulta enviada.

## <a name="encapsulate-validation-rules"></a>Encapsulación de reglas de validación

Los procedimientos almacenados permiten localizar la lógica de validación en un único módulo almacenado en SQL Database. En el ejemplo siguiente, puede ver cómo validar los valores de parámetros y cambiar su configuración predeterminada.

```sql
CREATE PROCEDURE count_objects_by_date_created 
                            @start_date DATETIME2,
                            @end_date DATETIME2
AS BEGIN 

    IF( @start_date >= GETUTCDATE() )
    BEGIN
        THROW 51000, 'Invalid argument @start_date. Value should be in past.', 1;  
    END

    IF( @end_date IS NULL )
    BEGIN
        SET @end_date = GETUTCDATE();
    END

    IF( @start_date >= @end_date )
    BEGIN
        THROW 51000, 'Invalid argument @end_date. Value should be greater than @start_date.', 2;  
    END

    SELECT
         year = YEAR(create_date),
         month = MONTH(create_date),
         objects_created = COUNT(*)
    FROM
        sys.objects
    WHERE
        create_date BETWEEN @start_date AND @end_date
    GROUP BY
        YEAR(create_date), MONTH(create_date);
END
```

La lógica del procedimiento de SQL validará los parámetros de entrada cuando se llame al procedimiento.

```sql

EXEC count_objects_by_date_created '2020-08-01', '2020-09-01'

EXEC count_objects_by_date_created '2020-08-01', NULL

EXEC count_objects_by_date_created '2020-09-01', '2020-08-01'
-- Error
-- Invalid argument @end_date. Value should be greater than @start_date.

EXEC count_objects_by_date_created '2120-09-01', NULL
-- Error
-- Invalid argument @start_date. Value should be in past.
```

## <a name="nesting-stored-procedures"></a>Anidamiento de los procedimientos almacenados

Cuando los procedimientos almacenados llaman a otros procedimientos almacenados o ejecutan SQL dinámico, se dice que la invocación interna de código o de procedimientos almacenados se anida.
En el código siguiente se muestra un ejemplo de procedimiento anidado:

```sql
CREATE PROCEDURE clean_up @name SYSNAME
AS BEGIN
    EXEC drop_external_table_if_exists @name;
    EXEC drop_external_file_format_if_exists @name;
    EXEC drop_external_data_source_if_exists @name;
END
```

Este procedimiento acepta un parámetro que representa un nombre y, a continuación, llama a otros procedimientos para quitar los objetos con este nombre.
El grupo de SQL de Synapse admite un máximo de ocho niveles de anidamiento. Esta funcionalidad es ligeramente diferente a la de SQL Server. El nivel de anidamiento en SQL Server es 32.

La llamada al procedimiento almacenado de nivel superior es igual al nivel de anidamiento 1.

```sql
EXEC clean_up 'mytest'
```

Si el procedimiento almacenado también realiza otra llamada EXEC, el nivel de anidamiento aumentará a 2.

```sql
CREATE PROCEDURE clean_up @name SYSNAME
AS
    EXEC drop_external_table_if_exists @name  -- This call is nest level 2
GO
EXEC clean_up 'mytest'  -- This call is nest level 1
```

Si el segundo procedimiento ejecuta luego SQL dinámico, el nivel de anidamiento aumentará a 3.

```sql
CREATE PROCEDURE drop_external_table_if_exists @name SYSNAME
AS BEGIN
    /* See full code in the previous example */
    EXEC sp_executesql @tsql = @drop_stmt;  -- This call is nest level 3
END
GO
CREATE PROCEDURE clean_up @name SYSNAME
AS
    EXEC drop_external_table_if_exists @name  -- This call is nest level 2
GO
EXEC clean_up 'mytest'  -- This call is nest level 1
```

> [!NOTE]
> Synapse SQL no admite [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) en este momento. Debe realizar un seguimiento del nivel de anidamiento. No es probable que supere el límite de nivel de anidamiento 8, pero si lo hace, deberá rehacer el código para ajustar los niveles de anidamiento dentro de este límite.

## <a name="insertexecute"></a>INSERT..EXECUTE

El grupo de Synapse SQL aprovisionado no permite utilizar el conjunto de resultados de un procedimiento almacenado con una instrucción INSERT. Sin embargo, se puede utilizar un enfoque alternativo. Para obtener un ejemplo, vea el artículo sobre [tablas temporales](develop-tables-temporary.md) de un grupo aprovisionado de Synapse SQL.

## <a name="limitations"></a>Limitaciones

Algunos aspectos de los procedimientos almacenados de Transact-SQL no se han implementado en Synapse SQL, como los siguientes:

| Característica/opción | aprovisionado | Sin servidor |
| --- | --- |
| Procedimientos almacenados temporales | No | Sí |
| Procedimientos almacenados numerados | No | No |
| Procedimientos almacenados extendidos | No | No |
| Procedimientos almacenados CLR | No | No |
| Opción de cifrado | No | Sí |
| Opción Replicación | No | No |
| Parámetros con valores de tabla | No | No |
| Parámetros de solo lectura | No | No |
| Parámetros predeterminados | No | Sí |
| Contextos de ejecución | No | No |
| Return (instrucción) | No | Sí |
| INSERT INTO... EXEC | No | Sí |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](develop-overview.md).
