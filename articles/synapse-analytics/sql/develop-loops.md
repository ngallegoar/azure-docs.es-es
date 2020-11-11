---
title: Uso de bucles de T-SQL
description: Sugerencias para usar bucles de T-SQL, reemplazar cursores y desarrollar soluciones relacionadas con Synapse SQL en Azure Synapse Analytics.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 99ee41de7ffd66191ff712a5ffbda65f3233196f
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93324444"
---
# <a name="use-t-sql-loops-with-synapse-sql-in-azure-synapse-analytics"></a>Uso de bucles de T-SQL con Synapse SQL en Azure Synapse Analytics

En este artículo encontrará sugerencias para usar bucles de T-SQL, reemplazar cursores y desarrollar soluciones relacionadas con Synapse SQL.

## <a name="purpose-of-while-loops"></a>Propósito de los bucles WHILE

SQL de Synapse admite el bucle [WHILE](https://docs.microsoft.com/sql/t-sql/language-elements/while-transact-sql?view=sql-server-ver15&preserve-view=true) para ejecutar bloques de instrucciones de forma repetida. El bucle WHILE continúa siempre y cuando las condiciones especificadas se cumplan o hasta que el código termine específicamente el bucle con la palabra clave BREAK. 

Los bucles en Synapse SQL son útiles para reemplazar los cursores definidos en código SQL. Afortunadamente, casi todos los cursores que están escritos en código SQL son de la variedad avance rápido y solo lectura. Por lo tanto, los bucles WHILE son una buena alternativa para reemplazar los cursores.

## <a name="replace-cursors-in-synapse-sql"></a>Sustitución de cursores en Synapse SQL

Antes de profundizar, se debe tener en cuenta la siguiente pregunta: "¿Se podría reescribir este cursor para usar operaciones basadas en conjuntos?". En muchos casos, la respuesta es afirmativa y este suele ser el mejor enfoque. Una operación basada en conjunto a menudo se ejecuta más rápido que un enfoque iterativo, fila a fila.

Los cursores de avance rápido y solo lectura se reemplazan fácilmente por una construcción de bucle. El siguiente código es un ejemplo sencillo. Este código de ejemplo actualiza las estadísticas de todas las tablas de la base de datos. Mediante la iteración a través de las tablas del bucle, cada comando se ejecuta en secuencia.

En primer lugar, cree una tabla temporal que contenga un número de fila único usado para identificar las instrucciones individuales:

```sql
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

En segundo lugar, inicialice las variables necesarias para ejecutar el bucle:

```sql
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Ahora, recorra las instrucciones ejecutándolas de una en una:

```sql
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Finalmente, elimine la tabla temporal creada en el primer paso

```sql
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](develop-overview.md).
