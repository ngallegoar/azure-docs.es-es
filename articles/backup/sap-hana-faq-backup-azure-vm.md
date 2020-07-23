---
title: 'Preguntas frecuentes: copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure'
description: En este artículo, descubra las respuestas a preguntas comunes sobre la copia de seguridad de bases de datos de SAP HANA con el servicio Azure Backup.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: dcbf1bf6b39b2afa3fb5aaf2a7f18c5d0e8e4afb
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513513"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Preguntas frecuentes: copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure

Este artículo responde preguntas comunes sobre la copia de seguridad de bases de datos de SAP HANA con el servicio Azure Backup.

## <a name="backup"></a>Copia de seguridad

### <a name="how-many-full-backups-are-supported-per-day"></a>¿Cuántas copias de seguridad completas se admiten al día?

Solo se admite una copia de seguridad completa al día. No se puede desencadenar una copia de seguridad diferencial y una copia de seguridad completa en el mismo día.

### <a name="do-successful-backup-jobs-create-alerts"></a>¿Generan alertas los trabajos de copia de seguridad que se han realizado correctamente?

No. Los trabajos de copia de seguridad correctos no generan alertas. Las alertas se envían únicamente para los trabajos de copia de seguridad con errores. El comportamiento detallado para alertas del portal está documentado [aquí](./backup-azure-monitoring-built-in-monitor.md). Sin embargo, si está interesado en recibir alertas incluso para trabajos realizados correctamente, puede usar [Azure Monitor](./backup-azure-monitoring-use-azuremonitor.md).

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>¿Se pueden ver los trabajos de copia de seguridad programados en el menú de Trabajos de copia de seguridad?

El menú Trabajo de copia de seguridad solo muestra los trabajos de copia de seguridad ad hoc. Para los trabajos programados, use [Azure Monitor](./backup-azure-monitoring-use-azuremonitor.md).

### <a name="are-future-databases-automatically-added-for-backup"></a>¿Las bases de datos futuras se agregan automáticamente para copia de seguridad?

No, no se admite actualmente.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Si elimino una base de datos de una instancia, ¿qué ocurrirá con las copias de seguridad?

Si se elimina una base de datos de una instancia de SAP HANA, se siguen intentando las copias de seguridad de la base de datos. Esto implica que la base de datos eliminada comienza a aparecer como incorrecta en **Elementos de copia de seguridad** y todavía está protegida.
La manera correcta de detener la protección de esta base de datos es realizar una acción **Detener copia de seguridad con eliminación de datos** en esta base de datos.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Si se cambia el nombre de la base de datos después de haberla protegido, ¿cuál será el comportamiento?

Una base de datos después de un cambio de nombre se trata como una nueva base de datos. Por lo tanto, el servicio trata esta situación como si no se encontrara la base de datos y producirá un error en las copias de seguridad. La base de datos cuyo nombre se ha cambiado aparecerá como una nueva base de datos y se debe configurar su protección.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>¿Cuáles son los requisitos previos para realizar copias de seguridad de bases de datos de SAP HANA en una máquina virtual de Azure?

Consulte las secciones relativas a los [requisitos previos](tutorial-backup-sap-hana-db.md#prerequisites) y [Qué hace el script de registro previo](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="what-permissions-should-be-set-so-azure-can-back-up-sap-hana-databases"></a>¿Qué permisos se deben establecer para que Azure pueda realizar copias de seguridad de bases de datos de SAP HANA?

Al ejecutar el script de registro previo, se establecen los permisos necesarios para permitir que Azure realice copias de seguridad de bases de datos de SAP HANA. Puede encontrar más información sobre el script de registro previo [aquí](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does).

### <a name="will-backups-work-after-migrating-sap-hana-from-sdc-to-mdc"></a>¿Funcionarán las copias de seguridad después de migrar SAP HANA de SDC a MDC?

Consulte [esta sección](./backup-azure-sap-hana-database-troubleshoot.md#sdc-to-mdc-upgrade-with-a-change-in-sid) de la guía de solución de problemas.

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>¿Se puede configurar la copia de seguridad de Azure HANA en una IP virtual (equilibrador de carga) y no en una máquina virtual?

Actualmente no se tiene la capacidad de configurar la solución en una IP virtual por sí sola. Se necesita una máquina virtual para ejecutar la solución.

### <a name="how-can-i-move-an-on-demand-backup-to-the-local-file-system-instead-of-the-azure-vault"></a>¿Cómo puedo trasladar una copia de seguridad a petición al sistema de archivos local en lugar de al almacén de Azure?

1. Espere a que se complete la copia de seguridad que se está ejecutando actualmente en la base de datos deseada (comprobar en Studio si ha finalizado).
1. Deshabilite las copias de seguridad de registros y establezca la copia de seguridad del catálogo en **Filesystem** para la base de que desee siguiendo estos pasos:
1. Haga doble clic en **SYSTEMDB** -> **Configuración** -> **Seleccionar base de datos** -> **Filtro (registro)** .
    1. Establezca enable_auto_log_backup en **No**.
    1. Establezca catalog_backup_using_backint en **false**.
1. Realice una copia de seguridad a petición (completa, diferencial o incremental) de la base de datos deseada y espere a que se completen la copia de seguridad del catálogo y la copia de seguridad completa.
1. Si también desea trasladar las copias de seguridad de registros al sistema de archivos, establezca enable_auto_log_backup en **sí**.
1. Revierta a la configuración anterior para permitir que las copias de seguridad fluyan al almacén de Azure:
    1. Establezca enable_auto_log_backup en **Sí**.
    1. Establezca catalog_backup_using_backint en **true**.

>[!NOTE]
>Trasladar las copias de seguridad al sistema de archivos local y volver a cambiar a Azure Vault puede provocar una interrupción de la cadena de registro de las copias de seguridad del registro en el almacén. Esto desencadenará una copia de seguridad completa, que una vez realizada correctamente, comenzará a ejecutar la copia de seguridad de los registros.

### <a name="how-can-i-use-sap-hana-backup-with-my-hana-replication-set-up"></a>¿Cómo puedo usar la copia de seguridad de SAP HANA con la configuración de la replicación de HANA?

Actualmente, Azure Backup no tiene la capacidad de comprender una configuración de HSR. Esto significa que los nodos principal y secundario del HSR se tratarán como dos máquinas virtuales individuales no relacionadas. Primero deberá configurar la copia de seguridad en el nodo principal. Cuando se produce una conmutación por error, se debe configurar la copia de seguridad en el nodo secundario (que ahora se convierte en el nodo principal). No hay ninguna conmutación errónea automática de la copia de seguridad en el otro nodo.

Para realizar una copia de seguridad de los datos del nodo activo (principal) en un momento dado, puede **cambiar la protección** al nodo secundario, que ahora se convierte en el principal después de la conmutación por error.

Para realizar este **cambio de la protección**, siga estos pasos:

- [Detener la protección](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) (con conservación de datos) en el nodo principal
- Ejecutar el [script de registro previo](https://aka.ms/scriptforpermsonhana) en el nodo secundario
- [Detectar las bases de datos](tutorial-backup-sap-hana-db.md#discover-the-databases) en el nodo secundario y [configurar las copias de seguridad](tutorial-backup-sap-hana-db.md#configure-backup) en ellas

Estos pasos deben realizarse manualmente después de cada conmutación por error. Puede realizar estos pasos mediante la línea de comandos/REST de HTTP y en Azure Portal. Para automatizar estos pasos, puede usar un runbook de Azure.

A continuación, se muestra un ejemplo detallado de cómo se debe realizar el **cambio de la protección**:

En este ejemplo, tiene dos nodos: nodo 1 (principal) y nodo 2 (secundario) en la configuración del HSR.  Las copias de seguridad se configuran en el nodo 1. Como se mencionó anteriormente, no intente configurar las copias de seguridad en el nodo 2.

Cuando se produce la primera conmutación por error, el nodo 2 se convierte en el principal. A continuación,

1. Detenga la protección del nodo 1 (principal anterior) con la opción de conservación de datos.
1. Ejecute el script de registro previo en el nodo 2 (que ahora es el principal).
1. Detecte bases de datos en el nodo 2, asigne directivas de copia de seguridad y configure copias de seguridad.

A continuación, se desencadena una primera copia de seguridad completa en el nodo 2 y, una vez completada, se inician las copias de seguridad de los registros.

Cuando se produce la siguiente conmutación por error, el nodo 1 vuelve a ser principal y el nodo 2 se convierte en secundario. Ahora repita el proceso:

1. Detenga la protección del nodo 2 con la opción de conservación de datos.
1. Ejecute el script de registro previo en el nodo 1 (que se ha convertido de nuevo en el principal).
1. A continuación, [reanude la copia de seguridad](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database) en el nodo 1 con la directiva necesaria (ya que las copias de seguridad se detuvieron antes en el nodo 1).

A continuación, se desencadenará de nuevo una copia de seguridad completa en el nodo 1 y, una vez completada, se inician las copias de seguridad de los registros.

## <a name="restore"></a>Restauración

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>¿Por qué no puedo ver el sistema HANA en el que deseo que se restaure la base de datos?

Compruebe si se cumplen todos los requisitos previos para la restauración en la instancia de SAP HANA de destino. Para más información, consulte [Requisitos previos: restauración de bases de datos de SAP HANA en máquinas virtuales de Azure](./sap-hana-db-restore.md#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>¿Por qué se produce un error en la restauración de la base de datos con sobrescritura para la base de datos?

Asegúrese de que la opción **Forzar sobrescritura** está seleccionada durante la restauración.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>¿Por qué veo el mensaje de error "Los sistemas de origen y destino para la restauración no son compatibles"?

Consulte la Nota [1642148](https://launchpad.support.sap.com/#/notes/1642148) de SAP HANA para ver qué tipos de restauración se admiten actualmente.

### <a name="can-i-use-a-backup-of-a-database-running-on-sles-to-restore-to-an-rhel-hana-system-or-vice-versa"></a>¿Puedo usar una copia de seguridad de una base de datos que se ejecuta en SLES para la restauración a un sistema RHEL HANA o viceversa?

Sí, puede usar las copias de seguridad de streaming desencadenadas en una base de datos de HANA que se ejecuta en SLES para restaurarla en un sistema RHEL HANA y viceversa. Es decir, la restauración entre sistemas operativos es posible mediante copias de seguridad de streaming. Sin embargo, tendrá que asegurarse de que el sistema HANA en el que desea realizar la restauración y el sistema HANA que usará para la restauración sean compatibles con la restauración de acuerdo con SAP. Consulte la nota [1642148](https://launchpad.support.sap.com/#/notes/1642148) de SAP HANA para ver qué tipos de restauración son compatibles.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [hacer copias de seguridad de bases de datos de SAP HANA](./backup-azure-sap-hana-database.md) que se ejecutan en máquinas virtuales de Azure.
