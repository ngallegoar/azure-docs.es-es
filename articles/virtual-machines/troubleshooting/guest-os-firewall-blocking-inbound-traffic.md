---
title: El firewall de SO invitado de máquina virtual de Azure está bloqueando el tráfico entrante | Microsoft Docs
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
ms.openlocfilehash: 0cbd1a24f5c460e248d55777735da6809befba63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72028799"
---
# <a name="azure-vm-guest-os-firewall-is-blocking-inbound-traffic"></a>El firewall de SO invitado de máquina virtual de Azure está bloqueando el tráfico entrante

En este artículo se describe cómo solucionar el problema del Portal de escritorio remoto (RDP) que se produce si el firewall del sistema operativo invitado bloquea el tráfico entrante.

## <a name="symptoms"></a>Síntomas

No se puede usar una conexión RDP para conectarse a una máquina virtual (VM) de Azure. Desde Diagnósticos de inicio -> Captura de pantalla, se muestra que el sistema operativo está completamente cargado en la pantalla de bienvenida (Ctrl+Alt+Supr).

## <a name="cause"></a>Causa

### <a name="cause-1"></a>Causa 1

La regla RDP no está configurada para permitir el tráfico RDP.

### <a name="cause-2"></a>Causa 2

Los perfiles del firewall del sistema invitado están configurados para bloquear todas las conexiones entrantes, incluido el tráfico RDP.

![Configuración del firewall](./media/guest-os-firewall-blocking-inbound-traffic/firewall-advanced-setting.png)

## <a name="solution"></a>Solución

Antes de seguir estos pasos, tome una instantánea del disco del sistema de la máquina virtual afectada como copia de seguridad. Para más información, consulte  [Crear una instantánea](../windows/snapshot-copy-managed-disk.md).

Para corregir el problema, utilice uno de los métodos de [Uso de herramientas remotas para solucionar problemas con la VM de Azure](remote-tools-troubleshoot-azure-vm-issues.md) para conectarse de forma remota a la máquina virtual y, a continuación, edite las reglas de firewall del sistema operativo invitado a fin de **permitir** el tráfico RDP.

### <a name="online-troubleshooting"></a>Solución de problemas en línea

Conéctese a la [consola serie y abra una instancia de PowerShell](serial-console-windows.md#use-cmd-or-powershell-in-serial-console). Si la consola serie no está habilitada en la máquina virtual, vaya a "[Reparación de la máquina virtual sin conexión](troubleshoot-rdp-internal-error.md#repair-the-vm-offline).

#### <a name="mitigation-1"></a>Mitigación 1

1.  Si el agente de Azure está instalado y funciona correctamente en la máquina virtual, puede usar la opción "Restablecer solo la configuración" en **Soporte técnico y solución de problemas** > **Restablecer contraseña** en el menú de la máquina virtual.

2.  Al ejecutar esta opción de recuperación:

    *   Se habilita un componente RDP si está deshabilitado.

    *   Se habilitan los perfiles del firewall de Windows.

    *   Se asegura que la regla RDP está activada en el firewall de Windows.

    *   Si los pasos anteriores no funcionan, restablezca manualmente la regla de firewall. Para ello, ejecute el comando siguiente para consultar todas las reglas que contengan el nombre "Escritorio remoto":

        ```cmd
        netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(Name.*Remote Desktop)" -context 9,4 | more
        ```

        Si se estableció el puerto RDP en cualquier otro puerto distinto de 3389, debe buscar cualquier regla personalizada que pueda haberse creado y configurado para este puerto. Para consultar todas las reglas de entrada que tengan un puerto personalizado, ejecute el siguiente comando:

        ```cmd
        netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalPort.*<CUSTOM PORT>)" -context 9,4 | more
        ```

3.  Si ve que la regla está deshabilitada, habilítela. Para abrir un grupo completo, como el grupo Escritorio remoto integrado, ejecute el siguiente comando:

    ```cmd
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
    ```

    En caso contrario, para abrir la regla Escritorio remoto (TCP-In) específica, ejecute el siguiente comando:

    ```cmd
    netsh advfirewall firewall set rule name="<CUSTOM RULE NAME>" new enable=yes
    ```

4.  Para solucionar el problema, puede establecer los perfiles de firewall en OFF (desactivados):

    ```cmd
    netsh advfirewall set allprofiles state off
    ```

    Cuando termine de solucionar los problemas y de configurar el firewall correctamente, vuelva a habilitar el firewall.

    > [!Note]
    > No tiene que reiniciar la máquina virtual para aplicar estos cambios.

5.  Intente realizar una conexión RDP para acceder a la máquina virtual.

#### <a name="mitigation-2"></a>Mitigación 2

1.  Consulte los perfiles de firewall para determinar si la directiva de firewall de entrada está establecida en *BlockInboundAlways*:

    ```cmd
    netsh advfirewall show allprofiles | more
    ```

    ![Allprofiles](./media/guest-os-firewall-blocking-inbound-traffic/firewall-profiles.png)

    > [!Note]
    > Las siguientes directrices se aplican a la directiva de firewall, dependiendo de cómo se configure:
    >    * *BlockInbound*: se bloqueará todo el tráfico entrante, a menos que tenga una regla en vigor para permitir ese tráfico.
    >    * *BlockInboundAlways*: se omitirán todas las reglas de firewall y se bloqueará todo el tráfico.

2.  Edite *DefaultInboundAction* para establecer estos perfiles en **Permitir** el tráfico. Para ello, ejecute el siguiente comando:

    ```cmd
    netsh advfirewall set allprofiles firewallpolicy allowinbound,allowoutbound
    ```

3.  Consulte los perfiles de nuevo para asegurarse de que el cambio se realizó correctamente. Para ello, ejecute el siguiente comando:

    ```cmd
    netsh advfirewall show allprofiles | more
    ```

    > [!Note]
    > No tiene que reiniciar la máquina virtual para aplicar estos cambios.

4.  Vuelva a intentar obtener acceso a la máquina virtual mediante RDP.

### <a name="offline-mitigations"></a>Mitigaciones sin conexión

1.  [Conecte el disco del sistema a una máquina virtual de recuperación](troubleshoot-recovery-disks-portal-windows.md).

2.  Inicie una conexión mediante el Escritorio remoto a la máquina virtual de recuperación.

3.  Asegúrese de que el disco aparece marcado como **En línea** en la consola de Administración de discos. Anote la letra de unidad asignada al disco del sistema conectado.

#### <a name="mitigation-1"></a>Mitigación 1

Consulte  [Habilitar o deshabilitar una regla del firewall en un sistema operativo invitado](enable-disable-firewall-rule-guest-os.md).

#### <a name="mitigation-2"></a>Mitigación 2

1.  [Conecte el disco del sistema a una máquina virtual de recuperación](troubleshoot-recovery-disks-portal-windows.md).

2.  Inicie una conexión mediante el Escritorio remoto a la máquina virtual de recuperación.

3.  Después de que el disco del sistema operativo esté asociado a una máquina virtual de recuperación, asegúrese de que se marca como **En línea** en la consola de administración de discos. Anote la letra de unidad que se asigna al disco del sistema operativo conectado.

4.  Abra una instancia de CMD con privilegios elevados y ejecute el siguiente script:

    ```cmd
    REM Backup the registry prior doing any change
    robocopy f:\windows\system32\config f:\windows\system32\config.BACK /MT

    REM Mount the hive
    reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM

    REM Delete the keys to block all inbound connection scenario
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile" /v DoNotAllowExceptions

    REM Unmount the hive
    reg unload HKLM\BROKENSYSTEM
    ```

5.  [Desconecte el disco del sistema y vuelva a crear la máquina virtual](troubleshoot-recovery-disks-portal-windows.md).

6.  Compruebe si se ha resuelto el problema.
