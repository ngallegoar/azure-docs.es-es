---
title: Copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services
description: Aquí se describe cómo realizar una copia de seguridad de VM de Azure en un almacén de Recovery Services con Azure Backup
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: aeadd7bc798f690c67eef38c6dc645204ff39115
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233944"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services

En este artículo se describe cómo realizar una copia de seguridad de VM de Azure en un almacén de Recovery Services con el servicio [Azure Backup](backup-overview.md).

En este artículo aprenderá a:

> [!div class="checklist"]
>
> * Prepare las máquinas virtuales de Azure.
> * Crear un almacén.
> * Detecte VM y configure una directiva de copia de seguridad.
> * Habilite la copia de seguridad de VM de Azure.
> * Realizar la copia de seguridad inicial.

> [!NOTE]
> EN Este artículo describe cómo configurar un almacén y seleccionar VM para hacer una copia de seguridad. Resulta útil si desea realizar copias de seguridad de varias máquinas virtuales. Como alternativa, puede [realizar una copia de seguridad de una VM de Azure](backup-azure-vms-first-look-arm.md) directamente desde la configuración de la VM.

## <a name="before-you-start"></a>Antes de comenzar

* [Revise](backup-architecture.md#architecture-built-in-azure-vm-backup) la arquitectura de copia de seguridad de máquinas virtuales de Azure.
* [Más información sobre](backup-azure-vms-introduction.md) la copia de seguridad de máquinas virtuales de Azure y la extensión de reserva.
* [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) antes de configurar la copia de seguridad.

Además, hay un par de cosas que puede que deba hacer en algunas circunstancias:

* **Instalar el agente de máquina virtual en la máquina virtual**: Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si la máquina virtual se creó a partir de una imagen de Azure Marketplace, el agente se instala y se ejecuta. Si crea una máquina virtual personalizada o migra una máquina local, es posible que deba [instalar el agente manualmente](#install-the-vm-agent).

## <a name="create-a-vault"></a>Creación de un almacén

 Un almacén almacena las copias de seguridad y los puntos de recuperación creados con el tiempo y almacena las directivas de copia de seguridad asociadas a máquinas de las que se han realizado copias de seguridad. Cree un almacén como se indica a continuación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En la búsqueda, escriba **Recovery Services**. En **Servicios**, haga clic en **Almacenes de Recovery Services**.

     ![Busque los almacenes de Recovery Services.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png)

3. En el menú **Almacenes de Recovery Services**, haga clic en **+Agregar**.

     ![Creación del almacén de Recovery Services, paso 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

4. En el **Almacén de Recovery Services**, escriba un nombre descriptivo para identificar el almacén.
    * El nombre debe ser único para la suscripción de Azure.
    * Puede contener entre 2 y 50 caracteres.
    * Debe comenzar por una letra y solo puede contener letras, números y guiones.
5. Seleccione la suscripción de Azure, el grupo de recursos y la región geográfica en la que se debe crear el almacén. A continuación, haga clic en **Crear**.
    * La creación del almacén puede tardar un tiempo.
    * Supervise las notificaciones del estado en la parte superior derecha del portal.

Una vez que se crea el almacén, aparece en la lista de almacenes de Recovery Services. Si no lo ve, haga clic en **Actualizar**.

![Lista de copias de seguridad](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

>[!NOTE]
> Azure Backup ahora permite personalizar el nombre del grupo de recursos creado por el servicio Azure Backup. Para más información, vea [Grupo de recursos de Azure Backup para máquinas virtuales](backup-during-vm-creation.md#azure-backup-resource-group-for-virtual-machines).

### <a name="modify-storage-replication"></a>Modificar la replicación de almacenamiento

De forma predeterminada, los almacenes usan el [almacenamiento con redundancia geográfica (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

* Si el almacén es su mecanismo principal de copia de seguridad, le recomendamos que use GRS.
* Puede usar el [almacenamiento con redundancia local (LRS) ](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) si busca una opción más barata.

Modifique el tipo de replicación del almacenamiento tal como se indica a continuación:

1. En el nuevo almacén, haga clic en **Propiedades** en la sección **Configuración**.
2. En **Propiedades**, en **Configuración de copia de seguridad**, haga clic en **Actualizar**.
3. Seleccione el tipo de replicación de almacenamiento y haga clic en **Guardar**.

      ![Establecimiento de la configuración de almacenamiento del nuevo almacén](./media/backup-try-azure-backup-in-10-mins/full-blade.png)

> [!NOTE]
   > No puede modificar el tipo de replicación de almacenamiento después de configurar el almacén y si este contiene elementos de copia de seguridad. Si quiere hacer esto, debe volver a crear el almacén.

## <a name="apply-a-backup-policy"></a>Aplicar una directiva de copia de seguridad

Configurar una directiva de copia de seguridad para el almacén.

1. En el almacén, haga clic en **+ Copia de seguridad** en la sección **Introducción**.

   ![Botón Backup](./media/backup-azure-arm-vms-prepare/backup-button.png)

2. En **Objetivo de copia de seguridad** >  **¿Dónde se ejecuta su carga de trabajo?** , seleccione **Azure**. En **What do you want to back up?** (¿De qué desea realizar una copia de seguridad?), seleccione **Máquina virtual** >  **Aceptar**. Esto registra la extensión de la máquina virtual en el almacén.

   ![Paneles Backup y Objetivo de Backup](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. En **Directiva de copia de seguridad**, seleccione la directiva que desea asociar al almacén.
    * La directiva predeterminada hace una copia de seguridad de la VM una vez al día. Asimismo, las copias de seguridad diarias se conservan durante 30 días. Las instantáneas de recuperación instantánea se conservan durante dos días.
    * Si no quiere usar la directiva predeterminada, seleccione **Crear nueva** y cree una directiva personalizada tal como se describe en el siguiente procedimiento.

      ![Directiva de copia de seguridad predeterminada](./media/backup-azure-arm-vms-prepare/default-policy.png)

4. En **Seleccionar máquinas virtuales**, seleccione las VM de las que quiere hacer una copia de seguridad usando la directiva. A continuación, haga clic en **Aceptar**.

   * Las VM seleccionadas son validadas.
   * Solo se puede seleccionar máquinas virtuales de la región en que se encuentre el almacén.
   * Las copias de seguridad de las máquinas virtuales solo se pueden realizar en un almacén individual.

     ![Panel "Seleccionar máquinas virtuales"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. En **Copia de seguridad**, haga clic en **Habilitar copia de seguridad**. Esto implementa la directiva en el almacén y las máquinas virtuales, e instala la extensión de copia de seguridad en el agente de máquina virtual que se ejecuta en la máquina virtual de Azure.

     ![Botón "Habilitar copia de seguridad"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Después de habilitar la copia de seguridad:

* El servicio Backup instala la extensión de copia de seguridad tanto si la máquina virtual está en ejecución como si no lo está.
* Una copia de seguridad inicial se ejecutará según la programación de copia de seguridad.
* Cuando se ejecuten las copias de seguridad, tenga en cuenta que:
  * Una VM que se está ejecutando tiene una mayor oportunidad de capturar un punto de recuperación coherente con la aplicación.
  * Sin embargo, incluso si la VM está desactivada, se hace una copia de seguridad. Esa VM se conoce como una VM sin conexión. En este caso, el punto de recuperación será coherente frente a bloqueos.
* No se necesita conectividad de salida explícita para permitir la copia de seguridad de máquinas virtuales de Azure.

### <a name="create-a-custom-policy"></a>Creación de una directiva personalizada

Si creó una nueva directiva de copia de seguridad, complete la configuración de la misma.

1. En **Nombre de directiva**, especifique un nombre descriptivo.
2. En **Programación de copia de seguridad**, especifique cuándo se deben realizar las copias de seguridad. Puede realizar copias de seguridad diarias o semanales para VM de Azure.
3. En **Restauración instantánea**, especifique cuánto tiempo quiere conservar las instantáneas localmente para la restauración instantánea.
    * Cuando realice la restauración, los discos de la VM con copia de seguridad se copian desde el almacén a través de la red, hasta la ubicación de almacenamiento de recuperación. Con la restauración instantánea, puede aprovechar las instantáneas almacenadas localmente y que se tomaron durante un trabajo de copia de seguridad, sin esperar a que los datos de la copia de seguridad se transfieran al almacén.
    * Puede conservar las instantáneas para la restauración instantánea durante uno y cinco días. Dos días es la configuración predeterminada.
4. En el **rango de retención** , especifique cuánto tiempo quiere conservar sus puntos de copia de seguridad diarios o semanales.
5. En **Retención del punto de copia de seguridad mensual**, especifique si quiere mantener una copia de seguridad mensual de sus copias de seguridad diarias o semanales.
6. Haga clic en **Aceptar** para guardar la directiva.

    ![Nueva directiva de copia de seguridad](./media/backup-azure-arm-vms-prepare/new-policy.png)

> [!NOTE]
   > Azure Backup no admite el ajuste automático del reloj para los cambios de horario de verano para realizar copias de seguridad de máquinas virtuales de Azure. A medida que se produzcan cambios horarios, modifique las directivas de copia de seguridad manualmente según sea necesario.

## <a name="trigger-the-initial-backup"></a>Desencadenar la copia de seguridad inicial

La copia de seguridad inicial se ejecutará según la programación, peor puede ejecutarla inmediatamente de la manera siguiente:

1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **Elementos de copia de seguridad**, haga clic en **Máquina virtual de Azure**.
3. En la lista **Elementos de copia de seguridad**, haga clic en el botón de puntos suspensivos (...).
4. Haga clic en **Realizar copia de seguridad ahora**.
5. En **Realizar copia de seguridad ahora**, use el control del calendario para seleccionar el último día que debería retenerse el punto de recuperación. A continuación, haga clic en **Aceptar**.
6. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > **En curso**. Según el tamaño de la máquina virtual, la creación de la copia de seguridad inicial puede tardar un tiempo.

## <a name="verify-backup-job-status"></a>Comprobar el estado del trabajo de copia de seguridad

Los detalles del trabajo de la copia de seguridad de cada una de las máquinas virtuales constan de dos fases: la fase **Instantánea** y, después, la fase **Transferir datos al almacén**.<br/>
La primera de estas fases garantiza la disponibilidad de un punto de recuperación almacenado junto con los discos para realizar **Restauraciones instantáneas** y está disponibles un máximo de cinco días, en función del valor de conservación de instantáneas que haya configurado el usuario. Transferir datos al almacén crea un punto de recuperación en el almacén para la retención a largo plazo. La transferencia de datos al almacén solo comienza una vez que se completa la fase de instantánea.

  ![Estado del trabajo de copia de seguridad](./media/backup-azure-arm-vms-prepare/backup-job-status.png)

Existen dos **tareas secundarias** que se ejecutan en el back-end; una de ellas se usa en el trabajo de copia de seguridad del front-end que se puede comprobar desde la hoja de detalles de la **Tarea de copia de seguridad** como se indica a continuación:

  ![Estado del trabajo de copia de seguridad](./media/backup-azure-arm-vms-prepare/backup-job-phase.png)

La fase de **Transferencia de datos al almacén** puede tardar varios días en completarse, según el tamaño de los discos, la actividad de cada disco y otros factores.

El estado del trabajo puede variar según los siguientes escenarios:

**Instantánea** | **Transferir los datos al almacén** | **Estado del trabajo**
--- | --- | ---
Completed | En curso | En curso
Completed | Omitido | Completed
Completed | Completed | Completed
Completed | Con error | Completado con advertencia
Con error | Con error | Con error

Con esta capacidad y para la misma VM, se pueden ejecutar dos copias de seguridad en paralelo, pero en cualquier fase (instantánea, transferir datos al almacén) solo se puede ejecutar una subtarea. Por lo tanto, si un trabajo de copia de seguridad en curso produce un error en la copia de seguridad del día siguiente, este se evitará con esta funcionalidad de desacoplamiento. Las copias de seguridad del día siguiente pueden tener una instantánea completada mientras que la opción **Transferir datos al almacén** se omite si el trabajo de copia de seguridad de un día anterior está en curso.
El punto de recuperación incremental creado en el almacén capturará toda la rotación desde el último punto de recuperación creado en ese almacén. Esto no costará nada al usuario.

## <a name="optional-steps"></a>Pasos opcionales

### <a name="install-the-vm-agent"></a>Instalar el agente de máquina virtual

Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si la VM se creó a partir de una imagen de Azure Marketplace, el agente se instala y se ejecuta. Si creó una VM personalizada o migra una máquina local, es posible que deba instalar el agente manualmente.

**VM** | **Detalles**
--- | ---
**Windows** | 1. [Descargue e instale](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) el archivo MSI del agente.<br/><br/> 2. Realice la instalación con permisos de administrador en el equipo.<br/><br/> 3. Compruebe la instalación. En *C:\WindowsAzure\Packages* en la VM, haga clic con el botón derecho en **WaAppAgent.exe** > **Propiedades**. En la pestaña **Detalles**, la **versión del producto** debe ser 2.6.1198.718 o superior.<br/><br/> Si va a actualizar el agente, asegúrese de que no se ejecuta ninguna operación de copia de seguridad y [vuelva a instalar el agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Realice la instalación con un paquete de RPM o DEB del repositorio de paquetes de su distribución. Este es el método preferido para instalar y actualizar el agente Linux de Azure. Todos los [proveedores de distribución aprobada](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) integran el paquete de agente Linux de Azure en sus imágenes y repositorios. El agente está disponible en [GitHub](https://github.com/Azure/WALinuxAgent), pero no se recomienda instalarlo desde allí.<br/><br/> Si va a actualizar el agente, asegúrese de que no se ejecuta ninguna operación de copia de seguridad y actualice los archivos binarios.

>[!NOTE]
> **Azure Backup admite ahora la copia de seguridad y restauración de discos selectivos mediante la solución de copia de seguridad de máquinas virtuales de Azure.**
>
>En la actualidad, Azure Backup admite la copia de seguridad de todos los discos (sistema operativo y datos) en una máquina virtual junto con la solución de copia de seguridad de máquinas virtuales. Con la funcionalidad de exclusión de disco, tiene la opción de realizar una copia de seguridad de uno o de varios de los múltiples discos de datos de una máquina virtual. Esto proporciona una solución eficaz y rentable para sus necesidades de copia de seguridad y restauración. Cada punto de recuperación contiene datos de los discos incluidos en la operación de copia de seguridad, lo que permite además tener un subconjunto de discos restaurados desde el punto de recuperación determinado durante la operación de restauración. Esto se aplica a la restauración tanto desde la instantánea como desde el almacén.
>
>**Para suscribirse a la versión preliminar, escriba a AskAzureBackupTeam@microsoft.com** .

## <a name="next-steps"></a>Pasos siguientes

* Solucione todos los problemas que se produzcan con los [agentes de VM de Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) o con la [copia de seguridad de VM de Azure](backup-azure-vms-troubleshoot.md).
* [Restaure](backup-azure-arm-restore-vms.md) las VM de Azure.
