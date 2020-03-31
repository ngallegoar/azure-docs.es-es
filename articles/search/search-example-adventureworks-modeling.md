---
title: 'Ejemplo: Modelado de la base de datos del inventario de AdventureWorks'
titleSuffix: Azure Cognitive Search
description: Aprenda a modelar datos relacionales, transformándolos en un conjunto de datos planos, para indexación y búsqueda de texto completo en Azure Cognitive Search.
author: HeidiSteen
manager: nitinme
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: heidist
ms.openlocfilehash: edb6162724938962df8a7340afea6e930a0b1049
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792988"
---
# <a name="example-model-the-adventureworks-inventory-database-for-azure-cognitive-search"></a>Ejemplo: Modelado de la base de datos del inventario de AdventureWorks para Azure Cognitive Search

Azure Cognitive Search acepta un conjunto de filas planas como entradas para la [canalización de indexación (ingesta de datos)](search-what-is-an-index.md). Si los datos de origen proceden de una base de datos relacional SQL Server, en este artículo se muestra un enfoque para crear un conjunto de filas planas antes de la indexación, la base de datos de AdventureWorks como ejemplo.

## <a name="about-adventureworks"></a>Acerca de AdventureWorks

Si tiene una instancia de SQL Server, es posible que esté familiarizado con la [base de datos de ejemplo de AdventureWorks](https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017). Entre las tablas incluidas en esta base de datos se encuentran cinco tablas que exponen información sobre los productos.

+ **ProductModel**: nombre
+ **Product**: nombre, color, costo, tamaño, peso, imagen y la categoría (cada fila se combina con un atributo ProductModel específico)
+ **ProductDescription**: descripción
+ **ProductModelProductDescription**: configuración regional (cada fila combina un atributo ProductModel con un ProductDescription específico para un idioma específico)
+ **ProductCategory**: nombre, categoría primaria

Combinar todos estos datos en un conjunto de filas planas que se puede ingerir en un índice de búsqueda es el objetivo de este ejemplo. 

## <a name="considering-our-options"></a>Consideración de nuestras opciones

El enfoque primitivo consistiría en indexar todas las filas de la tabla Product (combinadas en su caso), ya que la tabla Product contiene la información más específica. Sin embargo, ese enfoque expondría el índice de búsqueda a duplicados percibidos en un conjunto de resultados. Por ejemplo, el modelo Road-650 está disponible en dos colores y en seis tamaños. Una búsqueda de "bicicletas de carretera" estaría entonces dominada por doce instancias del mismo modelo, diferenciadas solo por el tamaño y el color. Los otros seis modelos específicos de bicicletas quedarían relegados al mundo inferior de la búsqueda: página dos.

  ![Lista de productos](./media/search-example-adventureworks/products-list.png "Lista de productos")
 
Observe que el modelo Road-650 tiene doce opciones. Las filas de la entidad de una a muchas se representan mejor como campos de valores múltiples o campos de valores preagregados en el índice de búsqueda.

Resolver este problema no es tan simple como mover el índice de destino a la tabla ProductModel. Si lo hace, ignorará los datos importantes de la tabla Product que aún deberían estar representados en los resultados de la búsqueda.

## <a name="use-a-collection-data-type"></a>Uso de un tipo de datos de colección

El "enfoque correcto" es utilizar una característica de esquema de búsqueda que no tiene un paralelo directo en el modelo de base de datos: **Collection(Edm.String)** . Esta construcción se define en el esquema de índice de Azure Cognitive Search. Se utiliza el tipo de datos Collection cuando es necesario representar una lista de cadenas individuales, en lugar de una cadena muy larga (única). Si tiene etiquetas o palabras clave, utilizaría un tipo de datos Collection para este campo.

Mediante la definición de campos de índice de valores múltiples de **Collection(Edm.String)** para "color", "tamaño" e "imagen", la información auxiliar se conserva para facetar y filtrar sin contaminar el índice con entradas duplicadas. Del mismo modo, aplique funciones de agregado a los campos numéricos del producto, indexando **minListPrice** en lugar de **listPrice** de cada uno de los productos.

Dado un índice con estas estructuras, una búsqueda de "bicicletas de montaña" mostraría modelos discretos de bicicletas, a la vez que preservaría metadatos importantes como el color, el tamaño y el precio más bajo. En la captura de pantalla siguiente se muestra una ilustración.

  ![Ejemplo de búsqueda de bicicletas de montaña](./media/search-example-adventureworks/mountain-bikes-visual.png "Ejemplo de búsqueda de bicicletas de montaña")

## <a name="use-script-for-data-manipulation"></a>Uso de un script para la manipulación de datos

Desafortunadamente, este tipo de modelado no se puede lograr fácilmente solo a través de instrucciones SQL. En su lugar, utilice un sencillo script de NodeJS para cargar los datos y luego asignarlos en entidades JSON aptas para la búsqueda.

La asignación final de la búsqueda en la base de datos tiene el siguiente aspecto:

+ model (Edm.String: se puede buscar, filtrable, recuperable) en "ProductModel.Name"
+ description_en (Edm.String: se puede buscar) en "ProductDescription" para el modelo donde culture=’en’
+ color (Collection(Edm.String): se puede buscar, filtrable, clasificable, recuperable): valores únicos de "Product.Color" para el modelo
+ size (Collection(Edm.String): se puede buscar, filtrable, clasificable, recuperable): valores únicos de "Product.Size" para el modelo
+ image (Collection(Edm.String): recuperable): valores únicos de "Product.ThumbnailPhoto" para el modelo
+ minStandardCost (Edm.Double: filtrable, clasificable, ordenable, recuperable): mínimo agregado de todo "Product.StandardCost" para el modelo
+ minListPrice (Edm.Double: filtrable, clasificable, ordenable, recuperable): mínimo agregado de todo "Product.ListPrice" para el modelo
+ minWeight (Edm.Double: filtrable, clasificable, ordenable, recuperable): mínimo agregado de todo "Product.Weight" para el modelo
+ products (Collection(Edm.String): se puede buscar, filtrable, recuperable): valores únicos de "Product.Name" para el modelo

Después de unir la tabla ProductModel con Product y ProductDescription, utilice [lodash](https://lodash.com/) (o Linq en C#) para transformar rápidamente el conjunto de resultados:

```javascript
var records = queryYourDatabase();
var models = _(records)
  .groupBy('ModelName')
  .values()
  .map(function(d) {
    return {
      model: _.first(d).ModelName,
      description: _.first(d).Description,
      colors: _(d).pluck('Color').uniq().compact().value(),
      products: _(d).pluck('ProductName').uniq().compact().value(),
      sizes: _(d).pluck('Size').uniq().compact().value(),
      images: _(d).pluck('ThumbnailPhotoFilename').uniq().compact().value(),
      minStandardCost: _(d).pluck('StandardCost').min(),
      maxStandardCost: _(d).pluck('StandardCost').max(),
      minListPrice: _(d).pluck('ListPrice').min(),
      maxListPrice: _(d).pluck('ListPrice').max(),
      minWeight: _(d).pluck('Weight').min(),
      maxWeight: _(d).pluck('Weight').max(),
    };
  })
  .value();
```

El JSON resultante tendrá este aspecto:

```json
[
  {
    "model": "HL Road Frame",
    "colors": [
      "Black",
      "Red"
    ],
    "products": [
      "HL Road Frame - Black, 58",
      "HL Road Frame - Red, 58",
      "HL Road Frame - Red, 62",
      "HL Road Frame - Red, 44",
      "HL Road Frame - Red, 48",
      "HL Road Frame - Red, 52",
      "HL Road Frame - Red, 56",
      "HL Road Frame - Black, 62",
      "HL Road Frame - Black, 44",
      "HL Road Frame - Black, 48",
      "HL Road Frame - Black, 52"
    ],
    "sizes": [
      "58",
      "62",
      "44",
      "48",
      "52",
      "56"
    ],
    "images": [
      "no_image_available_small.gif"
    ],
    "minStandardCost": 868.6342,
    "maxStandardCost": 1059.31,
    "minListPrice": 1431.5,
    "maxListPrice": 1431.5,
    "minWeight": 961.61,
    "maxWeight": 1043.26
  }
]
```

Finalmente, aquí está la consulta SQL para devolver el conjunto de registros inicial. He usado el módulo [mssql](https://www.npmjs.com/package/mssql) npm para cargar los datos en mi aplicación NodeJS.

```T-SQL
SELECT
  m.Name as ModelName,
  d.Description,
  p.Name as ProductName,
  p.*
FROM 
  SalesLT.ProductModel m
INNER JOIN 
  SalesLT.ProductModelProductDescription md
  ON m.ProductModelId = md.ProductModelId
INNER JOIN 
  SalesLT.ProductDescription d
  ON md.ProductDescriptionId = d.ProductDescriptionId
LEFT JOIN 
  SalesLT.product p
  ON m.ProductModelId = p.ProductModelId
WHERE
  md.Culture='en'
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejemplo: Taxonomías de facetas de varios niveles en Azure Cognitive Search](search-example-adventureworks-multilevel-faceting.md)