---
title: 'Preguntas frecuentes: Copias de seguridad de máquinas virtuales de Azure'
description: En este artículo, descubra las respuestas a preguntas comunes sobre la copia de seguridad de máquinas virtuales de Azure con el servicio Azure Backup.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: 5d2f702b49e1e7aeb2ab33008556e91264b39427
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705418"
---
# <a name="frequently-asked-questions-back-up-azure-vms"></a>Preguntas más frecuentes sobre la copia de seguridad de máquinas virtuales de Azure

En este artículo se responde a preguntas comunes sobre la copia de seguridad de máquinas virtuales de Azure con el servicio [Azure Backup](backup-introduction-to-azure-backup.md).

## <a name="backup"></a>Copia de seguridad

### <a name="which-vm-images-can-be-enabled-for-backup-when-i-create-them"></a>¿Qué imágenes de máquina virtual se pueden habilitar para la copia de seguridad cuando se crean?

Cuando se crea una máquina virtual, puede habilitar la copia de seguridad de máquinas virtuales que ejecutan [sistemas operativos admitidos](backup-support-matrix-iaas.md#supported-backup-actions).

### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>¿Están los costos de la copia de seguridad incluidos en el costo de la máquina virtual?

No. Los costos de la copia de seguridad son independientes de los costos de la máquina virtual. Más información sobre los [Precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

### <a name="which-permissions-are-required-to-enable-backup-for-a-vm"></a>¿Qué permisos son necesarios para habilitar la copia de seguridad de una máquina virtual?

Si es un colaborador de la máquina virtual, puede habilitar la copia de seguridad de la máquina virtual. Si utiliza un rol personalizado, necesita los permisos siguientes para habilitar la copia de seguridad en la máquina virtual:

- Microsoft.RecoveryServices/Vaults/write
- Microsoft.RecoveryServices/Vaults/read
- Microsoft.RecoveryServices/locations/*
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write
- Microsoft.RecoveryServices/Vaults/backupPolicies/read
- Microsoft.RecoveryServices/Vaults/backupPolicies/write

Si el almacén de Recovery Services y la máquina virtual tienen distintos grupos de recursos, asegúrese de que tiene permisos de escritura en el grupo de recursos del almacén de Recovery Services.  

### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>¿Usa un trabajo de copia de seguridad a petición la misma programación de retención que las copias de seguridad programadas?

No. Debe especificar el intervalo de retención para un trabajo de copia de seguridad a petición. De forma predeterminada, se conservará durante 30 días si se desencadena desde el portal.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Recientemente habilité Azure Disk Encryption en algunas máquinas virtuales. ¿Seguirán funcionando mis copias de seguridad?

Para acceder a Key Vault, debe conceder permisos para el servicio Azure Backup. Especifique los permisos en PowerShell como se indica en la sección **Habilitar copia de seguridad** de la documentación sobre [PowerShell de Azure Backup](backup-azure-vms-automation.md).

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>Migré los discos de una máquina virtual a discos administrados. ¿Seguirán funcionando mis copias de seguridad?

Sí, las copias de seguridad funcionan sin problemas. No hay necesidad de volver a configurar nada.

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>¿Por qué no aparece mi máquina virtual en el asistente para configuración de la copia de seguridad?

El asistente solo muestra las máquinas virtuales que están en la misma región que el almacén y de las que no se está realizando copia de seguridad.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Mi máquina virtual está apagada. ¿Funcionará un trabajo de copia de seguridad a petición o programado?

Sí. Las copias de seguridad se ejecutan cuando una máquina está apagada. El punto de recuperación se marca como coherente frente a bloqueos.

### <a name="can-i-cancel-an-in-progress-backup-job"></a>¿Puedo cancelar un trabajo de copia de seguridad en curso?

Sí. Puede cancelar el trabajo de copia de seguridad si se encuentra en el estado **Tomando instantánea**. No puede cancelar un trabajo si la transferencia de datos desde la instantánea está en curso.

### <a name="i-enabled-lock-on-resource-group-created-by-azure-backup-service-ie-azurebackuprg_geo_number-will-my-backups-continue-to-work"></a>He habilitado un bloqueo en el grupo de recursos creado por el servicio Azure Backup (por ejemplo, `AzureBackupRG_<geo>_<number>`), ¿seguirán funcionando mis copias de seguridad?

Si bloquea el grupo de recursos creado por el servicio Azure Backup, las copias de seguridad comenzarán a producir errores porque hay un límite máximo de 18 puntos de restauración.

El usuario debe eliminar el bloqueo y borrar la colección de puntos de restauración de ese grupo de recursos para que las futuras copias de seguridad sean correctas; [siga estos pasos](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal) para eliminar la colección de puntos de restauración.

### <a name="does-azure-backup-support-standard-ssd-managed-disks"></a>¿Admite Azure Backup los discos administrados SSD estándar?

Sí, Azure Backup admite [discos administrados SSD estándar](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/).

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>¿Podemos realizar copias de seguridad de una máquina virtual con un disco habilitado para el Acelerador de escritura?

No se pueden tomar instantáneas en un disco habilitado para el Acelerador de escritura. No obstante, el servicio Azure Backup puede excluir este tipo de disco de la copia de seguridad.

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>Tengo una máquina virtual con discos habilitados para el Acelerador de escritura (WA) y SAP HANA instalada. ¿Cómo puedo realizar la copia de seguridad?

Azure Backup no puede realizar la copia de seguridad de un disco habilitado para el Acelerador de escritura pero puede excluirlo de la copia. Sin embargo, la copia de seguridad no proporcionará coherencia de base de datos ya que no se ha realizado la copia de seguridad de la información contenida en este disco. Puede realizar copias de seguridad de discos con esta configuración si desea realizar la copia de seguridad de un disco de sistema operativo y la copia de seguridad de aquellos discos que no estén habilitados para el Acelerador de escritura.

Tenemos en ejecución una versión preliminar privada para una copia de seguridad de SAP HANA con un RPO de 15 minutos. Se ha creado de modo similar a la copia de seguridad de bases de datos SQL y usa la interfaz backInt para soluciones de terceros certificadas por SAP HANA. Si está interesado, envíenos un correo electrónico a `AskAzureBackupTeam@microsoft.com` con el asunto **Suscripción a la versión preliminar privada de la copia de seguridad de SAP HANA en máquinas virtuales de Azure**.

### <a name="what-is-the-maximum-delay-i-can-expect-in-backup-start-time-from-the-scheduled-backup-time-i-have-set-in-my-vm-backup-policy"></a>¿Cuál es el retraso máximo que puedo esperar en la hora de inicio de la copia de seguridad desde la hora de copia de seguridad programada que he configurado en mi directiva de copia de seguridad de máquina virtual?

La copia de seguridad programada se desencadenará en las 2 horas siguientes a la hora de la copia de seguridad programada. Por ejemplo, si la hora de inicio de la copia de seguridad de 100 máquinas virtuales está programada a las 2:00 a.m., el trabajo de copia de seguridad de las 100 máquinas virtuales estará en curso como máximo a las 4:00 a.m. Si las copias de seguridad programadas han estado en pausa debido a una interrupción y se han reanudado o vuelto a intentar, la copia de seguridad puede comenzar fuera de esta ventana de dos horas programadas.

### <a name="what-is-the-minimum-allowed-retention-range-for-daily-backup-point"></a>¿Cuál es el intervalo de retención mínimo permitido para el punto de copia de seguridad diaria?

La directiva de copia de seguridad de máquina virtual de Azure admite un intervalo de retención mínimo de siete días hasta 9999 días. Cualquier modificación de una directiva de copia de seguridad de máquina virtual existente con menos de siete días requerirá una actualización para cumplir el intervalo mínimo de retención de siete días.

### <a name="can-i-backup-or-restore-selective-disks-attached-to-a-vm"></a>¿Puedo realizar copias de seguridad o restaurar discos selectivos conectados a una máquina virtual?

Azure Backup admite ahora la copia de seguridad y restauración de discos selectivos mediante la solución de copia de seguridad de máquinas virtuales de Azure.

En la actualidad, Azure Backup admite la copia de seguridad de todos los discos (sistema operativo y datos) en una máquina virtual junto con la solución de copia de seguridad de máquinas virtuales. Con la funcionalidad de exclusión de disco, tiene la opción de realizar una copia de seguridad de uno o de varios de los múltiples discos de datos de una máquina virtual. Esto proporciona una solución eficaz y rentable para sus necesidades de copia de seguridad y restauración. Cada punto de recuperación contiene datos de los discos incluidos en la operación de copia de seguridad, lo que permite además tener un subconjunto de discos restaurados desde el punto de recuperación determinado durante la operación de restauración. Esto se aplica a la restauración tanto desde la instantánea como desde el almacén.

Para suscribirse a la versión preliminar, escriba a AskAzureBackupTeam@microsoft.com.

## <a name="restore"></a>Restauración

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>¿Cómo decidir si es necesario restaurar solo los discos o una máquina virtual completa?

Considere la restauración de máquinas virtuales como una opción de creación rápida para una máquina virtual de Azure. Esta opción cambia los nombres de los discos, los contenedores usados por esos discos, las direcciones IP públicas y los nombres de las interfaces de red. El cambio conserva recursos exclusivos cuando se crea una máquina virtual. La máquina virtual no se agrega a un conjunto de disponibilidad.

Puede usar la opción de restauración de disco si quiere:

- Personalizar la máquina virtual que se crea. Por ejemplo, cambiar el tamaño.
- Agregar opciones de configuración que no existían en el momento de la copia de seguridad.
- Controlar la convención de nomenclatura para los recursos que se crean.
- Agregar la máquina virtual a un conjunto de disponibilidad.
- Agregar cualquier otro valor que se deba configurar mediante PowerShell o una plantilla.

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>¿Puedo restaurar las copias de seguridad de discos de máquinas virtuales no administrados después de actualizar a discos administrados?

Sí, puede usar las copias de seguridad tomadas antes de migrar los discos de no administrados a administrados.

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>¿Cómo restauro una máquina virtual a un punto de restauración anterior a la migración de la máquina virtual a discos administrados?

El proceso de restauración sigue siendo el mismo. Si el punto de recuperación es de un momento dado en el que la máquina virtual tenía discos no administrados, puede [restaurar discos como no administrados](tutorial-restore-disk.md#unmanaged-disks-restore). Si la máquina virtual tuviera discos administrados, puede [restaurar discos como discos administrados](tutorial-restore-disk.md#managed-disk-restore). Luego puede [crear una máquina virtual a partir de esos discos](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk).

[Más información](backup-azure-vms-automation.md#restore-an-azure-vm) sobre cómo hacer esto en PowerShell.

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>¿Puedo restaurar la máquina virtual que se ha eliminado?

Sí. Incluso si elimina la máquina virtual, puede ir al elemento de la copia de seguridad correspondiente en el almacén y realizar la restauración desde un punto de recuperación.

### <a name="how-to-restore-a-vm-to-the-same-availability-sets"></a>¿Cómo restaurar una máquina virtual en los mismos conjuntos de disponibilidad?

Para una máquina virtual de Azure con discos administrados, se puede habilitar la restauración en los conjuntos de disponibilidad proporcionando una opción en una plantilla mientras se restaura como discos administrados. Esta plantilla tiene el parámetro de entrada denominado **Conjuntos de disponibilidad**.

### <a name="how-do-we-get-faster-restore-performances"></a>¿Cómo se consiguen rendimientos de restauración más rápidos?

La funcionalidad [Restauración instantánea](backup-instant-restore-capability.md) ayuda en copias de seguridad y restauraciones de las instantáneas más rápidas.

### <a name="what-happens-when-we-change-the-key-vault-settings-for-the-encrypted-vm"></a>¿Qué ocurre cuando se cambia la configuración de Key Vault para la máquina virtual cifrada?

Después de cambiar la configuración de KeyVault para la máquina virtual cifrada, las copias de seguridad seguirán funcionando con el nuevo conjunto de detalles. Sin embargo, después de la restauración desde un punto de recuperación antes del cambio, tendrá que restaurar los secretos en un KeyVault antes de poder crear la máquina virtual a partir de él. Para más información, consulte este [artículo](https://docs.microsoft.com/azure/backup/backup-azure-restore-key-secret).

Las operaciones como la sustitución de claves o secretos no requieren este paso y se puede usar la misma instancia de Key Vault después de la restauración.

### <a name="can-i-access-the-vm-once-restored-due-to-a-vm-having-broken-relationship-with-domain-controller"></a>¿Puedo acceder a la máquina virtual una vez que se ha restaurado porque una máquina virtual tiene una relación rota con el controlador de dominio?

Si, puede acceder a la máquina virtual una vez que se haya restaurado porque una máquina virtual tiene una relación rota con el controlador de dominio. Para más información, consulte este [artículo](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#post-restore-steps).

## <a name="manage-vm-backups"></a>Administrar copias de seguridad de máquina virtual

### <a name="what-happens-if-i-modify-a-backup-policy"></a>¿Qué ocurre si se modifica una directiva de copia de seguridad?

Se realiza la configuración de la máquina virtual con los valores de programación y retención de la directiva nueva o modificada.

- Si se amplía la retención, los puntos de recuperación existentes se marcarán para mantenerlos de acuerdo con la nueva directiva.
- Si se reduce la retención, se marcarán para eliminarse y, posteriormente, se eliminarán en el siguiente trabajo de limpieza.

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>¿Cómo se puede mover una máquina virtual de la que Azure Backup ha realizado una copia de seguridad a un grupo de recursos diferente?

1. Detenga temporalmente la copia de seguridad y conserve los datos de esta.
2. Traslade la máquina virtual al grupo de recursos de destino.
3. Vuelva a habilitar la copia de seguridad en el mismo almacén o en otro nuevo.

Puede restaurar la máquina virtual desde los puntos de restauración disponibles creados antes de la operación de traslado.

### <a name="is-there-a-limit-on-number-of-vms-that-can-beassociated-with-a-same-backup-policy"></a>¿Hay un límite en el número de máquinas virtuales que se pueden asociar con la misma directiva de copia de seguridad?

Sí, hay un límite de 100 máquinas virtuales que se pueden asociar a la misma directiva de copia de seguridad desde el portal. Para más de 100 máquinas virtuales, se recomienda crear varias directivas de copia de seguridad con la misma programación o con una programación diferente.
