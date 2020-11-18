---
title: 'Tutorial: Configuración de la recuperación ante desastres para máquinas virtuales Linux con Azure Site Recovery'
description: Aprenda a configurar mediante el servicio Azure Site Recovery la recuperación ante desastres para máquinas virtuales Linux que se encuentren en otra región de Azure.
author: rayne-wiselman
ms.service: virtual-machines-linux
ms.subservice: recovery
ms.topic: tutorial
ms.date: 11/05/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: d14d276c798e40d417a8038aee5b7550e84f4114
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93379976"
---
# <a name="tutorial-set-up-disaster-recovery-for-linux-virtual-machines"></a>Tutorial: Configuración de la recuperación ante desastres para máquinas virtuales Linux


En este tutorial se muestra cómo configurar la recuperación ante desastres para las máquinas virtuales de Azure que utilizan Linux. En este artículo, aprenderá a:

> [!div class="checklist"]
> * Habilitar la recuperación ante desastres para máquinas virtuales Linux
> * Ejecutar una exploración en profundidad de la recuperación ante desastres
> * Detener la replicación de la máquina virtual después del simulacro

Cuando se habilita la replicación de una máquina virtual, el servicio Mobility de Site Recovery se instala en la máquina virtual y lo registra en [Azure Site Recovery](../../site-recovery/site-recovery-overview.md). Durante la replicación, las escrituras en disco de la máquina virtual VM se envían a una cuenta de almacenamiento en la caché de la región de origen. Desde ahí, los datos se envían a la región de destino y los puntos de recuperación se generan a partir de los datos.  Cuando se realiza la conmutación por error de una máquina virtual a otra región durante la recuperación ante desastres, se usa un punto de recuperación para restaurar la máquina virtual en la región de destino.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

1. Compruebe que la suscripción de Azure permite crear una máquina virtual en la región de destino. Si acaba de crear una cuenta gratuita de Azure, es el administrador de la suscripción y tiene los permisos que necesita.
2. Si no es el administrador de la suscripción, solicite al administrador que le asigne:
    - El rol integrado Colaborador de la máquina virtual o permisos específicos para:
        - Crear una máquina virtual en la red virtual seleccionada.
        - Escribir en una cuenta de Azure Storage.
        - Escribir en un disco administrado de Azure.
     - El rol integrado Colaborador de la máquina virtual para administrar las operaciones de Site Recovery en el almacén. 
3. Compruebe que la máquina virtual Linux utiliza un [sistema operativo compatible](../../site-recovery/azure-to-azure-support-matrix.md#linux).
4. Si las conexiones salientes de la máquina virtual usan un proxy basado en URL, asegúrese de que puede acceder a estas direcciones URL. No se admite el uso de un proxy autenticado.

    **Nombre** | **Nube pública** | **Nube del gobierno** | **Detalles**
    --- | --- | --- | ---
    Storage | `*.blob.core.windows.net` | `*.blob.core.usgovcloudapi.net`| Escriba datos desde la máquina virtual a la cuenta de almacenamiento de caché en la región de origen. 
    Azure AD  | `login.microsoftonline.com` | `login.microsoftonline.us`| Realice la autorización y autenticación de las direcciones URL del servicio Site Recovery. 
    Replicación | `*.hypervrecoverymanager.windowsazure.com` | `*.hypervrecoverymanager.windowsazure.com`  |Comunicación de la máquina virtual con el servicio Site Recovery. 
    Service Bus | `*.servicebus.windows.net` | `*.servicebus.usgovcloudapi.net` | La máquina virtual escribe en Site Recovery para los datos de supervisión y diagnóstico. 

4. Si usa grupos de seguridad de red para limitar el tráfico de red en las máquinas virtuales, cree reglas de NSG que permitan la conectividad saliente (HTTPS 443) para la máquina virtual mediante estas etiquetas de servicio (grupos de direcciones IP). Lo primero que debe hacer es probar las reglas en un grupo de seguridad de red de prueba.

    **Tag** | **Permitir** 
    --- | --- 
    Etiqueta Storage | Permite que los datos se puedan escribir desde la máquina virtual a la cuenta de almacenamiento de caché.
    Etiqueta Azure AD | Permite el acceso a todas las direcciones IP que correspondan a Azure AD.
    Etiqueta EventsHub | Permite el acceso a la supervisión de Site Recovery.
    Etiqueta AzureSiteRecovery | Permite el acceso al servicio Site Recovery en cualquier región.
    GuestAndHybridManagement | Úselo si desea actualizar automáticamente el agente de movilidad de Site Recovery que se ejecuta en las máquinas virtuales habilitadas para la replicación.
5. Asegúrese de que las máquinas virtuales tienen los certificados raíz más recientes. En las máquinas virtuales Linux, siga las instrucciones proporcionadas por su distribuidor de Linux para obtener los últimos certificados raíz de confianza y la lista de revocación de certificados en la máquina virtual.

## <a name="enable-disaster-recovery"></a>Habilitación de la recuperación ante desastres

1. En Azure Portal, abra la página de propiedades de la máquina virtual.
2. En **Operaciones**, seleccione **Recuperación ante desastres**.
3. En **Aspectos básicos** > **Región de destino**, seleccione la región a la que desea migrar la máquina virtual. Tanto la región de origen como la de destino deben estar en el mismo inquilino de Azure Active Directory.
4. Haga clic en **Revisar e iniciar replicación**.

    :::image type="content" source="./media/tutorial-disaster-recovery/disaster-recovery.png" alt-text="Habilite la replicación en la página Recuperación ante desastres de las propiedades de la máquina virtual.":::

5. En **Revisar e iniciar replicación**, compruebe la configuración:

    - **Configuración de destino**. De forma predeterminada, Site Recovery crea un espejo de la configuración de origen para crear recursos de destino.
    - **Configuración de almacenamiento: cuenta de almacenamiento de caché**. Recovery utiliza una cuenta de almacenamiento en la región de origen. Los cambios en la máquina virtual de origen se almacenan en la caché en esta cuenta antes de replicarse en la ubicación de destino.
    - **Configuración de almacenamiento: disco de réplica**. De forma predeterminada, Site Recovery crea en la región de destino discos administrados de réplica que reflejan los discos administrados de la máquina virtual de origen con el mismo tipo de almacenamiento (Estándar o Premium).
    - **Configuración de la replicación**. Muestra los detalles del almacén e indica que los puntos de recuperación creados por Site Recovery se mantienen durante 24 horas.
    - **Configuración de la extensión**. Indica que Site Recovery administrará las actualizaciones de la extensión del servicio Mobility de Site Recovery que se instala en las máquinas virtuales que se repliquen. La cuenta de Azure Automation indicada administra el proceso de actualización.

    :::image type="content" source="./media/tutorial-disaster-recovery/settings-summary.png" alt-text="Página que muestra un resumen de la configuración de la replicación y del destino.":::

2. Seleccione **Iniciar replicación**. La implementación comienza y Site Recovery empieza a crear recursos de destino. El progreso de la replicación se puede supervisar en las notificaciones.

    :::image type="content" source="./media/tutorial-disaster-recovery/notifications.png" alt-text="Notificación del progreso de la replicación.":::

## <a name="check-vm-status"></a>Comprobación del estado de la máquina virtual

Cuando el trabajo de replicación finalice, puede comprobar el estado de replicación de la máquina virtual.

1. Abra la página de propiedades de la máquina virtual.
2. En **Operaciones**, seleccione **Recuperación ante desastres**.
3. Expanda la sección **Essentials** para revisar los valores predeterminados del almacén, la directiva de replicación y la configuración de destino.
4. En **Mantenimiento y estado**, obtenga información sobre el estado de replicación de la máquina virtual, la versión del agente, la preparación de la conmutación por error y los puntos de recuperación más recientes. 

    :::image type="content" source="./media/tutorial-disaster-recovery/essentials.png" alt-text="Vista de Essentials para la recuperación ante desastres de máquinas virtuales.":::

5. En **Vista de la infraestructura**, obtenga una visión general de las máquinas virtuales de origen y de destino, de los discos administrados y de la cuenta de almacenamiento en la caché.

    :::image type="content" source="./media/tutorial-disaster-recovery/infrastructure.png" alt-text="Mapa visual de la infraestructura para la recuperación ante desastres de máquinas virtuales.":::


## <a name="run-a-drill"></a>Ejecución de un simulacro

Realice un simulacro para asegurarse de que la recuperación ante desastres funciona según lo previsto. Cuando se realiza una conmutación por error de prueba, se crea una copia de la máquina virtual sin que ello afecte a la replicación en curso ni al entorno de producción.

1. En la página de recuperación ante desastres de la máquina virtual, seleccione **Conmutación por error de prueba**.
2. En **Conmutación por error de prueba**, deje el valor predeterminado de **Latest processed (low RPO)** [Procesado recientemente (RPO bajo)] del punto de recuperación.

   Esta opción proporciona el objetivo de punto de recuperación (RPO) más bajo y, habitualmente, pone en marcha la máquina virtual más rápidamente en la región de destino. Procesa primero todos los datos que se han enviado al servicio Site Recovery para crear un punto de recuperación para cada máquina virtual antes de conmutarla por error a dicho punto de recuperación. Este punto de recuperación tiene todos los datos replicados en Site Recovery cuando se desencadenó la conmutación por error.

3. Seleccione la red virtual en la que se encontrará la máquina virtual después de la conmutación por error. 

     :::image type="content" source="./media/tutorial-disaster-recovery/test-failover-settings.png" alt-text="Página en la que se establecen las opciones de la conmutación por error de prueba.":::

4. Comienza el proceso de conmutación por error de prueba. El progreso se puede supervisar en las notificaciones.

    :::image type="content" source="./media/tutorial-disaster-recovery/test-failover-notification.png" alt-text="Notificaciones de la conmutación por error de prueba."::: 
    
   Una vez que se complete la conmutación por error de prueba, la máquina virtual se encuentra en el estado *Limpieza de conmutación por error de prueba pendiente* en la página **Essentials**. 
  
## <a name="clean-up-resources"></a>Limpieza de recursos

Site Recovery limpia automáticamente la máquina virtual después del simulacro. 
 
1. Para iniciar la limpieza automática, seleccione **Limpiar conmutación por error de prueba**.

    :::image type="content" source="./media/tutorial-disaster-recovery/start-cleanup.png" alt-text="Iniciar la limpieza en la página Essentials."::: 

6. En **Limpieza de la conmutación por error de prueba**, escriba las notas que desee grabar para la conmutación por error y, después, seleccione **Testing is complete. Delete test failover virtual machine** (Se ha completado la prueba. Elimine las máquinas virtuales de la conmutación por error de prueba). Después, seleccione **Aceptar**.

    :::image type="content" source="./media/tutorial-disaster-recovery/delete-test.png" alt-text="Página para registrar las notas y eliminar la máquina virtual de prueba."::: 

7. Comienza el proceso de eliminación. El progreso se puede supervisar en las notificaciones.

    :::image type="content" source="./media/tutorial-disaster-recovery/delete-test-notification.png" alt-text="Notificaciones para supervisar la eliminación de una máquina virtual de prueba."::: 


### <a name="stop-replicating-the-vm"></a>Detención de la replicación de la máquina virtual

Después de completar un simulacro de la recuperación ante desastres, se recomienda seguir intentando realizar una conmutación por error completa. Si no desea realizar una conmutación por error completa, puede deshabilitar la replicación. Esta operación realiza las siguientes acciones:

- Quita la máquina virtual de la lista de máquinas replicadas de Site Recovery.
- Detiene la facturación que realiza Site Recovery de la máquina virtual.
- Limpia automáticamente la configuración de replicación de origen.

Detenga la replicación como se indica a continuación:

1. En la página de recuperación ante desastres de la máquina virtual, seleccione **Deshabilitar replicación**.
2. En **Deshabilitar replicación**, seleccione las razones por las que desea deshabilitar la replicación. Después, seleccione **Aceptar**.

    :::image type="content" source="./media/tutorial-disaster-recovery/disable-replication.png" alt-text="Página para deshabilitar la replicación y proporcionar un motivo."::: 


La extensión de Site Recovery instalada en la máquina virtual durante la replicación no se elimina de forma automática. Si deshabilita la replicación de la máquina virtual y no desea volver a replicarla más tarde, puede quitar la extensión de Site Recovery manualmente, como se indica a continuación: 

1. Vaya a la máquina virtual > **Configuración** > **Extensiones**.
2. En la página **Extensiones**, seleccione todas las entradas de *Microsoft.Azure.RecoveryServices* para Linux.
3. En la página de propiedades de la extensión, seleccione **Desinstalar**.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha configurado la recuperación ante desastres de una máquina virtual de Azure y se ha ejecutado un simulacro de la recuperación ante desastres. Ahora, puede realizar una conmutación por error completa de la máquina virtual.

> [!div class="nextstepaction"]
> [Conmutación por error de una máquina virtual en otra región](../../site-recovery/azure-to-azure-tutorial-dr-drill.md)
