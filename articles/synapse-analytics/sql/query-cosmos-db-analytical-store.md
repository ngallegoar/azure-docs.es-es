---
title: Consulta de datos de Azure Cosmos DB mediante un grupo de SQL sin servidor en Azure Synapse Link (versión preliminar)
description: En este artículo, aprenderá a consultar Azure Cosmos DB mediante un grupo de SQL sin servidor en Azure Synapse Link (versión preliminar).
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 09/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: 9f57d435134bffbb8e7576adffeacb92bf687124
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93310298"
---
# <a name="query-azure-cosmos-db-data-with-serverless-sql-pool-in-azure-synapse-link-preview"></a>Consulta de datos de Azure Cosmos DB con un grupo de SQL sin servidor en Azure Synapse Link (versión preliminar)

El grupo de SQL sin servidor de Synapse permite analizar los datos de los contenedores de Azure Cosmos DB que están habilitados con [Azure Synapse Link](../../cosmos-db/synapse-link.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) casi en tiempo real, sin afectar al rendimiento de las cargas de trabajo transaccionales. Ofrece una sintaxis T-SQL familiar para consultar los datos del [almacén analítico](../../cosmos-db/analytical-store-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) y la conectividad integrada en una amplia gama de herramientas de consulta ad hoc y de inteligencia empresarial a través de la interfaz de T-SQL.

Para consultar Azure Cosmos DB, se admite el área expuesta de [SELECT](/sql/t-sql/queries/select-transact-sql?view=sql-server-ver15) completa a través de la función [OPENROWSET](develop-openrowset.md), incluida la mayoría de las [funciones y operadores de SQL](overview-features.md). También puede almacenar los resultados de la consulta que lee los datos de Azure Cosmos DB junto con los de Azure Blob Storage o Azure Data Lake Storage mediante [create external table as select](develop-tables-cetas.md#cetas-in-serverless-sql-pool). Actualmente no se pueden almacenar los resultados de las consultas de un grupo de SQL sin servidor en Azure Cosmos DB mediante CETAS. 

En este artículo, aprenderá a escribir una consulta con un grupo de SQL sin servidor que consultará datos de contenedores de Azure Cosmos DB que están habilitados para Synapse Link. Después, puede obtener más información sobre la creación de vistas de un grupo de SQL sin servidor en contenedores de Azure Cosmos DB y cómo conectarlas a modelos de Power BI en [este](./tutorial-data-analyst.md) tutorial. 

> [!IMPORTANT]
> En este tutorial se usa un contenedor con un [esquema bien definido de Azure Cosmos DB](../../cosmos-db/analytical-store-introduction.md#schema-representation). La experiencia de consulta que proporciona el grupo SQL sin servidor para el [esquema de fidelidad completa de Azure Cosmos DB](#full-fidelity-schema) es un comportamiento temporal que se cambiará en función de los comentarios de la versión preliminar. No confíe en el esquema del conjunto de resultados de la función `OPENROWSET` sin una cláusula `WITH` que lea los datos de un contenedor con un esquema de fidelidad completa, ya que la experiencia de consulta podría cambiarse y alinearse con la del esquema bien definido. Publique los comentarios en el [Foro de comentarios de Azure Synapse Analytics](https://feedback.azure.com/forums/307516-azure-synapse-analytics), o bien póngase en contacto con el [equipo del producto de Synapse Link](mailto:cosmosdbsynapselink@microsoft.com) para proporcionarlos.

## <a name="overview"></a>Introducción

Para admitir la consulta y el análisis de datos en el almacén analítico de Azure Cosmos DB, el grupo de SQL sin servidor usa la sintaxis de `OPENROWSET` siguiente:

```sql
OPENROWSET( 
       'CosmosDB',
       '<Azure Cosmos DB connection string>',
       <Container name>
    )  [ < with clause > ] AS alias
```

La cadena de conexión de Azure Cosmos DB especifica el nombre de la cuenta de Azure Cosmos DB, el nombre de la base de datos, la clave maestra de la cuenta de base de datos y un nombre de región opcional para la función `OPENROWSET`. 

> [!IMPORTANT]
> Asegúrese de usar el alias después de `OPENROWSET`. Hay un [problema conocido](#known-issues) problemas de conexión con el punto de conexión SQL sin servidor de Synapse si no se especifica el alias después de la función `OPENROWSET`.

La cadena de conexión de dispositivo tiene el formato siguiente:
```sql
'account=<database account name>;database=<database name>;region=<region name>;key=<database account master key>'
```

El nombre del contenedor de Azure Cosmos DB se especifica sin comillas con la sintaxis `OPENROWSET`. Si el nombre del contenedor tiene algún carácter especial (por ejemplo, un guion "-"), se debe encapsular dentro de `[]` (corchetes) con la sintaxis `OPENROWSET`.

> [!NOTE]
> Un grupo de SQL sin servidor no admite la realización de consultas en el almacén transaccional de Azure Cosmos DB.

## <a name="sample-data-set"></a>Conjunto de datos de ejemplo

Los ejemplos de este artículo se basan en datos de [European Centre for Disease Prevention and Control (ECDC) COVID-19 Cases](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) (Casos de COVID-19 del Centro Europeo para la Prevención y Control de Enfermedades) y [COVID-19 Open Research Dataset (CORD-19), doi:10.5281/zenodo.3715505](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/). 

Puede ver la licencia y la estructura de los datos en estas páginas, y descargar los datos de ejemplo para los conjuntos de datos [ECDC](https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json) y [Cord19](https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json).

Para seguir con este artículo, en el que se muestra cómo consultar datos de Cosmos DB con un grupo de SQL sin servidor, asegúrese de que crea los recursos siguientes:
* Una cuenta de base de datos Azure Cosmos DB [habilitada para Synapse Link](../../cosmos-db/configure-synapse-link.md)
* Una base de datos de Azure Cosmos DB con el nombre `covid`
* Dos contenedores de Azure Cosmos DB denominados `EcdcCases` y `Cord19` con los conjuntos de datos de ejemplo anteriores cargados.

## <a name="explore-azure-cosmos-db-data-with-automatic-schema-inference"></a>Exploración de datos de Azure Cosmos DB con inferencia automática del esquema

La forma más fácil de explorar datos en Azure Cosmos DB consiste en aprovechar la función de inferencia automática del esquema. Al omitir la cláusula `WITH` de la instrucción `OPENROWSET`, puede indicar al grupo de SQL sin servidor que detecte automáticamente (infiera) el esquema del almacén analítico del contenedor de Azure Cosmos DB.

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases) as documents
```
En el ejemplo anterior, se indica a un grupo de SQL sin servidor que se conecte a la base de datos `covid` en la cuenta de Azure Cosmos DB `MyCosmosDbAccount` autenticada con la clave de Azure Cosmos DB (en el ejemplo anterior es ficticia). Después, se accede al almacén analítico del contenedor `EcdcCases` en la región `West US 2`. Como no se proyectan propiedades específicas, la función `OPENROWSET` devolverá todas las propiedades de los elementos de Azure Cosmos DB. 

Si los elementos del contenedor de Cosmos DB tienen las propiedades `date_rep`, `cases`y `geo_id`, los resultados de esta consulta se muestran en la tabla siguiente:

| date_rep | cases | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

Si tiene que explorar datos del otro contenedor de la misma base de datos de Azure Cosmos DB, puede usar la misma cadena de conexión y hacer referencia al contenedor necesario como tercer parámetro:

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19) as cord19
```

## <a name="explicitly-specify-schema"></a>Especificación explícita del esquema

Aunque la funcionalidad de inferencia de esquema automática en `OPENROWSET` proporciona una experiencia fácil de usar, es posible que para escenarios empresariales tenga que especificar de forma explícita el esquema para leer solo las propiedades pertinentes de los datos de Azure Cosmos DB.

`OPENROWSET` le permite especificar de forma explícita las propiedades que quiere leer de los datos del contenedor, junto a sus tipos de datos. Imagine que ha importado algunos datos del [conjunto de datos ECDC COVID](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) con la siguiente estructura en Azure Cosmos DB:

```json
{"date_rep":"2020-08-13","cases":254,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-12","cases":235,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-11","cases":163,"countries_and_territories":"Serbia","geo_id":"RS"}
```

Estos documentos JSON planos en Azure Cosmos DB se pueden representar como un conjunto de filas y columnas en Synapse SQL. La función `OPENROWSET` le permite especificar un subconjunto de propiedades que quiere leer y los tipos de columna exactos en la cláusula `WITH`:

```sql
SELECT TOP 10 *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```

El resultado de esta consulta podría ser similar al siguiente:

| date_rep | cases | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

Revise las [reglas de asignaciones de tipos SQL](#azure-cosmos-db-to-sql-type-mappings) al final del artículo para obtener más información sobre los tipos de SQL que se deben usar para los valores de Azure Cosmos DB.

## <a name="querying-nested-objects-and-arrays"></a>Consulta de objetos y matrices anidados

Azure Cosmos DB permite representar modelos de datos más complejos si se crean como matrices u objetos anidados. La funcionalidad de sincronización automática de Synapse Link para Azure Cosmos DB administra la representación del esquema en el almacén analítico de forma integrada, lo que incluye el control de los tipos de datos anidados, para así poder realizar consultas enriquecidas desde un grupo de SQL sin servidor.

Por ejemplo, el conjunto de datos [CORD-19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) tiene documentos JSON con la estructura siguiente:

```json
{
    "paper_id": <str>,                   # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": <array of objects>    # list of author dicts, in order
        ...
     }
     ...
}
```

Los objetos y las matrices anidados de Azure Cosmos DB se representan como cadenas JSON en el resultado de la consulta cuando los lee la función `OPENROWSET`. Una opción para leer los valores de estos tipos complejos como columnas SQL consiste en usar funciones JSON de SQL:

```sql
SELECT
    title = JSON_VALUE(metadata, '$.title'),
    authors = JSON_QUERY(metadata, '$.authors'),
    first_author_name = JSON_VALUE(metadata, '$.authors[0].first')
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( metadata varchar(MAX) ) AS docs;
```

El resultado de esta consulta podría ser similar al siguiente:

| title | authors | first_autor_name |
| --- | --- | --- |
| Información complementaria Un estudio de epide… |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien |  

Como opción, también puede especificar las rutas de acceso a los valores anidados en los objetos al usar la cláusula `WITH`:

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( title varchar(1000) '$.metadata.title',
           authors varchar(max) '$.metadata.authors'
    ) AS docs;
```

Obtenga más información sobre cómo analizar [tipos de datos complejos en Synapse Link](../how-to-analyze-complex-schema.md) y [estructuras anidadas en un grupo de SQL sin servidor](query-parquet-nested-types.md).

> [!IMPORTANT]
> Si ve caracteres inesperados en el texto, como `MÃƒÂ©lade` en lugar de `Mélade`, la intercalación de la base de datos no está establecida en [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8). 
> [Cambie la intercalación de la base de datos](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) por alguna intercalación UTF8 mediante una instrucción SQL como `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8`.

## <a name="flattening-nested-arrays"></a>Acoplamiento de matrices anidadas

Es posible que los datos de Azure Cosmos DB tengan submatrices anidadas como la matriz del creador del conjunto de datos [Cord19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/):

```json
{
    "paper_id": <str>,                      # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": [                        # list of author dicts, in order
            {
                "first": <str>,
                "middle": <list of str>,
                "last": <str>,
                "suffix": <str>,
                "affiliation": <dict>,
                "email": <str>
            },
            ...
        ],
        ...
}
```

En algunos casos, puede que tenga que "combinar" las propiedades del elemento superior (metadata) con todos los elementos de la matriz (authors). Un grupo de SQL sin servidor permite acoplar estructuras anidadas mediante la aplicación de la función `OPENJSON` en la matriz anidada:

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    ) WITH ( title varchar(1000) '$.metadata.title',
             authors varchar(max) '$.metadata.authors' ) AS docs
      CROSS APPLY OPENJSON ( authors )
                  WITH (
                       first varchar(50),
                       last varchar(50),
                       affiliation nvarchar(max) as json
                  ) AS a
```

El resultado de esta consulta podría ser similar al siguiente:

| title | authors | first | last | affiliation |
| --- | --- | --- | --- | --- |
| Información complementaria Un estudio de epide… |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien | Mélade | `   {"laboratory":"Centre de Recher…` |
Información complementaria Un estudio de epide… | `[{"first":"Nicolas","last":"4#","suffix":"","affiliation":{"laboratory":"","institution":"U…` | Nicolas | N.° 4 |`{"laboratory":"","institution":"U…` | 
| Información complementaria Un estudio de epide… |   `[{"first":"Beza","last":"Ramazindrazana","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Beza | Ramazindrazana | `{"laboratory":"Centre de Recher…` |
| Información complementaria Un estudio de epide… |   `[{"first":"Olivier","last":"Flores","suffix":"","affiliation":{"laboratory":"UMR C53 CIRAD, …` | Olivier | Flores |`{"laboratory":"UMR C53 CIRAD, …` |     

> [!IMPORTANT]
> Si ve caracteres inesperados en el texto, como `MÃƒÂ©lade` en lugar de `Mélade`, la intercalación de la base de datos no está establecida en [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8). 
> [Cambie la intercalación de la base de datos](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) por alguna intercalación UTF8 mediante una instrucción SQL como `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8`.

## <a name="azure-cosmos-db-to-sql-type-mappings"></a>Asignaciones de Azure Cosmos DB a tipos de SQL

Es importante indicar primero que, mientras que el almacén transaccional de Azure Cosmos DB es independiente del esquema, el almacén analítico está esquematizado para la optimización del rendimiento de las consultas analíticas. Con la funcionalidad de sincronización automática de Synapse Link, Azure Cosmos DB administra la representación del esquema en el almacén analítico de forma integrada, lo que incluye el control de los tipos de datos anidados. Dado que el grupo de SQL sin servidor consulta el almacén analítico, es importante comprender cómo asignar tipos de datos de entrada de Azure Cosmos DB a tipos de datos de SQL.

Las cuentas de Azure Cosmos DB de SQL (Core) API admiten tipos de propiedad JSON de número, cadena, booleano, nulo, objeto anidado o matriz. Tendrá que elegir tipos SQL que coincidan con estos tipos JSON si usa la cláusula `WITH` en `OPENROWSET`. Vea los tipos de columna SQL siguientes que se deben usar para los distintos tipos de propiedad en Azure Cosmos DB.

| Tipo de propiedad de Azure Cosmos DB | Tipo de columna SQL |
| --- | --- |
| Boolean | bit |
| Entero | bigint |
| Decimal | FLOAT |
| String | varchar (intercalación de base de datos UTF8) |
| Fecha y hora (cadena con formato ISO) | varchar(30) |
| Fecha y hora (marca de tiempo de UNIX) | bigint |
| Null | `any SQL type` 
| Objeto o matriz anidados | varchar(max) (intercalación de base de datos UTF8), serializado como texto JSON |

## <a name="full-fidelity-schema"></a>Esquema de fidelidad completa

El esquema de fidelidad completa de Azure Cosmos DB registra los valores y sus tipos de mejor coincidencia para cada propiedad de un contenedor.
La función `OPENROWSET` en un contenedor con un esquema de fidelidad completa proporciona el tipo y el valor real en cada celda. Imagine que la consulta siguiente lee los elementos de un contenedor con un esquema de fidelidad completa:

```sql
SELECT *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) as rows
```

El resultado de esta consulta devolverá tipos y valores con formato de texto JSON: 

| date_rep | cases | geo_id |
| --- | --- | --- |
| {"date":"2020-08-13"} | {"int32":"254"} | {"string":"RS"} |
| {"date":"2020-08-12"} | {"int32":"235"}| {"string":"RS"} |
| {"date":"2020-08-11"} | {"int32":"316"} | {"string":"RS"} |
| {"date":"2020-08-10"} | {"int32":"281"} | {"string":"RS"} |
| {"date":"2020-08-09"} | {"int32":"295"} | {"string":"RS"} |
| {"string":"2020/08/08"} | {"int32":"312"} | {"string":"RS"} |
| {"date":"2020-08-07"} | {"float64":"339.0"} | {"string":"RS"} |

En cada valor, puede ver el tipo identificado en el elemento del contenedor de Cosmos DB. La mayoría de los valores de la propiedad `date_rep` contienen valores `date`, pero algunos de ellos se almacenan incorrectamente como cadenas en Cosmos DB. El esquema de fidelidad completa devolverá valores `date` con el tipo correcto y valores `string` con formato incorrecto.
El número de casos es una información almacenada como valor `int32`, pero hay un valor que se especifica como número decimal. Este valor tiene el tipo `float64`. Si hay algunos valores que superan el número `int32` mayor, se almacenarían como tipo `int64`. Todos los valores `geo_id` de este ejemplo se almacenan como tipos `string`.

> [!IMPORTANT]
> La función `OPENROWSET` sin una cláusula `WITH` expone valores con los tipos esperados y valores con tipos especificados incorrectamente. Esta función está diseñada para la exploración de datos y no para la creación de informes. No analice los valores JSON devueltos por esta función para generar informes; use una [cláusula WITH](#querying-items-with-full-fidelity-schema) explícita para crearlos.
> Para que el almacén analítico de fidelidad completa sea correcto, tendrá que limpiar los valores que tienen tipos incorrectos en el contenedor de Azure Cosmos DB. 

Para consultar cuentas de Azure Cosmos DB del tipo Mongo DB API, [aquí](../../cosmos-db/analytical-store-introduction.md#analytical-schema) puede obtener más información sobre la representación de esquemas de fidelidad completa en el almacén analítico y los nombres de propiedad extendidos que se van a usar.

### <a name="querying-items-with-full-fidelity-schema"></a>Consulta de elementos con un esquema de fidelidad completa

Al consultar el esquema de fidelidad completa, debe especificar de forma explícita el tipo SQL y el tipo de propiedad de Cosmos DB que se espera en la cláusula `WITH`. No use `OPENROWSET` sin la cláusula `WITH` en los informes, ya que el formato del conjunto de resultados podría cambiarse en la vista previa en función de los comentarios.

En el ejemplo siguiente, se supone que `string` es el tipo correcto para la propiedad `geo_id` y `int32` el tipo correcto para la propiedad `cases`:

```sql
SELECT geo_id, cases = SUM(cases)
FROM OPENROWSET(
      'CosmosDB'
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) WITH ( geo_id VARCHAR(50) '$.geo_id.string',
             cases INT '$.cases.int32'
    ) as rows
GROUP BY geo_id
```

Los valores para `geo_id` y `cases` que tienen otros tipos se devolverán como valores `NULL`. Esta consulta solo hará referencia a `cases` con el tipo especificado en la expresión (`cases.int32`).

Si tiene valores con otros tipos (`cases.int64`, `cases.float64`) que no se pueden limpiar en el contenedor de Cosmos DB, tendrá que hacerles referencia de forma explícita en la cláusula `WITH` y combinar los resultados. La consulta siguiente agrega los elementos `int32`, `int64` y `float64` almacenados en la columna `cases`:

```sql
SELECT geo_id, cases = SUM(cases_int) + SUM(cases_bigint) + SUM(cases_float)
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) WITH ( geo_id VARCHAR(50) '$.geo_id.string', 
             cases_int INT '$.cases.int32',
             cases_bigint BIGINT '$.cases.int64',
             cases_float FLOAT '$.cases.float64'
    ) as rows
GROUP BY geo_id
```

En este ejemplo, el número de casos se almacena como valores `int32`, `int64` o `float64`, y se deben extraer todos los valores para calcular el número de casos por país. 

## <a name="known-issues"></a>Problemas conocidos

- El alias se **DEBE** especificar después de la función `OPENROWSET` (por ejemplo, `OPENROWSET (...) AS function_alias`). Si se omite el alias, es posible que haya problemas de conexión y que el punto de conexión SQL sin servidor de Synapse no esté disponible de manera temporal. Este problema se resolverá en noviembre de 2020.
- La experiencia de consulta que proporciona el grupo SQL sin servidor para el [esquema de total fidelidad de Azure Cosmos DB](#full-fidelity-schema) es un comportamiento temporal que se cambiará en función de los comentarios de la versión preliminar. No se base en el esquema que proporciona la función `OPENROWSET` sin la cláusula `WITH` durante la versión preliminar pública, ya que la experiencia de consulta podría estar alineada con un esquema bien definido en función de los comentarios de los clientes. Póngase en contacto con el [equipo del producto de Synapse Link](mailto:cosmosdbsynapselink@microsoft.com) para proporcionar comentarios.

En la tabla siguiente se enumeran los posibles errores y las acciones para solucionar problemas:

| Error | Causa principal |
| --- | --- |
| Errores de sintaxis:<br/> - Sintaxis incorrecta cerca de "OpenRowset"<br/> - `...` no es una opción de proveedor de BULK OPENROWSET reconocida.<br/> - Sintaxis incorrecta cerca de `...` | Posibles causas principales<br/> - No se usa "CosmosDB" como primer parámetro.<br/> - Se usa un literal de cadena, en lugar de un identificador en el tercer parámetro.<br/> - No se especifica el tercer parámetro (nombre del contenedor) |
| Error en la cadena de conexión de CosmosDB | - No se ha especificado la cuenta, la base de datos y la clave. <br/> -Hay una opción en la cadena de conexión que no se reconoce.<br/> -El signo de punto y coma `;` se coloca al final de la cadena de conexión |
| Error al resolver la ruta de acceso de CosmosDB: "Nombre de cuenta incorrecto" o "Nombre de base de datos incorrecto" | No se encuentra el nombre de cuenta, el nombre de la base de datos o el contenedor especificados, o bien no se han habilitado el almacenamiento analítico o la recopilación especificada|
| Error al resolver la ruta de acceso de CosmosDB: "Valor de secreto incorrecto" o "El secreto es nulo o está vacío" | La clave de cuenta falta o no es válida. |
| La columna `column name` del tipo `type name` no es compatible con el tipo de datos externos`type name` | El tipo de columna especificado en la cláusula `WITH` no coincide con el tipo del contenedor de Cosmos DB. Intente cambiar el tipo de columna como se describe en la sección [Asignaciones de Azure Cosmos DB a tipos de SQL](#azure-cosmos-db-to-sql-type-mappings) o use el tipo `VARCHAR`. |
| La columna contiene valores `NULL` en todas las celdas. | Es posible que el nombre de columna o la expresión de la ruta de acceso sean incorrectos en la cláusula `WITH`. El nombre de columna (o la expresión de la ruta de acceso después del tipo de columna) de la cláusula `WITH` debe coincidir con el nombre de una propiedad de un colección de Cosmos DB. La comparación **distingue entre mayúsculas y minúsculas** (por ejemplo, `productCode` y `ProductCode` son propiedades diferentes). |

Puede informar sugerencias y problemas en la [página de comentarios de Azure Synapse](https://feedback.azure.com/forums/307516-azure-synapse-analytics?category_id=387862).

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos.

- [Uso de Power BI y el grupo de SQL sin servidor con Azure Synapse Link](../../cosmos-db/synapse-link-power-bi.md)
- [Procedimiento para crear y usar vistas en el grupo de SQL sin servidor](create-use-views.md) 
- [Tutorial sobre la creación de vistas del grupo de SQL sin servidor en Azure Cosmos DB y su conexión a modelos de Power BI a través de DirectQuery](./tutorial-data-analyst.md)
