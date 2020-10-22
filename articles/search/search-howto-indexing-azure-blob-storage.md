---
title: Configuración de un indexador de blobs
titleSuffix: Azure Cognitive Search
description: Configure un indexador de blobs de Azure para automatizar la indexación de contenido de blobs en las operaciones de búsqueda de texto completo en Azure Cognitive Search.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: e3419711c9a7358914f85574f6dbd5af29def1cf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91403624"
---
# <a name="how-to-configure-a-blob-indexer-in-azure-cognitive-search"></a>Configuración de un indexador de blobs en Azure Cognitive Search

En este artículo se muestra cómo usar Azure Cognitive Search para indexar documentos de texto (como archivos PDF, documentos de Microsoft Office y otros formatos comunes) almacenados en Azure Blob Storage. En primer lugar, se explican los conceptos básicos de cómo instalar y configurar un indizador de blobs. A continuación, se ofrecen más detalles sobre los comportamientos y escenarios que es probable que encuentre.

<a name="SupportedFormats"></a>

## <a name="supported-formats"></a>Formatos compatibles

El indexador de blob puede extraer texto de los siguientes formatos de documento:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="set-up-blob-indexing"></a>Configuración de la indexación de blobs

Puede configurar un indexador de Azure Blob Storage utilizando:

* [Azure Portal](https://ms.portal.azure.com)
* [API REST](/rest/api/searchservice/Indexer-operations) de Azure Cognitive Search
* [SDK para .NET](/dotnet/api/overview/azure/search) de Azure Cognitive Search

> [!NOTE]
> Algunas características (por ejemplo, las asignaciones de campos) aún no están disponibles en el portal y tienen que usarse mediante programación.
>

En este caso, demostramos el flujo mediante la API de REST.

### <a name="step-1-create-a-data-source"></a>Paso 1: Creación de un origen de datos

Un origen de datos especifica los datos que se deben indexar, las credenciales necesarias para obtener acceso a estos y las directivas para identificar cambios en los datos de forma eficaz (filas nuevas, modificadas o eliminadas). Varios indexadores pueden usar el mismo origen de datos en el mismo servicio de búsqueda.

Para realizar la indexación de blobs, el origen de datos debe tener las siguientes propiedades obligatorias:

* **name** es el nombre único del origen de datos dentro del servicio de búsqueda.
* **type** debe ser `azureblob`.
* **las credenciales proporcionan la conexión de la cuenta de almacenamiento en forma del parámetro `credentials.connectionString`. Consulte [Especificación de credenciales](#Credentials) a continuación para más información.
* **container** especifica un contenedor en la cuenta de almacenamiento. De manera predeterminada, se pueden recuperar todos los blobs dentro del contenedor. Si solo desea indexar blobs en un directorio virtual determinado, puede especificar ese directorio. Para ello, use el parámetro opcional **query**.

Pasos para crear un origen de datos:

```http
    POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }
```

Para más información sobre la API de creación de origen de datos, consulte [Crear origen de datos](/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>

#### <a name="how-to-specify-credentials"></a>Especificación de credenciales

Puede proporcionar las credenciales para el contenedor de blobs de una de estas maneras:

* **Cadena de conexión de identidad administrada**: `ResourceId=/subscriptions/<your subscription ID>/resourceGroups/<your resource group name>/providers/Microsoft.Storage/storageAccounts/<your storage account name>/;` 

  Esta cadena de conexión no requiere una clave de cuenta, pero es preciso seguir las instrucciones de [Configuración de una conexión a una cuenta de Azure Storage mediante una identidad administrada](search-howto-managed-identities-storage.md).

* **Cadena de conexión de la cuenta de almacenamiento de acceso total**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`

  Para obtener la cadena de conexión del portal de Azure, vaya a la hoja de la cuenta de almacenamiento > Configuración > Claves (para las cuentas de almacenamiento del modelo clásico) o Configuración > Claves de acceso (para las cuentas de almacenamiento de Azure Resource Manager).

* Cadena de conexión de la **firma de acceso compartido de la cuenta de almacenamiento** (SAS): `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl`

  La firma de acceso compartido debe tener permisos de enumeración y lectura sobre los contenedores y objetos (en este caso, los blobs).

* **Firma de acceso compartido de contenedor**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl`

  La firma de acceso compartido debe tener permisos de enumeración y lectura sobre el contenedor.

Para más información sobre las firmas de acceso compartido de almacenamiento, consulte [Uso de firmas de acceso compartido (SAS)](../storage/common/storage-sas-overview.md).

> [!NOTE]
> Si usa credenciales SAS, deberá actualizar las credenciales del origen de datos periódicamente con firmas renovadas para evitar que caduquen. Si las credenciales de SAS expiran, el indexador producirá un mensaje de error similar a `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Paso 2: Creación de un índice

El índice especifica los campos de un documento, los atributos y otras construcciones que conforman la experiencia de búsqueda.

Aquí se muestra cómo crear un índice con un campo `content` utilizable en búsquedas para almacenar el texto extraído de los blobs:

```http
    POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }
```

Para más información, consulte [Creación de un índice (API REST)](/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>Paso 3: Creación de un indexador

Un indizador conecta un origen de datos con un índice de búsqueda de destino y proporciona una programación para automatizar la actualización de datos.

Una vez creados el origen de datos y los índices, ya podrá crear el indizador:

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }
```

Este indizador se ejecutará cada dos horas (el intervalo de programación se establece en PT2H). Para ejecutar un indizador cada 30 minutos, establézcalo en PT30M. El intervalo más breve que se admite es de 5 minutos. La programación es opcional: si se omite, el indizador solo se ejecuta una vez cuando se crea. Sin embargo, puede ejecutarlo a petición en cualquier momento.   

Para más información, consulte [Creación de un indexador (API REST)](/rest/api/searchservice/create-indexer). Para más información sobre cómo definir las programaciones del indexador, consulte [Programación de indexadores para Azure Cognitive Search](search-howto-schedule-indexers.md).

<a name="how-azure-search-indexes-blobs"></a>

## <a name="how-blobs-are-indexed"></a>Cómo se indexan los blobs

En función de la [configuración del indizador](#PartsOfBlobToIndex), el indizador de blob puede indizar solo los metadatos de almacenamiento (resulta útil cuando solo le interesan los metadatos y no necesita indizar el contenido de los blobs), los metadatos de almacenamiento y contenido, o bien el contenido textual y los metadatos. De forma predeterminada, el indizador extrae los metadatos y el contenido.

> [!NOTE]
> De forma predeterminada, se indexan blobs con contenido estructurado como JSON o CSV como un único fragmento de texto. Si desea indexar blobs CSV y JSON de forma estructurada, consulte [Indexación de blobs JSON](search-howto-index-json-blobs.md) e [Indexación de blobs CSV](search-howto-index-csv-blobs.md) para más información.
>
> Un documento compuesto o insertado (por ejemplo, un archivo ZIP o un documento de Word con correo electrónico de Outlook insertado que contiene datos adjuntos, o un archivo .MSG con datos adjuntos) también se indexa como un solo documento. Por ejemplo, todas las imágenes extraídas de los datos adjuntos de un archivo MSG se devolverán en el campo normalized_images.

* Se extrae el contenido textual en un campo de cadena denominado "`content`".

  > [!NOTE]
  > Azure Cognitive Search limita la cantidad de texto que extrae según el plan de tarifa: 32 000 caracteres en el nivel Gratis, 64 000 en Básico, 4 millones en Estándar, 8 millones en Estándar S2 y 16 millones en Estándar S3. Se incluye una advertencia en la respuesta de estado del indexador para documentos truncados.  

* Las propiedades de metadatos especificadas por el usuario en el blob, si las hubiera, se extraen textualmente. Tenga en cuenta que esto requiere que se defina un campo en el índice con el mismo nombre que la clave de metadatos del blob. Por ejemplo, si el blob tiene una clave de metadatos de `Sensitivity` con el valor `High`, debe definir un campo denominado `Sensitivity` en el índice de búsqueda y se rellenará con el valor `High`.

* Las propiedades de metadatos de blob estándar se extraen en los campos siguientes:

  * **metadata\_storage\_name** (Edm.String): Nombre de archivo del blob. Por ejemplo, si tiene un blob /my-container/my-folder/subfolder/resume.pdf, el valor de este campo es `resume.pdf`.

  * **metadata\_storage\_path**: El identificador URI completo del blob, incluida la cuenta de almacenamiento. Por ejemplo: `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`

  * **metadata\_storage\_content\_type** (Edm.String): Tipo de contenido tal como especifica el código que usó para cargar el blob. Por ejemplo, `application/octet-stream`.

  * **metadata\_storage\_last\_modified** (Edm.DateTimeOffset): La última marca de tiempo modificada para el blob. Azure Cognitive Search emplea esta marca de tiempo para identificar los blobs modificados a fin de evitar volver a indexar todo después de la indexación inicial.

  * **metadata\_storage\_size** (Edm.Int64): Tamaño del blob en bytes.

  * **metadata\_storage\_content\_md5** (Edm.String): Hash MD5 del contenido del blob, si está disponible.

  * **metadata\_storage\_sas\_token** (Edm.String): un token de SAS temporal que pueden utilizar las [aptitudes personalizadas](cognitive-search-custom-skill-interface.md) para obtener acceso al blob. Este token no se debe almacenar para su uso posterior, ya que podría expirar.

* Se extraen las propiedades de metadatos específicos de cada formato de documento en los campos enumerados [aquí](#ContentSpecificMetadata).

No es necesario definir campos para todas las propiedades anteriores en el índice de búsqueda, capture solo las propiedades que necesita para la aplicación.

> [!NOTE]
> A menudo, los nombres de campo en el índice existente serán diferentes de los nombres de campo generados durante la extracción del documento. Puede usar **asignaciones de campos** para asignar los nombres de propiedad proporcionados en Azure Cognitive Search a los nombres de campo en el índice de búsqueda. A continuación, verá un ejemplo del uso de las asignaciones de campos.
>

<a name="DocumentKeys"></a>

### <a name="defining-document-keys-and-field-mappings"></a>Definición de claves de documento y asignaciones de campos

En Azure Cognitive Search, la clave del documento identifica de forma exclusiva a un documento. Cada índice de búsqueda tiene que tener exactamente un campo de clave del tipo Edm.String. El campo de clave es necesario para cada documento que se va a agregar al índice (de hecho, es el único campo obligatorio).  

Debe considerar detenidamente qué campo extraído se debe asignar al campo de clave para el índice. Los candidatos son:

* **metadata\_storage\_name**: Este podría ser un candidato conveniente, pero tenga en cuenta que 1) los nombres podrían no ser exclusivos, ya que puede que tenga blobs con el mismo nombre en carpetas diferentes y 2) el nombre puede contener caracteres que no son válidos para las claves de documento, como los guiones. Para trabajar con caracteres no válidos, utilice la [función de asignación de campos](search-indexer-field-mappings.md#base64EncodeFunction)`base64Encode`; si lo hace, no olvide codificar las claves de documento al usarlas en llamadas a API, como Lookup. (Por ejemplo, en .NET puede usar el [método UrlTokenEncode](/dotnet/api/system.web.httpserverutility.urltokenencode) para ese fin).

* **metadata\_storage\_path**: El uso de la ruta de acceso completa garantiza la exclusividad, pero la ruta de acceso contiene siempre caracteres `/` que [no son válidos en una clave de documento](/rest/api/searchservice/naming-rules).  Como anteriormente, tiene la opción de codificar las claves mediante la [función](search-indexer-field-mappings.md#base64EncodeFunction)`base64Encode`.

* Una tercera opción es agregar una propiedad de metadatos personalizados a los blobs. Sin embargo, esta opción requiere que el proceso de carga de blobs agregue dicha propiedad a todos los blobs. Dado que la clave es una propiedad obligatoria, todos los blobs que no tengan esa propiedad no se indexarán.

> [!IMPORTANT]
> Si no hay ninguna asignación explícita para el campo de clave en el índice, Azure Cognitive Search usa automáticamente `metadata_storage_path` como clave y valores de clave de codificaciones Base64 (la segunda opción anterior).
>
>

En este ejemplo, vamos a seleccionar el campo `metadata_storage_name` como clave de documento. Supongamos también que el índice tiene un campo de clave denominado `key` y un campo `fileSize` para almacenar el tamaño del documento. Para conectar las cosas como desee, especifique las siguientes asignaciones de campo al crear o actualizar el indexador:

```http
    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]
```

Para conectar todo esto, aquí está la forma de agregar asignaciones de campo y habilitar la codificación de base 64 para las claves para un indexador ya existente:

```http
    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }
```

Para más información, consulte el artículo sobre [transformaciones y asignaciones de campos](search-indexer-field-mappings.md).

#### <a name="what-if-you-need-to-encode-a-field-to-use-it-as-a-key-but-you-also-want-to-search-it"></a>¿Qué ocurre si necesita codificar un campo para usarlo como clave, pero también desea realizar búsquedas en él?

Hay ocasiones en que se necesita usar una versión codificada de un cambio como metadata_storage_path como clave, pero también es preciso poder realizar búsquedas en ese campo (sin codificar). Para resolver este problema, puede asignarlo a dos campos; uno que se usará para la clave y otro que se usará para realizar búsquedas. En el ejemplo siguiente, el campo *key* contiene la ruta de acceso codificada, mientras que el campo *path* no está codificado y se usará en el índice como campo en el que se pueden realizar búsquedas.

```http
    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_path", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_path", "targetFieldName" : "path" }
      ]
    }
```
<a name="WhichBlobsAreIndexed"></a>

## <a name="index-by-file-type"></a>Índice por tipo de archivo

Puede controlar qué blobs se indizan y cuáles se pasan por alto.

### <a name="include-blobs-having-specific-file-extensions"></a>Inclusión de blobs con extensiones de archivo concretas

Puede indexar solo los blobs con las extensiones de nombre de archivo que especifique mediante el parámetro de configuración de indexador `indexedFileNameExtensions`. El valor es una cadena que contiene una lista separada por comas de extensiones de archivo (con un punto inicial). Por ejemplo, para indexar solamente los blobs .PDF y .DOCX, realice el siguiente procedimiento:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }
```

### <a name="exclude-blobs-having-specific-file-extensions"></a>Exclusión de blobs con extensiones de archivo concretas

Puede excluir blobs con extensiones de nombre de archivo específicas de la indexación mediante el parámetro de configuración `excludedFileNameExtensions`. El valor es una cadena que contiene una lista separada por comas de extensiones de archivo (con un punto inicial). Por ejemplo, para indexar todos los blobs excepto aquellos con las extensiones .PNG y .JPEG, realice el siguiente procedimiento:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }
```

Si los dos parámetros `indexedFileNameExtensions` y `excludedFileNameExtensions` existen, Azure Cognitive Search mira primero en `indexedFileNameExtensions` y, luego, en `excludedFileNameExtensions`. Esto significa que si la misma extensión de archivo está presente en las dos listas, se excluirá de la indexación.

<a name="PartsOfBlobToIndex"></a>

## <a name="index-parts-of-a-blob"></a>Índice de partes de un blob

Puede controlar qué partes de los blobs se indizan mediante el parámetro de configuración `dataToExtract`. Puede tomar los siguientes valores:

* `storageMetadata`: especifica que solamente se indizan las [propiedades de blob estándar y los metadatos especificados por el usuario](../storage/blobs/storage-blob-container-properties-metadata.md).
* `allMetadata`: especifica que se indizan los metadatos almacenamiento y los [metadatos específicos del tipo de contenido](#ContentSpecificMetadata) extraídos del contenido del blob.
* `contentAndMetadata`: especifica que se indizan todos los metadatos y el contenido textual extraído del blob. Este es el valor predeterminado.

Por ejemplo, para indizar solo los metadatos de almacenamiento, use lo siguiente:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }
```

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Uso de metadatos del blob para controlar cómo se indizan los blobs

Los parámetros de configuración que se han descrito anteriormente se aplican a todos los blobs. En ocasiones, puede que desee para controlar cómo indizar *blobs concretos*. Puede hacerlo agregando los siguientes valores y propiedades de metadatos de blobs:

| Nombre de propiedad | Valor de propiedad | Explicación |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Indica al indizador de blob que pase completamente el blob. No se trata de realizar la extracción de metadatos ni del contenido. Esto es útil cuando se produce un error repetidamente y se interrumpe el proceso de indización de un blob determinado. |
| AzureSearch_SkipContent |"true" |Esto es equivalente a la configuración `"dataToExtract" : "allMetadata"` descrita [anteriormente](#PartsOfBlobToIndex) en el ámbito de un blob determinado. |

## <a name="index-from-multiple-sources"></a>Índice de varios orígenes

Es posible que quiera "ensamblar" documentos de varios orígenes en el índice. Por ejemplo, puede que quiera combinar texto de blobs con otros metadatos almacenados en Cosmos DB. Incluso puede usar la API de indexación de inserción junto con varios indexadores para crear documentos de búsqueda a partir de varias partes.

Para que funcione, todos los indexadores y demás componentes deben coincidir con la clave del documento. Para más información sobre este tema, consulte el tutorial sobre la [indexación de varios orígenes de datos de Azure](./tutorial-multiple-data-sources.md) o la entrada de blog que versa sobre la [combinación de documentos con otros datos en Azure Cognitive Search](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

## <a name="index-large-datasets"></a>Indexación de conjuntos de datos grandes

La indización de blobs puede ser un proceso lento. En los casos donde hay millones de blobs que se van a indizar, puede acelerar la indización dividiendo los datos y utilizando varios indizadores para procesar los datos en paralelo. Le mostramos cómo puede configurar esta opción:

* Divida los datos en varios contenedores de blobs o carpetas virtuales.

* Configure varios orígenes de datos de Azure Cognitive Search, uno por cada contenedor o carpeta. Para seleccionar una carpeta de blobs, use el parámetro `query`:

    ```json
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

* Cree un indizador correspondiente a cada origen de datos. Todos los indizadores pueden apuntar al mismo índice de búsqueda de destino.  

* Una unidad de búsqueda del servicio puede ejecutar un indexador en cualquier momento dado. La creación de varios indexadores como se ha descrito arriba solo es útil si se ejecutan realmente en paralelo. Para ejecutar varios indexadores en paralelo, escale horizontalmente el servicio de búsqueda mediante la creación de un número adecuado de particiones y réplicas. Por ejemplo, si el servicio de búsqueda tiene 6 unidades de búsqueda (por ejemplo, 2 particiones x 3 réplicas), se pueden ejecutar 6 indexadores simultáneamente, lo que produce una multiplicación por seis del rendimiento de indexación. Para más información sobre el escalado y la planificación de capacidad, consulte [Ajuste de la capacidad de un servicio de Azure Cognitive Search](search-capacity-planning.md).

<a name="DealingWithErrors"></a>

## <a name="handle-errors"></a>Control de errores

De forma predeterminada, el indizador de blob se detiene cuando encuentra un blob con un tipo de contenido no admitido (por ejemplo, una imagen). Por supuesto, puede usar el parámetro `excludedFileNameExtensions` para omitir determinados tipos de contenido. Sin embargo, puede que necesite indizar blobs sin conocer de antemano todos los posibles tipos de contenido. Para reanudar la indización cuando se encuentra un tipo de contenido no admitido, establezca el `failOnUnsupportedContentType` parámetro de configuración en `false`:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }
```

En algunos blobs, Azure Cognitive Search no puede determinar el tipo de contenido o no puede procesar un documento de otro tipo de contenido admitido. Para ignorar este modo de error, establezca el parámetro de configuración `failOnUnprocessableDocument` en false:

```http
      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }
```

Azure Cognitive Search limita el tamaño de los blobs que se indexan. Estos límites se documentan en [Límites de servicio en Azure Cognitive Search](./search-limits-quotas-capacity.md). Los blobs demasiado grandes se tratan como errores de forma predeterminada. Sin embargo, puede indexar los metadatos de almacenamiento de blobs demasiado grandes si define el parámetro de configuración `indexStorageMetadataOnlyForOversizedDocuments` como true:

```http
    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }
```

También puede continuar con la indexación si se producen errores en cualquier punto del procesamiento, mientras se analizan blobs o se agregan documentos a un índice. Para omitir un número específico de errores, establezca los parámetros de configuración `maxFailedItems` y `maxFailedItemsPerBatch` en los valores deseados. Por ejemplo:

```http
    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }
```

<a name="ContentSpecificMetadata"></a>

## <a name="content-type-specific-metadata-properties"></a>Propiedades de metadatos específicas del tipo de contenido

En la tabla siguiente se resume el procesamiento que se realiza para cada formato de documento y se describen las propiedades de metadatos extraídas mediante Azure Cognitive Search.

| Formato de documento/Tipo de contenido | Metadatos extraídos | Detalles de procesamiento |
| --- | --- | --- |
| HTML (text/html) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Seccionar el marcado HTML y extraer texto |
| PDF (application/pdf) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Extraer texto, incluyendo los documentos insertados (excepto las imágenes) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extraer texto, incluyendo los documentos insertados |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extraer texto, incluyendo los documentos insertados |
| DOCM (application/vnd.ms-word.document.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extraer texto, incluyendo los documentos insertados |
| WORD XML (application/vnd.ms-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Seccionar el marcado XML y extraer texto |
| WORD 2003 XML (application/vnd.ms-wordml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |Seccionar el marcado XML y extraer texto |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extraer texto, incluyendo los documentos insertados |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extraer texto, incluyendo los documentos insertados |
| XLSM (application/vnd.ms-excel.sheet.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extraer texto, incluyendo los documentos insertados |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extraer texto, incluyendo los documentos insertados |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extraer texto, incluyendo los documentos insertados |
| PPTM (application/vnd.ms-powerpoint.presentation.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Extraer texto, incluyendo los documentos insertados |
| MSG (application/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Extraer texto, incluyendo el texto extraído de los datos adjuntos. `metadata_message_to_email`, `metadata_message_cc_email` y `metadata_message_bcc_email` son colecciones de cadenas, el resto de los campos son cadenas.|
| ODT (application/vnd.oasis.opendocument.text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Extraer texto, incluyendo los documentos insertados |
| ODS (application/vnd.oasis.opendocument.spreadsheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Extraer texto, incluyendo los documentos insertados |
| ODP (application/vnd.oasis.opendocument.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Extraer texto, incluyendo los documentos insertados |
| ZIP (application/zip) |`metadata_content_type` |Extraer el texto de todos los documentos en el archivo |
| GZ (application/gzip) |`metadata_content_type` |Extraer el texto de todos los documentos en el archivo |
| EPUB (application/epub+zip) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Extraer el texto de todos los documentos en el archivo |
| XML (application/xml) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |Seccionar el marcado XML y extraer texto |
| JSON (application/json) |`metadata_content_type`<br/>`metadata_content_encoding` |Extraer texto<br/>NOTA:  Si necesita extraer varios campos de documentos de un blob JSON, consulte [Indexación de blobs JSON](search-howto-index-json-blobs.md) para detalles. |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Extraer texto, incluidos los datos adjuntos |
| RTF (aplicación/rtf) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Extraer texto|
| Plain text (text/plain) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Extraer texto|

## <a name="see-also"></a>Consulta también

* [Indexadores de Azure Cognitive Search](search-indexer-overview.md)
* [Uso de la inteligencia artificial para comprender los datos de Blob Storage](search-blob-ai-integration.md)
* [Introducción a la indexación de blobs](search-blob-storage-integration.md)