---
title: Formato Parquet en Azure Data Factory
description: En este tema se describe cómo tratar con el formato Parquet en Azure Data Factory.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 223b1b996b82acaa753eb55723e251dc5901bbec
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417715"
---
# <a name="parquet-format-in-azure-data-factory"></a>Formato Parquet en Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Siga este artículo cuando quiera **analizar los archivos Parquet o escribir los datos en formato Parquet**. 

El formato Parquet se admite para los conectores siguientes: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [File System](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md) y [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Parquet.

| Propiedad         | Descripción                                                  | Obligatorio |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | La propiedad type del conjunto de datos debe establecerse en **Parquet**. | Sí      |
| ubicación         | Configuración de ubicación de los archivos. Cada conector basado en archivos tiene su propio tipo de ubicación y propiedades compatibles en `location`. **Vea los detalles en el artículo de conectores -> sección de propiedades del conjunto de datos**. | Sí      |
| compressionCodec | El códec de compresión que se usará al escribir en archivos Parquet. Al realizar la lectura desde archivos Parquet, las instancias de Data Factory determinan automáticamente el códec de compresión basado en los metadatos del archivo.<br>Los tipos admitidos son "**none**","**gzip**","**snappy**" (predeterminado) y "**lzo**". Tenga en cuenta que la actividad de copia no es compatible actualmente con LZO cuando hay archivos Parquet de lectura y escritura. | No       |

> [!NOTE]
> No se admiten espacios en blanco en el nombre de columna de los archivos Parquet.

A continuación encontrará un ejemplo de un conjunto de datos de Parquet en Azure Blob Storage:

```json
{
    "name": "ParquetDataset",
    "properties": {
        "type": "Parquet",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admiten el receptor y el origen Parquet.

### <a name="parquet-as-source"></a>Parquet como origen

En la sección ***\*source\**** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad      | Descripción                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | La propiedad type del origen de la actividad de copia debe establecerse en **ParquetSource**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo leer datos de un almacén de datos. Cada conector basado en archivos tiene su propia configuración de lectura admitida en `storeSettings`. **Vea los detalles en el artículo de conectores -> sección de propiedades de la actividad de copia**. | No       |

### <a name="parquet-as-sink"></a>Parquet como receptor

En la sección ***\*sink\**** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad      | Descripción                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | La propiedad type del origen de la actividad de copia debe establecerse en **ParquetSink**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo escribir datos en un almacén de datos. Cada conector basado en archivos tiene su propia configuración de escritura admitida en `storeSettings`. **Vea los detalles en el artículo de conectores -> sección de propiedades de la actividad de copia**. | No       |

## <a name="mapping-data-flow-properties"></a>Propiedades de Asignación de instancias de Data Flow

Obtenga información detallada de la [transformación de origen](data-flow-source.md) y la [transformación de receptor](data-flow-sink.md) en la asignación de flujo de datos.

## <a name="data-type-support"></a>Compatibilidad con tipos de datos

Actualmente no se admiten los tipos de datos complejos de Parquet (por ejemplo, MAP, LIST, STRUCT).

## <a name="using-self-hosted-integration-runtime"></a>Uso del entorno de ejecución de integración autohospedado

> [!IMPORTANT]
> En el caso de las copias autorizadas por el entorno de ejecución de integración (IR) autohospedado (por ejemplo, entre almacenes de datos locales y almacenes de datos en la nube), si no va a copiar archivos ORC **tal y como están**, tendrá que instalar **JRE (Java Runtime Environment) 8 de 64 bits u OpenJDK** y el **paquete Microsoft Visual C++ 2010 Redistributable** en la máquina de IR. Consulte el párrafo siguiente para más información.

En el caso de las copias que se ejecutan en el IR autohospedado con la serialización o deserialización de archivos Parquet, para encontrar el entorno de ejecución de Java, ADF consulta primero el Registro *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* de JRE; si no lo encuentra, comprueba la variable del sistema *`JAVA_HOME`* de OpenJDK.

- **Para usar JRE**: el IR de 64 bits necesita JRE de 64 bits. Puede encontrarlo [aquí](https://go.microsoft.com/fwlink/?LinkId=808605).
- **Para usar OpenJDK**: se admite desde la versión 3.13 de IR. Empaquete jvm.dll con todos los demás ensamblados de OpenJDK necesarios en la máquina del IR autohospedado y establezca la variable de entorno del sistema JAVA_HOME en el valor que corresponda.
- **Para instalar el paquete Visual C++ 2010 Redistributable**: el paquete Visual C++ 2010 Redistributable no se instala con las instalaciones de IR autohospedadas. Puede encontrarlo [aquí](https://www.microsoft.com/download/details.aspx?id=14632).

> [!TIP]
> Si copia datos desde o hacia Parquet mediante Integration Runtime autohospedado y recibe un error que indica que "Se produjo un error al invocar Java, mensaje: **Espacio en el montón java.lang.OutOfMemoryError:Java**", puede agregar una variable de entorno `_JAVA_OPTIONS` en la máquina que hospeda IR autohospedado para ajustar el tamaño del montón mínimo y máximo para JVM a fin de facilitar dicha copia y, a continuación, volver a ejecutar la canalización.

![Establecimiento del tamaño del montón JVM en IR autohospedado](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Ejemplo: establecimiento de la variable `_JAVA_OPTIONS` con el valor `-Xms256m -Xmx16g`. La marca `Xms` especifica el grupo de asignación de memoria inicial para una máquina virtual Java (JVM), mientras que `Xmx` especifica el grupo de asignación de memoria máxima. Esto significa que JVM se iniciará con la cantidad de memoria `Xms` y podrá utilizar `Xmx` como máximo. De forma predeterminada, ADF usa 64 MB como mínimo y 1 GB como máximo.

## <a name="next-steps"></a>Pasos siguientes

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Asignación de Data Flow](concepts-data-flow-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
