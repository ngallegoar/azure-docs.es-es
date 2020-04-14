---
title: Características de seguridad para proteger cargas de trabajo en la nube
description: Aprenda a usar las características de seguridad de Azure Backup para que las copias de seguridad sean más seguras.
ms.topic: conceptual
ms.date: 09/13/2019
ms.openlocfilehash: bd7c86e18114513a264a0f9252589533fb7ff2d3
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668737"
---
# <a name="security-features-to-help-protect-cloud-workloads-that-use-azure-backup"></a>Características de seguridad para proteger cargas de trabajo en la nube mediante Azure Backup

Cada vez es mayor la preocupación que generan problemas de seguridad como malware, ransomware e intrusión. Estos problemas de seguridad pueden ser costosos, en términos de dinero y datos. Para protegerse contra dichos ataques, Azure Backup proporciona características de seguridad que protegen los datos de las copias de seguridad incluso después de su eliminación.

Una de estas características es la eliminación temporal. Con la eliminación temporal, aunque un individuo malintencionado elimine la copia de seguridad de una máquina virtual (o se eliminen por accidente datos de copia de seguridad), los datos de copia de seguridad se conservan durante 14 días adicionales, lo que permite la recuperación de ese elemento de copia de seguridad sin pérdida de datos. Esta retención adicional de 14 días de los datos de copia de seguridad en el estado "eliminación temporal" no acarrea ningún costo para el cliente. Azure cifra también todos los datos en reposo con copia de seguridad mediante [Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) para proteger aún más los datos.

La protección de eliminación temporal para máquinas virtuales de Azure está disponible con carácter general.

>[!NOTE]
>La eliminación temporal de SQL Server en máquinas virtuales de Azure y la eliminación temporal de SAP HANA en cargas de trabajo de máquinas virtuales de Azure están ahora disponible en versión preliminar.<br>
>Para suscribirse a la versión preliminar, escriba a AskAzureBackupTeam@microsoft.com.

## <a name="soft-delete"></a>Eliminación temporal

### <a name="soft-delete-for-vms"></a>Eliminación temporal para máquinas virtuales

La eliminación temporal de máquinas virtuales protege las copias de seguridad de las máquinas virtuales de una eliminación imprevista. Incluso después de que se eliminen las copias de seguridad, se conservan en estado de eliminación temporal durante 14 días adicionales.

> [!NOTE]
> La eliminación temporal solo protege los datos de copia de seguridad eliminados. Si se elimina una máquina virtual sin una copia de seguridad, la característica de eliminación temporal no conservará los datos. Todos los recursos deben protegerse con Azure Backup para garantizar una resistencia total.
>

### <a name="supported-regions"></a>Regiones admitidas

La eliminación temporal se admite actualmente en Centro-oeste de EE. UU., Este de Asia, Centro de Canadá, Este de Canadá, Centro de Francia, Sur de Francia, Centro de Corea del Sur, Sur de Corea del Sur, Sur de Reino Unido, Oeste de Reino Unido, Este de Australia, Sudeste de Australia, Norte de Europa, Oeste de EE. UU., Oeste de EE. UU. 2, Centro de EE. UU., Sudeste de Asia, Centro-norte de EE. UU., Centro-sur de EE. UU., Este de Japón, Oeste de Japón, Sur de India, Centro de la India, Oeste de la India, Este de EE. UU. 2, Norte de Suiza, Oeste de Suiza, Oeste de Noruega, Este de Noruega y todas las regiones nacionales.

### <a name="soft-delete-for-vms-using-azure-portal"></a>Eliminación temporal para máquinas virtuales con Azure Portal

1. Para eliminar los datos de copia de seguridad de una máquina virtual, se debe detener la copia de seguridad. En Azure Portal, vaya al almacén de Recovery Services, haga clic con el botón derecho en el elemento de copia de seguridad y seleccione **Detener copia de seguridad**.

   ![Captura de pantalla de elementos de copia de seguridad de Azure Portal](./media/backup-azure-security-feature-cloud/backup-stopped.png)

2. En la ventana siguiente, se le ofrecerá la opción de eliminar o conservar los datos de copia de seguridad. Si elige la opción **Eliminar datos de copia de seguridad** y, a continuación, **Detener copia de seguridad**, la copia de seguridad de la máquina virtual no se eliminará de forma permanente. En su lugar, los datos de copia de seguridad se conservarán durante 14 días en el estado de eliminación temporal. Si selecciona **Eliminar datos de copia de seguridad**, se enviará una alerta de eliminación por correo electrónico al identificador de correo electrónico configurado que informa al usuario de que quedan 14 días de retención ampliada para los datos de copia de seguridad. Además, a los doce días se envía una alerta por correo electrónico para informar de que quedan otros dos días para recuperar los datos eliminados. La eliminación se aplaza hasta el decimoquinto día, cuando se produce la eliminación permanente y se envía una alerta final por correo electrónico que informa de la eliminación permanente de los datos.

   ![Captura de pantalla de Azure Portal, pantalla Detener copia de seguridad](./media/backup-azure-security-feature-cloud/delete-backup-data.png)

3. Durante esos 14 días, en el almacén de Recovery Services, la máquina virtual eliminada temporalmente aparecerá con un icono rojo de "eliminación temporal" junto a ella.

   ![Captura de pantalla de Azure Portal, máquina virtual en estado de eliminación temporal](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

   > [!NOTE]
   > Si algún elemento de copia de seguridad eliminado temporalmente está presente en el almacén, no se podrá eliminar el almacén en ese momento. Pruebe a eliminar el almacén después de que se eliminen de forma permanente los elementos de copia de seguridad y no quede ningún elemento en estado de eliminación temporal en el almacén.

4. Para restaurar la máquina virtual eliminada temporalmente, hay que eliminarla primero. Para ello, seleccione la máquina virtual eliminada temporalmente y, a continuación, seleccione la opción **Recuperar**.

   ![Captura de pantalla de Azure Portal, recuperación de máquina virtual](./media/backup-azure-security-feature-cloud/choose-undelete.png)

   Aparecerá una ventana con una advertencia que indica que si se selecciona Recuperar, todos los puntos de restauración de la máquina virtual se recuperarán y estarán disponibles para realizar una operación de restauración. La máquina virtual se conservará en el estado "Detener la protección con conservación de datos" con las copias de seguridad en pausa y los datos de copia de seguridad conservados indefinidamente sin ninguna directiva de copia de seguridad en vigor.

   ![Captura de pantalla de Azure Portal, confirmación de recuperación de máquina virtual](./media/backup-azure-security-feature-cloud/undelete-vm.png)

   En este punto, también puede restaurar la máquina virtual seleccionando **Restaurar VM** desde el punto de restauración elegido.  

   ![Captura de pantalla de Azure Portal, opción Restaurar VM](./media/backup-azure-security-feature-cloud/restore-vm.png)

   > [!NOTE]
   > El recolector de elementos no utilizados ejecutará y limpiará los puntos de recuperación expirados solo después de que el usuario realice la operación de **reanudar copia de seguridad**.

5. Una vez completado el proceso de recuperación, el estado volverá a "Detener copia de seguridad con conservación de datos" y, a continuación, podrá seleccionar **Reanudar copia de seguridad**. La operación **Reanudar copia de seguridad** devuelve el elemento de copia de seguridad al estado activo, asociado a una directiva de copia de seguridad seleccionada por el usuario que define las programaciones de copia de seguridad y retención.

   ![Captura de pantalla de Azure Portal, opción Reanudar copia de seguridad](./media/backup-azure-security-feature-cloud/resume-backup.png)

En este diagrama de flujo se explican los diferentes pasos y estados de un elemento de copia de seguridad cuando se habilita la eliminación temporal:

![Ciclo de vida del elemento de copia de seguridad eliminado temporalmente](./media/backup-azure-security-feature-cloud/lifecycle.png)

Para más información, consulte la sección [Preguntas frecuentes](backup-azure-security-feature-cloud.md#frequently-asked-questions) más abajo.

### <a name="soft-delete-for-vms-using-azure-powershell"></a>Eliminación temporal para máquinas virtuales con Azure PowerShell

> [!IMPORTANT]
> La versión de Az.RecoveryServices necesaria para usar la eliminación temporal con Azure PS es, como mínimo, la 2.2.0. Use ```Install-Module -Name Az.RecoveryServices -Force``` para obtener la versión más reciente.

Tal y como se ha descrito anteriormente para Azure Portal, la secuencia de pasos es la misma cuando también se usa Azure PowerShell.

#### <a name="delete-the-backup-item-using-azure-powershell"></a>Eliminación del elemento de copia de seguridad con Azure PowerShell

Puede eliminar el elemento de copia de seguridad mediante el cmdlet de PS [Disable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0).

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

El elemento "DeleteState" del elemento de copia de seguridad cambiará de "NotDeleted" a "ToBeDeleted". Los datos de la copia de seguridad se conservarán durante 14 días. Si desea revertir la operación de eliminación, se debe realizar la operación undo-delete.

#### <a name="undoing-the-deletion-operation-using-azure-powershell"></a>Acción de deshacer la operación de eliminación mediante Azure PowerShell

En primer lugar, capture el elemento de copia de seguridad pertinente que se encuentra en estado de eliminación temporal (es decir, que se va a eliminar).

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

A continuación, realice la operación para deshacer la eliminación con el cmdlet de PS [Undo-AzRecoveryServicesBackupItemDeletion](https://docs.microsoft.com/powershell/module/az.recoveryservices/undo-azrecoveryservicesbackupitemdeletion?view=azps-3.1.0).

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

El elemento "DeleteState" del elemento de copia de seguridad se revertirá a "NotDeleted". Pero la protección sigue detenida. [Reanude la copia de seguridad](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#change-policy-for-backup-items) para volver a habilitar la protección.

### <a name="soft-delete-for-vms-using-rest-api"></a>Eliminación temporal para máquinas virtuales con API REST

- Elimine las copias de seguridad mediante la API REST, como se mencionó [aquí](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data).
- Si el usuario desea deshacer estas operaciones de eliminación, consulte los pasos mencionados [aquí](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data).

## <a name="disabling-soft-delete"></a>Deshabilitación de la eliminación temporal

La eliminación temporal se habilita de forma predeterminada en los almacenes recién creados para proteger los datos de copia de seguridad de eliminaciones accidentales o malintencionadas.  No se recomienda deshabilitar esta característica. La única circunstancia en la que debe considerar la posibilidad de deshabilitar la eliminación temporal es si está planeando mover los elementos protegidos a un nuevo almacén y no puede esperar los 14 días necesarios para realizar la acción de eliminar y volver a proteger (por ejemplo, en un entorno de prueba). Solo el propietario del almacén puede deshabilitar esta característica. Si se deshabilita, todas las eliminaciones posteriores de elementos protegidos se convertirán en eliminaciones inmediatas, sin la posibilidad de restaurar. Los datos de copia de seguridad que se encuentren en estado de eliminación temporal antes de deshabilitar esta característica permanecerán en ese estado durante el período de 14 días. Si quiere eliminarlos permanentemente de inmediato, debe recuperarlos y eliminarlos de nuevo para eliminarlos de forma permanente.

### <a name="disabling-soft-delete-using-azure-portal"></a>Deshabilitación de la eliminación temporal con Azure Portal

Para deshabilitar la eliminación temporal, siga estos pasos:

1. En Azure Portal, vaya a su almacén y luego a **Configuración** -> **Propiedades**.
2. En el panel Propiedades, seleccione **Configuración de seguridad** -> **Actualizar**.  
3. En el panel Configuración de seguridad, en **Eliminación temporal**, seleccione **Deshabilitar**.

![Deshabilitación de la eliminación temporal](./media/backup-azure-security-feature-cloud/disable-soft-delete.png)

### <a name="disabling-soft-delete-using-azure-powershell"></a>Deshabilitación de la eliminación temporal con Azure PowerShell

> [!IMPORTANT]
> La versión de Az.RecoveryServices necesaria para usar la eliminación temporal con Azure PS es, como mínimo, la 2.2.0. Use ```Install-Module -Name Az.RecoveryServices -Force``` para obtener la versión más reciente.

Para deshabilitar, use el cmdlet de PS [Set-AzRecoveryServicesVaultBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty?view=azps-3.1.0).

```powershell
Set-AzRecoveryServicesVaultProperty -VaultId $myVaultID -SoftDeleteFeatureState Disable


StorageModelType       :
StorageType            :
StorageTypeState       :
EnhancedSecurityState  : Enabled
SoftDeleteFeatureState : Disabled
```

### <a name="disabling-soft-delete-using-rest-api"></a>Deshabilitación de la eliminación temporal con API REST

Para deshabilitar la funcionalidad de eliminación temporal mediante la API REST, consulte los pasos mencionados [aquí](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api).

## <a name="permanently-deleting-soft-deleted-backup-items"></a>Eliminar permanentemente los elementos de copia de seguridad eliminados temporalmente

Los datos de copia de seguridad con el estado de eliminación temporal antes de deshabilitar esta característica continuarán en ese estado. Si quiere eliminarlos de permanentemente de inmediato, restáurelos y vuelva a eliminarlos para eliminarlos de forma permanente.

### <a name="using-azure-portal"></a>Uso de Azure Portal

Siga estos pasos:

1. Siga los pasos para [deshabilitar la eliminación temporal](#disabling-soft-delete).
2. En Azure Portal, vaya al almacén, a **Elementos de copia de seguridad** y elija la máquina virtual eliminada temporalmente.

   ![Selección de una máquina virtual eliminada temporalmente](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

3. Seleccione la opción **Recuperar**.

   ![Elegir Recuperar](./media/backup-azure-security-feature-cloud/choose-undelete.png)

4. Se mostrará una ventana. Seleccione **Recuperar**.

   ![Seleccionar Recuperar](./media/backup-azure-security-feature-cloud/undelete-vm.png)

5. Elija **Eliminar datos de la copia de seguridad** para eliminar los datos de copia de seguridad de forma permanente.

   ![Elegir Eliminar datos de la copia de seguridad](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-buttom.png)

6. Escriba el nombre del elemento de copia de seguridad para confirmar que desea eliminar los puntos de recuperación.

   ![Escritura del nombre del elemento de copia de seguridad](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-data1.png)

7. Para eliminar los datos de copia de seguridad para el elemento, seleccione **Eliminar**. Un mensaje de notificación le confirma que se han eliminado los datos de copia de seguridad.

### <a name="using-azure-powershell"></a>Uso de Azure PowerShell

Si los elementos se eliminaron antes de que se deshabilitara la eliminación temporal, se encontrarán en un estado de eliminación temporal. Para eliminarlos de inmediato, la operación de eliminación debe invertirse y volver a ejecutarse.

Identifique los elementos que se encuentran en estado de eliminación temporal.

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

A continuación, invierta la operación de eliminación que se realizó cuando la eliminación temporal estaba habilitada.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Dado que la eliminación temporal ahora está deshabilitada, la operación de eliminación producirá la eliminación inmediata de los datos de la copia de seguridad.

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

### <a name="using-rest-api"></a>Uso de la API de REST

Si los elementos se eliminaron antes de que se deshabilitara la eliminación temporal, se encontrarán en un estado de eliminación temporal. Para eliminarlos de inmediato, la operación de eliminación debe invertirse y volver a ejecutarse.

1. En primer lugar, deshaga las operaciones de eliminación con los pasos mencionados [aquí](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data).
2. Después, deshabilite la funcionalidad de eliminación temporal mediante la API REST siguiendo los pasos mencionados [aquí](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api).
3. Después, elimine las copias de seguridad mediante la API REST, como se mencionó [aquí](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data).

## <a name="encryption"></a>Cifrado

Todos los datos de copia de seguridad se cifran automáticamente cuando se almacenan en la nube mediante el cifrado de Azure Storage, lo que le ayuda a cumplir los compromisos de seguridad y cumplimiento. Los datos en reposo se cifran mediante el cifrado AES de 256 bits, uno de los cifrados de bloques más seguros que hay disponibles y que es compatible con FIPS 140-2.

Además del cifrado en reposo, todos los datos de copia de seguridad en tránsito se transfieren a través de HTTPS y permanecen siempre en la red troncal de Azure.

Para más información, consulte [Cifrado de Azure Storage para datos en reposo](https://docs.microsoft.com/azure/storage/common/storage-service-encryption). Consulte las [preguntas más frecuentes sobre Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#encryption) para obtener respuesta a las dudas que pueda tener sobre el cifrado.

### <a name="encryption-of-backup-data-using-platform-managed-keys"></a>Cifrado de datos de copia de seguridad mediante claves administradas por la plataforma

De forma predeterminada, todos los datos se cifran mediante claves administradas por la plataforma. No es necesario que realice ninguna acción explícita por su parte para habilitar este cifrado y se aplica a todas las cargas de trabajo de las que se realiza una copia de seguridad en el almacén de Recovery Services.

### <a name="encryption-of-backup-data-using-customer-managed-keys"></a>Cifrado de datos de copia de seguridad mediante claves administradas por el cliente

Al realizar una copia de seguridad de Azure Virtual Machines, ahora puede cifrar los datos mediante las claves que posee y administra. Azure Backup le permite usar las claves RSA almacenadas en Azure Key Vault para cifrar las copias de seguridad. La clave de cifrado utilizada para cifrar las copias de seguridad puede ser diferente de la que se usa para el origen. Los datos se protegen mediante una clave de cifrado de datos (DEK) basada en AES 256, que, a su vez, está protegida con las claves del usuario. Esto le proporciona un control total sobre los datos y las claves. Para permitir el cifrado, se requiere que el almacén de Recovery Services tenga acceso a la clave de cifrado en Azure Key Vault. Puede deshabilitar la clave o revocar el acceso siempre que sea necesario. Sin embargo, debe habilitar el cifrado con las claves antes de intentar proteger los elementos en el almacén.

>[!NOTE]
>Actualmente, esta característica se encuentra en versión preliminar limitada. Rellene [esta encuesta](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapURE9TTDRIUEUyNFhNT1lZS1BNVDdZVllHWi4u) y envíenos un correo electrónico a AskAzureBackupTeam@microsoft.com si quiere cifrar los datos de copia de seguridad mediante claves administradas por el cliente. Tenga en cuenta que la posibilidad de utilizar esta característica está sujeta a la aprobación del servicio Azure Backup.

### <a name="backup-of-managed-disk-vms-encrypted-using-customer-managed-keys"></a>Copia de seguridad de las máquinas virtuales de disco administrado cifradas con claves administradas por el cliente

Azure Backup también le permite realizar copias de seguridad de las máquinas virtuales de Azure que usan la clave para el cifrado del lado servidor. La clave que se usa para cifrar los discos se almacena en Azure Key Vault y la administra el usuario. El cifrado del lado servidor mediante claves administradas por el cliente difiere de Azure Disk Encryption, ya que ADE aprovecha BitLocker (para Windows) y DM-Crypt (para Linux) a fin de realizar el cifrado en invitado, y SSE cifra los datos en el servicio de almacenamiento, lo que permite usar cualquier sistema operativo o imagen para las máquinas virtuales. Para obtener más información, consulte el artículo sobre [cifrado de discos administrados con claves administradas por el cliente](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys).

### <a name="backup-of-vms-encrypted-using-ade"></a>Copia de seguridad de máquinas virtuales cifradas mediante ADE

Con Azure Backup, también puede hacer una copia de seguridad de las máquinas virtuales de Azure que tienen el sistema operativo o los discos de datos cifrados mediante Azure Disk Encryption. ADE usa BitLocker para las VM Windows y DM-Crypt para las VM Linux a fin de realizar el cifrado de invitado. Para obtener instrucciones, consulte [Copia de seguridad y restauración de máquinas virtuales cifradas con Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

## <a name="private-endpoints"></a>Puntos de conexión privados

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

## <a name="other-security-features"></a>Otras características de seguridad

### <a name="protection-of-azure-backup-recovery-points"></a>Protección de los puntos de recuperación de Azure Backup

Las cuentas de almacenamiento usadas por los almacenes de Recovery Services están aisladas y son inaccesibles para usuarios con fines malintencionados. Solo se permite el acceso mediante operaciones de administración de Azure Backup, como la restauración. Estas operaciones de administración se controlan mediante el control de acceso basado en rol (RBAC).

Para más información, consulte [Uso del control de acceso basado en roles para administrar puntos de recuperación de Azure Backup](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="for-soft-delete"></a>Para la eliminación temporal

#### <a name="do-i-need-to-enable-the-soft-delete-feature-on-every-vault"></a>¿Es necesario habilitar la característica de eliminación temporal en cada almacén?

No, se crea y habilita de forma predeterminada para todos los almacenes de Recovery Services.

#### <a name="can-i-configure-the-number-of-days-for-which-my-data-will-be-retained-in-soft-deleted-state-after-delete-operation-is-complete"></a>¿Se puede configurar el número de días durante los que se conservarán los datos en estado de eliminación temporal tras completar la operación de eliminación?

No, este periodo está fijado en 14 días de retención adicional después de la operación de eliminación.

#### <a name="do-i-need-to-pay-the-cost-for-this-additional-14-day-retention"></a>¿Hay que pagar el costo de esta retención adicional de 14 días?

No, esta retención adicional de 14 días es gratuita como parte de la funcionalidad de eliminación temporal.

#### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-delete-state"></a>¿Puedo realizar una operación de restauración si los datos están en estado de eliminación temporal?

No, debe recuperar el recurso eliminado temporalmente para poder restaurarlo. La operación de recuperación devolverá el recurso al estado **Detener la protección con conservación de datos**, donde puede realizar la restauración a cualquier momento dado. El recolector de elementos no utilizados permanece en pausa en este estado.

#### <a name="will-my-snapshots-follow-the-same-lifecycle-as-my-recovery-points-in-the-vault"></a>¿Las instantáneas seguirán el mismo ciclo de vida que los puntos de recuperación en el almacén?

Sí.

#### <a name="how-can-i-trigger-the-scheduled-backups-again-for-a-soft-deleted-resource"></a>¿Cómo puedo volver a desencadenar las copias de seguridad programadas para un recurso eliminado temporalmente?

La recuperación seguida de la operación de reanudación volverá a proteger el recurso. La operación de reanudación asocia una directiva de copia de seguridad para desencadenar las copias de seguridad programadas con el período de retención seleccionado. Además, el recolector de elementos no utilizados se ejecuta tan pronto como se completa la operación de reanudación. Si quiere realizar una restauración desde un punto de recuperación que supere su fecha de expiración, le recomendamos que lo haga antes de desencadenar la operación de reanudación.

#### <a name="can-i-delete-my-vault-if-there-are-soft-deleted-items-in-the-vault"></a>¿Puedo eliminar mi almacén si contiene elementos eliminados temporalmente?

No es posible eliminar el almacén de Recovery Services si contiene elementos de copia de seguridad en estado de eliminación temporal. Los elementos eliminados temporalmente se eliminan de forma permanente 14 días después de la operación de eliminación. Si no puede esperar 14 días, [deshabilite la eliminación temporal](#disabling-soft-delete), recupere los elementos eliminados temporalmente y elimínelos de nuevo para eliminarlos de forma permanente. Después de asegurarse de que no hay elementos protegidos o eliminados temporalmente en el almacén, puede eliminarlo.  

#### <a name="can-i-delete-the-data-earlier-than-the-14-days-soft-delete-period-after-deletion"></a>¿Puedo eliminar los datos antes del período de eliminación temporal de 14 días posterior a la eliminación?

No. No se puede forzar la eliminación de los elementos eliminados temporalmente. Estos se eliminan automáticamente al cabo de 14 días. Esta característica de seguridad está habilitada para proteger los datos de copia de seguridad de eliminaciones accidentales o malintencionadas.  Debe esperar 14 días antes de realizar cualquier otra acción en la máquina virtual.  Los elementos eliminados temporalmente no se cobrarán.  Si tiene que volver a proteger las máquinas virtuales marcadas para eliminación temporal en un plazo de 14 días en un nuevo almacén, póngase en contacto con Soporte técnico de Microsoft.

#### <a name="can-soft-delete-operations-be-performed-in-powershell-or-cli"></a>¿Se pueden realizar operaciones de eliminación temporal en PowerShell o la CLI?

Las operaciones de eliminación temporal se pueden realizar mediante [PowerShell](#soft-delete-for-vms-using-azure-powershell). Actualmente no se admite la CLI.

#### <a name="is-soft-delete-supported-for-other-cloud-workloads-like-sql-server-in-azure-vms-and-sap-hana-in-azure-vms"></a>¿Se admite la eliminación temporal para otras cargas de trabajo en la nube, como SQL Server en máquinas virtuales de Azure y SAP HANA en máquinas virtuales de Azure?

No. Actualmente, la eliminación temporal solo es compatible con máquinas virtuales de Azure.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [Controles de seguridad para Azure Backup](backup-security-controls.md).
