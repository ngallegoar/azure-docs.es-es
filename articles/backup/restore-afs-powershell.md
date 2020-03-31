---
title: Restauración de Azure Files con PowerShell
description: En este artículo se aprende a restaurar Azure Files mediante el servicio Azure Backup y PowerShell.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 99aeaa6173bb5336e6e1719a9fc0df0c668374e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086827"
---
# <a name="restore-azure-files-with-powershell"></a>Restauración de Azure Files con PowerShell

En este artículo se explica cómo restaurar un recurso compartido de archivos completo, o archivos específicos, desde un punto de restauración creado por el servicio [Azure Backup](backup-overview.md) mediante Azure PowerShell.

Puede restaurar un recurso compartido de archivos al completo o archivos específicos de dicho recurso compartido. La restauración puede llevarse a cabo en la ubicación original o en una ubicación alternativa.

> [!WARNING]
> Asegúrese de que la versión de PS se actualice a la versión mínima de "Az.RecoveryServices 2.6.0" para las copias de seguridad de AFS. Para obtener más información, consulte [la sección](backup-azure-afs-automation.md#important-notice---backup-item-identification-for-afs-backups) que describe el requisito de este cambio.

## <a name="fetch-recovery-points"></a>Captura de puntos de recuperación

Use [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint?view=azps-1.4.0) para enumerar todos los puntos de recuperación del elemento del que se ha realizado la copia de seguridad.

En el script siguiente:

* La variable **$rp**es una matriz de puntos de recuperación para el elemento de copia de seguridad seleccionado de los últimos siete días.
* La matriz se ordena en orden inverso de tiempo con el punto de recuperación más reciente en el índice **0**.
* Use la indexación de matrices de PowerShell estándar para seleccionar el punto de recuperación.
* En el ejemplo, **$rp[0]** selecciona el último punto de recuperación.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

La salida será similar a la siguiente.

```powershell
FileShareSnapshotUri : https://testStorageAcct.file.core.windows.net/testAzureFS?sharesnapshot=2018-11-20T00:31:04.00000
                       00Z
RecoveryPointType    : FileSystemConsistent
RecoveryPointTime    : 11/20/2018 12:31:05 AM
RecoveryPointId      : 86593702401459
ItemName             : testAzureFS
Id                   : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Micros                      oft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;teststorageRG;testStorageAcct/protectedItems/AzureFileShare;testAzureFS/recoveryPoints/86593702401462
WorkloadType         : AzureFiles
ContainerName        : storage;teststorageRG;testStorageAcct
ContainerType        : AzureStorage
BackupManagementType : AzureStorage
```

Una vez seleccionado el punto de recuperación adecuado, restaure el recurso compartido de archivos o el archivo en la ubicación original o en una ubicación alternativa.

## <a name="restore-an-azure-file-share-to-an-alternate-location"></a>Restauración de un recurso compartido de archivos de Azure en una ubicación alternativa

Use [Restore-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) para restaurar en el punto de recuperación seleccionado. Especifique estos parámetros para identificar la ubicación alternativa:

* **TargetStorageAccountName**: La cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad. La cuenta de almacenamiento de destino debe estar en la misma ubicación que el almacén.
* **TargetFileShareName**: Los recursos compartidos de archivos de la cuenta de almacenamiento de destino en la que se restaura el contenido de la copia de seguridad.
* **TargetFolder**: La carpeta del recurso compartido de archivos en la que se restauran los datos. Si el contenido de la copia de seguridad debe restaurarse en una carpeta raíz, asigne los valores de la carpeta de destino como una cadena vacía.
* **ResolveConflict**: Instrucción en caso de conflicto con los datos restaurados. Acepta **Overwrite** o **Skip**.

Ejecute el cmdlet con los parámetros de la manera siguiente:

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
```

El comando devuelve un trabajo con un identificador del cual se puede realizar un seguimiento, como se indica en el ejemplo siguiente.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
```

## <a name="restore-an-azure-file-to-an-alternate-location"></a>Restauración de un archivo de Azure en una ubicación alternativa

Use [Restore-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) para restaurar en el punto de recuperación seleccionado. Especifique estos parámetros para identificar la ubicación alternativa y para identificar de forma única el archivo que quiere restaurar.

* **TargetStorageAccountName**: La cuenta de almacenamiento en la que se restaura el contenido de la copia de seguridad. La cuenta de almacenamiento de destino debe estar en la misma ubicación que el almacén.
* **TargetFileShareName**: Los recursos compartidos de archivos de la cuenta de almacenamiento de destino en la que se restaura el contenido de la copia de seguridad.
* **TargetFolder**: La carpeta del recurso compartido de archivos en la que se restauran los datos. Si el contenido de la copia de seguridad debe restaurarse en una carpeta raíz, asigne los valores de la carpeta de destino como una cadena vacía.
* **SourceFilePath**: La ruta de acceso absoluta del archivo que se va a restaurar en el recurso compartido de archivos, como una cadena. Esta ruta de acceso es la misma ruta utilizada en el cmdlet de PowerShell **Get-AzStorageFile**.
* **SourceFileType**: Indica si se ha seleccionado un directorio o un archivo. Admite **Directory** o **File**.
* **ResolveConflict**: Instrucción en caso de conflicto con los datos restaurados. Acepta **Overwrite** o **Skip**.

Los parámetros adicionales (SourceFilePath y SourceFileType) solo están relacionadas con el archivo individual que quiere restaurar.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Este comando devuelve un trabajo con un identificador del cual se puede realizar un seguimiento, como se indica en la sección anterior.

## <a name="restore-azure-file-shares-and-files-to-the-original-location"></a>Restauración de archivos y recursos compartidos de archivos de Azure en la ubicación original

Al restaurar en una ubicación original, no es necesario especificar parámetros relacionados con el destino. Solo debe proporcionarse **ResolveConflict**.

#### <a name="overwrite-an-azure-file-share"></a>Sobrescribir un recurso compartido de archivos de Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Sobrescribir un archivo de Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a](restore-afs.md) restaurar Azure Files en Azure Portal.
