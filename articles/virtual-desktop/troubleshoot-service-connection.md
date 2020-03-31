---
title: 'Solución de problemas con la conexión al servicio de Windows Virtual Desktop: Azure'
description: Cómo resolver problemas al configurar conexiones de cliente en un entorno de inquilinos de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 57d5198cb54dc096fb09bb52d76539b1e4bbc1f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127450"
---
# <a name="windows-virtual-desktop-service-connections"></a>Conexiones al servicio de Windows Virtual Desktop

Use este artículo para resolver problemas con conexiones de cliente de Windows Virtual Desktop.

## <a name="provide-feedback"></a>Envío de comentarios

Puede proporcionarnos comentarios y hablar sobre el servicio Windows Virtual Desktop con el equipo del producto y otros miembros activos de la comunidad en la [comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop).

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>El usuario se conecta, pero no se muestra nada (ninguna fuente)

Un usuario puede iniciar clientes de Escritorio remoto y es capaz de realizar la autenticación, pero no ve ningún icono en la fuente de detección de web.

Confirme que el usuario que notificó los problemas se ha asignado a grupos de aplicaciones mediante el uso de esta línea de comandos:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Confirme que el usuario ha iniciado sesión con las credenciales correctas.

Si se usa el cliente web, confirme que no hay ningún problema de credenciales almacenadas en caché.

## <a name="windows-10-enterprise-multi-session-virtual-machines-dont-respond"></a>Las máquinas virtuales de varias sesiones de Windows 10 Enterprise no responden

Si una máquina virtual no responde y no puede acceder a ella a través de RDP, deberá solucionar el problema con la característica de diagnóstico comprobando el estado del host.

Para comprobar el estado del host, ejecute este cmdlet:

```powershell
Get-RdsSessionHost -TenantName $TenantName -HostPoolName $HostPool | ft SessionHostName, LastHeartBeat, AllowNewSession, Status
```

Si el estado del host es `NoHeartBeat`, significa que la máquina virtual no responde y el agente no puede comunicarse con el servicio Windows Virtual Desktop.

```powershell
SessionHostName          LastHeartBeat     AllowNewSession    Status 
---------------          -------------     ---------------    ------ 
WVDHost1.contoso.com     21-Nov-19 5:21:35            True     Available 
WVDHost2.contoso.com     21-Nov-19 5:21:35            True     Available 
WVDHost3.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
WVDHost4.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
WVDHost5.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
```

Hay algunas acciones que puede llevar a cabo para corregir el estado NoHeartBeat.

### <a name="update-fslogix"></a>Actualización de FSLogix

Si FSLogix no está actualizado, especialmente si es la versión 2.9.7205.27375 de frxdrvvt.sys, podría producirse un interbloqueo. Asegúrese de [actualizar FSLogix a la versión más reciente](https://go.microsoft.com/fwlink/?linkid=2084562).

### <a name="disable-bgtaskregistrationmaintenancetask"></a>Deshabilitación de BgTaskRegistrationMaintenanceTask

Si la actualización de FSLogix no funciona, el problema podría ser que un componente de BiSrv está agotando los recursos del sistema durante una tarea de mantenimiento semanal. Deshabilite temporalmente la tarea de mantenimiento. Para ello, deshabilite BgTaskRegistrationMaintenanceTask con uno de estos dos métodos:

- Vaya al menú Inicio y busque **Programador de tareas**. Vaya a **Biblioteca del Programador de tareas** > **Microsoft** > **Windows** > **BrokerInfrastructure**. Busque una tarea llamada **BgTaskRegistrationMaintenanceTask**. Cuando la encuentre, haga clic con el botón secundario en ella y seleccione **Deshabilitar** en el menú desplegable.
- Abra un menú de la línea de comandos como administrador y ejecute el siguiente comando:
    
    ```cmd
    schtasks /change /tn "\Microsoft\Windows\BrokerInfrastructure\BgTaskRegistrationMaintenanceTask" /disable 
    ```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre cómo solucionar problemas de Windows Virtual Desktop y las pistas de escalación, consulte [Introducción, comentarios y soporte técnico para solucionar problemas](troubleshoot-set-up-overview.md).
- Para solucionar problemas durante la creación de un grupo de inquilinos y de hosts en un entorno de Windows Virtual Desktop, consulte [Creación de los grupos de inquilinos y de host](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en Windows Virtual Desktop, consulte [Configuración de la máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas al usar PowerShell con Windows Virtual Desktop, consulte [PowerShell para Windows Virtual Desktop](troubleshoot-powershell.md).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de las implementaciones de plantillas de Resource Manager](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
