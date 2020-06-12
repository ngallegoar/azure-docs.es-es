---
title: 'Preguntas frecuentes: copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure'
description: En este artículo, descubra las respuestas a preguntas comunes sobre la copia de seguridad de bases de datos de SAP HANA con el servicio Azure Backup.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 08e0eaf5f744ebb0ada07a944f627cc1ff1ac496
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248811"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Preguntas frecuentes: copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure

Este artículo responde preguntas comunes sobre la copia de seguridad de bases de datos de SAP HANA con el servicio Azure Backup.

## <a name="backup"></a>Copia de seguridad

### <a name="how-many-full-backups-are-supported-per-day"></a>¿Cuántas copias de seguridad completas se admiten al día?

Solo se admite una copia de seguridad completa al día. No se puede desencadenar una copia de seguridad diferencial y una copia de seguridad completa en el mismo día.

### <a name="do-successful-backup-jobs-create-alerts"></a>¿Generan alertas los trabajos de copia de seguridad que se han realizado correctamente?

No. Los trabajos de copia de seguridad correctos no generan alertas. Las alertas se envían únicamente para los trabajos de copia de seguridad con errores. El comportamiento detallado para alertas del portal está documentado [aquí](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor). Sin embargo, si está interesado en recibir alertas incluso para trabajos realizados correctamente, puede usar [Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor).

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>¿Se pueden ver los trabajos de copia de seguridad programados en el menú de Trabajos de copia de seguridad?

El menú Trabajo de copia de seguridad solo muestra los trabajos de copia de seguridad ad hoc. Para los trabajos programados, use [Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor).

### <a name="are-future-databases-automatically-added-for-backup"></a>¿Las bases de datos futuras se agregan automáticamente para copia de seguridad?

No, actualmente no se admite.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Si elimino una base de datos de una instancia, ¿qué ocurrirá con las copias de seguridad?

Si se elimina una base de datos de una instancia de SAP HANA, se siguen intentando las copias de seguridad de la base de datos. Esto implica que la base de datos eliminada comienza a aparecer como incorrecta en **Elementos de copia de seguridad** y todavía está protegida.
La manera correcta de detener la protección de esta base de datos es realizar una acción **Detener copia de seguridad con eliminación de datos** en esta base de datos.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Si se cambia el nombre de la base de datos después de haberla protegido, ¿cuál será el comportamiento?

Una base de datos después de un cambio de nombre se trata como una nueva base de datos. Por lo tanto, el servicio trata esta situación como si no se encontrara la base de datos y producirá un error en las copias de seguridad. La base de datos cuyo nombre se ha cambiado aparecerá como una nueva base de datos y se debe configurar su protección.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>¿Cuáles son los requisitos previos para realizar copias de seguridad de bases de datos de SAP HANA en una máquina virtual de Azure?

Consulte las secciones relativas a los [requisitos previos](tutorial-backup-sap-hana-db.md#prerequisites) y [Qué hace el script de registro previo](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="what-permissions-should-be-set-for-azure-to-be-able-to-back-up-sap-hana-databases"></a>¿Qué permisos se deben establecer para que Azure pueda realizar copias de seguridad de bases de datos de SAP HANA?

Al ejecutar el script de registro previo, se establecen los permisos necesarios para permitir que Azure realice copias de seguridad de bases de datos de SAP HANA. Puede encontrar más información sobre el script de registro previo [aquí](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="will-backups-work-after-migrating-sap-hana-from-sdc-to-mdc"></a>¿Funcionarán las copias de seguridad después de migrar SAP HANA de SDC a MDC?

Consulte [esta sección](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#sdc-to-mdc-upgrade-with-a-change-in-sid) de la guía de solución de problemas.

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>¿Se puede configurar la copia de seguridad de Azure HANA en una IP virtual (equilibrador de carga) y no en una máquina virtual?

Actualmente no se tiene la capacidad de configurar la solución en una IP virtual por sí sola. Se necesita una máquina virtual para ejecutar la solución.

### <a name="i-have-a-sap-hana-system-replication-hsr-how-should-i-configure-backup-for-this-setup"></a>Tengo una replicación del sistema SAP HANA (HSR), ¿cómo debo configurar la copia de seguridad para esta configuración?

Los nodos principal y secundario del HSR se tratarán como dos máquinas virtuales individuales no relacionadas. Debe configurar la copia de seguridad en el nodo principal y, cuando se produzca la conmutación errónea, deberá configurar la copia de seguridad en el nodo secundario (que ahora se convierte en el nodo principal). No hay ninguna "conmutación errónea" automática de la copia de seguridad en el otro nodo.

### <a name="how-can-i-move-an-on-demand-backup-to-the-local-file-system-instead-of-the-azure-vault"></a>¿Cómo puedo trasladar una copia de seguridad a petición al sistema de archivos local en lugar de al almacén de Azure?

1. Espere a que se complete la copia de seguridad que se está ejecutando actualmente en la base de datos deseada (comprobar en Studio si ha finalizado).
1. Deshabilite las copias de seguridad de registros y establezca la copia de seguridad del catálogo en **Filesystem** para la base de que desee siguiendo estos pasos:
1. Haga doble clic en **SYSTEMDB** -> **Configuración** -> **Seleccionar base de datos** -> **Filtro (registro)** .
    1. Establezca enable_auto_log_backup en **No**.
    1. Establezca log_backup_using_backint en **False**.
1. Realice una copia de seguridad a petición de la base de datos deseada y espere a que se completen la copia de seguridad del catálogo y la copia de seguridad completa.
1. Revierta a la configuración anterior para permitir que las copias de seguridad fluyan al almacén de Azure:
    1. Establezca enable_auto_log_backup en **Sí**.
    1. Establezca log_backup_using_backint en **True**.

## <a name="restore"></a>Restauración

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>¿Por qué no puedo ver el sistema HANA en el que deseo que se restaure la base de datos?

Compruebe si se cumplen todos los requisitos previos para la restauración en la instancia de SAP HANA de destino. Para más información, consulte [Requisitos previos: restauración de bases de datos de SAP HANA en máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/sap-hana-db-restore#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>¿Por qué se produce un error en la restauración de la base de datos con sobrescritura para la base de datos?

Asegúrese de que la opción **Forzar sobrescritura** está seleccionada durante la restauración.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>¿Por qué veo el mensaje de error "Los sistemas de origen y destino para la restauración no son compatibles"?

Consulte la Nota [1642148](https://launchpad.support.sap.com/#/notes/1642148) de SAP HANA para ver qué tipos de restauración se admiten actualmente.

### <a name="can-i-use-a-backup-of-a-database-running-on-sles-to-restore-to-a-rhel-hana-system-or-vice-versa"></a>¿Puedo usar una copia de seguridad de una base de datos que se ejecuta en SLES para la restauración a un sistema RHEL HANA o viceversa?

Sí, puede usar las copias de seguridad de streaming desencadenadas en una base de datos de HANA que se ejecuta en SLES para restaurarla en un sistema RHEL HANA y viceversa. Es decir, la restauración entre sistemas operativos es posible mediante copias de seguridad de streaming. Sin embargo, tendrá que asegurarse de que el sistema HANA en el que desea realizar la restauración y el sistema HANA que usará para la restauración sean compatibles con la restauración de acuerdo con SAP. Consulte la nota [1642148](https://launchpad.support.sap.com/#/notes/1642148) de SAP HANA para ver qué tipos de restauración son compatibles.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [hacer copias de seguridad de bases de datos de SAP HANA](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) que se ejecutan en máquinas virtuales de Azure.
