---
title: Tutorial para conmutar por recuperación máquinas virtuales de Azure a una región primaria durante la recuperación ante desastres con Azure Site Recovery.
description: Tutorial para obtener información sobre la conmutación por recuperación de máquinas virtuales de Azure en una región primaria con Azure Site Recovery.
ms.topic: tutorial
ms.date: 11/05/2020
ms.custom: mvc
ms.openlocfilehash: 5c127010a7988bf08c77340a4fc10bb32dc76f87
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93393945"
---
# <a name="tutorial-fail-back-azure-vm-to-the-primary-region"></a>Tutorial: Conmutación por recuperación a la región primaria

Después de realizar la conmutación por error de una máquina virtual de Azure a una región secundaria de Azure, siga este tutorial para conmutar la máquina virtual por recuperación a la región primaria de Azure mediante [Azure Site Recovery](site-recovery-overview.md).  En este artículo aprenderá a:

> [!div class="checklist"]
> 
> * Examinar los requisitos previos.
> * Conmutar por recuperación la máquina virtual en la región secundaria.
> * Volver a proteger las máquinas virtuales primarias en la región secundaria.
> 
> [!NOTE]
> En este tutorial se muestra cómo realizar la conmutación por recuperación con el mínimo número de pasos. Si desea ejecutar una conmutación por error con una configuración completa, obtenga información sobre las [redes](azure-to-azure-about-networking.md), la [automatización](azure-to-azure-powershell.md) y la [solución de problemas](azure-to-azure-troubleshoot-errors.md) de las máquinas virtuales de Azure.



## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar este tutorial, debe haber:

1. [Configurado la replicación](azure-to-azure-tutorial-enable-replication.md) para al menos una máquina virtual de Azure y probado un [simulacro de la recuperación ante desastres](azure-to-azure-tutorial-dr-drill.md) para ella.
2. [Conmutado por error la máquina virtual](azure-to-azure-tutorial-failover-failback.md) desde la región primaria a una región secundaria y la ha vuelto a proteger para que se replique desde la región secundaria a la primaria. 
3. Compruebe que la región primaria está disponible, y que puede crear y tener acceso a los recursos nuevos en ella.

## <a name="fail-back-to-the-primary-region"></a>Conmutación por recuperación a la región principal

Una vez que las máquinas virtuales vuelven a estar protegidas, puede conmutar por recuperación a la región primaria, si es preciso.

1. En el almacén > **Elementos replicados**, seleccione la máquina virtual.

2. Antes de ejecutar una conmutación por error, compruebe en la página de información general de la máquina virtual que la máquina virtual es la correcta y que la sincronización ha finalizado. La máquina virtual debe estar en un estado *protegido*.

    ![Página de información general de la máquina virtual en la que se muestra una máquina virtual en un estado protegido](./media/azure-to-azure-tutorial-failback/protected-state.png)

3. En la página de información general, seleccione **Conmutación por error**. Como esta vez no vamos a realizar una conmutación por error de prueba, se nos solicita que realicemos la comprobación.

    [Página en que se muestra que aceptamos realizar una conmutación por error sin una conmutación por error de prueba](./media/azure-to-azure-tutorial-failback/no-test.png)

4. En **Conmutación por error**, anote la dirección de la secundaria a la primaria y seleccione un punto de recuperación. Para crear la máquina virtual de Azure en el destino (región primaria) se usan datos de este punto.
   - **Procesado más recientemente**: usa el último punto de recuperación procesado por Site Recovery. Se muestra la marca de tiempo. No se emplea tiempo en el procesamiento de datos, por lo que se proporciona un objetivo de tiempo de recuperación (RTO) bajo.
   -  **Más reciente**: procesa todos los datos enviados a Site Recovery para crear un punto de recuperación para cada máquina virtual antes de conmutarla por error a dicho punto de recuperación. Proporciona el menor objetivo de punto de recuperación (RPO), ya que todos los datos se replican a Site Recovery cuando se desencadena la conmutación por error.
   - **Más reciente coherente con la aplicación**: esta opción conmuta por error las VM en el punto de recuperación más reciente coherente con la aplicación. Se muestra la marca de tiempo.
   - **Personalizado**: conmuta por error a un punto de recuperación concreto. La personalización solo está disponible cuando se conmuta por error una sola máquina virtual y no se usa un plan de recuperación.

    > [!NOTE]
    > Si conmuta por error una máquina virtual a la que agregó un disco después de habilitar la replicación para la máquina virtual, los puntos de la replicación mostrarán los discos disponibles para la recuperación. Por ejemplo, un punto de replicación que se creó antes de que agregara un segundo disco se mostrará como "1 de 2 discos".

4. Seleccione **Shut down machine before beginning failover** (Apagar la máquina antes de comenzar la conmutación por error) si desea que Site Recovery intente apagar las máquinas virtuales de origen antes de iniciar la conmutación por error. La operación de apagado ayuda a garantizar que no se pierden datos. La conmutación por error continúa aunque se produzca un error de cierre. 

    ![Página de configuración de la conmutación por error](./media/azure-to-azure-tutorial-failback/failover.png)    

3. Seleccione **Aceptar** para iniciar la conmutación por error.
4. Supervise la conmutación por error en las notificaciones.

    ![Notificación del progreso de la conmutación por error](./media/azure-to-azure-tutorial-failback/notification-progress.png)  
    ![Notificación de conmutación por error correcta](./media/azure-to-azure-tutorial-failback/notification-success.png)   

## <a name="reprotect-vms"></a>Reprotección de máquinas virtuales

Después de conmutar por recuperación las máquinas virtuales a la región primaria, es necesario volver a protegerlas, con el fin de que empiecen a replicarse de nuevo en la región secundaria.

1. En la página **Información general** de la máquina virtual, seleccione **Volver a proteger**.

    ![Botón para volver a proteger desde la región primaria](./media/azure-to-azure-tutorial-failback/reprotect.png)  

2. Examine la configuración del destino de la región primaria. Los recursos marcados como nuevos los crea Site Recovery como parte de la operación de reprotección.
3. Seleccione **Aceptar** para iniciar el proceso de reprotección. El proceso envía los datos iniciales a la ubicación de destino y, después, replica la información delta de las máquinas virtuales en el destino.

     ![Página que muestra la configuración de la replicación](./media/azure-to-azure-tutorial-failback/replication-settings.png) 

4. Supervisión del progreso de la reprotección en las notificaciones. 

    ![Notificación del progreso de la reprotección ](./media/azure-to-azure-tutorial-failback/notification-reprotect-start.png) [Notificación del progreso de la reprotección ](./media/azure-to-azure-tutorial-failback/notification-reprotect-finish.png)
    
  

## <a name="clean-up-resources"></a>Limpieza de recursos

En las máquinas virtuales con discos administrados, una vez que la conmutación por recuperación finaliza y que las máquinas virtuales se reprotegen para la replicación desde la región primaria a la secundaria, Site Recovery limpia automáticamente las máquinas de la región secundaria de recuperación ante desastres. No es preciso eliminar manualmente las máquinas virtuales ni las NIC de la región secundaria. Las máquinas virtuales con discos no administrados no se limpian.

Si se deshabilita completamente la replicación después de la conmutación por recuperación, Site Recovery limpia las máquinas que protege. En este caso, también limpia los discos de las máquinas virtuales que no usan discos administrados. 
 
## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha realizado la conmutación por recuperación de máquinas virtuales desde la región secundaria a la primaria. Este es el último paso en el proceso que incluye la habilitación de la replicación de una máquina virtual, el intento de realizar un simulacro de una recuperación ante desastres, la conmutación por error desde la región primaria a la secundaria y, por último, la conmutación por recuperación.

> [!div class="nextstepaction"]
> Ahora, pruebe a realizar una recuperación ante desastres de una [máquina virtual local](vmware-azure-tutorial-prepare-on-premises.md) a Azure.

