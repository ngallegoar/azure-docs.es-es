---
title: Ejecución de una conmutación por error durante la recuperación ante desastres con Azure Site Recovery
description: Conmutación por error de máquinas virtuales y servidores físicos a Azure con Azure Site Recovery.
ms.service: site-recovery
ms.topic: article
ms.date: 12/10/2019
ms.openlocfilehash: 6737f64773f91ede1631d42cd7f28c7d961c0454
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92368628"
---
# <a name="run-a-failover-from-on-premises-to-azure"></a>Ejecutar una conmutación por error del entorno local a Azure

En este artículo se describe cómo conmutar por error máquinas locales a Azure en [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Antes de comenzar

- [Obtenga información](failover-failback-overview.md) sobre el proceso de conmutación por error en la recuperación ante desastres.
- Si desea conmutar por error varias máquinas, [aprenda](recovery-plan-overview.md) a juntar todas ellas en un plan de recuperación.
- Antes de realizar una conmutación por error completa, realice una [exploración de la recuperación ante desastres](site-recovery-test-failover-to-azure.md) para asegurarse de que todo funciona según lo previsto.

## <a name="prepare-to-connect-after-failover"></a>Preparación para conectarse después de una conmutación por error

Para asegurarse de que puede conectarse a las máquinas virtuales de Azure que se crean después de la conmutación por error, estas son algunas de las cosas que debe realizar localmente antes de la conmutación por error.


### <a name="prepare-on-premises-to-connect-after-failover"></a>Preparación local para conectarse después de una conmutación por error

Si desea conectarse a las máquinas virtuales de Azure mediante RDP o SSH después de la conmutación por error, estas son algunas de las cosas que debe realizar localmente antes de la conmutación por error.

**Después de la conmutación por error** | **Ubicación** | **Acciones**
--- | --- | ---
**Máquina virtual de Azure que ejecuta Windows** | En la máquina local antes de la conmutación por error | Para acceder a la máquina virtual de Azure a través de Internet, habilite RDP, asegúrese de que se hayan agregado las reglas de TCP y UDP para **Público** , y que RDP esté permitido en **Firewall de Windows** > **Aplicaciones permitidas** , para todos los perfiles.<br/><br/> Para acceder a la máquina virtual de Azure a través de una conexión de sitio a sitio, habilite el protocolo RDP en la máquina y asegúrese de que este está permitido en **Firewall de Windows** -> **Aplicaciones y características permitidas** para redes **Dominio y Privadas** .<br/><br/> <br/><br/> Elimine todas las rutas estáticas persistentes y el proxy WinHTTP. Asegúrese de que la directiva SAN del sistema operativo está establecida en **OnlineAll** . [Más información](https://support.microsoft.com/kb/3031135).<br/><br/> Asegúrese de que no haya actualizaciones de Windows pendientes en la máquina virtual cuando se desencadena una conmutación por error. La actualización de Windows puede que comience cuando realice la conmutación por error y no podrá iniciar sesión en la máquina virtual hasta que se complete la actualización.
**Máquina virtual de Azure que ejecuta Linux** | En la máquina local antes de la conmutación por error | Asegúrese de que el servicio de Secure Shell en la máquina virtual está configurado para iniciarse automáticamente en el arranque del sistema.<br/><br/> Compruebe que las reglas de firewall permiten una conexión SSH.


## <a name="run-a-failover"></a>Ejecución de la conmutación por error

En este procedimiento se describe cómo ejecutar una conmutación por error para un [plan de recuperación](site-recovery-create-recovery-plans.md). Si desea ejecutar una conmutación por error para una sola máquina virtual, siga las instrucciones para una [máquina virtual de VMware](vmware-azure-tutorial-failover-failback.md), un [servidor físico](physical-to-azure-failover-failback.md) o una [máquina virtual de Hyper-V](hyper-v-azure-failover-failback-tutorial.md).


Ejecute la conmutación por error del plan de recuperación como sigue:

1. En el almacén de Site Recovery, seleccione **Planes de recuperación** > *recoveryplan_name* .
2. Haga clic en **Conmutación por error** .

    ![Captura de pantalla de Azure Site Recovery que muestra el panel ADRP con Conmutación por error seleccionado en el menú Más.](./media/site-recovery-failover/Failover.png)

3. En **Conmutación por error** > **Dirección de conmutación por error** , deje la opción predeterminada si va a replicar en Azure.
4. En **Conmutación por error** , seleccione un **punto de recuperación** en el que realizar la conmutación por error.

    - **Más reciente** : Utilice el punto más reciente. Esto procesa todos los datos que se han enviado al servicio Site Recovery y crea un punto de recuperación para cada máquina. Esta opción ofrece el objetivo de punto de recuperación (RPO) mínimo, ya que la máquina virtual de Azure creada después de la conmutación por error tiene todos los datos que se han replicado en Site Recovery al desencadenarse la conmutación por error.
    Tenga en cuenta que, cuando la región de origen deja de funcionar, ya no es posible el procesamiento de registros. Por lo tanto, tendrá que realizar la conmutación por error al punto de recuperación procesado más recientemente. Vea el siguiente punto para obtener más información.
   - **Procesado más recientemente** : Use esta opción para realizar una conmutación por error de las máquinas virtuales al último punto de recuperación que ya procesó Site Recovery. Puede ver el punto de recuperación procesado más reciente en **Puntos de recuperación más recientes** de la máquina virtual. Esta opción proporciona un objetivo de tiempo de recuperación bajo, ya que no se invierte tiempo en el procesamiento de datos sin procesar.
   - **Más reciente coherente con la aplicación** : Use esta opción para conmutar por error máquinas virtuales al punto de recuperación más reciente coherente con la aplicación que haya procesado Site Recovery.
   - **Último procesamiento de máquinas virtuales múltiples** :  Con esta opción, las máquinas virtuales que forman parte de un grupo de replicación conmutan por error al último punto de recuperación común coherente con varias máquinas virtuales. Otras máquinas virtuales conmutan por error a su punto de recuperación procesado más reciente. Esta opción solo está disponible para planes de recuperación que tienen al menos una máquina virtual con la coherencia para varias máquinas virtuales activada.
   - **Último coherente con aplicación de múltiples VM** : Con esta opción, las máquinas virtuales que forman parte de un grupo de replicación conmutan por error al último punto de recuperación común coherente con la aplicación de varias máquinas virtuales. Otras máquinas virtuales conmutan por error a su punto de recuperación coherente con la aplicación más reciente. Esta opción solo está disponible para planes de recuperación que tienen al menos una máquina virtual con la coherencia para varias máquinas virtuales activada.
   - **Personalizado** : No disponible para planes de recuperación. Esta opción solo está disponible para la conmutación por error de máquinas virtuales individuales.

5. Seleccione **Apague la máquina antes de comenzar con la conmutación por error** si desea que Site Recovery apague las máquinas virtuales de origen antes de iniciar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre.  

    > [!NOTE]
    > Si conmuta por error máquinas virtuales de Hyper-V, esta opción intenta sincronizar y replicar los datos locales que aún no se hayan enviado al servicio antes de desencadenar la conmutación por error. 

6. Siga el progreso de la conmutación por error en la página **Trabajos** . Aunque se produzcan errores, el plan de recuperación se ejecuta hasta que se completa.
7. Después de la conmutación por error, inicie sesión en la máquina virtual para validarla. 
8. Use **Cambiar punto de recuperación** si desea usar otro punto de recuperación para la conmutación por error.
9. Cuando esté listo, puede confirmar la conmutación por error. La acción **Confirmar** elimina todos los puntos de recuperación que hay disponibles con el servicio. La opción **Cambiar punto de recuperación** ya no está disponible.

## <a name="run-a-planned-failover-hyper-v"></a>Ejecución de una conmutación por error planeada (Hyper-V)

Puede ejecutar una conmutación por error planeada para las máquinas virtuales de Hyper-V.

- La conmutación por error planeada es una opción de conmutación por error sin pérdida de datos.
- Cuando se desencadena una conmutación por error planeada, primero se apagan las máquinas virtuales de origen, después se sincronizan los últimos datos y, por último, se desencadena la conmutación por error.
- Puede ejecutar una conmutación por error planeada mediante la opción **Conmutación por error planeada** . Se ejecuta de forma parecida a una conmutación por error regular.
 
## <a name="track-failovers"></a>Seguimiento de las conmutaciones por error

Hay una serie de trabajos asociados a la conmutación por error.

![Captura de pantalla de la página Trabajos en la que se muestra una lista de trabajos con Group 1: Start (1) expandido en la columna Name. La línea del trabajo SQLServer está resaltada.](./media/site-recovery-failover/FailoverJob.png)

- **Comprobación de requisitos previos** : Garantiza que se cumplen todas las condiciones necesarias para la conmutación por error.
- **Conmutación por error** : Sirve para procesar los datos, de modo que se pueda crear una máquina virtual de Azure a partir de ellos. Si ha elegido el punto de recuperación **más reciente** , se crea un punto de recuperación a partir de los datos enviados al servicio.
- **Iniciar** : Crea una máquina virtual de Azure mediante los datos procesados en el paso anterior.

> [!WARNING]
> **No cancele una conmutación por error en curso** : antes de iniciar la conmutación por error, se detiene la replicación de la máquina virtual. Si se cancela un trabajo en curso, la conmutación por error se detiene, pero la máquina virtual no comenzará la replicación. La replicación no se puede reiniciar.


### <a name="extra-failover-time"></a>Hora de la conmutación por error adicional

En algunos casos, la conmutación por error de la máquina virtual requiere un paso intermedio que normalmente tarda alrededor de 8 a 10 minutos en completarse. Este paso y tiempo adicional afectará a estas máquinas:

* Máquinas virtuales de VMware que ejecutan una versión del servicio Mobility anterior a la 9.8.
* Servidores físicos y máquinas virtuales de Hyper-V protegidas como servidores físicos.
* Máquinas virtuales de VMware Linux
* Máquinas virtuales de VMware en las que estos controladores no están presentes como controladores de arranque:
    * storvsc
    * vmbus
    * storflt
    * intelide
    * atapi
* Máquinas virtuales de VMware que no tienen DHCP habilitado, independientemente de si usa DHCP o direcciones IP estáticas.


## <a name="automate-actions-during-failover"></a>Automatización de acciones durante la conmutación por error

Es posible que desee automatizar acciones durante la conmutación por error. Para ello, puede utilizar scripts o runbooks de Azure Automation en planes de recuperación.

- [Aprenda](site-recovery-create-recovery-plans.md) a crear y personalizar planes de recuperación, lo que incluye la incorporación de scripts.
- [Aprenda](site-recovery-runbook-automation.md) a agregar runbooks de Azure Automation a los planes de recuperación.


## <a name="configure-settings-after-failover"></a>Configuración tras la conmutación por error

### <a name="retain-drive-letters-after-failover"></a>Conservación de las letras de unidad después de la conmutación por error

Site Recovery controla la conservación de las letras de unidad. Si va a excluir discos durante la replicación de la máquina virtual, [revise un ejemplo](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk) de cómo funciona esto.

### <a name="prepare-in-azure-to-connect-after-failover"></a>Preparación en Azure para conectarse después de una conmutación por error

Si desea conectarse a máquinas virtuales de Azure que se crearon después de la conmutación por error mediante RDP o SSH, siga los requisitos resumidos en la tabla.

**Conmutación por error** | **Ubicación** | **Acciones**
--- | --- | ---
**Máquina virtual de Azure que ejecuta Windows** | Máquina virtual de Azure después de la conmutación por error |  [Agregue una dirección IP pública](/archive/blogs/srinathv/how-to-add-a-public-ip-address-to-azure-vm-for-vm-failed-over-using-asr) para la máquina virtual.<br/><br/> Las reglas del grupo de seguridad de red de la máquina virtual conmutada por error (y la subred de Azure a la que esta se conecta) deben permitir las conexiones entrantes al puerto RDP.<br/><br/> Seleccione **Diagnósticos de arranque** para comprobar una captura de pantalla de la máquina virtual.<br/><br/> Si no puede conectarse, compruebe que se está ejecutando la máquina virtual y revise estas [sugerencias de solución de problemas](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).
**Máquina virtual de Azure que ejecuta Linux** | Máquina virtual de Azure después de la conmutación por error | Las reglas del grupo de seguridad de red de la máquina virtual conmutada por error (y la subred de Azure a la que esta se conecta) necesitan permitir las conexiones entrantes al puerto SSH.<br/><br/> [Agregue una dirección IP pública](/archive/blogs/srinathv/how-to-add-a-public-ip-address-to-azure-vm-for-vm-failed-over-using-asr) para la máquina virtual.<br/><br/> Seleccione **Diagnósticos de arranque** para obtener una captura de pantalla de la máquina virtual.<br/><br/>

Siga los pasos descritos [aquí](site-recovery-failover-to-azure-troubleshoot.md) para solucionar problemas de conectividad tras la conmutación por error.

## <a name="set-up-ip-addressing"></a>Configuración del direccionamiento de IP

- **Direcciones IP internas** : Para establecer la dirección IP interna de una máquina virtual de Azure después de la conmutación por error, tiene un par de opciones:
    - Conservar la misma dirección IP: puede usar la misma dirección IP en la máquina virtual de Azure que la asignada a la máquina local.
    - Usar una dirección IP distinta: puede usar una dirección IP distinta para la máquina virtual de Azure.
    - [Más información](concepts-on-premises-to-azure-networking.md#assign-an-internal-address) sobre la configuración de las direcciones IP internas.
- **Direcciones IP externas** : Puede conservar las direcciones IP públicas en la conmutación por error. A las máquinas virtuales de Azure que se crearon como parte del proceso de conmutación por error se les debe asignar una dirección IP pública de Azure que esté disponible en la región de Azure. Puede asignar una dirección IP pública manualmente o mediante la automatización del proceso con un plan de recuperación. [Más información](concepts-public-ip-address-with-site-recovery.md).


## <a name="next-steps"></a>Pasos siguientes

Una vez que haya realizado la conmutación por error, deberá volver a efectuar la protección para iniciar la replicación de las máquinas virtuales de Azure de nuevo en el sitio local. Una vez que la replicación ya está en funcionamiento, puede conmutar por recuperación en el entorno local cuando esté listo.

- [Obtenga más información](failover-failback-overview.md#reprotectionfailback) sobre los procesos de reprotección y conmutación por recuperación.
- [Preparación para](vmware-azure-reprotect.md) la reprotección y la conmutación por recuperación de VMware.
- [Conmutar por recuperación](hyper-v-azure-failback.md) máquinas virtuales de Hyper-V.
- [Más información](physical-to-azure-failover-failback.md) sobre los procesos de conmutación por error y conmutación por recuperación de los servidores físicos.