---
title: Habilitación y administración de la restauración a un momento dado para blobs en bloques (versión preliminar)
titleSuffix: Azure Storage
description: Aprenda a usar la restauración a un momento dado (versión preliminar) para restaurar blobs en bloques a un estado en un momento anterior.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/06/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: cbfc5667fb35b8f807a3a806dda4647af10e9392
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83118217"
---
# <a name="enable-and-manage-point-in-time-restore-for-block-blobs-preview"></a>Habilitación y administración de la restauración a un momento dado para blobs en bloques (versión preliminar)

Puede usar la restauración a un momento dado (versión preliminar) para restaurar blobs en bloques a un estado en un momento anterior. En este artículo se describe cómo habilitar la restauración a un momento dado para una cuenta de almacenamiento con PowerShell. También se muestra cómo realizar una operación de restauración con PowerShell.

Para obtener más información y para aprender a registrarse en la versión preliminar, consulte [Restauración a un momento dado para blobs en bloques (versión preliminar)](point-in-time-restore-overview.md).

> [!CAUTION]
> La restauración a un momento dado solo admite la restauración de operaciones en blobs en bloques. No se pueden restaurar las operaciones en contenedores. Si elimina un contenedor de la cuenta de almacenamiento mediante una llamada a la operación [Delete Container](/rest/api/storageservices/delete-container) durante la vista previa de la restauración a un momento dado, ese contenedor no se puede restaurar con una operación de restauración. Durante la versión preliminar, en lugar de eliminar un contenedor, elimine los blobs individuales si quiere restaurarlos.

> [!IMPORTANT]
> La versión preliminar de la restauración a un momento dado está pensada para usos que no tengan que ver con la producción. En este momento no hay contratos de nivel de servicio de producción disponibles.

## <a name="install-the-preview-module"></a>Instalación del módulo de versión preliminar

Para configurar la restauración a un momento dado de Azure con PowerShell, primero Instale la versión [1.14.1-preview](https://www.powershellgallery.com/packages/Az.Storage/1.14.1-preview) del módulo AZ.Storage de PowerShell. Para instalar el módulo de la versión preliminar, siga estos pasos:

1. Desinstale cualquier instalación anterior de Azure PowerShell desde Windows usando el ajuste **Aplicaciones y características** en **Configuración**.

1. Asegúrese de tener instalada la versión más reciente de PowerShellGet. Abra una ventana de Windows PowerShell y ejecute el siguiente comando para instalar la versión más reciente:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. Cierre y vuelva a abrir la ventana de PowerShell después de instalar PowerShellGet.

1. Instale la versión más reciente de Azure PowerShell:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Instale el módulo de versión preliminar Az.Storage:

    ```powershell
    Install-Module Az.Storage -Repository PSGallery -RequiredVersion 1.14.1-preview -AllowPrerelease -AllowClobber -Force
    ```

Para más información sobre cómo instalar Azure PowerShell, consulte [Instalación de Azure PowerShell con PowerShellGet](/powershell/azure/install-az-ps).

## <a name="enable-and-configure-point-in-time-restore"></a>Habilitar y configurar la restauración a un momento dado

Antes de habilitar y configurar la restauración a un momento dado, habilite los requisitos previos: eliminación temporal, fuente de cambios y control de versiones de blob. Para obtener más información sobre cómo habilitar cada una de estas características, consulte estos artículos:

- [Habilitación de la eliminación temporal para blobs](soft-delete-enable.md)
- [Habilitar y deshabilitar la fuente de cambios](storage-blob-change-feed.md#enable-and-disable-the-change-feed)
- [Habilitación y administración de las versiones de blob](versioning-enable.md)

Para configurar la restauración a un momento dado de Azure con PowerShell, llame al comando Enable-AzStorageBlobRestorePolicy. En el ejemplo siguiente se habilita la eliminación temporal y se establece el período de retención de eliminación temporal, se habilita la fuente de cambios y, posteriormente, se habilita la restauración a un momento dado. Antes de ejecutar el ejemplo, use Azure Portal o una plantilla de Azure Resource Manager para habilitar también el control de versiones de blobs.

Al ejecutar el ejemplo, no olvide reemplazar los valores entre corchetes angulares por sus propios valores:

```powershell
# Sign in to your Azure account.
Connect-AzAccount

# Set resource group and account variables.
$rgName = "<resource-group>"
$accountName = "<storage-account>"

# Enable soft delete with a retention of 6 days.
Enable-AzStorageBlobDeleteRetentionPolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RetentionDays 6

# Enable change feed.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EnableChangeFeed $true

# Enable point-in-time restore with a retention period of 5 days.
# The retention period for point-in-time restore must be at least one day less than that set for soft delete.
Enable-AzStorageBlobRestorePolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RestoreDays 5

# View the service settings.
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName
```

## <a name="perform-a-restore-operation"></a>Realizar una operación de restauración

Para iniciar una operación de restauración, llame al comando Restore-AzStorageBlobRange, especificando el punto de restauración como un valor UTC **DateTime**. Puede especificar uno o más rangos lexicográficos de blobs para restaurar u omitir el rango para restaurar todos los blobs en todos los contenedores de la cuenta de almacenamiento. La operación de restauración puede tardar varios minutos en completarse.

Al especificar un rango de blobs para restaurar, tenga en cuenta las siguientes reglas:

- El patrón de contenedor especificado para el rango de inicio y el rango de finalización debe incluir un mínimo de tres caracteres. La barra diagonal (/) que se usa para separar un nombre de contenedor de un nombre de blob no cuenta para este mínimo.
- Solo se puede especificar un rango por cada operación de restauración.
- No se admite el uso de caracteres comodín. Se tratan como caracteres estándar.
- Puede restaurar los blobs en los contenedores `$root` y `$web` si los especifica de forma explícita en un rango que se pasa a una operación de restauración. Los contenedores `$root` y `$web` se restauran solo si se especifican explícitamente. No se pueden restaurar otros contenedores del sistema.

### <a name="restore-all-containers-in-the-account"></a>Restaurar todos los contenedores de la cuenta

Para restaurar todos los contenedores y blobs de la cuenta de almacenamiento, llame al comando Restore-AzStorageBlobRange, omitiendo el parámetro `-BlobRestoreRange`. En el siguiente ejemplo se restauran los contenedores de la cuenta de almacenamiento a su estado 12 horas antes del momento actual:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddHours(-12)
```

### <a name="restore-a-single-range-of-block-blobs"></a>Restauración de un solo rango de blobs en bloques

Para restaurar un rango de blobs, llame al comando Restore-AzStorageBlobRange y especifique un rango lexicográfico de nombres de contenedor y blob para el parámetro `-BlobRestoreRange`. El inicio del rango es inclusivo y el final del rango es exclusivo.

Por ejemplo, para restaurar los blobs en un único contenedor denominado *sample-container*, puede especificar un rango que comience por *sample-container* y termine con *sample-container1*. No es necesario que existan los contenedores denominados en los rangos de inicio y finalización. Dado que el final del rango es exclusivo, incluso si la cuenta de almacenamiento incluye un contenedor denominado *sample-container1*, solo se restaurará el contenedor denominado *sample-container*:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container -EndRange sample-container1
```

Para especificar un subconjunto de blobs en un contenedor para restaurar, use una barra diagonal (/) para separar el nombre del contenedor del patrón de blobs. Por ejemplo, el intervalo siguiente selecciona los blobs en un único contenedor cuyos nombres comienzan por las letras *d* hasta la *f*:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container/d -EndRange sample-container/g
```

A continuación, proporcione el rango al comando Restore-AzStorageBlobRange command. Especifique el punto de restauración proporcionando un valor UTC **DateTime** para el parámetro `-TimeToRestore`. En el siguiente ejemplo se restauran los blobs del rango especificado a su estado 3 días antes del momento actual:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -BlobRestoreRange $range `
    -TimeToRestore (Get-Date).AddDays(-3)
```

### <a name="restore-multiple-ranges-of-block-blobs"></a>Restauración de varios rangos de blobs en bloques

Para restaurar varios rangos de blobs en bloques, especifique una matriz de rangos para el parámetro `-BlobRestoreRange`. En el ejemplo siguiente se restaura el contenido completo de *container1* y *container4*:

```powershell
$range1 = New-AzStorageBlobRangeToRestore -StartRange container1 -EndRange container2
$range2 = New-AzStorageBlobRangeToRestore -StartRange container4 -EndRange container5

Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddMinutes(-30) `
    -BlobRestoreRange @($range1, $range2)
```

## <a name="next-steps"></a>Pasos siguientes

- [Restauración a un momento dado para blobs en bloques (versión preliminar)](point-in-time-restore-overview.md)
- [Eliminación temporal](soft-delete-overview.md)
- [Fuente de cambios (versión preliminar)](storage-blob-change-feed.md)
- [Control de versiones de blobs (versión preliminar)](versioning-overview.md)
