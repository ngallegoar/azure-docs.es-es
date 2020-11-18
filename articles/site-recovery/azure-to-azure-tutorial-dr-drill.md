---
title: Tutorial para realizar un simulacro de la recuperación ante desastres de máquinas virtuales de Azure con Azure Site Recovery
description: En este tutorial, ejecutará un simulacro de la recuperación ante desastres de máquinas virtuales de Azure en otra región mediante Site Recovery.
services: site-recovery
ms.topic: tutorial
ms.date: 11/05/2020
ms.custom: mvc
ms.openlocfilehash: c7cd1898f27f3b7255009efb40f6bcc8938dbf9e
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93395657"
---
# <a name="tutorial-run-a-disaster-recovery-drill-for-azure-vms"></a>Tutorial: Ejecución de un simulacro de la recuperación ante desastres de máquinas virtuales de Azure

Aprenda a realizar un simulacro de la recuperación ante desastres a otra región de Azure, para máquinas virtuales de Azure con [Azure Site Recovery](site-recovery-overview.md). En este artículo:

> [!div class="checklist"]
> * Comprobará los requisitos previos.
> * Comprobará la configuración de la máquina virtual antes del simulacro.
> * Ejecutará una conmutación por error de prueba.
> * Realizará una limpieza después del simulacro.


> [!NOTE]
> En este tutorial se proporcionan los pasos mínimos para realizar un simulacro de la recuperación ante desastres. Si desea realizar un simulacro con una prueba de la infraestructura completa, obtenga información sobre las [redes](azure-to-azure-about-networking.md), la [automatización](azure-to-azure-powershell.md) y la [solución de problemas](azure-to-azure-troubleshoot-errors.md) de las máquinas virtuales de Azure.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar este tutorial, debe habilitar la recuperación ante desastres en una o varias máquinas virtuales de Azure. Para ello, [complete el primer tutorial](azure-to-azure-tutorial-enable-replication.md) de esta serie.

## <a name="verify-vm-settings"></a>Comprobación de la configuración de una máquina virtual

1. En el almacén > **Elementos replicados**, seleccione la máquina virtual.

    ![Opción para abrir la página Recuperación ante desastres en las propiedades de la máquina virtual](./media/azure-to-azure-tutorial-dr-drill/vm-settings.png)

2. En la página **Información general**, compruebe que la máquina virtual está protegida y en buen estado.
3. Cuando se ejecuta una conmutación por error de prueba, se selecciona una red virtual de Azure en la región de destino. La máquina virtual de Azure que se crea después de la conmutación por error se coloca en esta red. 

    - En este tutorial, se selecciona una red existente al ejecutar la conmutación por error de prueba.
    - Para el simulacro, es aconsejable elegir una red que no sea de producción, con el fin de que tanto las direcciones IP como los componentes de la red sigan estando disponibles en las redes de producción.
   - También puede preconfigurar los valores de la red que se van a usar para la conmutación por error de prueba. La configuración pormenorizada que se puede asignar a cada NIC incluye una subred, una dirección IP privada, una dirección IP pública, un equilibrador de carga y un grupo de seguridad de red. Aquí no se va a usar este método, pero si desea más información al respecto, puede [consultar este artículo](azure-to-azure-customize-networking.md#customize-failover-and-test-failover-networking-configurations).


## <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba


1. En la página **Información general**, seleccione **Conmutación por error de prueba**.

    
    ![Botón de conmutación por error de prueba del elemento replicado](./media/azure-to-azure-tutorial-dr-drill/test-failover-button.png)

2. En **Conmutación por error de prueba** , elija un punto de recuperación. Se usan los datos de este punto de recuperación para crear la máquina virtual de Azure en la región de destino.
  
   - **Procesado más recientemente**: usa el último punto de recuperación procesado por Site Recovery. Se muestra la marca de tiempo. No se emplea tiempo en el procesamiento de datos, por lo que se proporciona un objetivo de tiempo de recuperación (RTO) bajo.
   -  **Más reciente**: procesa todos los datos enviados a Site Recovery para crear un punto de recuperación para cada máquina virtual antes de conmutarla por error a dicho punto de recuperación. Proporciona el menor objetivo de punto de recuperación (RPO), ya que todos los datos se replican a Site Recovery cuando se desencadena la conmutación por error.
   - **Más reciente coherente con la aplicación**: esta opción conmuta por error las VM en el punto de recuperación más reciente coherente con la aplicación. Se muestra la marca de tiempo.
   - **Personalizado**: conmuta por error a un punto de recuperación concreto. La personalización solo está disponible cuando se conmuta por error una sola máquina virtual y no se usa un plan de recuperación.

3. En **Red virtual de Azure**, seleccione la red de destino en la que se van a colocar las máquinas virtuales de Azure creadas después de la conmutación por error. Si es posible, seleccione una red que no sea de producción, no la red que se creó al habilitar la replicación.

    ![Página de configuración de la conmutación por error de prueba](./media/azure-to-azure-tutorial-dr-drill/test-failover-settings.png)    

4. Seleccione **Aceptar** para iniciar la conmutación por error.
5. Supervise la conmutación por error de prueba en las notificaciones.

    ![Notificación de progreso](./media/azure-to-azure-tutorial-dr-drill/notification-start-test-failover.png) ![Notificación de éxito](./media/azure-to-azure-tutorial-dr-drill/notification-finish-test-failover.png)     


5. Cuando la conmutación por error termina, la máquina virtual de Azure creada en la región de destino aparece en **Máquinas virtuales**, en Azure Portal. Asegúrese de que la máquina virtual se ejecuta, que tiene el tamaño adecuado y que está conectada a la red que seleccionó.

## <a name="clean-up-resources"></a>Limpieza de recursos

1. En la página **Essentials**, seleccione **Limpiar conmutación por error de prueba**.

    ![Botón para iniciar el proceso de limpieza](./media/azure-to-azure-tutorial-dr-drill/select-cleanup.png)

2. En **Limpiar conmutación por error de prueba** > **Notas**, registre y guarde las observaciones asociadas a la conmutación por error de prueba. 
3. Seleccione **Testing is complete** (Se han completado la pruebas) para eliminar las máquinas virtuales creadas durante la conmutación por error de prueba.

    ![Página con opciones de limpieza](./media/azure-to-azure-tutorial-dr-drill/cleanup-failover.png)

4. Supervise el progreso de la limpieza en las notificaciones.

    ![Notificación de progreso de limpieza](./media/azure-to-azure-tutorial-dr-drill/notification-start-cleanup.png) ![Notificación de limpieza realizada correctamente](./media/azure-to-azure-tutorial-dr-drill/notification-finish-cleanup.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha realizado un simulacro de recuperación ante desastres para comprobar que la conmutación por error funciona como cabría esperar. Ahora puede probar una conmutación por error completa.

> [!div class="nextstepaction"]
> [Ejecutar una conmutación por error de producción](azure-to-azure-tutorial-failover-failback.md)
