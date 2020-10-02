---
title: Acciones con el Centro de copias de seguridad
description: En este artículo se explica cómo realizar acciones con el Centro de copia de seguridad.
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 81c81f0e9d955e0a5243485baaedff4e6f0fc10d
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90993830"
---
# <a name="perform-actions-using-backup-center"></a>Acciones con el Centro de copias de seguridad

El Centro de copias de seguridad permite realizar acciones clave relacionadas con la copia de seguridad desde una interfaz central sin necesidad de ir a un almacén individual. Estas son algunas de las acciones que puede realizar desde el Centro de copias de seguridad:

* Configurar la copia de seguridad de los orígenes de datos
* Restaurar una instancia de copia de seguridad
* Crear un nuevo almacén
* Crear una directiva de copia de seguridad
* Desencadenar una copia de seguridad a petición para una instancia de copia de seguridad
* Detener la copia de seguridad de una instancia de copia de seguridad

## <a name="supported-scenarios"></a>Escenarios admitidos

* El Centro de copias de seguridad admite actualmente copias de seguridad de máquinas virtuales de Azure y de servidores de Azure Database for PostgreSQL.
* Consulte la [matriz de compatibilidad](backup-center-support-matrix.md) para obtener una lista detallada de escenarios admitidos y no admitidos.

## <a name="configure-backup"></a>Configuración de la copia de seguridad

Siga las instrucciones siguientes que correspondan al tipo de origen de datos del que desee realizar una copia de seguridad.

### <a name="configure-backup-for-azure-virtual-machines"></a>Configuración de la copia de seguridad de máquinas virtuales de Azure

1. Vaya al Centro de copias de seguridad y seleccione **+ Copia de seguridad** en la parte superior de la pestaña **Información general**.

    ![Información general del Centro de copias de seguridad](./media/backup-center-actions/backup-center-overview-configure-backup.png)

2. Seleccione el tipo de origen de datos del que desea realizar la copia de seguridad (en este caso, máquina virtual de Azure).

    ![Seleccionar el origen de datos para configurar la copia de seguridad de la máquina virtual](./media/backup-center-actions/backup-select-datasource-vm.png)

3. Elija un almacén de Recovery Services y seleccione **Continuar**. Esto le lleva al área de configuración de copias de seguridad, idéntica al área a la que se accede desde un almacén de Recovery Services. [Obtenga más información sobre cómo configurar la copia de seguridad de máquinas virtuales de Azure con un almacén de Recovery Services](tutorial-backup-vm-at-scale.md).

### <a name="configure-backup-for-azure-database-for-postgresql-server"></a>Configuración de la copia de seguridad de servidores de Azure Database for PostgreSQL

1. Vaya al Centro de copias de seguridad y seleccione **+ Copia de seguridad** en la parte superior de la pestaña **Información general**.
2. Seleccione el tipo de origen de datos del que desea realizar la copia de seguridad (en este caso, servidor de Azure Database for PostgreSQL).

    ![Seleccione el origen de datos para configurar la copia de seguridad del servidor de Azure Database for PostgreSQL.](./media/backup-center-actions/backup-select-datasource-type-postgresql.png)

3. Seleccione **Continuar**. Esto le lleva al área de configuración de copias de seguridad, idéntica al área a la que se accede desde un almacén de Backup. [Obtenga más información sobre cómo configurar la copia de seguridad de servidores de Azure Database for PostgreSQL con un almacén de Backup](backup-azure-database-postgresql.md#configure-backup-on-azure-postgresql-databases).

## <a name="restore-a-backup-instance"></a>Restauración de una instancia de copia de seguridad

Siga las instrucciones siguientes que correspondan al tipo de origen de datos que desee restaurar.

### <a name="if-youre-restoring-an-azure-virtual-machine"></a>Si va a restaurar una máquina virtual de Azure

1. Vaya al Centro de copias de seguridad y seleccione **Restauración** en la parte superior de la pestaña **Información general**.

    ![Información general del Centro de copias de seguridad para restaurar la máquina virtual](./media/backup-center-actions/backup-center-overview-restore.png)

2. Seleccione el tipo de origen de orígenes que desea restaurar (en este caso, máquina virtual de Azure).

    ![Seleccionar el origen de datos de la restauración de la máquina virtual](./media/backup-center-actions/restore-select-datasource-vm.png)

3. Elija una instancia de copia de seguridad y seleccione **Continuar**. Esto le lleva al área de configuración de restauraciones, idéntica al área a la que se accede desde un almacén de Recovery Services. [Obtenga más información sobre cómo restaurar máquinas virtuales de Azure con un almacén de Recovery Services](backup-azure-arm-restore-vms.md#before-you-start).

### <a name="if-youre-restoring-an-azure-database-for-postgresql-server"></a>Si va a restaurar un servidor de Azure Database for PostgreSQL

1. Vaya al Centro de copias de seguridad y seleccione **Restauración** en la parte superior de la pestaña **Información general**.
2. Seleccione el tipo de origen de datos del que desea realizar la copia de seguridad (en este caso, servidor de Azure Database for PostgreSQL).

    ![Seleccionar el origen de datos para restaurar el servidor de Azure Database for PostgreSQL](./media/backup-center-actions/restore-select-datasource-postgresql.png)

3. Elija una instancia de copia de seguridad y seleccione **Continuar**. Esto le lleva al área de configuración de restauraciones, idéntica al área a la que se accede desde un almacén de Recovery Services. [Obtenga más información sobre cómo restaurar servidores de Azure Database for PostgreSQL con un almacén de Backup](backup-azure-database-postgresql.md#restore).

## <a name="create-a-new-vault"></a>Crear un nuevo almacén

Puede crear un nuevo almacén; para ello, vaya al Centro de copias de seguridad y seleccione **+ Almacén** en la parte superior de la pestaña **Información general**.

![Crear almacén](./media/backup-center-actions/backup-center-create-vault.png)

* [Más información sobre cómo crear un almacén de Recovery Services](backup-create-rs-vault.md)
* [Más información sobre cómo crear un almacén de Backup](backup-vault-overview.md)

## <a name="create-a-new-backup-policy"></a>Crear una directiva de copia de seguridad

Siga las instrucciones siguientes que correspondan al tipo de origen de datos del que desee realizar una copia de seguridad.

### <a name="if-youre-backing-up-an-azure-virtual-machine"></a>Si va a realizar una copia de seguridad de una máquina virtual de Azure

1. Vaya al Centro de copias de seguridad y seleccione **+ Directiva** en la parte superior de la pestaña **Información general**.

    ![Información general del Centro de copias de seguridad para la directiva de copia de seguridad](./media/backup-center-actions/backup-center-overview-policy.png)

2. Seleccione el tipo de origen de datos del que desea realizar la copia de seguridad (en este caso, máquina virtual de Azure).

    ![Seleccionar el origen de datos para la directiva de copia de seguridad de máquinas virtuales](./media/backup-center-actions/policy-select-datasource-vm.png)

3. Elija un almacén de Recovery Services y seleccione **Continuar**. Esto le lleva al área de creación de directivas, idéntica al área a la que se accede desde un almacén de Recovery Services. [Obtenga más información sobre cómo crear una nueva directiva de copia de seguridad para máquinas virtuales de Azure con un almacén de Recovery Services](backup-azure-arm-vms-prepare.md#create-a-custom-policy).

### <a name="if-youre-backing-up-an-azure-database-for-postgresql-server"></a>Si va a realizar una copia de seguridad de un servidor de Azure Database for PostgreSQL

1. Vaya al Centro de copias de seguridad y seleccione **+ Directiva** en la parte superior de la pestaña **Información general**.
2. Seleccione el tipo de origen de datos del que desea realizar la copia de seguridad (en este caso, servidor de Azure Database for PostgreSQL).

    ![Seleccionar el origen de datos para la directiva de copia de seguridad de servidores de Azure Database for PostgreSQL](./media/backup-center-actions/policy-select-datasource-postgresql.png)

3. Seleccione **Continuar**. Esto le lleva al área de creación de directivas, idéntica al área a la que se accede desde un almacén de Backup. [Obtenga más información sobre cómo crear una nueva directiva de copia de seguridad con un almacén de Backup](backup-azure-database-postgresql.md#create-backup-policy).

## <a name="execute-an-on-demand-backup-for-a-backup-instance"></a>Ejecución de una copia de seguridad a petición para una instancia de copia de seguridad

El Centro de copias de seguridad permite buscar instancias de copia de seguridad en todo el conjunto de copias de seguridad y ejecutar operaciones de copia de seguridad a petición.

Para desencadenar una copia de seguridad a petición, vaya al Centro de copias de seguridad y seleccione el elemento de menú **Instancias de copia de seguridad**. Si selecciona esta opción, podrá ver los detalles de todas las instancias de copia de seguridad a las que tenga acceso. Puede buscar la instancia de copia de seguridad de la que desea realizar la copia de seguridad. Al hacer clic con el botón secundario en un elemento de la cuadrícula, se abre una lista de acciones disponibles. Seleccione la opción **Hacer copia de seguridad ahora** para ejecutar una copia de seguridad a petición.

![Copia de seguridad a petición](./media/backup-center-actions/backup-center-on-demand-backup.png)

[Más información sobre la realización de copias de seguridad a petición para máquinas virtuales de Azure](backup-azure-manage-vms.md#run-an-on-demand-backup)

[Más información sobre la realización de copias de seguridad a petición para servidores de Azure Database for PostgreSQL](backup-azure-database-postgresql.md#on-demand-backup)

## <a name="stop-backup-for-a-backup-instance"></a>Detención de la copia de seguridad de una instancia de copia de seguridad

Pueden darse casos en los que desee detener la copia de seguridad de una instancia de copia de seguridad; por ejemplo, cuando el recurso subyacente del que se realiza esa copia ya no existe.

Para desencadenar una copia de seguridad a petición, vaya al Centro de copias de seguridad y seleccione el elemento de menú **Instancias de copia de seguridad**. Si selecciona esta opción, podrá ver los detalles de todas las instancias de copia de seguridad a las que tenga acceso. Puede buscar la instancia de copia de seguridad de la que desea realizar la copia de seguridad. Al hacer clic con el botón secundario en un elemento de la cuadrícula, se abre una lista de acciones disponibles. Seleccione la opción **Detener copia de seguridad** para detener la copia de seguridad de la instancia.

![Detener protección](./media/backup-center-actions/backup-center-stop-protection.png)

[Más información sobre cómo detener la copia de seguridad de máquinas virtuales de Azure](backup-azure-manage-vms.md#stop-protecting-a-vm)

[Más información sobre cómo detener la copia de seguridad de servidores de Azure Database for PostgreSQL](backup-azure-database-postgresql.md#stop-protection)

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión y funcionamiento de las copias de seguridad](backup-center-monitor-operate.md)
* [Gobernanza del conjunto de copias de seguridad](backup-center-govern-environment.md)
* [Obtención de información detallada sobre las copias de seguridad](backup-center-obtain-insights.md)
