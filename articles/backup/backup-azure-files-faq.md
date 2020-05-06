---
title: Preguntas frecuentes sobre la copia de seguridad de archivos de Azure
description: En este artículo, encontrará respuestas a preguntas habituales sobre cómo proteger los recursos compartidos de archivos de Azure con el servicio Azure Backup.
ms.date: 04/22/2020
ms.topic: conceptual
ms.openlocfilehash: d7b19fd11e6784a188a18f6a613eef5ff4f77764
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82105648"
---
# <a name="questions-about-backing-up-azure-files"></a>Preguntas acerca de la copia de seguridad de archivos de Azure

En este artículo se ofrecen respuestas a preguntas habituales acerca de la copia de seguridad de archivos de Azure. En algunas de las respuestas, hay vínculos a artículos que tienen información completa. También se pueden publicar preguntas sobre el servicio Azure Backup en el [foro de debate](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

Para examinar rápidamente las secciones de este artículo, use los vínculos de la derecha que aparecen debajo de **En este artículo**.

## <a name="configuring-the-backup-job-for-azure-files"></a>Configuración del trabajo de copia de seguridad de archivos de Azure

### <a name="why-cant-i-see-some-of-my-storage-accounts-that-i-want-to-protect-which-contain-valid-azure-file-shares"></a>¿Por qué no puedo ver algunas de las cuentas de almacenamiento que quiero proteger, que contienen recursos compartidos de archivos de Azure válidos?

Consulte la [matriz de compatibilidad para la copia de seguridad de recursos compartidos de archivos de Azure](azure-file-share-support-matrix.md) para asegurarse de que la cuenta de almacenamiento pertenece a uno de los tipos de cuenta de almacenamiento admitidos. También es posible que la cuenta de almacenamiento que está buscando ya esté protegida o registrada con otro almacén. [Anule el registro de la cuenta de almacenamiento](manage-afs-backup.md#unregister-a-storage-account) desde el almacén para descubrir la cuenta de almacenamiento en otros almacenes para la protección.

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup"></a>¿Por qué no veo algunos de mis recursos compartidos de archivos de Azure en la cuenta de almacenamiento cuando intento configurar la copia de seguridad?

Compruebe si el recurso compartido de archivos de Azure ya está protegido en el mismo almacén de Recovery Services o si se ha eliminado recientemente.

### <a name="can-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync"></a>¿Puedo proteger recursos compartidos de archivos conectado a un grupo de sincronización en Azure File Sync?

Sí. La protección de recursos compartidos de archivos de Azure conectados a grupos de sincronización está habilitada.

### <a name="when-trying-to-back-up-file-shares-i-clicked-on-a-storage-account-for-discovering-the-file-shares-in-it-however-i-didnt-protect-them-how-do-i-protect-these-file-shares-with-any-other-vault"></a>Al intentar realizar la copia de seguridad de recursos compartidos de archivos, hice clic en una cuenta de almacenamiento para detectar los recursos compartidos de archivos de ella. Sin embargo, no los he protegido. ¿Cómo puedo proteger estos recursos compartidos de archivos con cualquier otro almacén?

Al intentar realizar una copia de seguridad, si selecciono una cuenta de almacenamiento para detectar los recursos compartidos que hay en ella, se registra la cuenta de almacenamiento con el almacén desde el que se realiza esto. Si elige proteger los recursos compartidos de archivos con un almacén diferente, [anule el registro](manage-afs-backup.md#unregister-a-storage-account) de la cuenta de almacenamiento seleccionada de este almacén.

### <a name="can-i-change-the-vault-to-which-i-back-up-my-file-shares"></a>¿Puedo cambiar el almacén en el que hago la copia de seguridad de mis recursos compartidos de archivos?

Sí. Sin embargo, deberá [detener la protección en un recurso compartido de archivos](manage-afs-backup.md#stop-protection-on-a-file-share) desde el almacén conectado, [anular el registro](manage-afs-backup.md#unregister-a-storage-account) de esta cuenta de almacenamiento y protegerla desde un almacén distinto.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vault"></a>¿Cuántos recursos compartidos de archivos de Azure se pueden proteger en un almacén?

Puede proteger los recursos compartidos de archivos de Azure de hasta 50 cuentas de almacenamiento por almacén. También puede proteger hasta 200 recursos compartidos de archivos de Azure en un único almacén.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>¿Puedo proteger dos recursos compartidos de archivos diferentes desde la misma cuenta de almacenamiento en almacenes diferentes?

No. Todos los recursos compartidos de archivos de una cuenta de almacenamiento solo se pueden proteger en el mismo almacén.

## <a name="backup"></a>Copia de seguridad

### <a name="what-should-i-do-if-my-backups-start-failing-due-to-the-maximum-limit-reached-error"></a>¿Qué debo hacer si las copias de seguridad empiezan a generar errores debido al error de límite máximo alcanzado?

En cualquier momento dado, puede tener hasta 200 instantáneas de un recurso compartido de archivos. El límite incluye las instantáneas que toma Azure Backup como se define en la directiva. Si las copias de seguridad empiezan a generar errores después de alcanzar el límite, elimine las instantáneas a petición para que las próximas copias de seguridad sean correctas.

## <a name="restore"></a>Restauración

### <a name="can-i-recover-from-a-deleted-azure-file-share"></a>¿Se pueden recuperar los recursos compartidos de archivos de Azure eliminados?

Cuando se elimina un recurso compartido de archivos de Azure, se muestra la lista de copias de seguridad que se eliminarán y se solicita una confirmación. Actualmente, no se puede restaurar un recurso compartido de archivos de Azure eliminado.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share"></a>¿Puedo restaurar las copias de seguridad si detuve la protección en un recurso compartido de archivos de Azure?

Sí. Si ha elegido **Retener datos de copia de seguridad** cuando detuvo la protección, puede restaurarlas desde todos los puntos de restauración existentes.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>¿Qué ocurre si se cancela un trabajo de restauración en curso?

Si se cancela un trabajo de restauración en curso, el proceso de restauración se detiene y todos los archivos restaurados antes de la cancelación permanecen en el destino configurado (ubicación original o alternativa) sin las reversiones.

## <a name="manage-backup"></a>Administración de copias de seguridad

### <a name="can-i-use-powershell-to-configuremanagerestore-backups-of-azure-file-shares"></a>¿Se puede usar PowerShell para configurar/administrar/restaurar copias de seguridad de los recursos compartidos de Azure File?

Sí. Consulte [aquí](backup-azure-afs-automation.md) la documentación detallada.

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-them"></a>¿Puedo acceder a las instantáneas realizadas por las instancias de Azure Backup y montarlas?

Es posible acceder a todas las instantáneas realizadas por Azure Backup desde el portal, PowerShell o la CLI. Para más información acerca de las instantáneas de recurso compartido de Azure Files, consulte [Información general de instantáneas de recurso compartido de Azure Files](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups"></a>¿Cuál es la retención máxima que puedo configurar para las copias de seguridad?

Consulte la [matriz de compatibilidad](azure-file-share-support-matrix.md) para obtener más información sobre la retención máxima. Azure Backup hace un cálculo en tiempo real del número de instantáneas cuando se escriben los valores de retención al configurar la directiva de copia de seguridad. En cuanto el número de instantáneas correspondientes a los valores de retención definidos supera las 200, el portal mostrará una advertencia que le solicitará que ajuste los valores de retención. Esto es para que no supere el límite del número máximo de instantáneas que admite Azure Files para cualquier recurso compartido de archivos en cualquier momento.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share"></a>¿Qué ocurre cuando se cambia la directiva de copia de seguridad de un recurso compartido de archivos de Azure?

Cuando se aplica una nueva directiva en los recursos compartidos de archivos, se siguen las opciones de programación y retención de la nueva directiva. Si se amplía la retención, los puntos de recuperación existentes se marcarán para mantenerlos de acuerdo con la nueva directiva. Si se reduce la retención, se marcan para eliminarse, y se eliminan, en el siguiente trabajo de limpieza.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre otras áreas de Azure Backup, consulte algunas de estas otras preguntas frecuentes sobre la copia de seguridad:

- [Preguntas más frecuentes sobre el almacén de Recovery Services](backup-azure-backup-faq.md)
- [Preguntas más frecuentes sobre la copia de seguridad de máquinas virtuales de Azure](backup-azure-vm-backup-faq.md)
- [Preguntas más frecuentes sobre Azure Backup Agent](backup-azure-file-folder-backup-faq.md)
