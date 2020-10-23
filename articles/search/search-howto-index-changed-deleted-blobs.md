---
title: Blobs cambiados y eliminados
titleSuffix: Azure Cognitive Search
description: Después de la creación inicial del índice de búsqueda que importa desde Azure Blob Storage, la posterior indexación puede elegir los blobs que se cambian o eliminan. En este artículo se explican los detalles.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/25/2020
ms.openlocfilehash: 2e73039418233c97fc20242ed7af7df14c5b47ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91534784"
---
# <a name="how-to-set-up-change-and-deletion-detection-for-blobs-in-azure-cognitive-search-indexing"></a>Configuración de la detección de cambios y eliminaciones para blobs en la indexación de Azure Cognitive Search

Después de crear un índice de búsqueda inicial, es posible que quiera configurar los trabajos del indexador posteriores para que solo se seleccionen los documentos que se han creado o eliminado desde la ejecución inicial. En el caso del contenido de búsqueda cuyo origen sea Azure Blob Storage, la detección de cambios se produce automáticamente cuando se usa una programación para desencadenar la indexación. De forma predeterminada, el servicio reindexa solo los blobs modificados, tal como lo determina la marca de tiempo `LastModified` del blob. A diferencia de otros orígenes de datos compatibles con indexadores de búsqueda, los blobs siempre tienen una marca de tiempo, lo que elimina la necesidad de configurar manualmente una directiva de detección de cambios.

Aunque la detección de cambios es una obviedad, la detección de eliminaciones no lo es. Si desea detectar documentos eliminados, asegúrese de usar un enfoque de "eliminación temporal". Si elimina los blobs directamente, los documentos correspondientes no se quitarán del índice de búsqueda.

Hay dos maneras de implementar el enfoque de eliminación temporal. Ambos se describen a continuación.

## <a name="native-blob-soft-delete-preview"></a>Eliminación temporal de blobs nativos (versión preliminar)

> [!IMPORTANT]
> La compatibilidad con la eliminación temporal de blobs nativos está en versión preliminar. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). En la [API REST, versión 2020-06-30-Preview](./search-api-preview.md) se proporciona esta característica. Actualmente no hay compatibilidad con el portal ni con el SDK de .NET.

> [!NOTE]
> Al usar la directiva de eliminación temporal de blobs nativos, las claves de documento de los documentos del índice deben ser una propiedad de blob o metadatos de blob.

En este método, usará la característica de [eliminación temporal de blobs nativos](../storage/blobs/soft-delete-blob-overview.md) que ofrece Azure Blob Storage. Si la eliminación temporal de blobs nativos está habilitada en la cuenta de almacenamiento, el origen de datos tiene una directiva de eliminación temporal nativa establecida y el indexador encuentra un blob que ha pasado a un estado de eliminación temporal, el indexador quitará ese documento del índice. No se admite la directiva de eliminación temporal de blobs nativos al indexar blobs de Azure Data Lake Storage Gen2.

Siga estos pasos:

1. Habilite la [eliminación temporal nativa para Azure Blob Storage](../storage/blobs/soft-delete-blob-overview.md). Se recomienda establecer la directiva de retención en un valor mucho mayor que la programación del intervalo del indexador. De este modo, si hay un problema al ejecutar el indexador o si tiene un gran número de documentos para indexar, el indexador tendrá mucho tiempo para procesar los blobs eliminados temporalmente. Los indexadores de Azure Cognitive Search solo eliminarán un documento del índice si procesa el blob mientras se encuentra en un estado de eliminación temporal.

1. Configure una directiva de detección de eliminación temporal de blobs nativos en el origen de datos. A continuación se muestra un ejemplo. Dado que esta característica se encuentra en versión preliminar, debe usar la versión preliminar de la API de REST.

1. Ejecute el indexador o establezca el indexador para que se ejecute según una programación. Cuando el indexador ejecuta y procesa el blob, el documento se quita del índice.

    ```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30-Preview
    Content-Type: application/json
    api-key: [admin key]
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.NativeBlobSoftDeleteDeletionDetectionPolicy"
        }
    }
    ```

### <a name="reindexing-un-deleted-blobs-using-native-soft-delete-policies"></a>Reindexación de blobs recuperados (mediante directivas de eliminación temporal nativas)

Si elimina un blob de Azure Blob Storage con la eliminación temporal nativa habilitada en la cuenta de almacenamiento, el blob pasará a un estado de eliminación temporal, lo que le dará la opción de recuperarlo durante el período de retención. Si revierte una eliminación después de que el indexador la haya procesado, el indexador no siempre indexará el blob restaurado. Esto se debe a que el indexador determina qué blobs se indexan en función de la marca de tiempo `LastModified` del blob. Cuando se recupera un blob de eliminación temporal, su marca de tiempo `LastModified` no se actualiza, por lo que si el indexador ya ha procesado blobs con marcas de tiempo `LastModified` más recientes, no volverá a indexar el blob recuperado. 

Para garantizar que se vuelve a indexar un blob recuperado, debe actualizar la marca de tiempo `LastModified` del blob. Una forma de hacerlo consiste en volver a guardar los metadatos de ese blob. No es necesario cambiar los metadatos, pero si vuelve a guardar los metadatos, se actualizará la marca de tiempo `LastModified` del blob para que el indexador sepa que debe indexar este blob de nuevo.

## <a name="soft-delete-using-custom-metadata"></a>Eliminación temporal con metadatos personalizados

En este método, usará los metadatos de un blob para indicar cuándo se debe quitar un documento del índice de búsqueda. Este método requiere dos acciones independientes, la eliminación del documento de búsqueda del índice, seguida de la eliminación de blobs en Azure Storage.

Siga estos pasos:

1. Agregar un par clave-valor de metadatos personalizado al blob para indicar a Azure Cognitive Search que se ha eliminado de forma lógica.

1. Configurar una directiva de detección de columnas de eliminación temporal en el origen de datos. A continuación se muestra un ejemplo.

1. Una vez que el indexador ha procesado el blob y eliminado el documento del índice, puede eliminar el blob de Azure Blob Storage.

Por ejemplo, la siguiente directiva considera que un blob se va a eliminar si tiene una propiedad de metadatos `IsDeleted` con el valor `true`:

```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }
```

### <a name="reindexing-un-deleted-blobs-using-custom-metadata"></a>Reindexación de blobs recuperados (mediante metadatos personalizados)

Después de que un indexador procese un blob eliminado y quite el documento de búsqueda correspondiente del índice, no volverá a visitar ese blob si lo restaura más tarde si la marca de tiempo del `LastModified` del blob es anterior a la última ejecución del indexador.

Si desea volver a indexar ese documento, cambie el valor de `"softDeleteMarkerValue" : "false"` del blob y vuelva a ejecutar el indexador.

## <a name="help-us-make-azure-cognitive-search-better"></a>Ayúdenos a mejorar Azure Cognitive Search

Si tiene solicitudes o ideas para mejorar las características, comuníquelas en [UserVoice](https://feedback.azure.com/forums/263029-azure-search/). Si necesita ayuda para usar la característica existente, publique su pregunta en [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870).

## <a name="next-steps"></a>Pasos siguientes

* [Indexadores de Azure Cognitive Search](search-indexer-overview.md)
* [Configuración de un indexador de blobs](search-howto-indexing-azure-blob-storage.md)
* [Introducción a la indexación de blobs](search-blob-storage-integration.md)