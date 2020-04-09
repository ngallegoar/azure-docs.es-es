---
title: Solucionar problemas del agente y la extensión
description: Síntomas, causas y soluciones de errores de Azure Backup relacionados con el agente, la extensión y los discos.
ms.reviewer: saurse
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.service: backup
ms.openlocfilehash: 4583c02b52ab6b3a4e5056a47db096d4e34399ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226100"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Solución de problemas de Azure Backup: Problemas con el agente o la extensión

En este artículo se proporcionan los pasos de solución de problemas que pueden ayudar a resolver los errores de Azure Backup relacionados con la comunicación con el agente y la extensión de máquina virtual.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="usererrorguestagentstatusunavailable---vm-agent-unable-to-communicate-with-azure-backup"></a><a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable: El agente de máquina virtual no se puede comunicar con Azure Backup

**Código de error**: UserErrorGuestAgentStatusUnavailable <br>
**Mensaje de error**: El agente de máquina virtual no se puede comunicar con Azure Backup<br>

El agente de máquina virtual de Azure podría estar detenido, obsoleto, en un estado incoherente o no instalado. Estos estados impiden que el servicio Azure Backup desencadene instantáneas.

- **Abra Azure Portal > VM > Configuración > panel Propiedades** > asegúrese de que el valor de **Estado** sea **En ejecución** y que **Estado del agente** sea **Listo**. Si el agente de máquina virtual está detenido o se encuentra en un estado incoherente, reinicie el agente.<br>
  - Para máquinas virtuales Windows, siga estos [pasos](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms) para reiniciar el agente invitado.<br>
  - En el caso de las máquinas virtuales Linux, siga estos [pasos](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms) para reiniciar el agente invitado.
- Abra **Azure Portal > Máquina virtual > Configuración > Extensiones** y asegúrese de que el estado de todas las extensiones sea **Aprovisionamiento realizado correctamente**. Si no se resuelve la incidencia, siga estos [pasos](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#usererrorvmprovisioningstatefailed---the-vm-is-in-failed-provisioning-state).

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>GuestAgentSnapshotTaskStatusError: No se pudo comunicar con el agente de máquina virtual para el estado de la instantánea

**Código de error**: GuestAgentSnapshotTaskStatusError<br>
**Mensaje de error**: No se pudo comunicar con el agente de máquina virtual para ver el estado de la instantánea <br>

Después de registrar y programar una máquina virtual para el servicio de Azure Backup, Backup inicia el trabajo al comunicarse con la extensión de copia de seguridad de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Si la instantánea no se desencadena, se podría producir un error en la copia de seguridad. Realice los pasos de solución de problemas siguientes en el orden indicado y, a continuación, vuelva a intentar la operación:  

**Causa 1: [El agente está instalado en la máquina virtual, pero no responde (en máquinas virtuales Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  

**Causa 2: [el agente instalado en la máquina virtual está obsoleto (en el caso de máquinas virtuales Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

**Causa 3: [no se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**

**Causa 4: [No se han establecido las opciones de configuración del agente de máquina virtual (para máquinas virtuales Linux)](#vm-agent-configuration-options-are-not-set-for-linux-vms)**

## <a name="usererrorvmprovisioningstatefailed---the-vm-is-in-failed-provisioning-state"></a>UserErrorVmProvisioningStateFailed: la máquina virtual está en un estado de aprovisionamiento con errores

**Código de error**: UserErrorVmProvisioningStateFailed<br>
**Mensaje de error**: la máquina virtual está en un estado de aprovisionamiento con errores<br>

Este error se produce cuando uno de los errores de extensión deja a la máquina virtual en un estado de aprovisionamiento con errores.<br>Abra **Azure Portal > VM > Configuración > Extensiones > Estado de las extensiones** y compruebe que el estado de todas las extensiones es **Aprovisionamiento realizado correctamente**. Para más información, consulte [Estados de aprovisionamiento](https://docs.microsoft.com/azure/virtual-machines/windows/states-lifecycle#provisioning-states).

- Si la extensión VMSnapshot está en un estado con errores, haga clic con el botón derecho en la extensión con errores y elimínela. Desencadene una copia de seguridad a petición. Esta acción volverá a instalar las extensiones y ejecutará el trabajo de copia de seguridad.  <br>
- Si cualquier otra extensión está en un estado con errores, puede interferir con la copia de seguridad. Asegúrese que se resuelven esos problemas de extensiones y vuelva a intentar la operación de copia de seguridad.
- Si el estado de aprovisionamiento de la máquina virtual es un estado de actualización, puede interferir con la copia de seguridad. Asegúrese de que es correcto y vuelva a intentar la operación de copia de seguridad.

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached: Se ha alcanzado el límite máximo de colecciones del punto de restauración

**Código de error**: UserErrorRpCollectionLimitReached <br>
**Mensaje de error**: se ha alcanzado el límite máximo de colecciones del punto de restauración. <br>

- Este problema puede ocurrir si hay un bloqueo en el grupo de recursos del punto de recuperación que impida la limpieza automática del punto de recuperación.
- Este problema también puede ocurrir si se desencadenan varias copias de seguridad al día. Actualmente recomendamos solo una copia de seguridad por día, ya que los puntos de restauración instantáneos se retienen durante 1 a 5 días según la retención de instantáneas configurada y solo se pueden asociar 18 puntos de restauración instantáneos con una máquina virtual en cualquier momento dado. <br>
- El número de puntos de restauración entre las colecciones de puntos de restauración y los grupos de recursos de una máquina virtual no puede superar los 18. Para crear un nuevo punto de restauración, elimine los puntos de restauración existentes.

Acción recomendada:<br>
Para resolver este problema, elimine el bloqueo en el grupo de recursos de la máquina virtual y vuelva a intentar la operación para desencadenar la limpieza.
> [!NOTE]
> El servicio Backup crea un grupo de recursos diferente al de la máquina virtual para guardar la colección de puntos de restauración. Es conveniente que los clientes no bloqueen el grupo de recursos que se ha creado para que lo utilice el servicio Backup. El formato de nombres del grupo de recursos creado por el servicio Backup es: AzureBackupRG_`<Geo>`_`<number>`, por ejemplo: AzureBackupRG_northeurope_1

**Paso 1: [Eliminación del bloqueo del grupo de recursos de punto de restauración](#remove_lock_from_the_recovery_point_resource_group)** <br>
**Paso 2: [Eliminación de la colección de puntos de restauración](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured: Backup no tiene suficientes permisos en el almacén de claves para realizar copias de seguridad de las máquinas virtuales cifradas

**Código de error**: UserErrorKeyvaultPermissionsNotConfigured <br>
**Mensaje de error**: Backup no tiene suficientes permisos para el almacén de claves y no se puede realizar la copia de seguridad de las máquinas virtuales cifradas. <br>

Para que una operación de copia de seguridad se complete correctamente en las VM cifradas, debe tener permisos para acceder al almacén de claves. Los permisos se pueden establecer a través de [Azure Portal](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption) o a través de [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection).

## <a name="extensionsnapshotfailednonetwork---snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a><a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork: Error de la operación de instantánea debido a que no hay conectividad de red en la máquina virtual

**Código de error**: ExtensionSnapshotFailedNoNetwork<br>
**Mensaje de error**: Error de la operación de instantánea debido a que no hay conectividad de red en la máquina virtual<br>

Después de registrar y programar una máquina virtual para el servicio de Azure Backup, Backup inicia el trabajo al comunicarse con la extensión de copia de seguridad de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Si la instantánea no se desencadena, se podría producir un error en la copia de seguridad. Siga los pasos de solución de problemas siguientes y luego vuelva a intentar la operación:

**[No se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  

## <a name="extensionoperationfailedformanageddisks---vmsnapshot-extension-operation-failed"></a><a name="ExtensionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtensionOperationFailedForManagedDisks: error en la operación de extensión VMSnapshot

**Código de error**: ExtensionOperationFailedForManagedDisks <br>
**Mensaje de error**: Error en la operación de extensión de VMSnapshot<br>

Después de registrar y programar una máquina virtual para el servicio de Azure Backup, Backup inicia el trabajo al comunicarse con la extensión de copia de seguridad de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Si la instantánea no se desencadena, se podría producir un error en la copia de seguridad. Realice los pasos de solución de problemas siguientes en el orden indicado y, a continuación, vuelva a intentar la operación:  
**Causa 1: [no se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 2: [El agente está instalado en la máquina virtual, pero no responde (en máquinas virtuales Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 3: [el agente instalado en la máquina virtual está obsoleto (en el caso de máquinas virtuales Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed/BackUpOperationFailedV2: Se ha producido un error interno en la copia de seguridad

**Código de error**: BackUpOperationFailed/BackUpOperationFailedV2 <br>
**Mensaje de error**: Error interno en la copia de seguridad. Reintente la operación en unos minutos <br>

Después de registrar y programar una máquina virtual para el servicio de Azure Backup, Backup inicia el trabajo al comunicarse con la extensión de copia de seguridad de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Si la instantánea no se desencadena, se podría producir un error en la copia de seguridad. Realice los pasos de solución de problemas siguientes en el orden indicado y, a continuación, vuelva a intentar la operación:  
**Causa 1: [el agente está instalado en la máquina virtual pero no responde (en máquinas virtuales Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 2: [el agente instalado en la máquina virtual está obsoleto (en el caso de máquinas virtuales Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 3: [no se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 4: [el servicio Backup no tiene permiso para eliminar los puntos de restauración antiguos debido a un bloqueo del grupo de recursos](#remove_lock_from_the_recovery_point_resource_group)**<br>

## <a name="usererrorunsupporteddisksize---the-configured-disk-sizes-is-currently-not-supported-by-azure-backup"></a>UserErrorUnsupportedDiskSize: el tamaño de disco configurado no es compatible actualmente con Azure Backup.

**Código de error**: UserErrorUnsupportedDiskSize <br>
**Mensaje de error**: El tamaño de disco configurado no es compatible actualmente con Azure Backup. <br>

La operación de copia de seguridad podría generar un error cuando se realiza una copia de seguridad de una máquina virtual con un tamaño de disco superior a 32 TB. Además, actualmente no se admite la copia de seguridad de discos cifrados con un tamaño superior a 4 TB. Para asegurarse de que el tamaño de disco sea inferior o igual al límite admitido, divida los discos.

## <a name="usererrorbackupoperationinprogress---unable-to-initiate-backup-as-another-backup-operation-is-currently-in-progress"></a>UserErrorBackupOperationInProgress: no se puede iniciar la copia de seguridad, porque hay otra operación de copia de seguridad en curso actualmente

**Código de error**: UserErrorBackupOperationInProgress <br>
**Mensaje de error**: No se puede iniciar la copia de seguridad, ya que otra operación de copia de seguridad está en curso actualmente<br>

El trabajo de copia de seguridad reciente no se pudo completar porque hay un trabajo de copia de seguridad existente en curso. No se puede iniciar un nuevo trabajo de copia de seguridad hasta que no finalice el trabajo actual. Asegúrese de que la operación de copia de seguridad en curso se complete antes de activar o programar otras operaciones de copia de seguridad. Para comprobar el estado de los trabajos de copia de seguridad, siga estos pasos:

1. Inicie sesión en Azure Portal y haga clic en **Todos los servicios**. Escriba Recovery Services y haga clic en **Almacenes de Recovery Services**. Aparece la lista de almacenes de Servicios de recuperación.
2. En la lista de almacenes de Recovery Services, seleccione un almacén donde esté configurada la copia de seguridad.
3. En el menú del panel del almacén, haga clic en **Trabajos de copia de seguridad** para mostrar todos los trabajos de copia de seguridad.
   - Si un trabajo de copia de seguridad está en curso, espere a que se complete o cancele el trabajo de copia de seguridad.
     - Para cancelar el trabajo de copia de seguridad, haga clic con el botón derecho en el trabajo de copia de seguridad y haga clic en **Cancelar** o use [PowerShell](https://docs.microsoft.com/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0).
   - Si ha reconfigurado la copia de seguridad en otro almacén, asegúrese de no haya ningún trabajo de copia de seguridad en ejecución en el almacén antiguo. Si existe, cancele el trabajo de copia de seguridad.
     - Para cancelar el ratón de trabajo de copia de seguridad, haga clic con el botón derecho en el trabajo de copia de seguridad y haga clic en **Cancelar** o use [PowerShell](https://docs.microsoft.com/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0).
4. Reintente la operación de copia de seguridad.

Si la operación de copia de seguridad programada tarda más, generando un conflicto con la siguiente configuración de copia de seguridad, consulte [Procedimientos recomendados](backup-azure-vms-introduction.md#best-practices), [Rendimiento de Backup](backup-azure-vms-introduction.md#backup-performance) y [Consideraciones de la restauración](backup-azure-vms-introduction.md#backup-and-restore-considerations).

## <a name="usererrorcrpreportedusererror---backup-failed-due-to-an-error-for-details-see-job-error-message-details"></a>UserErrorCrpReportedUserError: no se pudo realizar la copia de seguridad debido a un error. Para más información, consulte los detalles del mensaje de error del trabajo.

**Código de error**: UserErrorCrpReportedUserError <br>
**Mensaje de error**: Se ha producido un error en la copia de seguridad. Para más información, consulte los detalles del mensaje de error del trabajo.

Este error se envía desde la máquina virtual de IaaS. Para identificar la causa principal del problema, vaya a la configuración del almacén de Recovery Services. En la sección **Supervisión**, seleccione **Trabajos de copia de seguridad** para filtrar y ver el estado. Haga clic en **Errores** para revisar los detalles del mensaje de error subyacente. Realice otras acciones según las recomendaciones de la página de detalles del error.

## <a name="causes-and-solutions"></a>Causas y soluciones

### <a name="the-agent-is-installed-in-the-vm-but-its-unresponsive-for-windows-vms"></a><a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>El agente está instalado en la máquina virtual, pero no responde (en máquinas virtuales Windows)

#### <a name="solution"></a>Solución

Es posible que el agente de máquina virtual se haya dañado o que el servicio se haya detenido. Puede intentar volver a instalar al agente de máquina virtual para obtener la versión más reciente. O bien, intente restablecer la comunicación con el servicio.

1. Determine si el servicio de agente invitado de Microsoft Azure se ejecuta en los servicios de máquina virtual (services.msc). Intente reiniciar el servicio de agente invitado de Microsoft Azure e inicie la copia de seguridad.
2. Si el servicio de agente invitado de Microsoft Azure no está visible en los servicios, en el Panel de control, vaya a **Programas y características** para determinar si está instalado.
3. Si el agente invitado de Microsoft Azure aparece en **Programas y características**, desinstálelo.
4. Descargue e instale la [versión más reciente del MSI del agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Debe tener derechos de administrador para completar la instalación.
5. Compruebe que el servicio de agente de invitado de Microsoft Azure aparece en los servicios.
6. Ejecute un trabajo de copia de seguridad a petición:
   - En el portal, seleccione **Crear copia de seguridad ahora**.

Además, compruebe que [Microsoft .NET 4.5](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) está instalado en la máquina virtual, ya que se requiere para que el agente de máquina virtual se comunique con el servicio.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>El agente instalado en la máquina virtual está obsoleto (en el caso de máquinas virtuales Linux)

#### <a name="solution"></a>Solución

La mayoría de los errores relacionados con el agente o la extensión de máquinas virtuales de Linux están provocados por problemas que afectan a un agente VM obsoleto. Para solucionar este problema, siga estas directrices generales:

1. Siga las instrucciones para [actualizar el agente de máquina virtual Linux ](../virtual-machines/linux/update-agent.md).

   > [!NOTE]
   > Se *recomienda encarecidamente* actualizar el agente solo a través de un repositorio de distribución. No recomendamos descargar el código de agente desde GitHub directamente y actualizarlo. Si el último agente no está disponible para su distribución, póngase en contacto con el soporte técnico de distribución para obtener instrucciones sobre cómo instalarlo. Para buscar el agente más reciente, vaya a la página del [agente Linux de Microsoft Azure](https://github.com/Azure/WALinuxAgent/releases) en el repositorio de GitHub.

2. Asegúrese de que el agente de Azure se ejecuta en la máquina virtual mediante la ejecución del comando siguiente: `ps -e`.

   Si el proceso no se está ejecutando, reinícielo mediante los siguientes comandos:

   - Para Ubuntu: `service walinuxagent start`
   - Para otras distribuciones: `service waagent start`

3. [Configure el agente de reinicio automático](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Ejecute una nueva copia de seguridad de prueba. Si el error persiste, recopile los registros siguientes de la máquina virtual:

   - /var/lib/waagent/*.xml
   - /var/log/waagent.log
   - /var/log/azure/*

Si requiere el registro detallado para waagent, siga estos pasos:

1. En el archivo /etc/waagent.conf, busque la línea siguiente: **Habilitar el registro detallado (s/n)**
2. Cambie el valor de **Logs.Verbose** de *n* a *y*.
3. Guarde el cambio y, a continuación, reinicie waagent siguiendo los pasos descritos anteriormente en esta sección.

### <a name="vm-agent-configuration-options-are-not-set-for-linux-vms"></a>No se han establecido las opciones de configuración del agente de máquina virtual (para máquinas virtuales Linux)

Un archivo de configuración (/etc/waagent.conf) controla las acciones de waagent. Las opciones del archivo de configuración **Extensions.Enable** y **Provisioning.Agent** se deben establecer en **y** para que la copia de seguridad funcione.
Para obtener una lista completa de las opciones del archivo de configuración del agente de máquina virtual, consulte <https://github.com/Azure/WALinuxAgent#configuration-file-options>.

### <a name="the-snapshot-status-cant-be-retrieved-or-a-snapshot-cant-be-taken"></a><a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>No se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas

La copia de seguridad de máquina virtual se basa en la emisión de comandos de instantánea para la cuenta del almacenamiento subyacente. La copia de seguridad puede producir un error porque no tiene ningún acceso a la cuenta de almacenamiento o porque se retrasa la ejecución de la tarea de instantáneas.

#### <a name="solution"></a>Solución

Las siguientes condiciones podrían hacer que la tarea de instantáneas no se realizara:

| Causa | Solución |
| --- | --- |
| El estado de la máquina virtual no se notifica correctamente porque la máquina virtual está apagada en el Protocolo de escritorio remoto (RDP). | Si ha apagado la máquina virtual en RDP, compruebe el portal para determinar si ese estado de la máquina virtual es correcto. Si no es así, apague la máquina virtual en el portal mediante la opción **Apagar** en el panel de la máquina virtual. |
| La máquina virtual no puede obtener la dirección de host o de tejido de DHCP. | DHCP debe estar habilitado dentro del invitado para que la copia de seguridad de la máquina virtual de IaaS funcione. Si la máquina virtual no puede obtener la dirección de host o de tejido de la respuesta 245 de DHCP, no podrá descargar ni ejecutar ninguna extensión. Si necesita una dirección IP privada estática, debe configurarla a través de **Azure Portal** o **PowerShell** y asegurarse de que está habilitada la opción DHCP dentro de la máquina virtual. [Obtenga más información](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface) acerca de cómo configurar una dirección IP estática con PowerShell.

### <a name="remove-lock-from-the-recovery-point-resource-group"></a><a name="remove_lock_from_the_recovery_point_resource_group"></a>Eliminación del bloqueo del grupo de recursos de punto de restauración

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Vaya a la opción **Todos los recursos**, seleccione el grupo de recursos de la colección de puntos de restauración en el siguiente formato AzureBackupRG_`<Geo>`_`<number>`.
3. En la sección **Configuración**, seleccione **Bloqueos** para mostrar los bloqueos.
4. Para quitar el bloqueo, seleccione los puntos suspensivos y haga clic en **Eliminar**.

    ![Eliminación del bloqueo](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean-up-restore-point-collection"></a><a name="clean_up_restore_point_collection"></a> Eliminación de la colección de puntos de restauración

Después de quitar el bloqueo, los puntos de restauración deben limpiarse.

Si elimina el grupo de recursos de la máquina virtual o la propia máquina virtual, las instantáneas de restauración inmediata de discos administrados permanecen activas y expiran según el conjunto de retención. Para eliminar las instantáneas de restauración inmediata (si ya no las necesita) almacenadas en la colección de puntos de restauración, limpie la colección de puntos de restauración según los pasos que se indican a continuación.

Para limpiar los puntos de restauración, siga cualquiera de los métodos siguientes:<br>

- [Limpieza de la colección de puntos de restauración mediante la ejecución de la copia de seguridad a petición](#clean-up-restore-point-collection-by-running-on-demand-backup)<br>
- [Eliminación de la colección de puntos de restauración desde Azure Portal](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-on-demand-backup"></a><a name="clean-up-restore-point-collection-by-running-on-demand-backup"></a>Limpieza de la colección de puntos de restauración mediante la ejecución de la copia de seguridad a petición

Después de quitar el bloqueo, desencadene una copia de seguridad a petición. Esto garantizará que los puntos de restauración se limpien automáticamente. Esta operación a petición probablemente produzca un error la primera vez; sin embargo, así se garantizará la limpieza automática en lugar de la eliminación manual de los puntos de restauración. Después de la limpieza, debería realizarse correctamente la siguiente copia de seguridad programada.

> [!NOTE]
> Se realizará la limpieza automática unas horas después de desencadenar la copia de seguridad a petición. Si la copia de seguridad programada sigue produciendo un error, pruebe a eliminar manualmente la colección de puntos de restauración mediante los pasos indicados [aquí](#clean-up-restore-point-collection-from-azure-portal).

#### <a name="clean-up-restore-point-collection-from-azure-portal-br"></a><a name="clean-up-restore-point-collection-from-azure-portal"></a>Eliminación de la colección de puntos de restauración desde Azure Portal <br>

Para borrar manualmente la colección de puntos de restauración que no se han borrado debido al bloqueo del grupo de recursos, pruebe con los siguientes pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En el menú **central**, haga clic en **Todos los recursos**, seleccione el grupo de recursos con el siguiente formato AzureBackupRG_`<Geo>`_`<number>` donde se encuentra la máquina virtual.

    ![Eliminación del bloqueo](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Haga clic en el grupo de recursos; se muestra el panel **Información general**.
4. Seleccione la opción **Mostrar tipos ocultos** para mostrar todos los recursos ocultos. Seleccione las colecciones de puntos de restauración con el siguiente formato AzureBackupRG_`<VMName>`_`<number>`.

    ![Eliminación del bloqueo](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Haga clic en **Eliminar** para borrar la colección de puntos de restauración.
6. Vuelva a intentar la operación de copia de seguridad.

> [!NOTE]
 >Si el recurso (colección RP) tiene un gran número de puntos de restauración, su eliminación desde el portal es posible que exceda el tiempo de expiración y produzca un error. Este es un problema conocido de CRP, debido al cual no se eliminan todos los puntos de restauración en el tiempo establecido y la operación agota el tiempo de expiración; sin embargo, la operación de eliminación se realiza normalmente después de dos o tres reintentos.
