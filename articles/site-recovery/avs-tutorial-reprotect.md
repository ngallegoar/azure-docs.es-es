---
title: Reprotección de máquinas virtuales de Azure en una nube privada de Azure VMware Solution con Azure Site Recovery
description: Aprenda a reproteger máquinas virtuales de Azure VMware Solution después de una conmutación por error a Azure con Azure Site Recovery.
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: 80ff2f3f3d5fdcf61770889dcdaaf075941b90ff
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91814295"
---
# <a name="reprotect-from-azure-to-azure-vmware-solution-private-cloud"></a>Reprotección de Azure a una nube privada de Azure VMware Solution

Tras la [conmutación por error](avs-tutorial-failover.md) de máquinas virtuales de Azure VMware Solution a Azure, el primer paso al realizar la conmutación por recuperación a una nube privada de Azure VMware Solution es reproteger las máquinas virtuales de Azure que se crearon durante la conmutación por error. En este artículo se describe cómo hacerlo. 

## <a name="before-you-begin"></a>Antes de empezar

1. Siga los pasos que se indican en [este artículo](vmware-azure-prepare-failback.md) para prepararse para la reprotección y la conmutación por recuperación, lo que incluye la configuración de un servidor de procesos en Azure y un servidor de destino maestro en la nube privada de Azure VMware Solution, así como la configuración de una VPN de sitio a sitio, o del emparejamiento privado de ExpressRoute para la conmutación por recuperación.
2. Asegúrese de que el servidor de configuración de la nube privada de Azure VMware Solution se está ejecutando y está conectado a Azure. Durante la conmutación por recuperación, la VM debe encontrarse en la base de datos del servidor de configuración. En caso contrario, la conmutación por recuperación no será correcta.
3. Elimine todas las instantáneas del servidor de destino maestro de la nube privada de Azure VMware Solution. La reprotección no funcionará si existe alguna instantánea.  Las instantáneas de la VM se combinan automáticamente durante los trabajos de reprotección.
4. Si va a reproteger máquinas virtuales agrupadas en un grupo de replicación para asegurar la coherencia de varias máquinas virtuales, asegúrese de que todas tienen el mismo sistema operativo (Windows o Linux) y de que el servidor de destino maestro que va a implementar tiene el mismo tipo de sistema operativo. Todas las máquinas virtuales de un grupo de replicación deben usar el mismo servidor de destino maestro.
5. Abra [los puertos necesarios](vmware-azure-prepare-failback.md#ports-for-reprotectionfailback) para realizar la conmutación por recuperación.
6. Asegúrese de que vCenter Server esté conectado antes de realizar la conmutación por recuperación. En caso contrario, se producirá un error al desconectar los discos y conectarlos a la máquina virtual.
7. Si una instancia de vCenter Server administra las máquinas virtuales a las que quiere conmutar por recuperación, asegúrese de tener los permisos necesarios. Si realiza la detección de usuarios de solo lectura de vCenter y protege las máquinas virtuales, la protección se ejecutará correctamente y la conmutación por error funcionará. Sin embargo, durante la reprotección, se produce un error de conmutación por error porque no se pueden detectar los almacenes de datos y no aparecen durante la reprotección. Para solucionar este problema, puede actualizar las credenciales de vCenter con la [cuenta o los permisos adecuados](avs-tutorial-prepare-avs.md#prepare-an-account-for-automatic-discovery) y volver a intentar el trabajo. 
8. Si ha usado una plantilla para crear las máquinas virtuales, asegúrese de que cada una tenga su propio UUID para los discos. Si el UUID de la máquina virtual de Azure VMware Solution entra en conflicto con el UUID del servidor de destino maestro porque ambos se crearon a partir de la misma plantilla, se produce un error de reprotección. En este caso, implemente desde una plantilla diferente.
9. Si va a conmutar por recuperación en una instancia de vCenter Server alternativa, asegúrese de que se detectan tanto el nuevo vCenter Server como el servidor de destino maestro. Normalmente, si no se detectaron, significa que los almacenes de datos no son accesibles o visibles en **Reproteger**.
10. Compruebe los siguientes escenarios en los que no se puede realizar la conmutación por recuperación:
    - Si utiliza las ediciones gratuitas de ESXi 5.5 o vSphere Hypervisor 6, actualice a una versión diferente.
    - Si tiene un servidor físico de Windows Server 2008 R2 SP1.
    - Las máquinas virtuales de VMware no pueden conmutar por recuperación en Hyper-V.
    - VM que se han migrado.
    - Un a máquina virtual que se ha migrado a otro grupo de recursos.
    - Una máquina virtual de Azure de réplica que se ha eliminado.
    - Una máquina virtual de Azure de réplica que no está protegida.
10. [Revise los tipos de conmutación por recuperación](concepts-types-of-failback.md) que puede usar: recuperación de ubicación original y recuperación de ubicación alternativa.


## <a name="enable-reprotection"></a>Habilitación de la reprotección

Habilite la replicación. Puede reproteger máquinas virtuales específicas o un plan de recuperación:

- Si reprotege un plan de recuperación, tiene que proporcionar los valores para cada máquina protegida.
- Si las máquinas virtuales pertenecen a un grupo de replicación para la coherencia entre varias máquinas virtuales, solo se pueden reproteger mediante un plan de recuperación. Las máquinas virtuales de un grupo de replicación deben usar el mismo servidor de destino maestro.

>[!NOTE]
>La cantidad de datos enviados desde Azure al origen anterior durante la reprotección puede oscilar entre 0 bytes y la suma del tamaño del disco de todas las máquinas protegidas, y no se puede calcular.

### <a name="before-you-start"></a>Antes de comenzar

- Después del arranque de una máquina virtual en Azure tras una conmutación por error, el agente tarda algún tiempo en registrarse de nuevo en el servidor de configuración (hasta 15 minutos). Durante este tiempo, no podrá realizar la reprotección y aparece un mensaje de error para indicar que el agente no está instalado. Si esto sucede, espere unos minutos y realice al reprotección.
- Si desea realizar la conmutación por recuperación de la máquina virtual de Azure en una máquina virtual de Azure VMware Solution existente, monte los almacenes de datos de la máquina virtual con acceso de lectura/escritura en el host ESXi del servidor de destino maestro.
- Si desea realizar la conmutación por recuperación en una ubicación alternativa (por ejemplo, si la máquina virtual de Azure VMware Solution no existe), seleccione la unidad de retención y el almacén de datos configurados para el servidor de destino maestro. Cuando se realiza la conmutación por recuperación en la nube privada de Azure VMware Solution, las máquinas virtuales del plan de protección de conmutación por recuperación usan el mismo almacén de datos que el servidor de destino maestro. Entonces se crea una nueva VM en vCenter.

Para habilitar la reprotección, siga estos pasos:

1. Seleccione **Almacén** > **Elementos replicados**. Haga clic con el botón derecho en la máquina virtual que ha sido objeto de la conmutación por error y seleccione **Reproteger**. O bien, con los botones de comando, seleccione la máquina y luego **Reproteger**.
2. Verifique que la dirección de protección se establece en **De Azure a local**.
3. En **Servidor de destino maestro** y **Servidor de procesos**, seleccione el servidor de destino maestro local y el servidor de procesos.  
4. En **Almacén de datos**, seleccione aquel en el que desea recuperar los discos en Azure VMware Solution. Esta opción se usa cuando se elimina la máquina virtual de Azure VMware Solution y es necesario crear discos. Esta opción se omite si los discos ya existen. Todavía debe especificar un valor.
5. Elija la unidad de retención.
6. La directiva de conmutación por recuperación se selecciona automáticamente.
7. Seleccione **Aceptar** para comenzar con la reprotección.

    ![Cuadro de diálogo Reprotección](./media/vmware-azure-reprotect/reprotectinputs.png)
    
8. Un trabajo empieza a replicar la máquina virtual de Azure en la nube privada de Azure VMware Solution. Puede realizar el seguimiento del progreso en la pestaña **Trabajos**.
    - Cuando la reprotección se completa satisfactoriamente, la máquina virtual adopta un estado protegido.
    - La máquina virtual de Azure VMware Solution está desactivada durante la reprotección. Esto ayuda a garantizar la coherencia de datos durante la replicación.
    - No la encienda cuando finalice la reprotección.
   

## <a name="next-steps"></a>Pasos siguientes

- Si tiene algún problema, revise el [artículo de solución de problemas](vmware-azure-troubleshoot-failback-reprotect.md).
- Una vez protegidas las máquinas virtuales de Azure, puede [realizar una conmutación por recuperación](avs-tutorial-failback.md). La conmutación por recuperación apaga la máquina virtual de Azure e inicia la máquina virtual de Azure VMware Solution. Debe prever cierto tiempo de inactividad para la aplicación y elegir un tiempo de conmutación por recuperación en consecuencia.


