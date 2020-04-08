---
title: Problemas conocidos con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Más información sobre las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 4f8fae6580272ed53b8d440ba3e74c6a1ed1e61a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061505"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conocidos con Azure Data Lake Storage Gen2

En este artículo se describen las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2.

## <a name="supported-blob-storage-features"></a>Características de Blob Storage compatibles

Ahora, un número creciente de características de Blob Storage funciona con cuentas que tienen un espacio de nombres jerárquico. Para obtener una lista completa, consulte [Características de Blob Storage disponibles en Azure Data Lake Storage Gen2](data-lake-storage-supported-blob-storage-features.md).

## <a name="supported-azure-service-integrations"></a>Integraciones de servicios de Azure admitidos

Data Lake Storage Gen2 admite varios servicios de Azure que se pueden usar para ingerir datos, realizar análisis y crear representaciones visuales. Para ver una lista de los servicios de Azure admitidos, consulte el documento en que se indica cuáles son los [servicios de Azure que admiten Azure Data Lake Storage Gen2](data-lake-storage-supported-azure-services.md).

Consulte [Servicios de Azure que admiten Azure Data Lake Storage Gen2](data-lake-storage-supported-azure-services.md).

## <a name="supported-open-source-platforms"></a>Plataformas de código abierto compatibles

Varias plataformas de código abierto son compatibles con Data Lake Storage Gen2. Para ver una lista completa, consulte [Plataformas de código abierto que admiten Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md).

Consulte [Plataformas de código abierto que admiten Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md).

## <a name="blob-storage-apis"></a>API de Blob Storage

Las API de Blob y Data Lake Storage Gen2 API pueden operar en los mismos datos.

En esta sección se describen los problemas y las limitaciones del uso de las API de blob y de las API de Data Lake Storage Gen2 para operar en los mismos datos.

* No puede usar las API de blob y las API de Data Lake Storage para escribir en la misma instancia de un archivo. Si escribe en un archivo con la API de Data Lake Storage Gen2, los bloques de ese archivo no estarán visibles para las llamadas a la API de blob [Get Block List](https://docs.microsoft.com/rest/api/storageservices/get-block-list). Puede sobrescribir un archivo con las API de Data Lake Storage Gen2 o con las API de blob. No afectará a las propiedades del archivo.

* Cuando use la operación [List Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) (Enumerar blobs) sin especificar un delimitador, los resultados incluirán tanto los directorios como los blobs. Si decide usar un delimitador, use solo una barra diagonal (`/`). Este es el único delimitador admitido.

* Si usa la API [Delete Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob) para eliminar un directorio, solo se eliminará si está vacío. Esto significa que no puede utilizar la API de blob para eliminar los directorios de forma recursiva.

Las siguientes API REST de blob no se admiten:

* [Put Blob (Page)](https://docs.microsoft.com/rest/api/storageservices/put-blob) [Poner blob (página)]
* [Put Page](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Get Page Ranges](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges) (Conseguir intervalos de páginas)
* [Incremental Copy Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob) (Copia incremental del blob)
* [Put Page from URL](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url) (Poner página de dirección URL)
* [Put Blob (Append)](https://docs.microsoft.com/rest/api/storageservices/put-blob) [Poner blob (anexar)]
* [Append Block](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [Append Block from URL](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url) (Anexar bloque desde dirección URL)

No se admiten discos de máquina virtual no administrados en cuentas que tengan un espacio de nombres jerárquico. Si desea habilitar un espacio de nombres jerárquico en una cuenta de almacenamiento, coloque los discos de máquina virtual no administrados en una cuenta de almacenamiento que no tenga habilitada la característica de espacio de nombres jerárquico.

<a id="api-scope-data-lake-client-library" />

## <a name="file-system-support-in-sdks"></a>Compatibilidad del sistema de archivos en SDK

Las operaciones de ACL Get y Set no son recursivas actualmente.

## <a name="file-system-support-in-powershell-and-azure-cli"></a>Compatibilidad del sistema de archivos en PowerShell y la CLI de Azure

- La compatibilidad con [PowerShell](data-lake-storage-directory-file-acl-powershell.md) y la [CLI de Azure](data-lake-storage-directory-file-acl-cli.md) se encuentra en versión preliminar pública.
- Las operaciones de ACL Get y Set no son recursivas actualmente.

## <a name="lifecycle-management-policies"></a>Directivas de administración del ciclo de vida

* Todavía no se admite la eliminación de instantáneas de blob.  

## <a name="archive-tier"></a>Nivel de archivo

Actualmente hay un error que afecta al nivel de acceso de archivo.


## <a name="blobfuse"></a>Blobfuse

Blobfuse no se admite.

<a id="known-issues-tools" />

## <a name="azcopy"></a>AzCopy

Use solo la versión más reciente de AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). No se admiten versiones anteriores de AzCopy, como AzCopy v8.1.

<a id="storage-explorer" />

## <a name="azure-storage-explorer"></a>Explorador de Azure Storage

Use solo las versiones  `1.6.0`  o superiores.

<a id="explorer-in-portal" />

## <a name="storage-explorer-in-the-azure-portal"></a>Explorador de Storage en Azure Portal

Las ACL aún no se admiten.

<a id="third-party-apps" />

## <a name="thirdpartyapplications"></a>Aplicaciones de terceros

Las aplicaciones de terceros que usan las API REST para funcionar seguirán funcionando si se usan con Data Lake Storage Gen2. Lo más probable es que las aplicaciones que llaman a las API de Blob funcionen.

## <a name="access-control-lists-acl-and-anonymous-read-access"></a>Listas de control de acceso (ACL) y acceso de lectura anónimo

Si se ha concedido [acceso de lectura anónimo](storage-manage-access-to-resources.md) a un contenedor, las ACL no tendrán ningún efecto en ese contenedor o los archivos de ese contenedor.

## <a name="windows-azure-storage-blob-wasb-driver"></a>Controlador Azure Storage Blob para Windows (WASB)

Actualmente, hay varios problemas asociados al uso del controlador de WASB junto con cuentas que tienen un espacio de nombres jerárquico. Se recomienda el uso del controlador de [Azure Blob File System (ABFS)](data-lake-storage-abfs-driver.md) en las cargas de trabajo. 





