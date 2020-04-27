---
title: Migración de máquinas virtuales de VMware sin agente mediante la migración de servidores de Azure Migrate
description: Aprenda a ejecutar una migración de máquinas virtuales de VMware sin agentes con Azure Migrate.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: 4612c9b0ea2ef8d53b0c04f47628f3789705d833
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535322"
---
# <a name="migrate-vmware-vms-to-azure-agentless"></a>Migración de máquinas virtuales de VMware a Azure (sin agente)

En este artículo se muestra cómo migrar máquinas virtuales de VMware locales a Azure mediante una migración sin agente con la herramienta Azure Migrate Server Migration.

[Azure Migrate](migrate-services-overview.md) proporciona un centro de conectividad para realizar el seguimiento de la detección, evaluación y migración a Azure de las aplicaciones y cargas de trabajo locales, así como de las instancias de AWS o VM de GCP. El centro proporciona herramientas de Azure Migrate para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV).

Este tutorial es el tercero de una serie en la que se muestra cómo evaluar y migrar máquinas virtuales de VMware a Azure mediante Azure Migrate Server Assessment y Migration. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Preparar las máquinas virtuales para la migración.
> * Agregar la herramienta Azure Migrate Server Migration.
> * Detectar las máquinas virtuales que desea migrar.
> * Iniciar la replicación de las máquinas virtuales.
> * Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada.
> * Ejecutar una migración completa de la máquina virtual.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="migration-methods"></a>Métodos de migración

Puede migrar las máquinas virtuales de VMware a Azure mediante la herramienta Azure Migrate Server Migration. Esta herramienta ofrece un par de opciones para la migración de máquinas virtuales de VMware:

- Migración mediante la replicación sin agente. Las máquinas virtuales se migran sin necesidad de instalar nada en ellas.
- Migración con un agente de replicación. Instale un agente en la máquina virtual para su replicación.

Para decidir si desea utilizar la migración sin agente o basada en agente, consulte estos artículos:

- [Aprenda](server-migrate-overview.md) cómo funciona la migración sin agente y [compare los métodos de migración](server-migrate-overview.md#compare-migration-methods).
- [Lea este artículo](tutorial-migrate-vmware-agent.md) si desea utilizar el método basado en agente.

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar este tutorial, debe:

1. [Complete el primer tutorial](tutorial-prepare-vmware.md) de esta serie para configurar Azure y VMware para la migración. En este tutorial:
    - [Preparará Azure](tutorial-prepare-vmware.md#prepare-azure) para la migración.
    - [Preparará el entorno local](tutorial-prepare-vmware.md#prepare-for-agentless-vmware-migration) para la migración.
    
2. Se recomienda intentar evaluar las máquinas virtuales de VMware con Azure Migrate Server Assessment antes de migrarlas a Azure. Para ello, [complete el segundo tutorial](tutorial-assess-vmware.md) de esta serie. Si no desea evaluar las máquinas virtuales, puede omitir este tutorial. Aunque se recomienda realizar la evaluación, no es necesaria para migrar las máquinas virtuales.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Incorporación de la herramienta Azure Migrate Server Migration

Agregue la herramienta Azure Migrate:Server Migration.

- Si siguió el segundo tutorial para [evaluar las máquinas virtuales de VMware](/tutorial-assess-vmware.md), puede continuar y agregar la herramienta.
- Si no ha seguido el segundo tutorial, [siga estas instrucciones](how-to-add-tool-first-time.md) para configurar un proyecto de Azure Migrate.  Al crear el proyecto, se agrega la herramienta Azure Migrate:Server Migration.

Si tiene un proyecto configurado, agregue la herramienta de la siguiente manera:

1. En el proyecto de Azure Migrate, haga clic en **Información general**. 
2. En **Detectar, evaluar y migrar servidores**, haga clic en **Evaluar y migrar servidores**.

     ![Evaluar y migrar servidores](./media/tutorial-migrate-vmware/assess-migrate.png)

3. En **Herramientas de migración** , seleccione **Click here to add a migration tool when you are ready to migrate** (Haga clic aquí para agregar una herramienta de migración cuando esté listo para migrar).

    ![Seleccionar una herramienta](./media/tutorial-migrate-vmware/select-migration-tool.png)

4. En la lista de herramientas, seleccione **Azure Migrate: Server Migration** > **Agregar herramienta**.

    ![Herramienta Server Migration](./media/tutorial-migrate-vmware/server-migration-tool.png)

## <a name="set-up-the-azure-migrate-appliance"></a>Configuración del dispositivo de Azure Migrate

Azure Migrate Server Migration ejecuta un dispositivo ligero de máquina virtual de VMware. Dicho dispositivo realiza la detección de la máquina virtual y envía los metadatos y los datos de rendimiento que contiene a Azure Migrate:Server Migration. La herramienta Azure Migrate:Server Assessment usa el mismo dispositivo para realizar la migración sin agente de máquinas virtuales de VMware.

- Si ha seguido el segundo [tutorial para evaluar las máquinas virtuales de VMware](tutorial-assess-vmware.md), en él ya ha configurado el dispositivo.
- Si no ha seguido ese tutorial, puede configurar el dispositivo ahora con uno de estos métodos:
    - [Configúrelo](how-to-set-up-appliance-vmware.md) en una máquina virtual de VMware mediante una plantilla de OVA descargada.
    - Configúrelo en una máquina virtual de VMware o en una máquina física con un script del instalador de PowerShell. Debe usarse [este método](deploy-appliance-script.md) si no se puede configurar una máquina virtual mediante una plantilla de OVA, o si se encuentra en Azure Government.

Una vez creada la aplicación, compruebe que se puede conectar a Azure Migrate:Server Assessment, configúrela por primera vez y regístrela en el proyecto de Azure Migrate.


## <a name="prepare-vms-for-migration"></a>Preparación de las máquinas virtuales para la migración

Azure Migrate requiere algunos cambios en las máquinas virtuales para asegurarse de que se pueden migrar a Azure.

- En algunos sistemas operativos, Azure Migrate realiza estos cambios automáticamente. [Más información](migrate-support-matrix-vmware-migration.md#agentless-vmware-vms)
- Si va a migrar una máquina virtual que no tiene uno de estos sistemas operativos, siga las instrucciones para preparar la máquina virtual.
- Es importante realizar estos cambios antes de comenzar la migración. Si migra la máquina virtual antes de realizar el cambio, es posible que la máquina virtual no arranque en Azure.
- Los cambios de configuración que realice en las máquinas virtuales locales se replican en Azure después de habilitar la replicación de la máquina virtual. Para asegurarse de que los cambios se replican, asegúrese de que el punto de recuperación que va a migrar sea posterior a la hora a la que se realizaron los cambios de configuración en el entorno local.


### <a name="prepare-windows-server-vms"></a>Preparación de las máquinas virtuales de Windows Server

**Acción** | **Detalles** | **Instrucciones**
--- | --- | ---
Asegúrese de que los volúmenes de Windows en la máquina virtual de Azure usan las mismas asignaciones de letra de unidad que la máquina virtual local. | Configure la directiva SAN como "todo en línea". | 1. Inicie sesión en la máquina virtual con una cuenta de administrador y abra una ventana de comandos.<br/> 2. Escriba **diskpart** para ejecutar la utilidad Diskpart.<br/> 3. Escriba **SAN POLICY=OnlineAll**.<br/> 4. Escriba Exit para salir de DiskPart y cierre el símbolo del sistema.
Habilitación de la consola de acceso serie de Azure para la máquina virtual de Azure | Esto ayuda a solucionar problemas. No es necesario reiniciar la máquina virtual. La máquina virtual de Azure se iniciará usando la imagen de disco y esto equivale a reiniciar la nueva máquina virtual. | Siga [estas instrucciones](https://docs.microsoft.com/azure/virtual-machines/windows/serial-console) para habilitarlo.
Instalación de Hyper-V Guest Integration Services | Si va a migrar equipos que ejecutan Windows Server 2003, instale Hyper-V Guest Integration Services en el sistema operativo de la máquina virtual. | [Más información](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).
Escritorio remoto | Habilite Escritorio remoto en la máquina virtual y compruebe que Firewall de Windows no está bloqueando el acceso a Escritorio remoto en ningún perfil de red. | [Más información](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).

### <a name="prepare-linux-vms"></a>Preparación de las máquinas virtuales Linux

**Acción** | **Detalles** 
--- | --- | ---
Instalar Hyper-V Linux Integration Services | La mayoría de las nuevas versiones de las distribuciones de Linux lo incluyen de forma predeterminada.
Recompilar la imagen init de Linux para que contenga los controladores de Hyper-V necesarios | Esto garantiza que la máquina virtual se iniciará en Azure y solo se requiere en algunas distribuciones.
Habilitar el registro de la consola serie de Azure | Esto ayuda a solucionar problemas. No es necesario reiniciar la máquina virtual. La máquina virtual de Azure se iniciará usando la imagen de disco y esto equivale a reiniciar la nueva máquina virtual.<br/> Siga [estas instrucciones](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) para habilitarlo.
Actualizar el archivo de asignación de dispositivos | Actualice el archivo de asignación de dispositivos, que contiene las asociaciones de nombre de dispositivo y volumen, para usar identificadores de dispositivo persistentes.
Actualizar las entradas de fstab | Actualice las entradas para utilizar identificadores de volumen persistentes.
Eliminar reglas udev | Elimine las reglas udev que reserven los nombres de interfaz en función de la dirección MAC, etc.
Actualizar las interfaces de red | Actualice las interfaces de red para recibir direcciones IP basadas en DHCP.
Habilitar SSH | Asegúrese de que SSH está habilitado y de que el servicio sshd está configurado para iniciarse automáticamente al reiniciar.<br/> Asegúrese de que las solicitudes de conexión SSH entrantes no estén bloqueadas por el firewall del sistema operativo o las reglas que admiten scripts.

[Siga este artículo](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic) en el que se describen estos pasos para ejecutar una máquina virtual Linux en Azure, así como instrucciones para algunas de las distribuciones de Linux más populares.  


## <a name="replicate-vms"></a>Replicación de máquinas virtuales

Una vez finalizada la detección, puede comenzar la replicación de máquinas virtuales de VMware en Azure. 

> [!NOTE]
> Puede replicar hasta 10 máquinas juntas. Si necesita replicar más, replíquelas simultáneamente en lotes de 10. En el caso de la migración sin agente, puede ejecutar hasta 100 réplicas simultáneas.

1. En el proyecto de Azure Migrate > **Servidores**, **Azure Migrate: Migración del servidor**, haga clic en **Replicar**.

    ![Replicación de máquinas virtuales](./media/tutorial-migrate-vmware/select-replicate.png)

2. En **Replicar** > **Configuración de origen** >  **¿Las máquinas están virtualizadas?** , seleccione **Sí, con VMware vSphere Hypervisor**.
3. En **Dispositivo local**, seleccione el nombre del dispositivo de Azure Migrate que configuró > **OK**. 

    ![Configuración de origen](./media/tutorial-migrate-vmware/source-settings.png)

    - En este paso se da por hecho que ha completado el tutoria y ha configurado un dispositivo.
    - Si no lo ha hecho, siga las instrucciones de [este artículo.](how-to-set-up-appliance-vmware.md)

4. En **Máquinas virtuales**, seleccione las máquinas que desea replicar.
    - Si ha ejecutado una evaluación para las máquinas virtuales, puede aplicar las recomendaciones de tamaño y tipo de disco (Premium/estándar) de máquina virtual que sugieren los resultados de dicha evaluación. Para ello, en **¿Quiere importar la configuración de migración de evaluación de Azure Migrate?** , seleccione la opción **Sí**.
    - Si no ha ejecutado una evaluación o no desea usar la configuración de evaluación, seleccione la opción **No**.
    - Si ha decidido usar la evaluación, seleccione el grupo de máquinas virtuales y el nombre de la evaluación.

    ![Seleccionar evaluación](./media/tutorial-migrate-vmware/select-assessment.png)

5. En **Máquinas virtuales**, busque las máquinas virtuales que necesite y compruebe todas las que desee migrar. A continuación, haga clic en **Siguiente: Configuración de destino**.

    ![Selección de las máquinas virtuales](./media/tutorial-migrate-vmware/select-vms.png)

6. En **Configuración de destino**, seleccione la suscripción y la región de destino a la que va a migrar, y especifique el grupo de recursos en el que residirán las máquinas virtuales de Azure después de la migración. En **Red virtual**, seleccione la red virtual o la subred de Azure a la que se unirán las máquinas virtuales de Azure después de la migración.
7. En **Ventaja híbrida de Azure**:

    - Seleccione **No** si no desea aplicar la Ventaja híbrida de Azure. A continuación, haga clic en **Siguiente**.
    - Seleccione **Sí** si tiene equipos con Windows Server que están incluidos en suscripciones activas de Software Assurance o Windows Server y desea aplicar el beneficio a las máquinas que va a migrar. A continuación, haga clic en **Siguiente**.

    ![Configuración de destino](./media/tutorial-migrate-vmware/target-settings.png)

8. En **Proceso**, revise el nombre de la máquina virtual, su tamaño, el tipo de disco del sistema operativo y el conjunto de disponibilidad. Las máquinas virtuales deben cumplir los [requisitos de Azure](migrate-support-matrix-vmware-migration.md#azure-vm-requirements).

    - **Tamaño de VM**: si usa las recomendaciones de la evaluación, el menú desplegable de tamaño de VM contendrá el tamaño recomendado. De lo contrario, Azure Migrate elige un tamaño en función de la coincidencia más cercana en la suscripción de Azure. También puede elegir un tamaño de manera manual en **Tamaño de la máquina virtual de Azure**. 
    - **Disco del sistema operativo**: especifique el disco del sistema operativo (arranque) de la máquina virtual. Este es el disco que tiene el cargador de arranque y el instalador del sistema operativo. 
    - **Conjunto de disponibilidad**: si la máquina virtual debe estar incluida en un conjunto de disponibilidad de Azure después de la migración, especifique el conjunto. El conjunto debe estar en el grupo de recursos de destino que especifique para la migración.

    ![Configuración de los recursos de proceso de la máquina virtual](./media/tutorial-migrate-vmware/compute-settings.png)

9. En **Discos**, especifique si los discos de la máquina virtual se deben replicar en Azure y seleccione el tipo de disco (discos SSD o HDD estándar, o bien discos administrados prémium) en Azure. A continuación, haga clic en **Siguiente**.
    - Puede excluir discos de la replicación.
    - Si excluye discos, no estarán presentes en la máquina virtual de Azure después de la migración. 

    ![Discos](./media/tutorial-migrate-vmware/disks.png)

10. En **Revisar e iniciar la replicación**, revise la configuración y haga clic en **Replicar** para iniciar la replicación inicial de los servidores.

> [!NOTE]
> Puede actualizar la configuración de replicación en cualquier momento antes de que esta comience; para ello, vaya a **Administrar** > **Replicación de máquinas**. Una vez iniciada la replicación, su configuración no se puede cambiar.

### <a name="provisioning-for-the-first-time"></a>Primer aprovisionamiento

Si se trata de la primera máquina virtual que va a replicar en el proyecto de Azure Migrate, Azure Migrate Server Migration aprovisiona automáticamente estos recursos en el mismo grupo de recursos que el proyecto.

- **Service Bus**: Azure Migrate Server Migration usa Service Bus para enviar mensajes de orquestación de replicación al dispositivo.
- **Cuenta de almacenamiento de puerta de enlace**: Server Migration usa la cuenta de almacenamiento de puerta de enlace para almacenar información del estado de las máquinas virtuales que se replican.
- **Cuenta de almacenamiento de registros**: el dispositivo con Azure Migrate carga los registros de replicación de las máquinas virtuales en una cuenta de almacenamiento de registros. Azure Migrate aplica la información de replicación a los discos administrados de réplica.
- **Almacén de claves**: el dispositivo con Azure Migrate usa el almacén de claves para administrar las cadenas de conexión de Service Bus y las claves de acceso de las cuentas de almacenamiento utilizadas en la replicación. Debe haber configurado los permisos que necesita el almacén de claves para acceder a la cuenta de almacenamiento cuando esté preparado. [Revise dichos permisos](tutorial-prepare-vmware.md#assign-permissions-to-create-a-key-vault).   


## <a name="track-and-monitor"></a>Seguimiento y supervisión


- Al hacer clic en **Replicar**, comienza el trabajo de inicio de replicación. 
- Cuando el trabajo de inicio de replicación finaliza correctamente, las máquinas comienzan su replicación inicial en Azure.
- Durante la replicación inicial, se crea una instantánea de la máquina virtual. Los datos de disco de la instantánea se replican en los discos administrados de réplica en Azure.
- Cuando finaliza esta replicación inicial, comienza la replicación diferencial. Los cambios incrementales de los discos locales se replican periódicamente en los discos de réplica de Azure.

Puede realizar un seguimiento del estado del trabajo en las notificaciones del portal.

Para supervisar el estado de la replicación, haga clic en **Replicando servidores** en **Azure Migrate: Server Migration**.
![Supervisión de la replicación](./media/tutorial-migrate-vmware/replicating-servers.png)




## <a name="run-a-test-migration"></a>Ejecutar una migración de prueba


Cuando comienza la replicación diferencial, puede ejecutar una migración de prueba para las máquinas virtuales antes de ejecutar una migración completa a Azure. Le recomendamos encarecidamente que lo haga al menos una vez en cada máquina, antes de migrarla.

- La ejecución de una migración de prueba comprueba que la migración funcionará según lo previsto, sin afectar a las máquinas locales, que seguirán estando operativas y continuarán realizando la replicación. 
- Para simular la migración, la migración de prueba crea una máquina virtual de Azure usando datos replicados (normalmente, con una migración a una red virtual que no es de producción en la suscripción a Azure).
- Puede usar la máquina virtual de Azure de prueba replicada para validar la migración, realizar pruebas de aplicaciones y resolver los problemas antes de la migración completa.

Realice una migración de prueba como se indica a continuación:


1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Migration**, haga clic en **Probar servidores migrados**.

     ![Probar servidores migrados](./media/tutorial-migrate-vmware/test-migrated-servers.png)

2. Haga clic con el botón derecho en la máquina virtual que va a probar y haga clic en **Migración de prueba**.

    ![Migración de prueba](./media/tutorial-migrate-vmware/test-migrate.png)

3. En **Migración de prueba**, seleccione la red virtual de Azure en la que se ubicará la máquina virtual de Azure después de la migración. Se recomienda usar una red virtual que no sea de producción.
4. Comienza el trabajo de **Migración de prueba**. Supervise el trabajo en las notificaciones del portal.
5. Una vez finalizada la migración, la máquina virtual de Azure migrada se puede ver en **Máquinas virtuales** en Azure Portal. El nombre de la máquina tiene el sufijo **-Test**.
6. Una vez finalizada la prueba, haga clic con el botón derecho en la máquina virtual de Azure, en **Replicación de máquinas**, y haga clic en **Limpiar la migración de prueba**.

    ![Limpiar la migración](./media/tutorial-migrate-vmware/clean-up.png)


## <a name="migrate-vms"></a>Migración de máquinas virtuales

Después de comprobar que la migración de prueba funciona según lo previsto, puede migrar las máquinas locales.

1. En el proyecto de Azure Migrate > **Servidores** > **Azure Migrate: Server Migration**, haga clic en **Replicando servidores**.

    ![Replicando servidores](./media/tutorial-migrate-vmware/replicate-servers.png)

2. En **Replicación de máquinas**, haga clic con el botón derecho en la máquina virtual > **Migrar**.
3. En **Migrar** >  **¿Quiere apagar las máquinas virtuales y realizar una migración planificada sin perder datos?** , seleccione **Sí** > **Aceptar**.
    - De forma predeterminada, Azure Migrate apaga la máquina virtual local y ejecuta una replicación a petición para sincronizar los cambios que se han producido en la máquina virtual desde la última replicación. De esta forma se garantiza que no se pierden datos.
    - Si no desea apagar la máquina virtual, seleccione **No**
4. Se inicia un trabajo de migración de la máquina virtual. Realice un seguimiento del trabajo en las notificaciones de Azure.
5. Una vez finalizado el trabajo, la máquina virtual puede ver y administrar desde la página **Máquinas virtuales**.

## <a name="complete-the-migration"></a>Completar la migración

1. Una vez finalizada la migración, haga clic con el botón derecho en la máquina virtual > **Detener replicación**. Así se detiene la replicación en la máquina local y se limpia la información acerca del estado de replicación de la máquina virtual.
2. Instale el agente de [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) o [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) de la máquina virtual de Azure en las máquinas migradas.
3. Realice los ajustes de la aplicación posteriores a la migración, como actualizar las cadenas de conexión de la base de datos y las configuraciones del servidor web.
4. Realice las pruebas finales de la aplicación y la aceptación de la migración en la aplicación migrada que ahora se ejecuta en Azure.
5. Pase el tráfico a la instancia de máquina virtual de Azure migrada.
6. Quite las máquinas virtuales locales del inventario de máquinas virtuales local.
7. Quite las máquinas virtuales locales de las copias de seguridad locales.
8. Actualice la documentación interna para mostrar la nueva ubicación y la dirección IP las máquinas virtuales de Azure. 

## <a name="post-migration-best-practices"></a>Procedimientos recomendados después de la migración

- Para aumentar la resistencia:
    - Proteja los datos mediante la copia de seguridad de máquinas virtuales de Azure mediante el servicio Azure Backup. [Más información](../backup/quick-backup-vm-portal.md).
    - Mantenga las cargas de trabajo en ejecución y disponibles continuamente mediante la replicación de máquinas virtuales de Azure en una región secundaria con Site Recovery. [Más información](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Para aumentar la seguridad:
    - Bloquee y limite el acceso de tráfico de entrada con la [administración Just-In-Time de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - Restrinja el tráfico de red a los puntos de conexión de administración con [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Implemente [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) para ayudar a proteger discos y datos frente al robo y acceso no autorizado.
    - Obtenga más información sobre la [protección de recursos IaaS](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) y visite [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- Para supervisión y administración:
-  Considere la posibilidad de implementar [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) para supervisar el gasto y el uso de recursos.


## <a name="next-steps"></a>Pasos siguientes

Investigue el [proceso de la migración en la nube](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) en el marco de Cloud Adoption Framework para Azure.
