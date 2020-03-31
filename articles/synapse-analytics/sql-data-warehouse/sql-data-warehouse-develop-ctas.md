---
title: CREATE TABLE AS SELECT (CTAS)
description: Explicación y ejemplos de la instrucción CREATE TABLE AS SELECT (CTAS) en SQL Analytics para el desarrollo de soluciones.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/26/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seoapril2019, azure-synapse
ms.openlocfilehash: bb9ff52bd7d2e4cfd1a1df4d780a4c369380284f
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350598"
---
# <a name="create-table-as-select-ctas-in-sql-analytics"></a>CREATE TABLE AS SELECT (CTAS) en SQL Analytics

En este artículo se explica la instrucción CREATE TABLE AS SELECT (CTAS) de T-SQL en SQL Analytics para el desarrollo de soluciones. En el artículo también se proporcionan ejemplos de código.

## <a name="create-table-as-select"></a>CREATE TABLE AS SELECT

La instrucción [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) (CTAS) es una de las características más importantes de T-SQL disponibles. CTAS es una operación paralela que crea una nueva tabla basada en la salida de una instrucción SELECT. CTAS es la forma más sencilla y rápida de crear e insertar datos en una tabla con un solo comando.

## <a name="selectinto-vs-ctas"></a>SELECT...INTO frente a CTAS

CTAS es una versión personalizable de la instrucción [SELECT...INTO](/sql/t-sql/queries/select-into-clause-transact-sql).

El siguiente es un ejemplo de una instrucción simple SELECT...INTO:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

Sin embargo, SELECT...INTO no permite cambiar el método de distribución o el tipo de índice como parte de la operación. Debe crear `[dbo].[FactInternetSales_new]` usando el tipo de distribución predeterminado de ROUND_ROBIN y la estructura de tabla predeterminada de CLUSTERED COLUMNSTORE INDEX.

Por otro lado, con CTAS puede especificar tanto la distribución de los datos de la tabla como el tipo de estructura de la misma. Para convertir el ejemplo anterior a CTAS:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
 DISTRIBUTION = ROUND_ROBIN
 ,CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales];
```

> [!NOTE]
> Si solo quiere cambiar el índice en su operación de CTAS y la tabla de origen tiene una distribución de hash, mantenga la misma columna de distribución y el mismo tipo de datos. Esto evitará la distribución entre el movimiento de datos durante la operación, lo que será más eficaz.

## <a name="use-ctas-to-copy-a-table"></a>Uso de CTAS para copiar una tabla

Quizás uno de los usos más comunes de CTAS es crear la copia de una tabla para cambiar el DDL. Digamos que originalmente creó su tabla como `ROUND_ROBIN`, y que ahora quiere cambiarla a una tabla distribuida en una columna. CTAS es el modo de cambiar esa columna de distribución. También se puede usar CTAS para cambiar las particiones, el indexado o los tipos de columnas.

Supongamos que creó esta tabla con el tipo de distribución predeterminado de `ROUND_ROBIN`, sin especificar una columna de distribución en el `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25));
```

Ahora quiere crear una copia nueva de esta tabla con un `Clustered Columnstore Index`, a fin de poder aprovechar el rendimiento de las tablas del almacén de columnas en clúster. También quiere distribuir esta tabla en `ProductKey`, ya que prevé combinaciones en esta columna y quiere evitar el movimiento de datos durante las mismas en `ProductKey`. Por último, también quiere agregar la creación de particiones en `OrderDateKey`, con el fin de eliminar rápidamente datos antiguos mediante la anulación de particiones anteriores. Esta es la instrucción CTAS que copiará la tabla antigua en una nueva.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Finalmente, puede cambiar el nombre de las tablas y ponerle a la tabla nueva el nombre de la antigua, para luego anular esta última.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

## <a name="use-ctas-to-work-around-unsupported-features"></a>Uso de CTAS para solucionar características no admitidas

CTAS también se puede usar para solucionar algunas de las características no admitidas que se indican a continuación. Este método a menudo puede resultar útil, ya que no solo su código será compatible, sino que con frecuencia se ejecutará más rápido en SQL Analytics. Este rendimiento es el resultado de un diseño totalmente paralelizado. Los escenarios incluyen:

* ANSI JOINS en UPDATE
* ANSI JOINS en DELETE
* Instrucción MERGE

> [!TIP]
> Intente pensar: "primero CTAS". Solucionar un problema mediante el uso de CTAS es generalmente un buen enfoque, incluso si está escribiendo más datos como resultado.

## <a name="ansi-join-replacement-for-update-statements"></a>Reemplazo de la unión de ANSI para las instrucciones de actualización

Es posible que tenga una actualización compleja. Esa actualización combina más de dos tablas mediante la sintaxis de combinación ANSI para realizar una operación UPDATE o DELETE.

Imagine que tiene que actualizar esta tabla:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
( [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
, [CalendarYear]                    SMALLINT        NOT NULL
, [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
);
```

La consulta original puede parecerse a este ejemplo:

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT [EnglishProductCategoryName]
        , [CalendarYear]
        , SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear];
```

SQL Analytics no admite combinaciones ANSI en la cláusula `FROM` de una instrucción `UPDATE`, por lo que no puede usar el ejemplo anterior sin modificarlo.

Puede usar una combinación de CTAS y una combinación implícita para reemplazar el ejemplo anterior:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0) AS [EnglishProductCategoryName]
, ISNULL(CAST([CalendarYear] AS SMALLINT),0)  AS [CalendarYear]
, ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)  AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY [EnglishProductCategoryName]
, [CalendarYear];

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]  = AnnualCategorySales.[CalendarYear] ;

--Drop the interim table
DROP TABLE CTAS_acs;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>Sustitución de una combinación ANSI en instrucciones delete

A veces, el mejor método para eliminar datos es usar CTAS, especialmente para las instrucciones `DELETE` que usan la sintaxis de combinación ANSI. Esto se debe a que SQL Analytics no admite combinaciones ANSI en la cláusula `FROM` de una instrucción `DELETE`. En lugar de eliminar los datos, simplemente seleccione los datos que quiera conservar.

El siguiente es un ejemplo de una instrucción convertida `DELETE`:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you want to keep
SELECT p.ProductKey
, p.EnglishProductName
,  p.Color
FROM  dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON p.ProductKey = s.ProductKey;

RENAME OBJECT dbo.DimProduct TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Sustitución de instrucciones MERGE

Puede reemplazar las instrucciones de combinación, al menos en parte, mediante CTAS. También puede combinar `INSERT` y `UPDATE` en una única instrucción. Cualquier registro eliminado debe estar restringido de la instrucción `SELECT` para poder omitir los resultados.

El ejemplo siguiente es para un `UPSERT`:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT s.[ProductKey]
, s.[EnglishProductName]
, s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
, p.[EnglishProductName]
, p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
);

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];
```

## <a name="explicitly-state-data-type-and-nullability-of-output"></a>establezca explícitamente el tipo de datos y la nulabilidad de salida

Al migrar código, podría encontrarse ejecutando este tipo modelo de codificación:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f;
```

Es posible que piense que debe migrar este código a CTAS, y estaría en lo correcto. Sin embargo, existe un problema oculto aquí.

El siguiente código no produce el mismo resultado:

```sql
DECLARE @d decimal(7,2) = 85.455
, @f float(24)    = 85.455;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result;
```

Observe que la columna "result" traslada el tipo de datos y los valores de nulabilidad de la expresión. Desviar el tipo de datos puede llevar a variaciones sutiles en los valores si no se tiene cuidado.

Pruebe este ejemplo:

```sql
SELECT result,result*@d
from result;

SELECT result,result*@d
from ctas_r;
```

El valor almacenado para el resultado es diferente. Como el valor persistente en la columna de resultados se usa en otras expresiones, el error se vuelve más importante incluso.

![Captura de pantalla de los resultados de CTAS](./media/sql-data-warehouse-develop-ctas/ctas-results.png)

Esto es importante para las migraciones de datos. Aunque se puede decir que la segunda consulta es más precisa, hay un problema. Los datos serán diferentes en comparación con el sistema de origen y eso conduce a preguntas sobre la integridad de la migración. Este es uno de los pocos casos en los que la respuesta "incorrecta" es en realidad la correcta.

El motivo de que veamos esta disparidad entre los dos resultados se debe a la conversión implícita de los tipos. En el primer ejemplo, la tabla define la definición de columna. Cuando se inserta la fila, se produce una conversión implícita de los tipos. En el segundo ejemplo, no hay ninguna conversión de tipos implícita ya que la expresión define el tipo de datos de la columna.

Observe también que la columna del segundo ejemplo se ha definido como una columna que acepta valores NULL mientras que en el primer ejemplo no. Cuando se creó la tabla en el primer ejemplo, la nulabilidad se definió explícitamente. En el segundo ejemplo, simplemente se dejó con la expresión y, de forma predeterminada, esto dará lugar a una definición de NULL.

Para resolver estos problemas, debe establecer explícitamente la conversión de tipos y la nulabilidad en la parte SELECT de la instrucción CTAS. No se pueden establecer estas propiedades en "CREATE TABLE".
En el siguiente ejemplo se muestra cómo corregir el código:

```sql
DECLARE @d decimal(7,2) = 85.455
, @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Tenga en cuenta lo siguiente:

* Puede usar CAST o CONVERT.
* Use ISNULL, y no COALESCE, para forzar el elemento NULLability. Consulte la nota siguiente.
* ISNULL es la función más externa.
* La segunda parte de ISNULL es una constante, es decir, 0.

> [!NOTE]
> Para que la nulabilidad se establezca correctamente, es fundamental usar ISNULL y no COLAESCE. COALESCE no es una función determinista y, por tanto, el resultado de la expresión siempre será NULLable. ISNULL es diferente. Es determinista. Por lo tanto, cuando la segunda parte de la función ISNULL es una constante o un literal, el valor resultante será NOT NULL.

Asegurar la integridad de sus cálculos también es importante para la conmutación de partición de tabla. Imagine que tiene esta tabla definida como una tabla de hechos:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
, [product]   INT     NOT NULL
, [store]     INT     NOT NULL
, [quantity]  INT     NOT NULL
, [price]     MONEY   NOT NULL
, [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
);
```

Sin embargo, el campo de cantidad es una expresión calculada. No es parte de los datos de origen.

Para crear su conjunto de datos particionado, es posible que quiera usar el siguiente código:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH
( DISTRIBUTION = HASH([product])
, PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]
,   [product]
,   [store]
,   [quantity]
,   [price]
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

La consulta se ejecutará perfectamente. El problema se produce al intentar realizar el cambio de partición. Las definiciones de la tabla no coinciden. Para hacer coincidir las definiciones de la tabla, modifique el CTAS para agregar una función `ISNULL` y así preservar el atributo de nulabilidad de la columna.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH
( DISTRIBUTION = HASH([product])
, PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
  [date]
, [product]
, [store]
, [quantity]
, [price]   
, ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Por lo tanto, puede ver que la coherencia de los tipos y el mantenimiento de las propiedades de nulabilidad en CTAS es una buena práctica de ingeniería. Esto le permitirá mantener la integridad de los cálculos y también garantiza que la modificación de particiones sea posible.

CTAS es una de las instrucciones más importantes de SQL Analytics. Asegúrese de que la comprende perfectamente. Consulte la [documentación de CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).

