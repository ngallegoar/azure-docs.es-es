---
title: Comprobación de la coherencia de los datos en la actividad de copia
description: Obtenga información sobre cómo habilitar la comprobación de la coherencia de datos en la actividad de copia en Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: ''
ms.reviewer: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 3/27/2020
ms.author: yexu
ms.openlocfilehash: a45c8ce820532d11f18758924dc3399818cb9158
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84610243"
---
#  <a name="data-consistency-verification-in-copy-activity-preview"></a>Comprobación de la coherencia de los datos en la actividad de copia (versión preliminar)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Cuando se mueven datos del almacén de origen al de destino, la actividad de copia de Azure Data Factory proporciona una opción para realizar la comprobación de la coherencia de datos adicionales con el fin de garantizar que los datos no solo se copian correctamente del almacén de origen al de destino, así como la coherencia entre el almacén de origen y el de destino. Si se encuentran datos incoherentes durante el movimiento de datos, puede anular la actividad de copia o habilitar la opción de configuración de tolerancia a errores para seguir copiando el resto y omitir los datos incoherentes. Para obtener los nombres de los objetos omitidos, habilite la configuración del registro de sesión en la actividad de copia. 

> [!IMPORTANT]
> Esta característica se encuentra actualmente en versión preliminar con las siguientes limitaciones en las que estamos trabajando activamente:
>- La comprobación de la coherencia de datos solo está disponible en la copia de archivos binarios entre almacenes basados en archivos con el comportamiento de "PreserveHierarchy" en la actividad de copia. Para copiar datos tabulares, la comprobación de la coherencia de datos no está disponible todavía en la actividad de copia.
>- Cuando se habilita la opción de registro de sesión en la actividad de copia para registrar los archivos incoherentes que se omiten, la integridad del archivo de registro no se puede garantizar 100 % si se produce un error en la actividad de copia.
>- El registro de sesión solo contiene archivos incoherentes, donde los archivos copiados correctamente no se registran por ahora.

## <a name="supported-data-stores"></a>Almacenes de datos compatibles

### <a name="source-data-stores"></a>Almacenes de datos de origen

-   [Almacenamiento de blobs de Azure](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)
-   [Azure File Storage](connector-azure-file-storage.md)
-   [Amazon S3](connector-amazon-simple-storage-service.md)
-   [Sistema de archivos](connector-file-system.md)
-   [HDFS](connector-hdfs.md)

### <a name="destination-data-stores"></a>Almacenes de datos de destino

-   [Almacenamiento de blobs de Azure](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)
-   [Azure File Storage](connector-azure-file-storage.md)
-   [Sistema de archivos](connector-file-system.md)


## <a name="configuration"></a>Configuración
En el ejemplo siguiente se proporciona una definición de JSON para habilitar la comprobación de la coherencia de datos en la actividad de copia: 

```json
"typeProperties": { 
"source": { 
        "type": "BinarySource", 
        "storeSettings": { 
            "type": "AzureDataLakeStoreReadSettings", 
            "recursive": true 
        } 
    }, 
    "sink": { 
        "type": "BinarySink", 
        "storeSettings": { 
            "type": "AzureDataLakeStoreWriteSettings" 
        } 
}, 
    "validateDataConsistency": true, 
    "skipErrorFile": { 
        "dataInconsistency": true 
    }, 
    "logStorageSettings": { 
        "linkedServiceName": { 
            "referenceName": "ADLSGen2_storage", 
            "type": "LinkedServiceReference" 
        }, 
        "path": "/sessionlog/" 
} 
} 
```

Propiedad | Descripción | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | -------- 
validateDataConsistency | Si establece true para esta propiedad, la actividad de copia comprobará el tamaño del archivo, el valor de lastModifiedDate y la suma de comprobación de MD5 de cada objeto copiado del almacén de origen al de destino para garantizar la coherencia de los datos entre el almacén de origen y el de destino. Tenga en cuenta que el rendimiento de la copia se verá afectado al habilitar esta opción.  | True<br/>False (valor predeterminado) | No
dataInconsistency | Uno de los pares clave-valor dentro del contenedor de propiedades de skipErrorFile para determinar si desea omitir los datos incoherentes.<br/> \- True: desea copiar el resto omitiendo los datos incoherentes.<br/> - False: desea anular la actividad de copia una vez que se encuentren datos incoherentes.<br/>Tenga en cuenta que esta propiedad solo es válida cuando se establece validateDataConsistency como True.  | True<br/>False (valor predeterminado) | No
logStorageSettings | Grupo de propiedades que puede especificarse para habilitar el registro de la sesión a fin de registrar los objetos omitidos. | | No
linkedServiceName | Servicio vinculado de [Azure Blob Storage](connector-azure-blob-storage.md#linked-service-properties) o [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#linked-service-properties) para almacenar los archivos de registro de sesión. | Nombres de un servicio vinculado a los tipos `AzureBlobStorage` o `AzureBlobFS`, que hace referencia a la instancia que usa para almacenar los archivos de registro. | No
path | Ruta de acceso de los archivos de registro. | Especifique la ruta de acceso donde quiere almacenar los archivos de registro. Si no se proporciona una ruta de acceso, el servicio creará un contenedor para usted. | No

>[!NOTE]
>- No se admite la coherencia de datos en el escenario de copia de almacenamiento provisional. 
>- Al copiar archivos desde o hacia Blob de Azure o Azure Data Lake Storage Gen2, ADF realiza la comprobación de la suma de comprobación MD5 de nivel de bloque aprovechando la [API de Blob de Azure](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions?view=azure-dotnet-legacy) y la [API de Azure Data Lake Storage Gen2](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/path/update#request-headers). Si ContentMD5 en los archivos se encuentra en Blob de Azure o en Azure Data Lake Storage Gen2 como orígenes de datos, ADF realiza la comprobación de la suma de comprobación MD5 del nivel de archivo también después de leer los archivos. Después de copiar los archivos en Blob de Azure o en Azure Data Lake Storage Gen2 como destino de los datos, ADF escribe ContentMD5 en el Blob de Azure o en Azure Data Lake Storage Gen2, y las aplicaciones de nivel inferior pueden usarlo después para la comprobación de la coherencia de los datos.
>- ADF realiza la comprobación del tamaño de archivo al copiar los archivos entre almacenes de almacenamiento.

## <a name="monitoring"></a>Supervisión

### <a name="output-from-copy-activity"></a>Salida de la actividad de copia
Cuando la actividad de copia se ha ejecutado completamente, puede ver el resultado de la comprobación de la coherencia de datos desde la salida de cada ejecución de la actividad de copia:

```json
"output": {
            "dataRead": 695,
            "dataWritten": 186,
            "filesRead": 3,  
            "filesWritten": 1, 
            "filesSkipped": 2, 
            "throughput": 297,
            "logPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "dataConsistencyVerification": 
           { 
                "VerificationResult": "Verified", 
                "InconsistentData": "Skipped" 
           } 
        }

```
Puede ver los detalles de la comprobación de la coherencia de datos desde la "propiedad dataConsistencyVerification".

Valor de **VerificationResult**: 
-   **Verified**:  se ha comprobado que los datos copiados son coherentes entre el almacén de origen y el de destino. 
-   **NotVerified**: no se ha comprobado que los datos copiados sean coherentes porque no se ha habilitado validateDataConsistency en la actividad de copia. 
-   **No admitidas**: no se ha comprobado que los datos copiados sean coherentes porque la comprobación de la coherencia de datos no se admite para este par de copia concreto. 

Valor de **InconsistentData**: 
-   **Found**: la actividad de copia de ADF encontró datos incoherentes. 
-   **Omitidas**: la actividad de copia de ADF encontró y omitió datos incoherentes. 
-   **Ninguna**: la actividad de copia de ADF no encontró datos incoherentes. Puede deberse a que se ha comprobado que los datos son coherentes entre el almacén de origen y el de destino, o bien porque se ha deshabilitado validateDataConsistency en la actividad de copia. 

### <a name="session-log-from-copy-activity"></a>Registro de sesión de la actividad de copia

Si configura el registro del archivo incoherente, puede encontrar el archivo de registro en esta ruta de acceso: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/copyactivity-logs/[copy-activity-name]/[copy-activity-run-id]/[auto-generated-GUID].csv`.  Los archivos de registro serán los archivos CSV. 

El esquema de un archivo de registro es el siguiente:

Columna | Descripción 
-------- | -----------  
Timestamp | Marca de tiempo en que ADF omite los archivos incoherentes.
Nivel | Nivel de registro de este elemento. Estará en el nivel "Warning" para el elemento que muestra la omisión del archivo.
OperationName | Comportamiento operativo de la actividad de copia de ADF en cada archivo. Será "FileSkip" para especificar el archivo que se va a omitir.
OperationItem | Nombre del archivo que se va a omitir.
Message | Más información para ilustrar por qué se omiten los archivos.

El ejemplo de un archivo de registro es el siguiente: 
```
Timestamp, Level, OperationName, OperationItem, Message
2020-02-26 06:22:56.3190846, Warning, FileSkip, "sample1.csv", "File is skipped after read 548000000 bytes: ErrorCode=DataConsistencySourceDataChanged,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Source file 'sample1.csv' is changed by other clients during the copy activity run.,Source=,'." 
```
En el archivo de registro anterior, puede ver que sample1.csv se ha omitido porque no se pudo comprobar su coherencia entre el almacén de origen y el de destino. El archivo sample1.csv se vuelve incoherente porque otras aplicaciones lo han modificado mientras la actividad de copia de ADF estaba realizando la copia. 



## <a name="next-steps"></a>Pasos siguientes
Consulte los otros artículos de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Tolerancia a errores de la actividad de copia](copy-activity-fault-tolerance.md)


