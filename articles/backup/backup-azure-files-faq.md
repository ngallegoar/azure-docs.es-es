---
title: Preguntas frecuentes sobre la copia de seguridad de archivos de Azure
description: En este artículo, encontrará respuestas a preguntas habituales sobre cómo proteger los recursos compartidos de archivos de Azure con el servicio Azure Backup.
ms.date: 07/29/2019
ms.topic: conceptual
ms.openlocfilehash: c69d4642aefbd599d3783dcdfa059a0cd9d129d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302549"
---
# <a name="questions-about-backing-up-azure-files"></a>Preguntas acerca de la copia de seguridad de archivos de Azure

En este artículo se ofrecen respuestas a preguntas habituales acerca de la copia de seguridad de archivos de Azure. En algunas de las respuestas, hay vínculos a artículos que tienen información completa. También se pueden publicar preguntas sobre el servicio Azure Backup en el [foro de debate](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

Para examinar rápidamente las secciones de este artículo, use los vínculos de la derecha que aparecen debajo de **En este artículo**.

## <a name="configuring-the-backup-job-for-azure-files"></a>Configuración del trabajo de copia de seguridad de archivos de Azure

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-azure-file-shares"></a>¿Por qué no puedo ver algunas de las cuentas de almacenamiento que quiero proteger, que contienen recursos compartidos de archivos de Azure válidos?

Durante la versión preliminar, la copia de seguridad de los recursos compartidos de archivos de Azure no admite todos los tipos de cuentas de almacenamiento. Consulte esta lista de [aquí](troubleshoot-azure-files.md#limitations-for-azure-file-share-backup-during-preview) para ver las cuentas de almacenamiento admitidas. También es posible que la cuenta de almacenamiento que está buscando ya esté protegida o registrada con otro almacén. [Anule el registro](troubleshoot-azure-files.md#configuring-backup) desde el almacén para descubrir la cuenta de almacenamiento en otros almacenes para la protección.

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup"></a>¿Por qué no veo algunos de mis recursos compartidos de archivos de Azure en la cuenta de almacenamiento cuando intento configurar la copia de seguridad?

Compruebe si el recurso compartido de archivos de Azure ya está protegido en el mismo almacén de Recovery Services o si se ha eliminado recientemente.

### <a name="can-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync"></a>¿Puedo proteger recursos compartidos de archivos conectado a un grupo de sincronización en Azure File Sync?

Sí. La protección de recursos compartidos de archivos de Azure conectados a grupos de sincronización está habilitada y forma parte de la versión preliminar pública.

### <a name="when-trying-to-back-up-file-shares-i-clicked-on-a-storage-account-for-discovering-the-file-shares-in-it-however-i-did-not-protect-them-how-do-i-protect-these-file-shares-with-any-other-vault"></a>Al intentar realizar la copia de seguridad de recursos compartidos de archivos, hice clic en una cuenta de almacenamiento para detectar los recursos compartidos de archivos de ella. Sin embargo, no los he protegido. ¿Cómo puedo proteger estos recursos compartidos de archivos con cualquier otro almacén?

Al intentar realizar una copia de seguridad, si selecciono una cuenta de almacenamiento para detectar los recursos compartidos que hay en ella, se registra la cuenta de almacenamiento con el almacén desde el que se realiza esto. Si elige proteger los recursos compartidos de archivos con un almacén diferente, [anule el registro](troubleshoot-azure-files.md#configuring-backup) de la cuenta de almacenamiento seleccionada de este almacén.

### <a name="can-i-change-the-vault-to-which-i-back-up-my-file-shares"></a>¿Puedo cambiar el almacén en el que hago la copia de seguridad de mis recursos compartidos de archivos?

Sí. Sin embargo, deberá [detener la protección en un recurso compartido de archivos](manage-afs-backup.md#stop-protection-on-a-file-share) desde el almacén conectado, [anular el registro](troubleshoot-azure-files.md#configuring-backup) de esta cuenta de almacenamiento y protegerla desde un almacén distinto.

### <a name="in-which-geos-can-i-back-up-azure-file-shares"></a>¿En qué zonas geográficas se puede realizar una copia de seguridad de los recursos compartidos de archivos de Azure?

La copia de seguridad de recursos compartidos de archivos de Azure se encuentra actualmente en versión preliminar y solo está disponible en las siguientes zonas geográficas:

- Este de Australia (AE)
- Sudeste de Australia (ASE)
- Sur de Brasil (BRS)
- Centro de Canadá (CNC)
- Este de Canadá (CE)
- Centro de EE. UU. (CUS)
- Este de Asia (EA)
- Este de EE. UU. (EUS)
- Este de EE. UU. 2 (EUS2)
- Japón Oriental (JPE)
- Japón Occidental (JPW)
- India central (INC)
- India del Sur (INS)
- Centro de Corea del Sur (KRC)
- Sur de Corea del Sur (KRS)
- Centro-norte de EE. UU. (NCUS)
- Norte de Europa (NE)
- Centro-sur de EE. UU. (SCUS)
- Sudeste de Asia (SEA)
- Sur de Reino Unido (UKS)
- Oeste de Reino Unido (UKW)
- Oeste de Europa (WE)
- Oeste de EE. UU. (WUS)
- Centro-oeste de EE. UU. (WCUS)
- Oeste de EE. UU. 2 (WUS 2)
- US Gov Arizona (UGA)
- US Gov Texas (UGT)
- US Gov Virginia (UGV)
- Centro de Australia (ACL)
- India occidental (INW)
- Norte de Sudáfrica (SAN)
- Norte de Emiratos Árabes (UAN)
- Centro de Francia (FRC)
- Norte de Alemania (GN)                       
- Centro-oeste de Alemania (GWC)
- Oeste de Sudáfrica (SAW)
- Centro de Emiratos Árabes Unidos (UAC)
- NWE (Este de Noruega)     
- NWW (Oeste de Noruega)
- SZN (Norte de Suiza)

Escriba a [AskAzureBackupTeam@microsoft.com](mailto:askazurebackupteam@microsoft.com) si tiene que usarla en una zona geográfica específica que no se haya indicado anteriormente.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vault"></a>¿Cuántos recursos compartidos de archivos de Azure se pueden proteger en un almacén?

Durante la versión preliminar, puede proteger los recursos compartidos de archivos de Azure de hasta 50 cuentas de almacenamiento por almacén. También puede proteger hasta 200 recursos compartidos de archivos de Azure en un único almacén.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>¿Puedo proteger dos recursos compartidos de archivos diferentes desde la misma cuenta de almacenamiento en almacenes diferentes?

No. Todos los recursos compartidos de archivos de una cuenta de almacenamiento solo se pueden proteger en el mismo almacén.

## <a name="backup"></a>Copia de seguridad

### <a name="how-many-scheduled-backups-can-i-configure-per-file-share"></a>¿Cuántas copias de seguridad programadas puedo configurar por recurso compartido de archivos?

Actualmente, Azure Backup admite la configuración de copias de seguridad programadas una vez al día para los recursos compartidos de archivos de Azure.

### <a name="how-many-on-demand-backups-can-i-take-per-file-share"></a>¿Cuántas copias de seguridad a petición se pueden realizar por recurso compartido de archivos?

En cualquier momento dado, puede tener hasta 200 instantáneas de un recurso compartido de archivos. El límite incluye las instantáneas que toma Azure Backup como se define en la directiva. Si las copias de seguridad empiezan a generar errores después de alcanzar el límite, elimine los puntos de restauración a petición para que las próximas copias de seguridad sean correctas.

## <a name="restore"></a>Restauración

### <a name="can-i-recover-from-a-deleted-azure-file-share"></a>¿Se pueden recuperar los recursos compartidos de archivos de Azure eliminados?

Cuando se elimina un recurso compartido de archivos de Azure, se muestra la lista de copias de seguridad que se eliminarán y se solicita una confirmación. No se puede restaurar un recurso compartido de archivos de Azure eliminado.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share"></a>¿Puedo restaurar las copias de seguridad si detuve la protección en un recurso compartido de archivos de Azure?

Sí. Si ha elegido **Retener datos de copia de seguridad** cuando detuvo la protección, puede restaurarlas desde todos los puntos de restauración existentes.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>¿Qué ocurre si se cancela un trabajo de restauración en curso?

Si se cancela un trabajo de restauración en curso, el proceso de restauración se detiene y todos los archivos restaurados antes de la cancelación permanecen en el destino configurado (ubicación original o alternativa) sin las reversiones.

## <a name="manage-backup"></a>Administración de copias de seguridad

### <a name="can-i-use-powershell-to-configuremanagerestore-backups-of-azure-file-shares"></a>¿Se puede usar PowerShell para configurar/administrar/restaurar copias de seguridad de los recursos compartidos de Azure File?

Sí. Consulte [aquí](backup-azure-afs-automation.md) la documentación detallada

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it"></a>¿Puedo acceder a las instantáneas realizadas por las instancias de Azure Backup y montarlas?

Es posible acceder a todas las instantáneas realizadas por Azure Backup desde el portal, PowerShell o la CLI. Para más información acerca de las instantáneas de recurso compartido de Azure Files, consulte [Información general de instantáneas de recurso compartido de Azure Files (versión preliminar)](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups"></a>¿Cuál es la retención máxima que puedo configurar para las copias de seguridad?

La copia de seguridad de los recursos compartidos de archivos de Azure ofrece la capacidad de configurar directivas con retención de hasta 180 días. Sin embargo, con la opción de [Copia de seguridad a petición en PowerShell](backup-azure-afs-automation.md#trigger-an-on-demand-backup) puede conservar un punto de recuperación incluso durante 10 años.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share"></a>¿Qué ocurre cuando se cambia la directiva de copia de seguridad de un recurso compartido de archivos de Azure?

Cuando se aplica una nueva directiva en los recursos compartidos de archivos, se siguen las opciones de programación y retención de la nueva directiva. Si se amplía la retención, los puntos de recuperación existentes se marcarán para mantenerlos de acuerdo con la nueva directiva. Si se reduce la retención, se marcan para eliminarse, y se eliminan, en el siguiente trabajo de limpieza.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre otras áreas de Azure Backup, consulte algunas de estas otras preguntas frecuentes sobre la copia de seguridad:

- [Preguntas más frecuentes sobre el almacén de Recovery Services](backup-azure-backup-faq.md)
- [Preguntas más frecuentes sobre la copia de seguridad de máquinas virtuales de Azure](backup-azure-vm-backup-faq.md)
- [Preguntas más frecuentes sobre Azure Backup Agent](backup-azure-file-folder-backup-faq.md)
