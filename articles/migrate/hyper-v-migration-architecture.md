---
title: ¿Cómo funciona la migración de Hyper-V en Azure Migrate?
description: Más información sobre la migración de Hyper-V con Azure Migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: 390a8a49e9a47ee5e6845d85fe4fe02f514708e8
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91362204"
---
# <a name="how-does-hyper-v-replication-work"></a>Funcionamiento de la replicación de Hyper-V

En este artículo se proporciona información general sobre la arquitectura y los procesos que se usan al migrar máquinas virtuales de Hyper-V con la herramienta Azure Migrate Server Migration.

[Azure Migrate](migrate-services-overview.md) proporciona un centro de conectividad para realizar el seguimiento de la detección, evaluación y migración a Azure de las aplicaciones y cargas de trabajo locales, así como de máquinas virtuales en la nube privadas o públicas. El centro proporciona herramientas de Azure Migrate para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV).

## <a name="agentless-migration"></a>Migración sin agentes

La herramienta Azure Migrate Server Migration proporciona replicación sin agente para máquinas virtuales de Hyper-V locales mediante un flujo de trabajo de migración optimizado para Hyper-V. Necesita instalar únicamente un agente de software en los nodos de clúster o hosts de Hyper-V. No es necesario instalar nada en las máquinas virtuales de Hyper-V.

## <a name="server-migration-and-azure-site-recovery"></a>Server Migration y Azure Site Recovery

Azure Migrate Server Migration es una herramienta para migrar a Azure cargas de trabajo locales y máquinas virtuales basadas en la nube. Site Recovery es una herramienta de recuperación ante desastres. Las herramientas comparten algunos componentes tecnológicos comunes que se usan para la replicación de datos, pero sirven para propósitos diferentes. 


## <a name="architectural-components"></a>Componentes de la arquitectura

![En el diagrama se muestra una red de Hyper-V de origen con un canal de datos HTTPS a Microsoft Azure, con los detalles que se explican en una tabla.](./media/hyper-v-replication-architecture/architecture.png)



**Componente** | **Implementación** | 
--- | --- 
**Proveedor de replicación** | El proveedor de Microsoft Azure Site Recovery se instala en los hosts de Hyper-V y se registra con Azure Migration Server Migration.<br/> El proveedor orquesta la replicación para las máquinas virtuales de Hyper-V.
**Agente de Recovery Services** | El agente de Microsoft Azure Recovery Services se encarga de la replicación de datos. Trabaja junto con el proveedor para replicar los datos de las máquinas virtuales de Hyper-V en Azure.<br/> Los datos replicados se cargan en una cuenta de almacenamiento en la suscripción de Azure. La herramienta Server Migration procesa los datos replicados y los aplica a los discos de réplica de la suscripción. Los discos de réplica se usan para crear las máquinas virtuales de Azure durante la migración.

- Los componentes se instalan mediante un único archivo de instalación, que se descarga de Azure Migrate Server Migration, en el portal.
- El proveedor y el dispositivo usan conexiones de puerto HTTPS 443 saliente para comunicarse con Azure Migrate Server Migration.
- Las comunicaciones procedentes del proveedor y del agente son seguras y están cifradas.


## <a name="replication-process"></a>Proceso de replicación

1. Cuando habilita la replicación para una máquina virtual de Hyper-V, comienza la replicación inicial.
2. Se toma una instantánea de la máquina virtual de Hyper-V.
3. Los discos duros virtuales de la máquina virtual se replican uno a uno hasta que se copian todos en Azure. El tiempo de replicación inicial depende del tamaño de la máquina virtual y del ancho de banda de red.
4. Los cambios que se producen en los discos durante la replicación inicial se supervisan mediante Réplica de Hyper-V y se almacenan en archivos de registro (archivos .hrl).
    - Estos archivos de registro se encuentran en la misma carpeta que los discos.
    - Cada disco tiene un archivo .hrl asociado que se envía al almacenamiento secundario.
    - Los archivos de instantáneas y de registro consumen recursos de disco mientras la replicación inicial está en curso.
4. Una vez que ha finalizado la replicación inicial, se elimina la instantánea de la máquina virtual y la replicación diferencial comienza.
5. Se realiza un seguimiento en los archivos .hrl de los cambios incrementales que se producen en los discos. El agente de Recovery Services carga periódicamente los registros de replicación en una cuenta de almacenamiento de Azure.


## <a name="performance-and-scaling"></a>Rendimiento y escalado

El rendimiento de replicación de Hyper-V se ve afectado por factores como el tamaño de la máquina virtual, la frecuencia de cambio de datos (renovación) de las máquinas virtuales, el espacio disponible en el host de Hyper-V para el almacenamiento de archivos de registro, el ancho de banda de carga para los datos de replicación y el almacenamiento de destino en Azure.

- Si va a replicar varias máquinas al mismo tiempo, use [Azure Site Recovery Deployment Planner](../site-recovery/hyper-v-deployment-planner-overview.md) de Hyper-V para optimizar la replicación.
- Planifique la replicación de Hyper-V y distribúyala en varias cuentas de almacenamiento de Azure, de acuerdo con la capacidad.

### <a name="control-upload-throughput"></a>Control del rendimiento de carga

Puede limitar la cantidad de ancho de banda que se usa para cargar datos a Azure en cada host de Hyper-V. Pero tenga cuidado. Si establece valores demasiado bajos, afectará negativamente a la replicación y retrasará la migración.


1. Inicie sesión en el nodo de clúster o en el host de Hyper-V.
2. Ejecute **C:\Archivos de programa\Microsoft Azure Recovery Services Agent\bin\wabadmin.msc** para abrir el complemento MMC de Azure Backup de Windows.
3. En el complemento, seleccione **Cambiar propiedades**.
4. En **Limitación**, seleccione **Habilitar límite de uso del ancho de banda de Internet para las operaciones de copia de seguridad**. Establezca los límites para las horas laborables y no laborables. Los intervalos válidos van de 512 Kbps a 1023 Mbps.
I

### <a name="influence-upload-efficiency"></a>Control de la eficiencia de carga

Si tiene ancho de banda de reserva para la replicación y quiere aumentar las cargas, puede incrementar el número de subprocesos asignados a la tarea de carga, como se indica a continuación:

1. Abra el Registro con regedit.
2. Vaya a la clave HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM.
3. Aumente el valor del número de subprocesos usados para la carga de datos para cada máquina virtual de replicación. El valor predeterminado es 4 y el valor máximo es 32. 




## <a name="next-steps"></a>Pasos siguientes

Pruebe la [migración de Hyper-V](tutorial-migrate-hyper-v.md) con Azure Migrate Server Migration.
