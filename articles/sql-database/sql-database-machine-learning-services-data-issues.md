---
title: Trabajo con objetos y tipos de datos de R y SQL
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Obtenga información sobre cómo trabajar con tipos de datos y objetos de datos en R con Azure SQL Database mediante Machine Learning Services (versión preliminar), incluidos los problemas comunes que puedan surgir.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: e81cca3e20d5b6c050489e80b91d013d5e934cce
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81453206"
---
# <a name="work-with-r-and-sql-data-in-azure-sql-database-machine-learning-services-preview"></a>Trabajo con datos de SQL y R en Azure SQL Database Machine Learning Services (versión preliminar)

En este artículo se describen algunos de los problemas comunes que se pueden producir al mover datos entre R y SQL Database en [Machine Learning Services (con R) en Azure SQL Database](sql-database-machine-learning-services-overview.md). La experiencia que se obtiene en este ejercicio proporciona información básica sobre el uso de datos en su propio script.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

Entre los problemas comunes que pueden surgir destacan los siguientes:

- A veces, los tipos de datos no coinciden
- Pueden producirse conversiones implícitas
- En ocasiones se requieren operaciones de conversión
- En R y SQL se usan objetos de datos distintos

## <a name="prerequisites"></a>Prerrequisitos

- Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/) antes de empezar.

- Para ejecutar el código de ejemplo en estos ejercicios, primero debe tener habilitado [Azure SQL Database con Machine Learning Services (con R)](sql-database-machine-learning-services-overview.md).

- Asegúrese de que tiene instalada la versión más reciente de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Puede ejecutar los scripts de R con herramientas de consulta o de otro tipo de administración de base de datos, pero en este tutorial usará SSMS.

## <a name="working-with-a-data-frame"></a>Uso de una trama de datos

Cuando el script devuelve resultados de R para SQL, debe devolver los datos como un objeto **data.frame**. Cualquier otro tipo de objeto que se genere en el script (sea una lista, un factor, un vector o datos binarios) debe convertirse en una trama de datos si se quiere incluir en los resultados del procedimiento almacenado. Afortunadamente, hay varias funciones de R que permiten convertir otros objetos en una trama de datos. Incluso se puede serializar un modelo binario y devolverlo en una trama de datos, como hará más adelante en este artículo.

En primer lugar, se experimentará con algunos objetos básicos de R (vectores, matrices y listas), y se verá cómo la conversión a una trama de datos cambia el resultado que se pasa a SQL.

Compararemos estos dos scripts "Hola mundo" en R. Los scripts son casi idénticos, pero el primero devuelve una única columna de tres valores, mientras que el segundo devuelve tres columnas con un valor único cada una.

**Ejemplo 1**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
'
    , @input_data_1 = N'';
```

**Ejemplo 2**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
    , @input_data_1 = N'';
```

¿Por qué son tan diferentes los resultados?

Normalmente la respuesta se puede encontrar mediante el comando `str()` de R. Agregue la función `str(object_name)` en cualquier lugar del script de R para que el esquema de datos del objeto de R especificado se devuelva como un mensaje informativo. Puede ver los mensajes en la pestaña **Mensajes** de SSMS.

Para averiguar por qué en los ejemplos 1 y 2 los resultados son tan diferentes, inserte la línea `str(OutputDataSet)` al final de la definición de variable `@script`de cada instrucción, de esta forma:

**Ejemplo 1 con la función str agregada**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

**Ejemplo 2 con la función str agregada**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(c("hello"), " ", c("world"));
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

Ahora, revise el texto de **Mensajes** para ver por qué el resultado es diferente.

**Resultados: Ejemplo 1**

```text
STDOUT message(s) from external script:
'data.frame':    3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Resultados: Ejemplo 2**

```text
STDOUT message(s) from external script:
'data.frame':    1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Como puede ver, un pequeño cambio en la sintaxis de R ha tenido un gran efecto en el esquema de los resultados. Para obtener todos los detalles, las diferencias en los tipos de datos de R se explican con detalle en la sección *Data Structures* (Estructuras de datos) de ["Advanced R" (R avanzado) por Hadley Wickham](http://adv-r.had.co.nz).

Por ahora, simplemente tenga presente que deberá comprobar los resultados esperados al convertir objetos de R en tramas de datos.

> [!TIP]
> También puede usar las funciones de identidad de R, como `is.matrix` y `is.vector`, para devolver información sobre la estructura de datos interna.

## <a name="implicit-conversion-of-data-objects"></a>Conversión implícita de objetos de datos

Cada objeto de datos de R tiene sus propias reglas sobre cómo controlar los valores cuando se combinan con otros objetos de datos si los dos objetos de datos tienen el mismo número de dimensiones, o bien si un objeto de datos contiene tipos de datos heterogéneos.

Por ejemplo, imagine que quiere multiplicar matrices mediante R. Quiere multiplicar una matriz de una sola columna con los tres valores por una matriz con cuatro valores y esperar como resultado una matriz de 4 x 3.

En primer lugar, cree una tabla pequeña de datos de prueba.

```sql
CREATE TABLE RTestData (col1 INT NOT NULL)

INSERT INTO RTestData
VALUES (1);

INSERT INTO RTestData
VALUES (10);

INSERT INTO RTestData
VALUES (100);
GO
```

Ahora, ejecute el siguiente script.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:15);
OutputDataSet <- as.data.frame(x %*% y);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col1] INT
            , [Col2] INT
            , [Col3] INT
            , Col4 INT
            ));
```

En segundo plano, la columna de tres valores se convierte en una matriz de una sola columna. Como una matriz es simplemente un caso especial de una matriz de R, la matriz `y` se convierte de forma implícita en una matriz de una sola columna para que los dos argumentos coincidan.

**Resultados**

|Col1|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1\.500|

Con todo, observe lo que ocurre cuando se cambia el tamaño de la matriz `y`.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:14);
OutputDataSet <- as.data.frame(y %*% x);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS(([Col1] INT));
```

Ahora R devuelve un valor único como resultado.

**Resultados**
    
|Col1|
|---|
|1542|

¿Por qué? En este caso, dado que los dos argumentos se pueden tratar como vectores de la misma longitud, R devuelve el producto interior como una matriz.  Este es el comportamiento esperado según las reglas de álgebra lineal. Pero podría producir problemas si la aplicación que sigue espera que el esquema de salida no cambie nunca.

## <a name="merge-or-multiply-columns-of-different-length"></a>Combinación o multiplicación de columnas de longitud diferente

R proporciona una gran flexibilidad a la hora de trabajar con vectores de diferentes tamaños y de combinar estas estructuras (similares a las columnas) en tramas de datos. Las listas de vectores pueden ser similares a una tabla, pero no siguen todas las reglas que rigen las tablas de base de datos.

Por ejemplo, en el script siguiente se define una matriz numérica de longitud 6 y se almacena en la variable de R `df1`. Después, la matriz numérica se combina con los enteros de la tabla RTestData (que se ha creado antes) que contiene tres valores, para crear una trama de datos, `df2`.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
df1 <- as.data.frame( array(1:6) );
df2 <- as.data.frame( c( InputDataSet , df1 ));
OutputDataSet <- df2;
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col2] INT NOT NULL
            , [Col3] INT NOT NULL
            ));
```

Para rellenar la trama de datos, R repite los elementos recuperados de RTestData tantas veces como sea necesario para que coincidan con el número de elementos de la matriz `df1`.

**Resultados**
    
|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

Recuerde que una trama de datos solo se parece a una tabla, pero que en realidad es una lista de vectores.

## <a name="cast-or-convert-sql-data"></a>Conversión de datos de SQL

En R y SQL no se usan los mismos tipos de datos, por lo que al ejecutar una consulta en SQL para obtener datos y después pasarlos al runtime de R, normalmente se realiza algún tipo de conversión implícita. Otro conjunto de conversiones tiene lugar cuando se devuelven datos de R a SQL.

- SQL inserta los datos de la consulta en el proceso de R y los convierte en una representación interna para maximizar la eficacia.
- El runtime de R carga los datos en una variable data.frame y realiza sus propias operaciones en los datos.
- El motor de base de datos devuelve los datos a SQL mediante una conexión interna segura y los presenta como tipos de datos de SQL.
- Los datos se obtienen mediante la conexión a SQL a través de una biblioteca de cliente o de red que puede emitir consultas SQL y procesar conjuntos de datos tabulares. Esta aplicación cliente puede afectar a los datos de otras maneras.

Para ver cómo funciona esto, ejecute una consulta como esta en el almacén de datos [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). Esta vista devuelve datos de ventas que se usan en la creación de previsiones.

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
    , CAST(ModelRegion AS VARCHAR(50)) AS ProductSeries
    , Amount
FROM [AdventureWorksDW].[dbo].[vTimeSeries]
WHERE [ModelRegion] = 'M200 Europe'
ORDER BY ReportingDate ASC
```

> [!NOTE]
> Puede usar cualquier versión de AdventureWorks o crear una consulta diferente con una base de datos propia. El objetivo es intentar controlar algunos datos que contienen valores de texto, fecha y hora, y numéricos.

Ahora, intente usar esta consulta como entrada para el procedimiento almacenado.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
str(InputDataSet);
OutputDataSet <- InputDataSet;
'
    , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Si se produce un error, probablemente tendrá que realizar algunas modificaciones en el texto de consulta. Por ejemplo, el predicado de cadena en la cláusula WHERE se debe incluir entre dos conjuntos de comillas simples.

Después de conseguir que la consulta funcione, revise los resultados de la función `str` para ver cómo R procesa los datos de entrada.

**Resultados**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- La columna de fecha y hora se ha procesado con el tipo de datos de R, **POSIXct**.
- La columna de texto "ProductSeries" se ha identificado como un **factor** esto es, como una variable categórica. Los valores de cadena se procesan como factores de forma predeterminada. Si pasa una cadena a R, se convierte en un entero para uso interno y después se vuelve a asignar a la cadena de salida.

## <a name="summary"></a>Resumen

Incluso en estos pequeños ejemplos, puede ver la necesidad de comprobar los efectos de la conversión de datos al pasar consultas SQL como entrada. Como en R no se admiten algunos tipos de datos SQL, considere estas formas de evitar errores:

- Compruebe los datos de antemano y discierna qué columnas o valores del esquema pueden ser problemáticos cuando se pasen al código de R.
- En el origen de datos de entrada, especifique las columnas de forma individual, en lugar de usar `SELECT *`, y sepa cómo se va a procesar cada columna.
- Para evitar sorpresas, realice las conversiones explícitas necesarias al preparar los datos de entrada.
- Evite pasar columnas de datos (como GUID o ROWGUID) que causan errores y no son útiles para el modelado.

Para más información sobre los tipos de datos de R admitidos y no admitidos, vea [Bibliotecas y tipos de datos de R](/sql/advanced-analytics/r/r-libraries-and-data-types).
