---
title: Asignación de esquemas en la actividad de copia
description: Obtenga información acerca de cómo la actividad de copia de Azure Data Factory asigna esquemas y tipos de datos desde datos de origen hasta datos receptores al copiar datos.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: jingwang
ms.openlocfilehash: 9f04955fb910a6159dc09ac40a87a398e67d59d6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414127"
---
# <a name="schema-mapping-in-copy-activity"></a>Asignación de esquemas en la actividad de copia
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Este artículo describe el modo en que la actividad de copia de Azure Data Factory realiza la asignación de esquemas y la asignación del tipo de datos de los datos de origen a los datos receptores al ejecutar la copia de datos.

## <a name="schema-mapping"></a>Asignación de esquemas

La asignación de columnas se aplica cuando se copian datos del origen al receptor. De forma predeterminada, la actividad de copia **asigna los datos de origen al receptor por nombres de columna**. Puede especificar una [asignación explícita](#explicit-mapping) para personalizar la asignación de columnas según sus necesidades. Más concretamente, la actividad de copia:

1. Lee los datos desde el origen y determina el esquema de origen.
2. Utilice la asignación de columnas predeterminada para asignar las columnas por nombre o aplique la asignación de columnas explícita si se especifica.
3. Escritura de datos en el receptor

### <a name="explicit-mapping"></a>Asignación explícita

Puede especificar las columnas que quiere asignar en actividad de copia -> propiedad `translator` -> `mappings`. El ejemplo siguiente define una actividad de copia en una canalización para copiar datos de texto delimitado a Azure SQL Database.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [{
        "referenceName": "DelimitedTextInput",
        "type": "DatasetReference"
    }],
    "outputs": [{
        "referenceName": "AzureSqlOutput",
        "type": "DatasetReference"
    }],
    "typeProperties": {
        "source": { "type": "DelimitedTextSource" },
        "sink": { "type": "SqlSink" },
        "translator": {
            "type": "TabularTranslator",
            "mappings": [
                {
                    "source": {
                        "name": "UserId",
                        "type": "Guid"
                    },
                    "sink": {
                        "name": "MyUserId"
                    }
                }, 
                {
                    "source": {
                        "name": "Name",
                        "type": "String"
                    },
                    "sink": {
                        "name": "MyName"
                    }
                }, 
                {
                    "source": {
                        "name": "Group",
                        "type": "String"
                    },
                    "sink": {
                        "name": "MyGroup"
                    }
                }
            ]
        }
    }
}
```

Se admiten las siguientes propiedades en `translator` -> `mappings` -> objeto con `source` y `sink`:

| Propiedad | Descripción                                                  | Obligatorio |
| -------- | ------------------------------------------------------------ | -------- |
| name     | Nombre de la columna de origen o receptor.                           | Sí      |
| ordinal  | Índice de columna. Comienza con 1. <br>Se aplica y es obligatoria cuando se usa texto delimitado sin línea de encabezado. | No       |
| path     | Expresión de ruta de acceso JSON de cada campo para su extracción o asignación. Se aplica para los datos jerárquicos; por ejemplo, MongoDB o REST.<br>Para los campos situados bajo el objeto raíz, la ruta de acceso JSON comienza con root $; para los campos incluidos dentro de la matriz elegida mediante la propiedad `collectionReference`, la ruta de acceso JSON empieza desde el elemento de matriz. | No       |
| type     | Tipo de datos provisionales de Data Factory de la columna de origen o receptor. | No       |
| culture  | Cultura de la columna de origen o receptor. <br>Se aplica cuando el tipo es `Datetime` o `Datetimeoffset`. El valor predeterminado es `en-us`. | No       |
| format   | Cadena de formato que se usa cuando el tipo es `Datetime` o `Datetimeoffset`. Consulte [Cadenas con formato de fecha y hora personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) para obtener información sobre el formato de fecha y hora. | No       |

Se admiten las siguientes propiedades en `translator` -> `mappings`, así como en un objeto con `source` y `sink`:

| Propiedad            | Descripción                                                  | Obligatorio |
| ------------------- | ------------------------------------------------------------ | -------- |
| collectionReference | Solo se admite si el origen son datos jerárquicos; por ejemplo, MongoDB o REST.<br>Si desea iterar y extraer datos de los objetos **dentro de un campo de matriz** con el mismo patrón y convertir al modo por fila por objeto, especifique la ruta de acceso JSON de esa matriz para realizar la aplicación cruzada. | No       |

### <a name="alternative-column-mapping"></a>Asignación de columnas alternativa

Puede especificar actividad de copia -> `translator` -> `columnMappings` para realizar asignaciones de datos en forma tabular. En este caso, la sección "structure" es necesaria tanto para los conjuntos de datos de entrada como para los de salida. La asignación de columnas admite **la asignación de todas las columnas o un subconjunto de ellas del conjunto de datos de origen "structure" a todas las columnas del conjunto de datos receptor "structure"** . Las siguientes son las condiciones de error que tienen como resultado una excepción:

* El resultado de la consulta del almacén de datos de origen no tiene un nombre de columna que se especifique en la sección "structure" del conjunto de datos de entrada.
* El almacén de datos receptor (si está en el esquema predefinido) no tiene un nombre de columna que se especifique en la sección "structure" del conjunto de datos de salida.
* O bien menos columnas o más columnas en "structure" del conjunto de datos receptor de las que se especifican en la asignación.
* Asignación duplicada.

En el siguiente ejemplo, el conjunto de datos de entrada tiene una estructura y apunta a una tabla en una base de datos de Oracle local.

```json
{
    "name": "OracleDataset",
    "properties": {
        "structure":
         [
            { "name": "UserId"},
            { "name": "Name"},
            { "name": "Group"}
         ],
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "OracleLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTable"
        }
    }
}
```

En este ejemplo, el conjunto de datos de salida tiene una estructura y apunta a una tabla en Salesfoce.

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "structure":
        [
            { "name": "MyUserId"},
            { "name": "MyName" },
            { "name": "MyGroup"}
        ],
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "SalesforceLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SinkTable"
        }
    }
}
```

El siguiente fragmento JSON define una actividad de copia en una canalización. Las columnas del origen se asignan a columnas del receptor mediante la propiedad **translator** -> **columnMappings**.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [
        {
            "referenceName": "OracleDataset",
            "type": "DatasetReference"
        }
    ],
    "outputs": [
        {
            "referenceName": "SalesforceDataset",
            "type": "DatasetReference"
        }
    ],
    "typeProperties":    {
        "source": { "type": "OracleSource" },
        "sink": { "type": "SalesforceSink" },
        "translator":
        {
            "type": "TabularTranslator",
            "columnMappings":
            {
                "UserId": "MyUserId",
                "Group": "MyGroup",
                "Name": "MyName"
            }
        }
    }
}
```

Si estaba usando la sintaxis de `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` para especificar una asignación de columnas, esta se sigue admitiendo tal cual.

### <a name="alternative-schema-mapping"></a>Asignación de esquemas alternativa

Puede especificar actividad de copia -> `translator` -> `schemaMapping` para realizar la asignación de tablas de datos jerárquicos y datos tabulares. Por ejemplo, al copiar de MongoDB o REST a un archivo de texto, o al copiar de Oracle a la API de Azure Cosmos DB para MongoDB. Se admiten las siguientes propiedades en la sección `translator` de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del traductor de la actividad de copia debe establecerse en: **TabularTranslator** | Sí |
| schemaMapping | Colección de pares clave-valor, que representa la relación de la asignación **del lado origen al lado receptor**.<br/>- **Clave:** representa el origen. Para un **origen tabular**, especifique el nombre de columna tal como se define en la estructura del conjunto de datos; para un **origen jerárquico**, especifique la expresión de ruta de acceso JSON para todos los campos que va a extraer y asignar.<br>- **Valor:** representa el receptor. Para un **receptor tabular**, especifique el nombre de columna tal como se define en la estructura del conjunto de datos; para un **receptor jerárquico**, especifique la expresión de ruta de acceso JSON para todos los campos que va a extraer y asignar. <br>En el caso de los datos jerárquicos, para los campos en el objeto raíz, la ruta de acceso JSON comienza con root $; para los campos dentro de la matriz elegida mediante la propiedad `collectionReference`, la ruta de acceso JSON empieza desde el elemento de matriz.  | Sí |
| collectionReference | Si desea iterar y extraer datos de los objetos **dentro de un campo de matriz** con el mismo patrón y convertir al modo por fila por objeto, especifique la ruta de acceso JSON de esa matriz para realizar la aplicación cruzada. Esta propiedad solo se admite si el origen son datos jerárquicos. | No |

**Ejemplo: copia de MongoDB a Oracle:**

Por ejemplo, si tiene un documento de MongoDB con el siguiente contenido:

```json
{
    "id": {
        "$oid": "592e07800000000000000000"
    },
    "number": "01",
    "date": "20170122",
    "orders": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "name": "Seattle" } ]
}
```

y desea copiarlo en una tabla de Azure SQL del formato siguiente, puede hacerlo mediante el acoplamiento de los datos dentro de la matriz *(order_pd y order_price)* y la combinación cruzada con la información de la raíz habitual *(número, fecha y ciudad)* :

| orderNumber | orderDate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | Seattle |
| 01 | 20170122 | P2 | 13 | Seattle |
| 01 | 20170122 | P3 | 231 | Seattle |

Configure la regla de asignación de esquemas como el siguiente ejemplo JSON de actividad de copia:

```json
{
    "name": "CopyFromMongoDBToOracle",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "MongoDbV2Source"
        },
        "sink": {
            "type": "OracleSink"
        },
        "translator": {
            "type": "TabularTranslator",
            "schemaMapping": {
                "$.number": "orderNumber",
                "$.date": "orderDate",
                "prod": "order_pd",
                "price": "order_price",
                "$.city[0].name": "city"
            },
            "collectionReference":  "$.orders"
        }
    }
}
```

## <a name="data-type-mapping"></a>Asignación de tipos de datos

La actividad de copia realiza la asignación de tipos de origen a tipos receptores con este proceso de dos pasos:

1. Conversión de los tipos de origen nativo a tipos de datos provisionales de Azure Data Factory
2. Conversión de los tipos de datos provisionales de Azure Data Factory al tipo de receptor nativo

La actividad de copia admite los siguientes tipos de datos provisionales: 

* Byte[]
* Boolean
* Datetime
* Datetimeoffset
* Decimal
* Double
* Guid
* Int16
* Int32
* Int64
* Single
* String
* TimeSpan

## <a name="next-steps"></a>Pasos siguientes
Consulte los otros artículos de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
