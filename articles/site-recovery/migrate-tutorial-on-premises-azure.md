---
title: Migre sus máquinas locales con Azure Site Recovery
description: En este artículo se describe cómo migrar máquinas locales a Azure mediante Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 24015810a295ef88b7d3e63bfc464ddddef6b55f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "73939633"
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migración de máquinas locales a Azure


En este artículo se describe cómo migrar máquinas locales a Azure mediante [Azure Site Recovery](site-recovery-overview.md). Por lo general, Azure Site Recovery se usa para administrar y orquestar la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure. Sin embargo, también se puede usar para la migración. La migración utiliza los mismos pasos que la recuperación ante desastres, salvo una excepción. En la migración, la conmutación por error de las maquinas desde el sitio local es el último paso. A diferencia de la recuperación ante desastres, en los escenarios de migración no se puede realizar la conmutación por recuperación en un entorno local.


En este tutorial se muestra cómo migrar máquinas virtuales locales y servidores físicos a Azure. Aprenderá a:

> [!div class="checklist"]
> * Configurar los entornos de origen y destino para la migración
> * Configurar una directiva de replicación
> * Habilitar replicación
> * Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada
> * Ejecutar una conmutación por error única en Azure


> [!TIP]
> Ahora puede migrar servidores de entornos locales a Azure mediante el servicio Azure Migrate. [Más información](../migrate/migrate-services-overview.md)

## <a name="before-you-start"></a>Antes de comenzar

Tenga en cuenta que no se admiten los dispositivos exportados por controladores paravirtualizados.


## <a name="prepare-azure-and-on-premises"></a>Preparación de Azure y del entorno local

1. Prepare Azure como se describe en [este artículo](tutorial-prepare-azure.md). Aunque en dicho artículo se describen los pasos necesarios para preparar una recuperación ante desastres, dichos pasos también son válidos para la migración.
2. Prepare los servidores de [VMware](vmware-azure-tutorial-prepare-on-premises.md) o [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) locales. Si va a migrar equipos físicos, no es necesario preparar nada. Solo hay que comprobar la [matriz de compatibilidad](vmware-physical-azure-support-matrix.md).


## <a name="select-a-protection-goal"></a>Selección de un objetivo de protección

Seleccione aquello que desea replicar y la ubicación donde se va a realizar la replicación.
1. Haga clic en **Almacenes de Recovery Services** > almacén.
2. En el menú de recursos, haga clic en **Site Recovery** > **Preparar la infraestructura** > **Objetivo de protección**.
3. En **Objetivo de protección**, seleccione el contenido que quiera migrar.
    - **VMware**: seleccione **To Azure (En Azure)**  > **Yes, with VMware vSphere Hypervisor** (Sí, con VMware vSphere Hypervisor).
    - En **Máquina física**: seleccione **To Azure (En Azure)**  > **No virtualizado/Otro**.
    - **Hyper-V**: seleccione **To Azure (En Azure)**  > **Yes, with Hyper-V (Sí, con Hyper-V)** . Si VMM administra las máquinas virtuales de Hyper-V, seleccione **Sí**.


## <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

**Escenario** | **Detalles**
--- | --- 
VMware | Configuración del [entorno de origen](vmware-azure-set-up-source.md) y del [servidor de configuración](vmware-azure-deploy-configuration-server.md).
Máquina física | [Configure](physical-azure-set-up-source.md) el entorno de origen y el servidor de configuración.
Hyper-V | Configuración del [entorno de origen](hyper-v-azure-tutorial.md#set-up-the-source-environment)<br/><br/> Configure el [entorno de origen](hyper-v-vmm-azure-tutorial.md#set-up-the-source-environment) para Hyper-V implementado con System Center VMM.

## <a name="set-up-the-target-environment"></a>Configuración del entorno de destino

Seleccione y compruebe los recursos de destino.

1. Haga clic en **Preparar infraestructura** > **Destino** y seleccione la suscripción de Azure que quiere usar.
2. Especifique el modelo de implementación de Resource Manager.
3. Site Recovery comprueba los recursos de Azure.
    - Si va a migrar máquinas virtuales de VMware o servidores físicos, Site Recovery comprueba que dispone de una red de Azure en el que se encontrarán las máquinas virtuales de Azure cuando se creen después de la conmutación por error.
    - Si va a migrar máquinas virtuales de Hyper-V, Site Recovery comprueba que tiene una red y una cuenta de Azure Storage compatibles.
4. Si va a migrar máquinas virtuales de Hyper-V administradas por System Center VMM, configure la [asignación de red](hyper-v-vmm-azure-tutorial.md#configure-network-mapping).

## <a name="set-up-a-replication-policy"></a>Configurar una directiva de replicación

**Escenario** | **Detalles**
--- | --- 
VMware | Configure una [directiva de replicación](vmware-azure-set-up-replication.md) para las máquinas virtuales de VMware.
Máquina física | Configure una [directiva de replicación](physical-azure-disaster-recovery.md#create-a-replication-policy) para los equipos físicos.
Hyper-V | Configuración de una [directiva de replicación](hyper-v-azure-tutorial.md#set-up-a-replication-policy)<br/><br/> Configure una [directiva de replicación](hyper-v-vmm-azure-tutorial.md#set-up-a-replication-policy) para Hyper-V implementado con System Center VMM.

## <a name="enable-replication"></a>Habilitar replicación

**Escenario** | **Detalles**
--- | --- 
VMware | [Habilite la replicación](vmware-azure-enable-replication.md) para las máquinas virtuales de VMware.
Máquina física | [Habilite la replicación](physical-azure-disaster-recovery.md#enable-replication) de los equipos físicos.
Hyper-V | [Habilitar replicación](hyper-v-azure-tutorial.md#enable-replication)<br/><br/> [Habilite la replicación](hyper-v-vmm-azure-tutorial.md#enable-replication) para Hyper-V implementado con System Center VMM.


## <a name="run-a-test-migration"></a>Ejecutar una migración de prueba

Ejecute una [conmutación por error de prueba](tutorial-dr-drill-azure.md) en Azure para asegurarse de que todo funciona de la forma esperada.


## <a name="migrate-to-azure"></a>Migración a Azure

Ejecute una conmutación por error para las máquinas que desea migrar.

1. En **Configuración** > **Elementos replicados**, haga clic en la máquina > **Conmutación por error**.
2. En **Conmutación por error**, seleccione un **punto de recuperación** en el que realizar la conmutación por error. Seleccione el punto de recuperación más reciente.
3. La configuración de la clave de cifrado no es importante para este escenario.
4. Seleccione **Apague la máquina antes de comenzar con la conmutación por error**. Site Recovery intentará apagar las máquinas virtuales antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.
5. Compruebe que la máquina virtual de Azure aparece en Azure según lo previsto.
6. En **Elementos replicados**, haga clic con el botón derecho en la máquina virtual > **Completar migración**. Esto hace lo siguiente:

   - Finaliza el proceso de migración, se detiene la replicación de la máquina virtual local y se detiene la facturación de Site Recovery para la máquina virtual.
   - Este paso limpia los datos de replicación. No elimina las máquinas virtuales migradas.

     ![Completar migración](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **No cancele una conmutación por error en curso**: antes de iniciar la conmutación por error, se detiene la replicación de la máquina virtual. Si se cancela una conmutación por error en curso, la conmutación por error se detiene, pero no se replica la máquina virtual de nuevo.

En algunos escenarios, la conmutación por error requiere un procesamiento adicional que tarda aproximadamente de ocho a diez minutos en completarse. Puede observar tiempos de conmutación por error de prueba más largos en los servidores físicos, las máquinas de VMware Linux, las máquinas virtuales de VMware que no tienen el servicio DHCP habilitado y las máquinas virtuales de VMware que no tienen los siguientes controladores de arranque: storvsc, vmbus, storflt, intelide y atapi.

## <a name="after-migration"></a>Después de la migración

Una vez migradas las máquinas a Azure, hay una serie de pasos que debe completar.

Algunos pasos se pueden automatizar como parte del proceso de migración con la funcionalidad de scripts de automatización integrada de los [planes de recuperación](site-recovery-runbook-automation.md).   


### <a name="post-migration-steps-in-azure"></a>Pasos posteriores a la migración a Azure

- Realice los ajustes de la aplicación posteriores a la migración, como actualizar las cadenas de conexión de la base de datos y las configuraciones del servidor web. 
- Realice las pruebas finales de la aplicación y la aceptación de la migración en la aplicación migrada que ahora se ejecuta en Azure.
- El [agente de máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) administra la interacción de una máquina virtual con el controlador de tejido de Azure. Se requiere para algunos servicios de Azure, como Azure Backup, Site Recovery y Azure Security.
    - Si va a migrar servidores físicos y máquinas de VMware, el instalador de Mobility Service instala el agente de máquina virtual de Azure disponible en máquinas Windows. En máquinas virtuales Linux, se recomienda que instale al agente después de la conmutación por error.
    - Si va a migrar máquinas virtuales de Azure en una región secundaria, se debe aprovisionar el agente de máquina virtual de Azure en la máquina virtual antes de la migración.
    - Si va a migrar máquinas virtuales de Hyper-V a Azure, instale al agente de máquina virtual de Azure en la máquina virtual de Azure después de la migración.
- Quite manualmente cualquier proveedor/agente de Site Recovery de la máquina virtual. Si migra máquinas virtuales de VMware o servidores físicos, desinstale Mobility Service de la máquina virtual.
- Para aumentar la resistencia:
    - Proteja los datos mediante la copia de seguridad de máquinas virtuales de Azure mediante el servicio Azure Backup. [Más información]( https://docs.microsoft.com/azure/backup/quick-backup-vm-portal).
    - Mantenga las cargas de trabajo en ejecución y disponibles continuamente mediante la replicación de máquinas virtuales de Azure en una región secundaria con Site Recovery. [Más información](azure-to-azure-quickstart.md).
- Para aumentar la seguridad:
    - Bloquee y limite el acceso de tráfico de entrada con la [administración Just-In-Time]( https://docs.microsoft.com/azure/security-center/security-center-just-in-time) de Azure Security Center.
    - Restrinja el tráfico de red a los puntos de conexión de administración con [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Implemente [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) para ayudar a proteger discos y datos frente al robo y acceso no autorizado.
    - Obtenga más información sobre la [protección de recursos IaaS]( https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/ ) y visite [Azure Security Center](https://azure.microsoft.com/services/security-center/ ).
- Para supervisión y administración:
    - Considere la posibilidad de implementar [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) para supervisar el gasto y el uso de recursos.

### <a name="post-migration-steps-on-premises"></a>Pasos posteriores a la migración local

- Mueva el tráfico de la aplicación hasta la aplicación que se ejecuta en la instancia de máquina virtual de Azure migrada.
- Quite las máquinas virtuales locales del inventario de máquinas virtuales local.
- Quite las máquinas virtuales locales de las copias de seguridad locales.
- Actualice la documentación interna para mostrar la nueva ubicación y la dirección IP las máquinas virtuales de Azure.




## <a name="next-steps"></a>Pasos siguientes

En este tutorial se migran máquinas virtuales locales a máquinas virtuales de Azure. Ahora

> [!div class="nextstepaction"]
> [Configure la recuperación ante desastres](azure-to-azure-replicate-after-migration.md) en una región secundaria de Azure para las máquinas virtuales de Azure.

  
