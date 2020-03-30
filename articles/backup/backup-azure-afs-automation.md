---
title: Copia de seguridad de Azure Files con PowerShell
description: En este artículo se aprende a realizar copias de seguridad de Azure Files mediante el servicio Azure Backup y PowerShell.
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: f85451e0da6458de34aea936836b46781f4c4a21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233952"
---
# <a name="back-up-azure-files-with-powershell"></a>Copia de seguridad de Azure Files con PowerShell

En este artículo se describe cómo usar Azure PowerShell para realizar una copia de seguridad de un recurso compartido de archivos de Azure Files mediante el almacén de Recovery Services de [Azure Backup](backup-overview.md).

En este artículo se explica lo siguiente:

> [!div class="checklist"]
>
> * Configurar PowerShell y registrar el proveedor de Azure Recovery Services.
> * Cree un almacén de Recovery Services.
> * Configurar la copia de seguridad de un recurso compartido de archivos de Azure.
> * Ejecutar un trabajo de copia de seguridad.

## <a name="before-you-start"></a>Antes de comenzar

* [Más información](backup-azure-recovery-services-vault-overview.md) sobre los almacenes de Recovery Services.
* Descubra las funcionalidades de vista previa para realizar una [copia de seguridad de recursos compartidos de archivos de Azure](backup-afs.md).
* Revise la jerarquía de objetos de PowerShell para Recovery Services.

## <a name="recovery-services-object-hierarchy"></a>Jerarquía de objetos de Recovery Services

En el diagrama siguiente, se resume la jerarquía de objetos.

![Jerarquía de objetos de Recovery Services](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Revise la **referencia de cmdlet** de [Az.RecoveryServices](/powershell/module/az.recoveryservices) en la biblioteca de Azure.

## <a name="set-up-and-install"></a>Configuración e instalación

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Configure PowerShell como sigue:

1. [Descargue la versión más reciente de Azure PowerShell](/powershell/azure/install-az-ps). La versión 1.0.0 es la versión mínima requerida.

> [!WARNING]
> La versión mínima de PS necesaria para la versión preliminar era "Az 1.0.0". Debido a los próximos cambios de disponibilidad general, la versión mínima de PS necesaria será "Az.RecoveryServices 2.6.0". Es muy importante actualizar todas las versiones de PS existentes a esta versión. De lo contrario, los scripts existentes se interrumpirán después de la disponibilidad general. Instalación de la versión mínima con los siguientes comandos de PS

```powershell
Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
```

2. Busque los cmdlets de PowerShell de Azure Backup con este comando:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Revise los alias y cmdlets de Azure Backup, Azure Site Recovery y el almacén de Recovery Services. Este es un ejemplo de lo que puede ver. No es la lista completa de los cmdlets.

    ![Lista de cmdlets de Recovery Services](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. Inicie sesión en su cuenta de Azure con el cmdlet **Connect-AzAccount**.
5. En la página web que aparece, se le pedirá que escriba las credenciales de su cuenta.

    * Como alternativa, puede incluir las credenciales de su cuenta como un parámetro en el cmdlet **Connect-AzAccount** mediante el parámetro **-Credential**.
    * Si usted es un asociado CSP que trabaja en nombre de un inquilino, especifique el cliente como inquilino mediante el uso del TenantID o del nombre de dominio principal de inquilino. Un ejemplo es **Connect-AzAccount -Tenant** fabrikam.com.

6. Asocie la suscripción que quiere usar con la cuenta, dado que una cuenta puede tener varias suscripciones.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Si utiliza Azure Backup por primera vez, use el cmdlet **Register-AzResourceProvider** para registrar el proveedor de Azure Recovery Services con su suscripción.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Compruebe que los proveedores se han registrado correctamente:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. En la salida del comando, compruebe que **RegistrationState** cambia a **Registered**. En caso contrario, vuelva a ejecutar el cmdlet **Register-AzResourceProvider**.

## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

El almacén de Recovery Services es un recurso de Resource Manager, por lo que deberá colocarlo dentro de un grupo de recursos. Puede usar un grupo de recursos existente o crear uno con el cmdlet **New-AzResourceGroup**. Al crear un grupo de recursos, especifique el nombre y la ubicación.

Siga estos pasos para crear un almacén de Recovery Services.

1. Un almacén se coloca en un grupo de recursos. Si no tiene un grupo de recursos, cree uno con [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0). En el ejemplo, se crea un grupo de recursos en la región Oeste de EE. UU.

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```

2. Use el cmdlet [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) para crear el almacén. Especifique para el almacén la misma ubicación del grupo de recursos.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. Especifique el tipo de redundancia para el almacenamiento.

   * Puede usar [almacenamiento con redundancia local](../storage/common/storage-redundancy-lrs.md) o [almacenamiento con redundancia geográfica](../storage/common/storage-redundancy-grs.md).
   * En el siguiente ejemplo se establece la opción **-BackupStorageRedundancy** para el cmdlet [Set-AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) para **testvault** establecida en **GeoRedundant**.

     ```powershell
     $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
     Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
     ```

### <a name="view-the-vaults-in-a-subscription"></a>Visualización de los almacenes de una suscripción

Para ver todos los almacenes de la suscripción, use [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0).

```powershell
Get-AzRecoveryServicesVault
```

La salida será similar a la siguiente. Observe que se proporcionan el grupo de recursos y la ubicación asociados.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

### <a name="set-the-vault-context"></a>Establecer el contexto de almacén

Almacene el objeto de almacén en una variable y establezca el contexto de almacén.

* Muchos de los cmdlets de Azure Backup requieren el objeto de almacén de Recovery Services como una entrada, por lo que es conveniente almacenar el objeto de almacén en una variable.
* El contexto de almacén es el tipo de los datos protegidos en el almacén. Establézcalo con [Set-AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0). Una vez que se haya establecido el contexto, se aplica a todos los cmdlets posteriores.

En el ejemplo siguiente se establece el contexto del almacén de **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Recuperación del identificador del almacén

Tenemos previsto dejar de usar la configuración del contexto de almacén según las directrices de Azure PowerShell. En su lugar, puede almacenar o recuperar el identificador del almacén y pasarlo a los comandos pertinentes. Por tanto, si no ha establecido el contexto de almacén o quiere especificar el comando que se va a ejecutar para un determinado almacén, pase el identificador de almacén como "-vaultID" a todos los comandos pertinentes de la manera siguiente:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultID $vaultID
```

## <a name="configure-a-backup-policy"></a>Configuración de una directiva de copia de seguridad

Una directiva de copia de seguridad especifica la programación para las copias de seguridad y cuánto tiempo se deben mantener los puntos de recuperación de copia de seguridad:

* Una directiva de copia de seguridad está asociada con al menos una directiva de retención. Una directiva de retención define el tiempo que se conserva un punto de recuperación antes de que se elimine.
* Vea la retención de la directiva de copia de seguridad predeterminada con [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0).
* Vea la programación de la directiva de copia de seguridad predeterminada con [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0).
* El cmdlet [New AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) se usa para crear una directiva de copia de seguridad. Introduzca los objetos de directiva de retención y programación.

De forma predeterminada, se define una hora de inicio en el objeto de directiva de programación. Use el siguiente ejemplo para cambiar la hora de inicio a la hora de inicio deseada. La hora de inicio deseada debe estar también en formato UTC. En el ejemplo siguiente se supone que la hora de inicio deseada es la 1:00 UTC para las copias de seguridad diarias.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$UtcTime = Get-Date -Date "2019-03-20 01:30:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Solo tiene que proporcionar la hora de inicio en múltiplos de 30 minutos. En el ejemplo anterior, solo puede ser "01:00:00" o "02:30:00". La hora de inicio no puede ser "01:15:00".

En el ejemplo siguiente se almacenan la directiva de programación y la directiva de retención en variables. Después usa estas variables como parámetros para una nueva directiva (**NewAFSPolicy**). **NewAFSPolicy** realiza una copia de seguridad diaria y la conserva durante 30 días.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

La salida será similar a la siguiente.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2019 1:30:00 AM
```

## <a name="enable-backup"></a>Habilitación de la copia de seguridad

Una vez que haya definido la directiva de copia de seguridad, puede habilitar la protección para el recurso compartido de archivos de Azure con esta directiva.

### <a name="retrieve-a-backup-policy"></a>Recuperación de una directiva de copia de seguridad

Recupere el objeto de directiva pertinente con [Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0). Use este cmdlet para obtener una directiva específica o para ver las directivas asociadas a un tipo de carga de trabajo.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>Recuperación de una directiva para un tipo de carga de trabajo

En el ejemplo siguiente se recuperan las directivas para el tipo de carga de trabajo **AzureFiles**.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

La salida será similar a la siguiente.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> La zona horaria del campo **BackupTime** de PowerShell es Hora universal coordinada (UTC). Cuando el tiempo de copia de seguridad se muestra en Azure Portal, la hora se ajusta a la zona horaria local.

### <a name="retrieve-a-specific-policy"></a>Recuperación de una directiva específica

La directiva siguiente recupera la directiva de copia de seguridad llamada **dailyafs**.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-backup-and-apply-policy"></a>Habilitación de la copia de seguridad y aplicación de la directiva

Habilite la protección con [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). Una vez que la directiva se ha asociado al almacén, las copias de seguridad se desencadenan según la programación de la directiva.

En el ejemplo siguiente se habilita la protección para el recurso compartido de archivos de Azure **testAzureFileShare**, en la cuenta de almacenamiento **testStorageAcct**, con la directiva **dailyafs**.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

El comando espera hasta que el trabajo de protección configurado termina y proporciona una salida similar, tal como se muestra.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

## <a name="important-notice---backup-item-identification-for-afs-backups"></a>Aviso importante: Identificación de elemento de copia de seguridad para copias de seguridad de AFS

En esta sección se esboza un cambio importante con respecto a la copia de seguridad de AFS en preparación para la disponibilidad general.

Al habilitar la copia de seguridad para AFS, el usuario proporciona el nombre del recurso compartido de archivos descriptivo del cliente como nombre de la entidad y se crea un elemento de copia de seguridad. El elemento "name" del elemento de copia de seguridad es un identificador único creado por el servicio Azure Backup. Normalmente, el identificador incluye el nombre descriptivo del usuario. Pero para controlar el importante escenario de eliminación temporal, donde se puede eliminar un recurso compartido de archivos y crear otro con el mismo nombre, la identidad única del recurso compartido de archivos de Azure ahora es un identificador en lugar de un nombre descriptivo de cliente. Para conocer el nombre o la identidad únicos de cada elemento, simplemente ejecute el comando ```Get-AzRecoveryServicesBackupItem``` con los filtros relevantes para backupManagementType y WorkloadType a fin de obtener todos los elementos pertinentes y luego observe el campo de nombre en el objeto o la respuesta devueltos de PS. Siempre se recomienda mostrar los elementos y luego recuperar su nombre único del campo "name" de la respuesta. Use este valor para filtrar los elementos con el parámetro "Name". De lo contrario, use el parámetro "FriendlyName" para recuperar el elemento con su identificador o nombre descriptivo de cliente.

> [!WARNING]
> Asegúrese de que la versión de PS se actualice a la versión mínima de "Az.RecoveryServices 2.6.0" para las copias de seguridad de AFS. Con esta versión, el filtro "friendlyName" está disponible para el comando ```Get-AzRecoveryServicesBackupItem```. Pase el nombre del recurso compartido de archivos de Azure al parámetro "friendlyName". Si pasa el nombre del recurso compartido de archivos de Azure al parámetro "Name", esta versión genera la advertencia de pasar este nombre descriptivo al parámetro "friendlyName". La no instalación de esta versión mínima puede dar lugar a un error en los scripts existentes. Instale la versión mínima de PS con el siguiente comando.

```powershell
Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
```

## <a name="trigger-an-on-demand-backup"></a>Desencadenamiento de una copia de seguridad a petición

Use [Backup-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem?view=azps-1.4.0) para ejecutar una copia de seguridad a petición para un recurso compartido de archivos protegido de Azure.

1. Recupere la cuenta de almacenamiento del contenedor del almacén que contenga los datos de copia de seguridad con [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer).
2. Para iniciar un trabajo de copia de seguridad, obtenga información sobre el recurso compartido de archivos de Azure con [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem).
3. Ejecute una copia de seguridad a petición con [Backup-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem).

Ejecute la copia de seguridad a petición de la manera siguiente:

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -FriendlyName "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

El comando devuelve un trabajo con un identificador del cual se puede realizar un seguimiento, como se indica en el ejemplo siguiente.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Las instantáneas de recursos compartidos de archivos de Azure se usan al realizar copias de seguridad, por lo que el trabajo normalmente se completa en el momento en que el comando devuelve esta salida.

### <a name="using-on-demand-backups-to-extend-retention"></a>Uso de copias de seguridad a petición para ampliar la retención

Las copias de seguridad a petición se pueden usar para conservar las instantáneas durante 10 años. Los programadores pueden usarse para ejecutar scripts de PowerShell a petición con la retención seleccionada. De este modo, se toman instantáneas a intervalos regulares cada semana, mes o año. Al tomar instantáneas periódicas, haga referencia a las [limitaciones de las copias de seguridad a petición](https://docs.microsoft.com/azure/backup/backup-azure-files-faq#how-many-on-demand-backups-can-i-take-per-file-share) mediante Azure Backup.

Si busca scripts de ejemplo, puede ver el script de ejemplo en GitHub (<https://github.com/Azure-Samples/Use-PowerShell-for-long-term-retention-of-Azure-Files-Backup>) con el runbook de Azure Automation que le permite programar las copias de seguridad de forma periódica y conservarlas hasta 10 años.

> [!WARNING]
> Asegúrese de que la versión de PS se actualice a la versión mínima de "Az.RecoveryServices 2.6.0" para las copias de seguridad de AFS de los runbooks de Automation. Tiene que reemplazar el antiguo módulo " AzureRM" por el módulo "Az". Con esta versión, el filtro "friendlyName" está disponible para el comando ```Get-AzRecoveryServicesBackupItem```. Pase el nombre del recurso compartido de archivos de Azure al parámetro "friendlyName". Si pasa el nombre del recurso compartido de archivos de Azure al parámetro "Name", esta versión genera la advertencia de pasar este nombre descriptivo al parámetro "friendlyName".

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información](backup-afs.md) sobre cómo realizar copias de seguridad de archivos de Azure Files en Azure Portal.
