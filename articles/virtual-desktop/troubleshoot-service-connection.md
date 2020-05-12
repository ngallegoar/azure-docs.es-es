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
ms.openlocfilehash: a6298b3a9c5769b1d82f89956736b451935b2c5d
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612646"
---
# <a name="windows-virtual-desktop-service-connections"></a>Conexiones al servicio de Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a la actualización Spring 2020 con objetos de Windows Virtual Desktop para Azure Resource Manager. Si usa la versión Windows Virtual Desktop Fall 2019 sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/troubleshoot-service-connection-2019.md).
>
> La actualización Spring 2020 de Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. 
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Use este artículo para resolver problemas con conexiones de cliente de Windows Virtual Desktop.

## <a name="provide-feedback"></a>Envío de comentarios

Puede proporcionarnos comentarios y hablar sobre el servicio Windows Virtual Desktop con el equipo del producto y otros miembros activos de la comunidad en la [comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop).

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>El usuario se conecta, pero no se muestra nada (ninguna fuente)

Un usuario puede iniciar clientes de Escritorio remoto y es capaz de realizar la autenticación, pero no ve ningún icono en la fuente de detección de web.

Confirme que el usuario que notificó los problemas se ha asignado a grupos de aplicaciones mediante el uso de esta línea de comandos:

```PowerShell
Get-AzRoleAssignment -SignInName <userupn>
```

Confirme que el usuario inicia sesión con las credenciales correctas.

Si se usa el cliente web, confirme que no hay ningún problema de credenciales almacenadas en caché.

## <a name="windows-10-enterprise-multi-session-virtual-machines-dont-respond"></a>Las máquinas virtuales de varias sesiones de Windows 10 Enterprise no responden

Si una máquina virtual no responde y no puede acceder a ella a través de RDP, deberá solucionar el problema con la característica de diagnóstico comprobando el estado del host.

Para comprobar el estado del host, ejecute este cmdlet:

```powershell
Get-AzWvdSessionHost -HostPoolName <hostpoolname> -ResourceGroupName <resourcegroupname>| Format-List Name, LastHeartBeat, AllowNewSession, Status
```

Si el estado del host es `NoHeartBeat`, significa que la máquina virtual no responde y el agente no puede comunicarse con el servicio Windows Virtual Desktop.

```powershell
Name            : 0301HP/win10pd-0.contoso.com 
LastHeartBeat   : 4/8/2020 1:48:35 AM 
AllowNewSession : True 
Status          : Available 

Name            : 0301HP/win10pd-1.contoso.com 
LastHeartBeat   : 4/8/2020 1:45:44 AM 
AllowNewSession : True 
Status          : NoHeartBeat
```

Hay algunas acciones que puede llevar a cabo para corregir el estado NoHeartBeat.

### <a name="update-fslogix"></a>Actualización de FSLogix

Si FSLogix no está actualizado, especialmente si es la versión 2.9.7205.27375 de frxdrvvt.sys, podría producirse un interbloqueo. Asegúrese de [actualizar FSLogix a la versión más reciente](https://go.microsoft.com/fwlink/?linkid=2084562).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre cómo solucionar problemas de Windows Virtual Desktop y las pistas de escalación, consulte [Introducción, comentarios y soporte técnico para solucionar problemas](troubleshoot-set-up-overview.md).
- Para solucionar problemas durante la creación de un entorno de Windows Virtual Desktop y de un grupo de hosts de este, consulte [Creación de entorno y grupo de hosts](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en Windows Virtual Desktop, consulte [Configuración de la máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas al usar PowerShell con Windows Virtual Desktop, consulte [PowerShell para Windows Virtual Desktop](troubleshoot-powershell.md).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de las implementaciones de plantillas de Resource Manager](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
