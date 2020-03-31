---
title: Replicación de máquinas virtuales de Azure mediante la ejecución de Espacios de almacenamiento directo con Azure Site Recovery
description: Obtenga información sobre cómo replicar máquinas virtuales de Azure mediante la ejecución de Espacios de almacenamiento directo con Azure Site Recovery.
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 01/29/2019
ms.openlocfilehash: 9f394fa8d618c97d74a47ff6e42a002f177cf7d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75973663"
---
# <a name="replicate-azure-vms-running-storage-spaces-direct-to-another-region"></a>Replicación de máquinas virtuales de Azure mediante la ejecución de Espacios de almacenamiento directo en otra región

En este artículo se describe cómo habilitar la recuperación ante desastres de VM de Azure que ejecutan Espacios de almacenamiento directo.

>[!NOTE]
>Solo se admiten los puntos de recuperación coherentes frente a bloqueos para los clústeres de Espacios de almacenamiento directo.
>

[Espacios de almacenamiento directo (S2D)](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct) es almacenamiento definido por software, que proporciona una manera de crear [clústeres invitados](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure) en Azure.  Un clúster invitado en Microsoft Azure es un clúster de conmutación por error formado por máquinas virtuales IaaS. Permite que las cargas de trabajo de VM hospedadas se conmuten por error a través de clústeres invitados para lograr SLA de mayor disponibilidad para las aplicaciones, en comparación con lo que puede proporcionar una sola VM de Azure. Resulta útil en escenarios donde la máquina virtual hospeda una aplicación crítica, como un servidor de archivo de escalabilidad horizontal o SQL.

## <a name="disaster-recovery-with-storage-spaces-direct"></a>Recuperación ante desastres con espacios de almacenamiento directo

En un escenario típico, puede tener el clúster de invitado de máquinas virtuales en Azure para lograr mayor resistencia de la aplicación, como servidor de archivos de escalabilidad horizontal. Si bien esto puede proporcionar mayor disponibilidad para las aplicaciones, le gustaría proteger estas aplicaciones mediante Site Recovery en caso de errores a nivel regional. Site Recovery replica los datos de una región a otra región de Azure, y trae el clúster en la región de recuperación ante desastres en caso de conmutación por error.

En el diagrama siguiente se muestra un clúster de conmutación por error de máquinas virtuales de Azure de dos nodos en el que se usan espacios de almacenamiento directo.

![storagespacesdirect](./media/azure-to-azure-how-to-enable-replication-s2d-vms/storagespacedirect.png)


- Dos máquinas virtuales de Azure en un clúster de conmutación por error de Windows, y cada máquina virtual tienen dos o más discos de datos.
- S2D sincroniza los datos del disco de datos y presenta el almacenamiento sincronizado como grupo de almacenamiento.
- El grupo de almacenamiento se presenta como volumen compartido de clúster (CSV) al clúster de conmutación por error.
- El clúster de conmutación por error usa el CSV para las unidades de datos.

**Consideraciones acerca de la recuperación ante desastres**

1. Cuando está configurando el [testigo de la nube](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp) para el clúster, mantenga el testigo en la región de recuperación ante desastres.
2. Si va a conmutar por error las máquinas virtuales a la subred en una región de recuperación ante desastres que es diferente de la región de origen, la dirección IP del clúster debe cambiarse después de la conmutación por error.  Para cambiar la dirección IP del clúster, debe usar el [script del plan de recuperación](https://docs.microsoft.com/azure/site-recovery/site-recovery-runbook-automation) de Site Recovery.</br>
[Script de ejemplo](https://github.com/krnese/azure-quickstart-templates/blob/master/asr-automation-recovery/scripts/ASR-Wordpress-ChangeMysqlConfig.ps1) para ejecutar el comando dentro de la VM mediante la extensión de script personalizado 

### <a name="enabling-site-recovery-for-s2d-cluster"></a>Habilitación de Site Recovery para el clúster de S2D:

1. Dentro del almacén de los servicios de recuperación, haga clic en "+ replicar".
1. Seleccione todos los nodos del clúster y hágalos que sean parte de un [grupo de coherencia de varias VM](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-common-questions#multi-vm-consistency).
1. Seleccione la directiva de replicación con la coherencia de la aplicación desactivada* (solo está disponible la compatibilidad con coherencia frente a bloqueos).
1. Habilite la replicación.

   ![protección de storagespacesdirect](./media/azure-to-azure-how-to-enable-replication-s2d-vms/multivmgroup.png)

2. Vaya a los elementos replicados y podrá ver el estado de ambas máquinas virtuales.
3. Ambas máquinas virtuales están protegidas y también se muestran como parte del grupo de coherencia de varias VM.

   ![protección de storagespacesdirect](./media/azure-to-azure-how-to-enable-replication-s2d-vms/storagespacesdirectgroup.PNG)

## <a name="creating-a-recovery-plan"></a>Creación de un plan de recuperación
Un plan de recuperación admite la secuenciación de distintas capas en una aplicación de varios niveles durante una conmutación por error. La secuenciación ayuda a mantener la coherencia de la aplicación. Cuando cree un plan de recuperación para una aplicación web de varios niveles, complete los pasos descritos en [Creación de un plan de recuperación mediante Site Recovery](site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Incorporación de máquinas virtuales a grupos de conmutación por error

1.  Cree un plan de recuperación agregando las máquinas virtuales.
2.  Haga clic en Personalizar para agrupar las máquinas virtuales. De forma predeterminada, todas las máquinas virtuales forman parte del grupo 1.


### <a name="add-scripts-to-the-recovery-plan"></a>Incorporación de scripts al plan de recuperación
Para que las aplicaciones funciones correctamente, es posible que tenga que realizar algunas operaciones en las máquinas virtuales de Azure después de la conmutación por error o durante una conmutación por error de prueba. Algunas de las operaciones posteriores a la conmutación por error se pueden automatizar. Por ejemplo, aquí se adjunta un equilibrador de carga y se cambia la dirección IP de clúster.


### <a name="failover-of-the-virtual-machines"></a>Conmutación por error de las máquinas virtuales 
Los dos nodos de las máquinas virtuales se deben conmutar por error mediante el [plan de recuperación](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) de Site Recovery. 

![protección de storagespacesdirect](./media/azure-to-azure-how-to-enable-replication-s2d-vms/recoveryplan.PNG)

## <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba
1.  En Azure Portal, seleccione el almacén de Recovery Services.
2.  Seleccione el plan de recuperación que creó.
3.  Seleccione **Conmutación por error de prueba**.
4.  Para iniciar el proceso de conmutación por error de prueba, seleccione el punto de recuperación y la red virtual de Azure.
5.  Cuando el entorno secundario esté activo, realice las validaciones.
6.  Cuando se completen las validaciones, seleccione **Limpiar conmutación por error de prueba** para limpiar el entorno de conmutación por error.

Para más información, consulte [Conmutación por error de prueba a Azure en Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Ejecución de la conmutación por error

1.  En Azure Portal, seleccione el almacén de Recovery Services.
2.  Seleccione el plan de recuperación que creó para las aplicaciones SAP.
3.  Seleccione **Failover** (Conmutación por error).
4.  Para iniciar el proceso de conmutación por error, seleccione el punto de recuperación.

Para más información, consulte [Conmutación por error en Site Recovery](site-recovery-failover.md).
## <a name="next-steps"></a>Pasos siguientes

[Más información](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) sobre la ejecución de conmutaciones por recuperación.
