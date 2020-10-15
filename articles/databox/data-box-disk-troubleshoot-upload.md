---
title: Solución de problemas de cargas de datos mediante registros
titleSuffix: Azure Data Box Disk
description: Se describe cómo utilizar los registros y solucionar problemas al cargar datos en Azure Data Box Disk.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: troubleshooting
ms.date: 06/17/2019
ms.author: alkohli
ms.openlocfilehash: 7225b04908753bb7c07ac89510859bac9db5b89c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85565011"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Descripción de registros para solucionar incidencias de carga de datos en Azure Data Box Disk

Este artículo se aplica a Microsoft Azure Data Box Disk y describe los problemas que se producen al cargar datos en Azure.

## <a name="about-upload-logs"></a>Acerca de los registros de carga

Cuando los datos se cargan en Azure en el centro de datos, se generan los archivos `_error.xml` y `_verbose.xml` para cada cuenta de almacenamiento. Estos registros se cargan en la misma cuenta de almacenamiento que se usó para cargar los datos. 

Ambos registros tienen el mismo formato e incluyen descripciones XML de los eventos que se produjeron al copiar los datos del disco en la cuenta de Azure Storage.

El registro detallado contiene información completa acerca del estado de la operación de copia de todos los blobs o archivos, mientras que el registro de errores contiene solo la información de los blobs o archivos que encontraron errores durante la carga.

El registro de errores tiene la misma estructura que el registro detallado, pero filtra las operaciones correctas.

## <a name="download-logs"></a>Descargar registros

Realice los pasos siguientes para buscar los registros de carga.

1. Si se producen errores durante la carga de los datos en Azure, en el portal se muestra la ruta de acceso a la carpeta donde se encuentran los registros de diagnóstico.

    ![Vínculos a registros en el portal](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

2. Vaya a **waies**.

    ![registros detallados y de errores](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs-1.png)

En cada caso, verá los registros de errores y los registros detallados. Seleccione cada registro y descargue una copia local.

## <a name="sample-upload-logs"></a>Ejemplos de registros de carga

A continuación se muestra un ejemplo de `_verbose.xml`. En este caso, el pedido se ha completado correctamente sin errores.

```xml

<?xml version="1.0" encoding="utf-8"?>
<DriveLog Version="2018-10-01">
  <DriveId>184020D95632</DriveId>
  <Blob Status="Completed">
    <BlobPath>$root/botetapageblob.vhd</BlobPath>
    <FilePath>\PageBlob\botetapageblob.vhd</FilePath>
    <Length>1073742336</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <PageRangeList>
      <PageRange Offset="0" Length="4194304" Status="Completed" />
      <PageRange Offset="4194304" Length="4194304" Status="Completed" />
      <PageRange Offset="8388608" Length="4194304" Status="Completed" />
      --------CUT-------------------------------------------------------
      <PageRange Offset="1061158912" Length="4194304" Status="Completed" />
      <PageRange Offset="1065353216" Length="4194304" Status="Completed" />
      <PageRange Offset="1069547520" Length="4194304" Status="Completed" />
      <PageRange Offset="1073741824" Length="512" Status="Completed" />
    </PageRangeList>
  </Blob>
  <Blob Status="Completed">
    <BlobPath>$root/botetablockblob.txt</BlobPath>
    <FilePath>\BlockBlob\botetablockblob.txt</FilePath>
    <Length>19</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <BlockList>
      <Block Offset="0" Length="19" Status="Completed" />
    </BlockList>
  </Blob>
  <File Status="Completed">
    <FileStoragePath>botetaazurefilesfolder/botetaazurefiles.txt</FileStoragePath>
    <FilePath>\AzureFile\botetaazurefilesfolder\botetaazurefiles.txt</FilePath>
    <Length>20</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <FileRangeList>
      <FileRange Offset="0" Length="20" Status="Completed" />
    </FileRangeList>
  </File>
  <Status>Completed</Status>
</DriveLog>
```

A continuación se muestra un ejemplo de `_error.xml` para el mismo pedido.

```xml

<?xml version="1.0" encoding="utf-8"?>
<DriveLog Version="2018-10-01">
  <DriveId>184020D95632</DriveId>
  <Summary>
    <ValidationErrors>
      <None Count="3" />
    </ValidationErrors>
    <CopyErrors>
      <None Count="3" Description="No errors encountered" />
    </CopyErrors>
  </Summary>
  <Status>Completed</Status>
</DriveLog>
```

A continuación se muestra un ejemplo de `_error.xml` en el que el pedido se ha completado con errores. 

En este caso, el archivo de error tiene una sección `Summary` y otra sección que contiene todos los errores de nivel de archivo. 

`Summary` contiene `ValidationErrors` y `CopyErrors`. En este caso, se cargaron 8 archivos o carpetas en Azure y no hubo ningún error de validación. Cuando los datos se copiaron en la cuenta de Azure Storage, se cargaron correctamente 5 archivos o carpetas. Se cambió el nombre de los 3 archivos o carpetas restantes según las convenciones de nomenclatura de contenedores de Azure y, después, se cargaron correctamente en Azure.

Los estados de nivel de archivo están en `BlobStatus`, que describe las acciones que se realizan para cargar los blobs. En este caso, se cambia el nombre de tres contenedores porque las carpetas en las que se copiaron los datos no se ajustaban a las convenciones de nomenclatura de Azure para los contenedores. En el caso de los blobs cargados en esos contenedores, se incluye el nuevo nombre de contenedor, la ruta de acceso del blob en Azure, la ruta de acceso de archivo no válida original y el tamaño del blob.
    
```xml
 <?xml version="1.0" encoding="utf-8"?>
    <DriveLog Version="2018-10-01">
      <DriveId>18041C582D7E</DriveId>
      <Summary>
     <!--Summary for validation and data copy to Azure -->
        <ValidationErrors>
          <None Count="8" />
        </ValidationErrors>
        <CopyErrors>
          <Completed Count="5" Description="No errors encountered" />
          <ContainerRenamed Count="3" Description="Renamed the container as the original container name does not follow Azure conventions." />
        </CopyErrors>
      </Summary>
    <!--List of renamed containers with the new names, new file path in Azure, original invalid file path, and size -->
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/private.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\private.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/resource.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\resource.vhd</OriginalFilePath>
        <SizeInBytes>71528448</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/role.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\role.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Status>CompletedWithErrors</Status>
    </DriveLog>
```

## <a name="data-upload-errors"></a>Errores de carga de datos

En la tabla siguiente se resumen los errores generados al cargar los datos en Azure.

| Código de error | Descripción                   |
|-------------|------------------------------|
|`None` |  Se completó correctamente.           |
|`Renamed` | El nombre del blob se cambió correctamente.   |
|`CompletedWithErrors` | La carga se completó con errores. Los detalles de los archivos con errores se incluyen en el archivo de registro.  |
|`Corrupted`|El valor de CRC calculado durante la ingesta de datos no coincide con el valor de CRC calculado durante la carga.  |  
|`StorageRequestFailed` | La solicitud de Azure Storage no se realizó.   |     
|`LeasePresent` | Esté artículo se ha concedido y lo usa otro usuario. |
|`StorageRequestForbidden` |No se pudo cargar debido a problemas de autenticación. |
|`ManagedDiskCreationTerminalFailure` | No se pudo cargar como discos administrados. Los archivos están disponibles en la cuenta de almacenamiento provisional como blobs en páginas. Puede convertir manualmente los blobs en páginas a discos administrados.  |
|`DiskConversionNotStartedTierInfoMissing` | Puesto que el archivo de disco duro virtual se copió fuera de las carpetas del nivel creadas previamente, no se ha creado un disco administrado. El archivo se carga como blob en páginas en la cuenta de almacenamiento provisional que se especificó durante la creación del pedido. Puede convertirlo manualmente en un disco administrado.|
|`InvalidWorkitem` | No se pudieron cargar los datos ya que no se ajustan a las convenciones de nomenclatura y límites de Azure.|
|`InvalidPageBlobUploadAsBlockBlob` | Se cargaron como blobs en bloques en un contenedor con el prefijo `databoxdisk-invalid-pb-`.|
|`InvalidAzureFileUploadAsBlockBlob` | Se cargaron como blobs en bloques en un contenedor con el prefijo `databoxdisk-invalid-af`-.|
|`InvalidManagedDiskUploadAsBlockBlob` | Se cargaron como blobs en bloques en un contenedor con el prefijo `databoxdisk-invalid-md`-.|
|`InvalidManagedDiskUploadAsPageBlob` |Se cargaron como blobs en páginas en un contenedor con el prefijo `databoxdisk-invalid-md-`. |
|`MovedToOverflowShare` |Los archivos se cargaron en un nuevo recurso compartido porque el tamaño del recurso compartido original superaba el límite de tamaño máximo de Azure. El nombre del nuevo recurso compartido es el nombre original con el sufijo `-2`.   |
|`MovedToDefaultAzureShare` |Los archivos que no formaban parte de ninguna carpeta se cargaron en un recurso compartido predeterminado. El nombre del recurso compartido comienza por `databox-`. |
|`ContainerRenamed` |El contenedor de estos archivos no se ajustaba a las convenciones de nomenclatura de Azure y se le cambió el nombre. El nuevo nombre empieza por `databox-` e incluye el hash SHA1 del nombre original como sufijo. |
|`ShareRenamed` |El recurso compartido de estos archivos no se ajustaba a las convenciones de nomenclatura de Azure y se le cambió el nombre. El nuevo nombre empieza por `databox-` e incluye el hash SHA1 del nombre original como sufijo. |
|`BlobRenamed` |Estos archivos no se ajustaban a las convenciones de nomenclatura de Azure y se les cambió el nombre. Compruebe el nuevo nombre en el campo `BlobPath`. |
|`FileRenamed` |Estos archivos no se ajustaban a las convenciones de nomenclatura de Azure y se les cambió el nombre. Compruebe el nuevo nombre en el campo `FileStoragePath`. |
|`DiskRenamed` |Estos archivos no se ajustaban a las convenciones de nomenclatura de Azure y se les cambió el nombre. Compruebe el nuevo nombre en el campo `BlobPath`. |


## <a name="next-steps"></a>Pasos siguientes

- [Apertura de una incidencia de soporte técnico en Data Box Disk](data-box-disk-contact-microsoft-support.md).
