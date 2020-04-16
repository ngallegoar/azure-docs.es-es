---
title: Copia de datos en Azure Database for PostgreSQL como origen o destino
description: Aprenda a copiar datos en Azure Database for PostgreSQL como origen o destino mediante una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/16/2019
ms.openlocfilehash: b85e72ae6698cd9fa018c940e158bfcf25279ed5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81410462"
---
# <a name="copy-data-to-and-from-azure-database-for-postgresql-by-using-azure-data-factory"></a>Copia de datos en Azure Database for PostgreSQL como origen o destino mediante Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe el uso de la actividad de copia de actividades de Azure Data Factory para copiar datos de Azure Database for PostgreSQL. Este se compila en función del artículo [Actividad de copia en Azure Data Factory](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

Ese conector se usa especialmente para el [servicio Azure Database for PostgreSQL](../postgresql/overview.md). Para copiar datos desde una base de datos PostgreSQL genérica ubicada en el entorno local o en la nube, use el [conector PostgreSQL](connector-postgresql.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Azure Database for PostgreSQL es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos de Azure Database for PostgreSQL en cualquier almacén de datos receptor compatible. También puede copiar datos en Azure Database for PostgreSQL desde cualquier almacén de datos de origen admitido. Consulte la tabla de [almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de los almacenes de datos que la actividad de copia admite como orígenes y receptores.

Azure Data Factory proporciona un controlador integrado para permitir la conectividad. Por lo tanto, no es necesario instalar manualmente cualquier controlador para usar este conector.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas para el conector de Azure Database for PostgreSQL.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Database for PostgreSQL:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **AzurePostgreSql**. | Sí |
| connectionString | Cadena de conexión de ODBC para conectarse a Azure Database for PostgreSQL.<br/>También puede establecer una contraseña en Azure Key Vault y extraer la configuración de `password` de la cadena de conexión. Consulte los siguientes ejemplos y el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) con información detallada. | Sí |
| connectVia | Esta propiedad representa el [tiempo de ejecución de integración](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) (si el almacén de datos se encuentra en una red privada) o Azure Integration Runtime. Si no se especifica, se usará Azure Integration Runtime. |No |

Una cadena de conexión típica es `Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Aquí encontrará más propiedades que puede establecer para su caso:

| Propiedad | Descripción | Opciones | Obligatorio |
|:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Método que usa el controlador para cifrar los datos enviados entre el controlador y el servidor de bases de datos. Por ejemplo: `EncryptionMethod=<0/1/6>;`| 0 (sin cifrado) **(valor predeterminado)** / 1 (SSL) / 6 (RequestSSL) | No |
| ValidateServerCertificate (VSC) | Determina si el controlador valida el certificado que envía el servidor de bases de datos cuando está habilitado el cifrado SSL (método de cifrado = 1). Por ejemplo: `ValidateServerCertificate=<0/1>;`| 0 (deshabilitado) **(valor predeterminado)** / 1 (habilitado) | No |

**Ejemplo**:

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
        }
    }
}
```

**Ejemplo**:

***Guardar la contraseña en Azure Key Vault***

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si quiere ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos en Azure Data Factory](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que Azure Database for PostgreSQL admite en los conjuntos de datos.

Para copiar datos de Azure Database for PostgreSQL, establezca la propiedad type del conjunto de datos en **AzurePostgreSqlTable**. Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad "type" del conjunto de datos debe establecerse en **AzurePostgreSqlTable**. | Sí |
| tableName | Nombre de la tabla | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**:

```json
{
    "name": "AzurePostgreSqlDataset",
    "properties": {
        "type": "AzurePostgreSqlTable",
        "linkedServiceName": {
            "referenceName": "<AzurePostgreSql linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Para ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte [Canalizaciones y actividades en Azure Data Factory](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el origen de Azure Database for PostgreSQL.

### <a name="azure-database-for-postgresql-as-source"></a>Azure Database for PostgreSQL como origen

Para copiar datos de Azure Database for PostgreSQL, establezca el tipo de origen de la actividad de copia en **AzurePostgreSqlSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad "type" del origen de la actividad de copia debe establecerse en **AzurePostgreSqlSource**. | Sí |
| Query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM MyTable"` | No (si se especifica la propiedad tableName en el conjunto de datos) |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromAzurePostgreSql",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AzurePostgreSql input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzurePostgreSqlSource",
                "query": "<custom query e.g. SELECT * FROM MyTable>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-database-for-postgresql-as-sink"></a>Azure Database for PostgreSQL como receptor

Para copiar datos en Azure Database for PostgreSQL, se admiten las siguientes propiedades en la sección de **receptor** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad "type" del receptor de la actividad de copia debe establecerse en **AzurePostgreSQLSink**. | Sí |
| preCopyScript | Especifique una consulta de SQL para que la actividad de copia se ejecute antes de escribir datos en Azure Database for PostgreSQL en cada ejecución. Puede usar esta propiedad para limpiar los datos cargados previamente. | No |
| writeBatchSize | Inserta datos en la tabla de Azure Database for PostgreSQL cuando el tamaño de búfer alcanza el valor de writeBatchSize.<br>El valor permitido es un entero que representa el número de filas. | No (el valor predeterminado es 10 000) |
| writeBatchTimeout | Tiempo de espera para que la operación de inserción por lotes se complete antes de que se agote el tiempo de espera.<br>Los valores permitidos son las cadenas de intervalo de tiempo. Un ejemplo es 00:30:00 (30 minutos). | No (el valor predeterminado es 00:00:30) |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyToAzureDatabaseForPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure PostgreSQL output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzurePostgreSQLSink",
                "preCopyScript": "<custom SQL script>",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener más información sobre las propiedades, consulte [Actividad de búsqueda en Azure Data Factory](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
