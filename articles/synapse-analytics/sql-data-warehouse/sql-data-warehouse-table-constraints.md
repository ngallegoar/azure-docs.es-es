---
title: Claves principales, externas y únicas
description: Compatibilidad con las restricciones de tabla del grupo de SQL dedicado en Azure Synapse Analytics
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: dd1d92dd6be47b2bdf6b8ca2f9a99c62e35eb12a
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93313056"
---
# <a name="primary-key-foreign-key-and-unique-key-using-dedicated-sql-pool-in-azure-synapse-analytics"></a>Clave principal, clave externa y clave única mediante un grupo de SQL dedicado en Azure Synapse Analytics

Conozca las restricciones de tabla del grupo de SQL dedicado, incluida la clave principal, la clave externa y la clave única.

## <a name="table-constraints"></a>Restricciones de tabla

El grupo de SQL dedicado admite estas restricciones de tabla: 
- PRIMARY KEY solo se admite cuando se usan NONCLUSTERED y NOT ENFORCED.    
- Solo se admite la restricción UNIQUE cuando se usa NOT ENFORCED.

Para conocer la sintaxis, consulte [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) y [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse). 

No se admite la restricción FOREIGN KEY en el grupo de SQL dedicado.  


## <a name="remarks"></a>Observaciones

Tener una clave principal o una clave única permite que el motor del grupo de SQL dedicado genere un plan de ejecución óptimo para una consulta.  Todos los valores de una columna de clave principal o de una columna de restricción única deben ser únicos.

Después de crear una tabla con una clave principal o una restricción única en el grupo de SQL dedicado, los usuarios deben asegurarse de que todos los valores de esas columnas sean únicos.  En caso contrario, la consulta podría devolver resultados inexactos.  En este ejemplo se muestra cómo una consulta puede devolver resultados inexactos si la columna de restricción única o de clave principal incluye valores duplicados.  

```sql
 -- Create table t1
CREATE TABLE t1 (a1 INT NOT NULL, b1 INT) WITH (DISTRIBUTION = ROUND_ROBIN)

-- Insert values to table t1 with duplicate values in column a1.
INSERT INTO t1 VALUES (1, 100)
INSERT INTO t1 VALUES (1, 1000)
INSERT INTO t1 VALUES (2, 200)
INSERT INTO t1 VALUES (3, 300)
INSERT INTO t1 VALUES (4, 400)

-- Run this query.  No primary key or unique constraint.  4 rows returned. Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
1           2
2           1
3           1
4           1

(4 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 ADD CONSTRAINT unique_t1_a1 unique (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, count(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key constraint
ALTER TABLE t1 add CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Manually fix the duplicate values in a1
UPDATE t1 SET a1 = 0 WHERE b1 = 1000

-- Verify no duplicate values in column a1 
SELECT * FROM t1

/*
a1          b1
----------- -----------
2           200
3           300
4           400
0           1000
1           100

(5 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 add CONSTRAINT unique_t1_a1 UNIQUE (a1) NOT ENFORCED  

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) as total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key contraint
ALTER TABLE t1 ADD CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

```

## <a name="examples"></a>Ejemplos

Cree una tabla de grupo de SQL dedicado con una clave principal: 

```sql 
CREATE TABLE mytable (c1 INT PRIMARY KEY NONCLUSTERED NOT ENFORCED, c2 INT);
```

Cree una tabla de grupo de SQL dedicado con una restricción única:

```sql
CREATE TABLE t6 (c1 INT UNIQUE NOT ENFORCED, c2 INT);
```

## <a name="next-steps"></a>Pasos siguientes

Después de crear las tablas para el grupo de SQL dedicado, el paso siguiente es cargar datos en la tabla. Para ver un tutorial de carga, consulte [Carga de datos en un grupo de SQL dedicado](load-data-wideworldimportersdw.md).
