---
title: Claves principales, externas y únicas
description: Compatibilidad con las restricciones de tabla del grupo de SQL de Synapse en Azure Synapse Analytics
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: f97163d02836442430037e18439bcf0724046332
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80990776"
---
# <a name="primary-key-foreign-key-and-unique-key-in-synapse-sql-pool"></a>Clave principal, clave externa y clave única en el grupo de SQL de Synapse

Conozca las restricciones de tabla del grupo de SQL de Synapse, incluida la clave principal, la clave externa y la clave única.

## <a name="table-constraints"></a>Restricciones de tabla

El grupo de SQL de Synapse admite estas restricciones de tabla: 
- PRIMARY KEY solo se admite cuando se usan NONCLUSTERED y NOT ENFORCED.    
- Solo se admite la restricción UNIQUE cuando se usa NOT ENFORCED.

Para conocer la sintaxis, consulte [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) y [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse). 

No se admite la restricción FOREIGN KEY en el grupo de SQL de Synapse.  


## <a name="remarks"></a>Observaciones

Tener una clave principal o una clave única permite que el motor del grupo de SQL de Synapse genere un plan de ejecución óptimo para una consulta.  Todos los valores de una columna de clave principal o de una columna de restricción única deben ser únicos.

Después de crear una tabla con una clave principal o una restricción única en el grupo de SQL de Synapse, los usuarios deben asegurarse de que todos los valores de esas columnas sean únicos.  En caso contrario, la consulta podría devolver resultados inexactos.  En este ejemplo se muestra cómo una consulta puede devolver resultados inexactos si la columna de restricción única o de clave principal incluye valores duplicados.  

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

Cree una tabla de grupo de SQL de Synapse con una clave principal: 

```sql 
CREATE TABLE mytable (c1 INT PRIMARY KEY NONCLUSTERED NOT ENFORCED, c2 INT);
```
Cree una tabla de grupo de SQL de Synapse con una restricción única:

```sql
CREATE TABLE t6 (c1 INT UNIQUE NOT ENFORCED, c2 INT);
```

## <a name="next-steps"></a>Pasos siguientes

Después de crear las tablas para el grupo de SQL de Synapse, el paso siguiente es cargar datos en la tabla. Para obtener un tutorial de carga, consulte [Carga de datos en el grupo de SQL de Synapse](load-data-wideworldimportersdw.md).
