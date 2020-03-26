---
title: Ejecución de un simulacro de recuperación ante desastres en máquinas virtuales de Azure con Azure Site Recovery
description: Aprenda a ejecutar un simulacro de recuperación ante desastres en máquinas virtuales de Azure de una región de Azure secundaria mediante el servicio Azure Site Recovery.
services: site-recovery
ms.topic: tutorial
ms.date: 01/16/2020
ms.custom: mvc
ms.openlocfilehash: b2ce157f0f192135ab0507e4aae4c0a282bda1ea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76166189"
---
# <a name="run-a-disaster-recovery-drill-to-a-secondary-region-for-azure-vms"></a>Ejecución de un simulacro de recuperación ante desastres en máquinas virtuales de Azure de una región secundaria de Azure

El servicio [Azure Site Recovery](site-recovery-overview.md) contribuye a la estrategia de recuperación ante desastres y continuidad empresarial (BCDR) al mantener sus aplicaciones empresariales al día y disponibles durante interrupciones planeadas y no planeadas. Azure Site Recovery administra y coordina la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure, lo que incluye la replicación, la conmutación por error y la recuperación.

En este tutorial se muestra cómo ejecutar una exploración de recuperación ante desastres en una máquina virtual de Azure, desde una región de Azure a otra, con una conmutación por error de prueba. Mediante una exploración se valida su estrategia de replicación sin ocasionar ninguna pérdida de datos ni tiempo de inactividad, y sin afectar al entorno de producción. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Comprobar los requisitos previos
> * Ejecutar una conmutación por error de prueba en una sola máquina virtual

> [!NOTE]
> Este tutorial le ayuda a obtener los detalles de una recuperación ante desastres con un mínimo de pasos. Para más información sobre las distintas funciones relacionadas con la obtención de los detalles de recuperación ante desastres, consulte la documentación de [replicación](azure-to-azure-how-to-enable-replication.md), [redes](azure-to-azure-about-networking.md), [automatización](azure-to-azure-powershell.md) o [solución de problemas](azure-to-azure-troubleshoot-errors.md) para las máquinas virtuales de Azure.

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar con este tutorial, compruebe los elementos siguientes:

- Antes de ejecutar una conmutación por error de prueba, se recomienda que compruebe las propiedades de la máquina virtual para asegurarse de que está configurada para la recuperación ante desastres. Vaya a **Operaciones** > **Recuperación ante desastres** > **Propiedades** de la máquina virtual para ver las propiedades de replicación y conmutación por error.
- **Se recomienda usar una red de máquina virtual de Azure independiente en la conmutación por error de prueba** en lugar de la red predeterminada que se configuró cuando habilitó la replicación.
- Según la configuración de la red de origen de cada NIC, puede especificar los valores de **Subred**, **Dirección IP privada**, **Dirección IP pública**, **Grupo de seguridad de red** o **Equilibrador de carga** para asociarlos a cada NIC en **Proceso y red** de la configuración de conmutación por error de prueba antes de realizar el simulacro de recuperación ante desastres.

## <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

En este ejemplo se muestra cómo usar un almacén de Recovery Services para realizar una conmutación por error de prueba de máquina virtual.

1. Seleccione un almacén,vaya a **Elementos protegidos** > **Elementos replicados** y seleccione una máquina virtual.
1. En **Conmutación por error**, seleccione el punto de recuperación que usará para la conmutación por error:
   - **Más reciente**: procesa todos los datos en el servicio Site Recovery y proporciona el objetivo de tiempo de recuperación (RTO) más bajo.
   - **Procesado más recientemente**: conmuta por error la máquina virtual en el último punto de recuperación procesado por Site Recovery. Se muestra la marca de tiempo. Con esta opción no se emplea tiempo en el procesamiento de datos, por lo que proporciona un objetivo de tiempo de recuperación bajo.
   - **Más reciente coherente con la aplicación**: esta opción conmuta por error todas las VM en el punto de recuperación más reciente coherente con la aplicación. Se muestra la marca de tiempo.
   - **Personalizado**: conmuta por error a un punto de recuperación concreto. La personalización solo está disponible cuando se conmuta por error una sola máquina virtual y no para la conmutación por error con un plan de recuperación.
1. Seleccione la red virtual de Azure de destino a la que se conectarán las máquinas virtuales de Azure en la región secundaria tras la conmutación por error.

   > [!NOTE]
   > El menú desplegable para seleccionar la red virtual de Azure no estará visible si la configuración de conmutación por error de prueba está preconfigurada para el elemento replicado.

1. Seleccione **Aceptar** para iniciar la conmutación por error. Para realizar el seguimiento del progreso desde el almacén, vaya a **Supervisión** > **Trabajos de Site Recovery** y seleccione el trabajo **Conmutación por error de prueba**.
1. Una vez finalizada la conmutación por error, la réplica de la máquina virtual de Azure aparece en **Máquinas virtuales** en Azure Portal. Asegúrese de que la máquina virtual está en funcionamiento, tiene el tamaño adecuado y está conectada a la red apropiada.
1. Para eliminar las máquinas virtuales que se crearon durante la conmutación por error de prueba, seleccione **Limpiar conmutación por error de prueba** en el elemento replicado o el plan de recuperación. En **Notas**, registre y guarde las observaciones asociadas a la conmutación por error de prueba.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejecutar una conmutación por error de producción](azure-to-azure-tutorial-failover-failback.md)
