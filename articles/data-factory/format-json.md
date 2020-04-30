---
title: Formato JSON en Azure Data Factory
description: En este tema se describe cómo tratar el formato JSON en Azure Data Factory.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: jingwang
ms.openlocfilehash: 7b554ea5c2868559574979c58697fd31f8d2a2c4
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686278"
---
# <a name="json-format-in-azure-data-factory"></a>Formato JSON en Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Siga este artículo cuando quiera **analizar los archivos JSON o escribir los datos en formato JSON**. 

El formato JSON es compatible con los conectores siguientes: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [File System](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md) y [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades compatibles con el conjunto de datos de JSON.

| Propiedad         | Descripción                                                  | Obligatorio |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | La propiedad type del conjunto de datos debe establecerse en **Json**. | Sí      |
| ubicación         | Configuración de ubicación de los archivos. Cada conector basado en archivos tiene su propio tipo de ubicación y propiedades compatibles en `location`. **Vea los detalles en el artículo de conectores -> sección de propiedades del conjunto de datos**. | Sí      |
| encodingName     | El tipo de codificación usado para leer y escribir archivos de prueba. <br>Los valores permitidos son los siguientes: "UTF-8", "UTF-16", "UTF-16BE", "UTF-32", "UTF-32BE", "US-ASCII", "UTF-7", "BIG5", "EUC-JP", "EUC-KR", "GB2312", "GB18030", "JOHAB", "SHIFT-JIS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", "IBM737", "IBM775", "IBM850", "IBM852", "IBM855", "IBM857", "IBM860", "IBM861", "IBM863", "IBM864", "IBM865", "IBM869", "IBM870", "IBM01140", "IBM01141", "IBM01142", "IBM01143", "IBM01144", "IBM01145", "IBM01146", "IBM01147", "IBM01148", "IBM01149", "ISO-2022-JP", "ISO-2022-KR", "ISO-8859-1", "ISO-8859-2", "ISO-8859-3", "ISO-8859-4", "ISO-8859-5", "ISO-8859-6", "ISO-8859-7", "ISO-8859-8", "ISO-8859-9", "ISO-8859-13", "ISO-8859-15", "WINDOWS-874", "WINDOWS-1250", "WINDOWS-1251", "WINDOWS-1252", "WINDOWS-1253", "WINDOWS-1254", "WINDOWS-1255", "WINDOWS-1256", "WINDOWS-1257", "WINDOWS-1258".| No       |
| compression | Grupo de propiedades para configurar la compresión de archivo. Configure esta sección si desea realizar la compresión o descompresión durante la ejecución de la actividad. | No |
| type | El códec de compresión usado para leer y escribir archivos JSON. <br>Los valores permitidos son **bzip2**, **gzip**, **deflate**, **ZipDeflate**, **snappy** o **lz4** para usar al guardar el archivo. La opción predeterminada no se comprime.<br>**Tenga en cuenta** que actualmente la actividad de copia no admite "snappy" ni "lz4", y que el flujo de datos de asignación no admite "ZipDeflate".<br>**Tenga en cuenta** que cuando utilice la actividad de copia para descomprimir archivos ZipDeflate y escribir en el almacén de datos de receptores basado en archivos, los archivos se extraerán a la carpeta `<path specified in dataset>/<folder named as source zip file>/`. | No.  |
| level | La razón de compresión. <br>Los valores permitidos son **Optimal** o **Fastest**.<br>- **Fastest:** la operación de compresión debe completarse tan pronto como sea posible, incluso si el archivo resultante no se comprime de forma óptima.<br>- **Optimal**: la operación de compresión se debe comprimir óptimamente, incluso si tarda más tiempo en completarse. Para más información, consulte el tema [Nivel de compresión](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) . | No       |

A continuación, se muestra un ejemplo de un conjunto de datos de JSON en Azure Blob Storage:

```json
{
    "name": "JSONDataset",
    "properties": {
        "type": "Json",
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
            "compression": {
                "type": "gzip"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el receptor y el origen de JSON.

### <a name="json-as-source"></a>JSON como origen

En la sección ***\*source\**** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad      | Descripción                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | La propiedad type del origen de la actividad de copia debe establecerse en: **JSONSource**. | Sí      |
| storeSettings | Un grupo de propiedades sobre cómo leer datos de un almacén de datos. Cada conector basado en archivos tiene su propia configuración de lectura admitida en `storeSettings`. **Vea los detalles en el artículo de conectores -> sección de propiedades de la actividad de copia**. | No       |

### <a name="json-as-sink"></a>JSON como receptor

En la sección ***\*sink\**** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad      | Descripción                                                  | Obligatorio |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | La propiedad type del origen de la actividad de copia debe establecerse en **JSONSink**. | Sí      |
| formatSettings | Un grupo de propiedades. Consulte la tabla **Configuración de escritura de JSON** a continuación. | No       |
| storeSettings | Un grupo de propiedades sobre cómo escribir datos en un almacén de datos. Cada conector basado en archivos tiene su propia configuración de escritura admitida en `storeSettings`. **Vea los detalles en el artículo de conectores -> sección de propiedades de la actividad de copia**. | No       |

**Configuración de escritura de JSON** compatible en `formatSettings`:

| Propiedad      | Descripción                                                  | Obligatorio                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | La propiedad type de formatSettings debe establecerse en **JsonWriteSettings**. | Sí                                                   |
| filePattern |Indica el patrón de los datos almacenados en cada archivo JSON. Estos son los valores permitidos: **setOfObjects** y **arrayOfObjects**. El valor **predeterminado** es **setOfObjects**. Consulte la sección [patrones de archivo JSON](#json-file-patterns) para obtener más información acerca de estos patrones. |No |

### <a name="json-file-patterns"></a>Patrones de archivo JSON

La actividad de copia puede detectar y analizar automáticamente los siguientes patrones de archivos JSON. 

- **Tipo I: setOfObjects**

    Cada archivo contiene un único objeto o bien varios objetos concatenados/delimitados por líneas. 
    Si se elige esta opción en el receptor de la actividad de copia, la actividad de copia genera un único archivo JSON con cada objeto por línea (delimitado por líneas).

    * **ejemplo de JSON de objeto único**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **ejemplo de JSON delimitado por líneas**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **ejemplo de JSON concatenado**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Tipo II: arrayOfObjects**

    Cada archivo contiene una matriz de objetos.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

## <a name="mapping-data-flow-properties"></a>Propiedades de Asignación de instancias de Data Flow

Los tipos de archivo JSON se pueden usar como receptor y origen en el flujo de datos de asignación.

### <a name="creating-json-structures-in-a-derived-column"></a>Creación de estructuras JSON en una columna derivada

Puede agregar una columna compleja al flujo de datos mediante el generador de expresiones de columna derivada. En la transformación de columna derivada, agregue una nueva columna y abra el generador de expresiones haciendo clic en el cuadro azul. Para que una columna sea compleja, puede escribir la estructura JSON manualmente o usar la experiencia de usuario para agregar subcolumnas de forma interactiva.

#### <a name="using-the-expression-builder-ux"></a>Uso de la experiencia de usuario del generador de expresiones

En el panel lateral del esquema de salida, mantenga el puntero sobre una columna y haga clic en el icono de signo más. Seleccione **Agregar subcolumna** para convertir la columna en un tipo complejo.

![Add subcolumn (Agregar subcolumna)](media/data-flow/addsubcolumn.png "Agregar subcolumna")

Puede agregar columnas y subcolumnas adicionales de la misma manera. Para cada campo no complejo, se puede agregar una expresión en el editor de expresiones a la derecha.

![Columna compleja](media/data-flow/complexcolumn.png "Columna compleja")

#### <a name="entering-the-json-structure-manually"></a>Introducción manual de la estructura JSON

Para agregar manualmente una estructura JSON, agregue una nueva columna y escriba la expresión en el editor. La expresión tiene el siguiente formato general:

```
@(
    field1=0,
    field2=@(
        field1=0
    )
)
```

Si esta expresión se especificara para una columna denominada "complexColumn", se escribiría en el receptor como el siguiente JSON:

```
{
    "complexColumn": {
        "field1": 0,
        "field2": {
            "field1": 0
        }
    }
}
```

#### <a name="sample-manual-script-for-complete-hierarchical-definition"></a>Script manual de ejemplo para una definición jerárquica completa
```
@(
    title=Title,
    firstName=FirstName,
    middleName=MiddleName,
    lastName=LastName,
    suffix=Suffix,
    contactDetails=@(
        email=EmailAddress,
        phone=Phone
    ),
    address=@(
        line1=AddressLine1,
        line2=AddressLine2,
        city=City,
        state=StateProvince,
        country=CountryRegion,
        postCode=PostalCode
    ),
    ids=[
        toString(CustomerID), toString(AddressID), rowguid
    ]
)
```

### <a name="source-format-options"></a>Opciones de formato de origen

El uso de un conjunto de datos JSON como origen en el flujo de datos le permite establecer cinco opciones de configuración adicionales. Esta configuración se puede encontrar en el acordeón **Configuración de JSON** en la pestaña **Opciones de origen**.  

![Configuración de JSON](media/data-flow/json-settings.png "Configuración de JSON")

#### <a name="default"></a>Valor predeterminado

De forma predeterminada, los datos JSON se leen en el formato siguiente.

```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

#### <a name="single-document"></a>Documento único

Si se selecciona **Documento único**, los flujos de datos de asignación leen un documento JSON de cada archivo. 

``` json
File1.json
{
    "json": "record 1"
}
File2.json
{
    "json": "record 2"
}
File3.json
{
    "json": "record 3"
}
```
> [!NOTE]
> Si los flujos de datos producen un error que indica "corrupt_record" al obtener una vista previa de los datos JSON, es probable que los datos contengan un único documento en el archivo JSON. Si establece la opción "Documento único", debería desaparecer ese error.

#### <a name="unquoted-column-names"></a>Nombres de columnas sin comillas

Si se selecciona **Nombres de columnas sin comillas**, los flujos de datos de asignación leen columnas JSON que no están entre comillas. 

```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

#### <a name="has-comments"></a>Tiene comentarios

Seleccione **Tiene comentarios** si los datos JSON tienen comentarios de estilo C o C++.

``` json
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

#### <a name="single-quoted"></a>Con comillas simples

Seleccione **Con comillas simples** si los valores y campos JSON usan comillas simples en lugar de comillas dobles.

```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

#### <a name="backslash-escaped"></a>Barra diagonal inversa con escape

Seleccione **Barra diagonal inversa con escape** si se usan barras diagonales inversas como caracteres de escape en los datos JSON.

```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

## <a name="next-steps"></a>Pasos siguientes

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Asignación de Data Flow](concepts-data-flow-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
