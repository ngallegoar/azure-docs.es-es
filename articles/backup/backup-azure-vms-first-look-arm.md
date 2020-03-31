---
title: Copia de seguridad de una máquina virtual de Azure desde la configuración de esta
description: En este artículo, aprenderá a realizar copias de seguridad de una sola máquina virtual o de varias máquinas virtuales de Azure con el servicio Azure Backup.
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: 72d6e5657add3e815bb0d77fadbdbc716712bee5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705452"
---
# <a name="back-up-an-azure-vm-from-the-vm-settings"></a>Copia de seguridad de una máquina virtual de Azure desde la configuración de esta

En este artículo se explica cómo realizar copias de seguridad de máquinas virtuales de Azure con el servicio [Azure Backup](backup-overview.md). Puede realizar copias de seguridad de máquinas virtuales de Azure con un par de métodos:

- Una sola máquina virtual de Azure: en las instrucciones de este artículo se describe cómo realizar una copia de seguridad de una máquina virtual de Azure directamente desde la configuración de la máquina virtual.
- Varias máquinas virtuales de Azure: puede configurar un almacén de Recovery Services y configurar la copia de seguridad de varias máquinas virtuales de Azure. Siga las instrucciones de [este artículo](backup-azure-arm-vms-prepare.md) para este escenario.

## <a name="before-you-start"></a>Antes de comenzar

1. [Obtenga información](backup-architecture.md#how-does-azure-backup-work) sobre cómo funciona la copia de seguridad y [verifique](backup-support-matrix.md#azure-vm-backup-support) los requisitos de compatibilidad.
2. [Obtenga información general](backup-azure-vms-introduction.md) sobre la copia de seguridad de máquinas virtuales de Azure.

### <a name="azure-vm-agent-installation"></a>Instalación del agente de máquina virtual de Azure

Para realizar una copia de seguridad de máquinas virtuales de Azure, Azure Backup instala una extensión en el agente de máquina virtual que se ejecuta en la máquina. Si la máquina virtual se creó a partir de una imagen de Azure Marketplace, el agente se ejecutará. En algunos casos, por ejemplo, si crea una máquina virtual personalizada o si migra una máquina desde un entorno local. Puede que necesite instalar el agente manualmente.

- Si necesita instalar el agente de máquina virtual manualmente, siga las instrucciones para máquinas virtuales [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) o [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux).
- Después de instalar el agente, al habilitar la copia de seguridad, Azure Backup instala la extensión de copia de seguridad en el agente. Actualiza la extensión y le aplica parches sin la intervención del usuario.

## <a name="back-up-from-azure-vm-settings"></a>Copia de seguridad a partir de la configuración de la máquina virtual de Azure

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Haga clic en **Todos los servicios** y, en el filtro, escriba **Máquinas virtuales** y después haga clic en **Máquinas virtuales**.
3. En la lista de máquinas virtuales, seleccione la máquina virtual de la que desea realizar una copia de seguridad.
4. En el menú de la máquina virtual, haga clic en **Copia de seguridad**.
5. En **Almacén de Recovery Services**, haga lo siguiente:
   - Si ya tiene un almacén, haga clic en **Seleccionar existente** y seleccione un almacén.
   - Si no tiene un almacén, haga clic en **Crear nuevo**. Especifique un nombre para el almacén. Se crea en la misma región y en el mismo grupo de recursos que la máquina virtual. No puede modificar esta configuración si habilita la copia de seguridad directamente desde la configuración de la máquina virtual.

   ![Habilitar el Asistente para copia de seguridad](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

6. En **Elegir directiva de copia de seguridad**, realice lo siguiente:

   - Deje la directiva predeterminada. Esta realiza una copia de seguridad de la máquina virtual una vez al día a la hora especificada y conserva las copias de seguridad en el almacén durante 30 días.
   - Seleccione una directiva de copia de seguridad existente si tiene alguna.
   - Cree una directiva y defina su configuración.  

   ![Seleccionar directiva de copia de seguridad](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. Haga clic en **Habilitar copia de seguridad**. Esto asocia la directiva de copia de seguridad con la máquina virtual.

    ![Botón Enable Backup (Habilitar la copia de seguridad)](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. Puede realizar el seguimiento del progreso de la configuración en las notificaciones del portal.
9. Una vez completado el trabajo, en el menú de la máquina virtual, haga clic en **Copia de seguridad**. La página muestra el estado de copia de seguridad de la máquina virtual, la información sobre puntos de recuperación, los trabajos en ejecución y las alertas emitidas.

   ![Estado de copia de seguridad](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

10. Una vez habilitada la copia de seguridad, se realiza una copia de seguridad inicial. Puede iniciar la copia de seguridad inicial inmediatamente o esperar hasta que se inicie según la programación de copia de seguridad.
    - Hasta que se haya completado la copia de seguridad inicial, el **estado de la última copia de seguridad** se muestra como **Advertencia (copia de seguridad inicial pendiente)** .
    - Para ver cuándo se ejecutará la siguiente copia de seguridad programada, haga clic en el nombre de la directiva de copia de seguridad.

## <a name="run-a-backup-immediately"></a>Ejecutar una copia de seguridad inmediatamente

1. Para ejecutar una copia de seguridad inmediatamente, en el menú de la máquina virtual, haga clic en **Copia de seguridad** > **Realizar copia de seguridad ahora** .

    ![Ejecutar copia de seguridad](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

2. En **Realizar copia de seguridad ahora**, use el control del calendario para seleccionar hasta cuándo se conservará el punto de recuperación y, después, haga clic en **Aceptar**.

    ![Día de retención de copia de seguridad](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

3. Las notificaciones del portal permite saber si se ha desencadenado el trabajo de copia de seguridad. Para supervisar el progreso de copia de seguridad, haga clic en **Ver todos los trabajos**.

## <a name="back-up-from-the-recovery-services-vault"></a>Copia de seguridad desde el almacén de Recovery Services

Siga las instrucciones de este artículo para habilitar la copia de seguridad para máquinas virtuales de Azure mediante la configuración de un almacén de Azure Backup Recovery Services y la habilitación de la copia de seguridad en el almacén.

>[!NOTE]
> **Azure Backup admite ahora la copia de seguridad y restauración de discos selectivos mediante la solución de copia de seguridad de máquinas virtuales de Azure.**
>
>En la actualidad, Azure Backup admite la copia de seguridad de todos los discos (sistema operativo y datos) en una máquina virtual junto con la solución de copia de seguridad de máquinas virtuales. Con la funcionalidad de exclusión de disco, tiene la opción de realizar una copia de seguridad de uno o de varios de los múltiples discos de datos de una máquina virtual. Esto proporciona una solución eficaz y rentable para sus necesidades de copia de seguridad y restauración. Cada punto de recuperación contiene datos de los discos incluidos en la operación de copia de seguridad, lo que permite además tener un subconjunto de discos restaurados desde el punto de recuperación determinado durante la operación de restauración. Esto se aplica a la restauración tanto desde la instantánea como desde el almacén.
>
>**Para suscribirse a la versión preliminar, escriba a AskAzureBackupTeam@microsoft.com** .

## <a name="next-steps"></a>Pasos siguientes

- Si tiene dificultades con cualquiera de los procedimientos de este artículo, consulte la [guía de solución de problemas](backup-azure-vms-troubleshoot.md).
- [Obtenga información sobre](backup-azure-manage-vms.md) cómo administrar las copias de seguridad.
