---
title: Uso de opciones de Group by
description: Sugerencias para implementar las opciones de Group by en el grupo de SQL de Synapse.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 260bd7cc6402a7cb4ed17d004d0f61b7462c51d3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85213455"
---
# <a name="group-by-options-in-synapse-sql-pool"></a>Opciones de Group by en el grupo de SQL de Synapse

En este artículo, encontrará sugerencias para implementar las opciones de Group by en el grupo de SQL.

## <a name="what-does-group-by-do"></a>¿Qué hace GROUP BY?

La cláusula [GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) de T-SQL se usa para agregar datos a un conjunto de filas de resumen. GROUP BY tiene algunas opciones que el grupo de SQL no admite. Estas opciones tienen soluciones alternativas, que son las siguientes:

* GROUP BY con ROLLUP
* GROUPING SETS
* GROUP BY con CUBE

## <a name="rollup-and-grouping-sets-options"></a>Opciones Rollup y Grouping sets

Aquí, la opción más sencilla consiste en usar UNION ALL para realizar la acumulación en lugar de depender de la sintaxis explícita. El resultado es exactamente el mismo.

En el siguiente ejemplo se utiliza la instrucción GROUP BY con la opción de ROLLUP:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Al utilizar ROLLUP, en el ejemplo anterior se solicitan las agregaciones siguientes:

* País y región
* Country
* Total general

Para reemplazar ROLLUP y obtener los mismos resultados, puede utilizar UNION ALL y especificar explícitamente las agregaciones necesarias:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Para reemplazar GROUPING SETS, se aplica el principio de ejemplo. Basta con crear secciones UNION ALL para los niveles de agregación que desea ver.

## <a name="cube-options"></a>Opciones de Cube

Es posible crear una cláusula GROUP BY WITH CUBE con el método UNION ALL. El problema es que el código puede volverse rápidamente complicado y difícil de manejar. Para mitigar este problema, puede usar este enfoque más avanzado.

Con el ejemplo anterior, el primer paso es definir el "cubo" que define todos los niveles de agregación que se van a crear.

Tome nota de la CROSS JOIN de las dos tablas derivadas, ya que esto es lo que genera todos los niveles. El resto del código está realmente ahí para dar formato:

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

A continuación se muestran los resultados de CTAS:

![Agrupar por cubo](./media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png)

El segundo paso es especificar una tabla de destino para almacenar los resultados temporales:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

El tercer paso es recorrer el cubo de columnas al realizar la agregación. La consulta se ejecutará una vez por cada fila de la tabla temporal #Cube. Los resultados se almacenan en la tabla temporal #Results:

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Por último, se pueden devolver los resultados simplemente leyendo en la tabla temporal #Results:

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Al dividir el código en secciones y generar una construcción de bucle, el código resulta más fácil de administrar y mantener.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).
