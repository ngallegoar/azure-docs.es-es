---
title: El firewall de SO invitado de máquina virtual de Azure no está correctamente configurado | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/22/2018
ms.author: delhan
ms.openlocfilehash: 8f04d943e1db49beed13c183fbd06e401546fc03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "71153889"
---
# <a name="azure-vm-guest-os-firewall-is-misconfigured"></a>El firewall de SO invitado de máquina virtual de Azure no está correctamente configurado

Este artículo describe cómo corregir el firewall mal configurado del sistema operativo invitado en una máquina virtual de Azure.

## <a name="symptoms"></a>Síntomas

1.  La pantalla de bienvenida de la máquina virtual muestra que la máquina virtual se ha cargado completamente.

2.  Según cómo esté configurado el sistema operativo invitado, es posible que a la máquina virtual llegue algo de tráfico de red o no llegue nada.

## <a name="cause"></a>Causa

Una configuración incorrecta del firewall del sistema invitado puede bloquear total o parcialmente el tráfico de red a la máquina virtual.

## <a name="solution"></a>Solución

Antes de seguir estos pasos, tome una instantánea del disco del sistema de la máquina virtual afectada como copia de seguridad. Para obtener más información, consulte [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md).

Para solucionar este problema, utilice la consola serie o [repare la máquina virtual sin conexión](troubleshoot-rdp-internal-error.md#repair-the-vm-offline) mediante la conexión del disco del sistema de la máquina virtual a una máquina virtual de recuperación.

## <a name="online-mitigations"></a>Mitigaciones con conexión

Conéctese a la [consola serie y abra una instancia de PowerShell](serial-console-windows.md#use-cmd-or-powershell-in-serial-console). Si la consola serie no está habilitada en la máquina virtual, vaya a la sección de reparación de la máquina virtual sin conexión del siguiente artículo de Azure:

 [Se produce un error interno al intentar conectarse a una máquina virtual de Azure a través de Escritorio remoto](troubleshoot-rdp-internal-error.md#repair-the-vm-offline)

Para habilitar el acceso a la máquina virtual (mediante RDP) o para proporcionar una experiencia de solución de problemas más sencilla, se pueden editar las reglas siguientes:

*   Escritorio remoto (TCP de entrada): esta es la regla estándar que proporciona acceso principal a la máquina virtual, ya que permite RDP en Azure.

*   Administración remota de Windows (HTTP de entrada): esta regla permite conectarse a la máquina virtual mediante PowerShell. En Azure, este tipo de acceso permite usar el aspecto de scripting de scripting remoto y solución de problemas.

*   Compartir archivos e impresoras (SMB de entrada): esta regla permite el acceso de recurso compartido de red como una opción de solución de problemas.

*   Compartir archivos e impresoras (solicitud de eco - ICMPv4 de entrada): esta regla le permite hacer ping a la máquina virtual.

En la instancia de acceso a la consola serie, puede consultar el estado actual de la regla de firewall.

*   Consulta con el nombre para mostrar como un parámetro:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(DisplayName.*<FIREWALL RULE NAME>)" -context 9,4 | more
    ```

*   Consulta con el puerto local que la aplicación está usando:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalPort.*<APPLICATION PORT>)" -context 9,4 | more
    ```

*   Consulta con la dirección IP local que la aplicación está usando:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalIP.*<CUSTOM IP>)" -context 9,4 | more
    ```

*   Si ve que la regla está deshabilitada, puede habilitarla ejecutando el comando siguiente:

    ```cmd
    netsh advfirewall firewall set rule name="<RULE NAME>" new enable=yes
    ```

*   Para solucionar problemas, puede establecer los perfiles de firewall en OFF (desactivados):

    ```cmd
    netsh advfirewall set allprofiles state off
    ```

    Si hace esto para configurar correctamente el firewall, vuelva a habilitar el firewall una vez solucionado el problema.

    > [!Note]
    > No tiene que reiniciar la máquina virtual para aplicar este cambio.

*   Intente conectarse de nuevo a la máquina virtual mediante RDP.

### <a name="offline-mitigations"></a>Mitigaciones sin conexión

1.  Para habilitar o deshabilitar reglas de firewall, consulte [Habilitar o deshabilitar una regla de firewall en un sistema operativo invitado de máquina virtual de Azure](enable-disable-firewall-rule-guest-os.md).

2.  Compruebe si se encuentra en el [escenario de tráfico entrante bloqueado por el firewall del SO invitado](guest-os-firewall-blocking-inbound-traffic.md).

3.  Si aún no sabe exactamente si el firewall está bloqueando su acceso, consulte [Deshabilitar el firewall del sistema operativo invitado en Azure Virtual Machine](disable-guest-os-firewall-windows.md) y, a continuación, vuelva a habilitar el firewall del sistema invitado mediante las reglas correctas.

