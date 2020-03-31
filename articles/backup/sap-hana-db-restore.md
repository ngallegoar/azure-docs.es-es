---
title: Restauración de bases de datos de SAP HANA en máquinas virtuales de Azure
description: En este artículo se describe cómo restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 999edba61177758ad9039e81e789efcef99ca1de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74287390"
---
# <a name="restore-sap-hana-databases-on-azure-vms"></a>Restauración de bases de datos de SAP HANA en máquinas virtuales de Azure

En este artículo se describe cómo restaurar bases de datos de SAP HANA que se ejecutan en una máquina virtual de Azure, de las que el servicio Azure Backup ha realizado una copia de seguridad en un almacén de Recovery Services de Azure Backup. Las restauraciones se pueden usar para crear copias de los datos para escenarios de desarrollo y pruebas, o para volver a un estado anterior.

Para más información sobre cómo hacer una copia de seguridad de bases de datos de SAP HANA, consulte [Copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

## <a name="restore-to-a-point-in-time-or-to-a-recovery-point"></a>Restauración a un momento dado o a un punto de recuperación

Azure Backup puede restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure como se indica a continuación:

* Restaurar a una fecha u hora específicas (con precisión de segundos) mediante copias de seguridad de registros. Azure Backup determina automáticamente la copia de seguridad diferencial o completa apropiada, y la cadena de copias de seguridad de registros necesarias para restaurar los datos en función del tiempo seleccionado.

* Restaurar una copia de seguridad completa o diferencial específica para restaurar a un punto de recuperación específico.

## <a name="prerequisites"></a>Prerrequisitos

Antes de restaurar una base de datos, tenga en cuenta lo siguiente:

* Solo puede restaurar la base de datos en una instancia de SAP HANA que se encuentre en la misma región.

* La instancia de destino debe estar registrada en el mismo almacén que la de origen.

* Azure Backup no puede identificar dos instancias de SAP HANA diferentes en la misma máquina virtual. Por lo tanto, no es posible restaurar los datos de una instancia en otra en la misma máquina virtual.

* Para asegurarse de que la instancia de SAP HANA de destino está lista para la restauración, compruebe su estado de **Preparación para la copia de seguridad**:

  * Abra el almacén en el que está registrada la instancia de SAP HANA de destino.

  * En el panel del almacén, en **Introducción**, elija **Copia de seguridad**.

![Copia de seguridad en el panel del almacén](media/sap-hana-db-restore/getting-started-backup.png)

* En **Copia de seguridad**, en **¿De qué quiere realizar una copia de seguridad?** , elija **SAP HANA en Azure VM**.

![Elección de SAP HANA en Azure VM](media/sap-hana-db-restore/sap-hana-backup.png)

* En **Detectar bases de datos en máquinas virtuales**, haga clic en **Ver detalles**.

![Ver detalles](media/sap-hana-db-restore/view-details.png)

* Revise el estado de **Preparación para la copia de seguridad** de la máquina virtual de destino.

![Los servidores protegidos.](media/sap-hana-db-restore/protected-servers.png)

* Para más información acerca de los tipos de restauración que admite SAP HANA, consulte la nota [1642148](https://launchpad.support.sap.com/#/notes/1642148) de SAP HANA.

## <a name="restore-a-database"></a>Restaurar una base de datos

* Abra el almacén en el que está registrada la base de datos de SAP HANA que se va a restaurar.

* En el panel del almacén, en **Elementos protegidos**, elija **Elementos de copia de seguridad**.

![Elementos de copia de seguridad](media/sap-hana-db-restore/backup-items.png)

* En **Elementos de copia de seguridad**, en **Tipo de administración de copias de seguridad**, seleccione **SAP HANA en Azure VM**.

![Tipo de administración de copias de seguridad](media/sap-hana-db-restore/backup-management-type.png)

* Seleccione la base de datos que se va a restaurar.

 ![Base de datos para restaurar](media/sap-hana-db-restore/database-to-restore.png)

* Revise el menú de la base de datos. Proporciona información sobre la copia de seguridad de la base de datos, incluidos:

  * Los puntos de restauración más antiguos y más recientes

  * El estado de la copia de seguridad de registros de la base de datos para las últimas 24 y 72 horas

![Menú de la base de datos](media/sap-hana-db-restore/database-menu.png)

* Seleccione **Restaurar base de datos**.

* En **Configuración de la restauración**, especifique dónde o cómo quiere restaurar los datos:

  * **Ubicación alternativa**: restaura la base de datos en una ubicación alternativa y mantiene la base de datos de origen original.

  * **Sobrescribir la base de datos**: restaura los datos en la misma instancia de SAP HANA que el origen. Esta opción sobrescribe la base de datos original.

![Configuración de la restauración](media/sap-hana-db-restore/restore-configuration.png)

### <a name="restore-to-alternate-location"></a>Restauración a una ubicación alternativa

* En el menú **Restaurar configuración**, en **Where to Restore** (Ubicación de restauración), seleccione **Ubicación alternativa**.

![Restauración a una ubicación alternativa](media/sap-hana-db-restore/restore-alternate-location.png)

* Seleccione el nombre de host de SAP HANA y el nombre de la instancia en la que desea restaurar la base de datos.
* Compruebe si la instancia de SAP HANA de destino está lista para la restauración; para ello, compruebe su estado de **Preparación para la copia de seguridad**. Consulte la sección [Requisitos previos](#prerequisites) para más información.
* En el cuadro de diálogo **Nombre de la base de datos restaurada**, escriba el nombre de la base de datos de destino.

> [!NOTE]
> Las restauraciones de Contenedor de base de datos única (SDC) deben seguir estas [comprobaciones](backup-azure-sap-hana-database-troubleshoot.md#single-container-database-sdc-restore).

* Si procede, seleccione **Sobrescribir si ya existe una base de datos con el mismo nombre en la instancia de HANA seleccionada**.
* Seleccione **Aceptar**.

![Configuración de la restauración: pantalla final](media/sap-hana-db-restore/restore-configuration-last.png)

* En **Seleccionar punto de restauración**, seleccione **Registros (a un momento dado)** a [Restauración a un momento dado](#restore-to-a-specific-point-in-time). O bien, seleccione **Completa y diferencial** para [restaurar a un punto de recuperación específico](#restore-to-a-specific-recovery-point).

### <a name="restore-and-overwrite"></a>Restauración y sobrescritura

* En el menú **Restaurar configuración**, en **Where to Restore** (Ubicación de restauración), seleccione **Sobrescribir la base de datos** > **Aceptar**.

![Sobrescribir la base de datos](media/sap-hana-db-restore/overwrite-db.png)

* En **Seleccionar punto de restauración**, seleccione **Registros (a un momento dado)** a [Restauración a un momento dado](#restore-to-a-specific-point-in-time). O bien, seleccione **Completa y diferencial** para [restaurar a un punto de recuperación específico](#restore-to-a-specific-recovery-point).

### <a name="restore-to-a-specific-point-in-time"></a>Restauración a un momento dado

Si ha seleccionado **Registros (punto en el tiempo)** como el tipo de restauración, haga lo siguiente:

* Seleccione un punto de recuperación en el gráfico del registro y seleccione **Aceptar** para elegir el punto de restauración.

![Punto de restauración](media/sap-hana-db-restore/restore-point.png)

* En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración.

![Selección de restauración](media/sap-hana-db-restore/restore-restore.png)

* Realice un seguimiento del progreso de la restauración en el área **Notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

![Restauración desencadenada correctamente](media/sap-hana-db-restore/restore-triggered.png)

### <a name="restore-to-a-specific-recovery-point"></a>Restauración a un punto de recuperación específico

Si ha seleccionado **Completo y diferencial** como el tipo de restauración, haga lo siguiente:

* Seleccione un punto de recuperación en la lista y seleccione **Aceptar** para elegir el punto de restauración.

![Restauración a un punto de recuperación específico](media/sap-hana-db-restore/specific-recovery-point.png)

* En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración.

![Inicio del trabajo de restauración](media/sap-hana-db-restore/restore-specific.png)

* Realice un seguimiento del progreso de la restauración en el área **Notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

![Progreso de la restauración](media/sap-hana-db-restore/restore-progress.png)

> [!NOTE]
> En las restauraciones de Contenedor de varias bases de datos (MDC), cuando la base de datos del sistema se restaura en una instancia de destino, es necesario volver a ejecutar el script de registro previo. Solo entonces las restauraciones de bases de datos de inquilino subsiguientes se realizarán correctamente. Para más información, consulte [Solución de problemas: restauración de MDC](backup-azure-sap-hana-database-troubleshoot.md#multiple-container-database-mdc-restore).

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga información acerca de cómo](sap-hana-db-manage.md) administrar bases de datos de SAP HANA de las que se realizan copias de seguridad mediante Azure Backup.
