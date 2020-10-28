---
title: Ejemplos de Azure Storage con C++ | Microsoft Docs
description: Consulte, descargue y ejecute código de ejemplo y aplicaciones para Azure Storage. Descubra ejemplos introductorios de blobs, colas, tablas y archivos, con las bibliotecas de cliente de almacenamiento para C++.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 10/01/2020
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: 158f908dfd52a3365d19f65eb00faf1787893af5
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92372906"
---
# <a name="azure-storage-samples-using-v12-c-client-libraries"></a>Ejemplos de Azure Storage con las bibliotecas de cliente para C++ v12

En la tabla siguiente se proporciona información general sobre el repositorio de ejemplos y los escenarios que abarca cada ejemplo. Haga clic en los vínculos para ver el código de ejemplo correspondiente en GitHub.

> [!NOTE]
> En estos ejemplos se usa la biblioteca para C++ v12 más reciente de Azure Storage.

## <a name="blob-samples"></a>Ejemplos de blob

:::row:::
   :::column:::
        [Autenticación mediante una cadena de conexión.](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-blobs/sample/blob_getting_started.cpp#L18)
   :::column-end:::
   :::column:::
        [Creación de un contenedor de blobs](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-blobs/sample/blob_getting_started.cpp#L20)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
        [Obtención de un cliente de blob](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-blobs/sample/blob_getting_started.cpp#L30)
   :::column-end:::
   :::column:::
        [Carga de un blob](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-blobs/sample/blob_getting_started.cpp#L32)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
        [Establecimiento de metadatos en un blob](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-blobs/sample/blob_getting_started.cpp#L34)
   :::column-end:::
   :::column:::
        [Obtención de propiedades del blob](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-blobs/sample/blob_getting_started.cpp#L37)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
        [Descarga de un blob](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-blobs/sample/blob_getting_started.cpp#L44)
   :::column-end:::
:::row-end:::

## <a name="data-lake-storage-gen2-samples"></a>Ejemplos de Data Lake Storage Gen2

:::row:::
   :::column:::
        [Creación de un cliente de servicio mediante una cadena de conexión](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L22)
   :::column-end:::
   :::column:::
        [Creación de un cliente del sistema de archivos mediante una cadena de conexión](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L25)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
        [Creación de un sistema de archivos](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L30)
   :::column-end:::
   :::column:::
        [Creación de un directorio](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L48)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
        [Creación de un archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L52)
   :::column-end:::
   :::column:::
        [Anexión de datos a un archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L68)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
        [Vaciado de datos del archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L77)
   :::column-end:::
   :::column:::
        [Lectura de un archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L80)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
        [Enumeración de todos los sistemas de archivos](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L88)
   :::column-end:::
   :::column:::
        [Eliminación de un sistema de archivos](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-datalake/sample/datalake_getting_started.cpp#L102)
   :::column-end:::
:::row-end:::

## <a name="azure-files-samples"></a>Ejemplos de Azure Files

:::row:::
    :::column:::
        [Creación de un cliente de recurso compartido mediante una cadena de conexión](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-shares/sample/file_share_getting_started.cpp#L18)
    :::column-end:::
    :::column:::
        [Creación de un recurso compartido de archivos](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-shares/sample/file_share_getting_started.cpp#L21)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Obtención de un cliente de archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-shares/sample/file_share_getting_started.cpp#L29)
    :::column-end:::
    :::column:::
        [Carga de un archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-shares/sample/file_share_getting_started.cpp#L31)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [Establecimiento de metadatos en un archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-shares/sample/file_share_getting_started.cpp#L33)
    :::column-end:::
    :::column:::
        [Obtención de las propiedades del archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-shares/sample/file_share_getting_started.cpp#L36)
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="2":::
        [Descarga de un archivo](https://github.com/Azure/azure-sdk-for-cpp/blob/master/sdk/storage/azure-storage-files-shares/sample/file_share_getting_started.cpp#L43)
    :::column-end:::
:::row-end:::

## <a name="azure-code-sample-libraries"></a>Bibliotecas de ejemplo de código de Azure

Para ver todas las bibliotecas de ejemplo de C++, vaya a:

* [Ejemplos de código para blobs de Azure](https://github.com/Azure/azure-sdk-for-cpp/tree/master/sdk/storage/azure-storage-blobs/sample)
* [Ejemplos de código de Azure Data Lake](https://github.com/Azure/azure-sdk-for-cpp/tree/master/sdk/storage/azure-storage-files-datalake/sample)
* [Ejemplos de código de Azure Files](https://github.com/Azure/azure-sdk-for-cpp/tree/master/sdk/storage/azure-storage-files-shares/sample)

Puede explorar y clonar el repositorio de GitHub para cada biblioteca.

## <a name="getting-started-guides"></a>Guías de introducción

Consulte las guías siguientes si busca instrucciones sobre cómo instalar las bibliotecas de cliente de Azure Storage y cómo empezar a usarlas.

* [Inicio rápido: Biblioteca de Azure Blob Storage v12: C++](../blobs/quickstart-blobs-c-plus-plus.md)

## <a name="next-steps"></a>Pasos siguientes

Para información sobre ejemplos para otros lenguajes:

* .NET: [Azure Storage samples using .NET](storage-samples-dotnet.md) (Ejemplos de Azure Storage con .NET)
* Java: [ejemplos de Azure Storage con Java](storage-samples-java.md)
* Python: [Ejemplos de Azure Storage con Python](storage-samples-python.md)
* JavaScript/Node.js: [Ejemplos de Azure Storage con JavaScript](storage-samples-javascript.md)
* El resto de lenguajes: [Ejemplos de Azure Storage](storage-samples.md)
