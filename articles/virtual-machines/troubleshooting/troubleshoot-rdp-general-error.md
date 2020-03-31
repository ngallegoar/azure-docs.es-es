---
title: Solución de problemas de un error general de RDP en una máquina virtual Windows en Azure | Microsoft Docs
description: Aprenda a solucionar un error general de RDP en una máquina virtual Windows en Azure | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 7fc0fbf3362d18284ad6a80afa6396b6be1270a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "71057999"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Solución de problemas de un error general de RDP en una máquina virtual Windows en Azure

En este artículo se describe un error general que puede experimentar al realizar una conexión de Protocolo de escritorio remoto (RDP) a una máquina virtual Windows (VM) en Azure.

## <a name="symptom"></a>Síntoma

Al realizar una conexión RDP a una máquina virtual Windows en Azure, puede recibir el siguiente mensaje de error general:

**Escritorio remoto no se puede conectar al equipo remoto por una de estas razones:**

1. **No está habilitado el acceso remoto al servidor.**

2. **El equipo remoto está desactivado.**

3. **El equipo remoto no está disponible en la red.**

**Asegúrese de que el equipo remoto está encendido y conectado a la red, y de que está habilitado el acceso remoto.**

## <a name="cause"></a>Causa

Este problema se produce debido a las siguientes causas:

### <a name="cause-1"></a>Causa 1

El componente de RDP está deshabilitado:

- En el nivel de componente
- En el nivel de agente de escucha
- En el servidor de Terminal Server
- En el rol de host de sesión de Escritorio remoto

### <a name="cause-2"></a>Causa 2

Servicios de escritorio remoto (TermService) no se está ejecutando.

### <a name="cause-3"></a>Causa 3

El agente de escucha de RDP está mal configurado.

## <a name="solution"></a>Solución

Para resolver este problema, [realice una copia de seguridad del disco del sistema operativo](../windows/snapshot-copy-managed-disk.md) y [asocie el disco del sistema operativo a una máquina virtual de rescate](troubleshoot-recovery-disks-portal-windows.md). A continuación, siga los pasos.

### <a name="serial-console"></a>Consola serie

#### <a name="step-1-open-cmd-instance-in-serial-console"></a>Paso 1: Abrir la instancia CMD en la consola serie

1. Acceda a [Serial Console](serial-console-windows.md) seleccionando **Soporte técnico y solución de problemas** > **Serial Console (versión preliminar)** . Si la característica está habilitada en la máquina virtual, puede conectar la máquina virtual correctamente.

2. Creación de un nuevo canal para una instancia CMD. Escriba **CMD** para iniciar el canal con el fin de obtener el nombre del canal.

3. Cambie al canal que ejecuta la instancia CMD, en este caso debe ser el canal 1.

   ```
   ch -si 1
   ```

#### <a name="step-2-check-the-values-of-rdp-registry-keys"></a>Paso 2: Comprobación de los valores de las claves del Registro de RDP:

1. Compruebe si el RDP está deshabilitado por las directivas.

      ```
      REM Get the local policy 
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      - Si la directiva de dominio existe, se sobrescribe la configuración de la directiva local.
      - Si la directiva de dominio indica que RDP está deshabilitado (1), actualice directiva de AD del controlador de dominio.
      - Si la directiva de dominio indica que RDP está habilitado (0), no hay que actualizar.
      - Si no existe la directiva de dominio y la directiva local indica que RDP está deshabilitado (1), habilite RDP mediante el comando siguiente: 
      
            reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
                  

2. Compruebe la configuración actual del servidor de Terminal Server.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
      ```

      Si el comando devuelve 0, el servidor de Terminal Server está deshabilitado. A continuación, habilite el servidor de Terminal Server:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

3. El módulo de Terminal Server está establecido en el modo de purga si el servidor no se encuentra en una granja de servidores de terminal server (RDS o Citrix). Compruebe el modo actual del módulo de Terminal Server.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

      Si el comando devuelve 1, el módulo de Terminal Server está establecido en el modo de purga. A continuación, establezca el módulo en el modo de trabajo:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

4. Compruebe si puede conectarse al servidor de Terminal Server.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

      Si el comando devuelve 1, no se puede conectar al servidor de Terminal Server. A continuación, habilite la conexión de la manera siguiente:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```
5. Compruebe la configuración actual del agente de escucha de RDP.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

      Si el comando devuelve 0, el agente de escucha de RDP está deshabilitado. A continuación, habilite el agente de escucha como se indica:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

6. Compruebe si puede conectarse al agente de escucha de RDP.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   Si el comando devuelve 1, no se puede conectar al agente de escucha de RDP. A continuación, habilite la conexión de la manera siguiente:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

7. Reinicie la VM.

8. Salga de la instancia CMD escribiendo `exit` y, a continuación, presione la tecla **Entrar** dos veces.

9. Escriba `restart` para reiniciar la máquina virtual y, luego, conéctese a la máquina virtual.

Si sigue produciéndose el problema, vaya al paso 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Paso 2: Habilitar Servicios de Escritorio remoto

Para obtener más información, consulte [Los Servicios de Escritorio remoto no se inician en la máquina virtual de Azure](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Paso 3: Restablecer el agente de escucha de RDP

Para obtener más información, consulte [Escritorio remoto se desconecta con frecuencia en la máquina virtual de Azure](troubleshoot-rdp-intermittent-connectivity.md).

### <a name="offline-repair"></a>Reparación sin conexión

#### <a name="step-1-turn-on-remote-desktop"></a>Paso 1: Activación de Escritorio remoto

1. [Conecte el disco del sistema operativo a una máquina virtual de recuperación](../windows/troubleshoot-recovery-disks-portal.md).
2. Inicie una conexión mediante el Escritorio remoto a la máquina virtual de recuperación.
3. Asegúrese de que el disco aparece marcado como **En línea** en la consola de Administración de discos. Anote la letra de unidad que se asigna al disco del sistema operativo conectado.
4. Inicie una conexión mediante el Escritorio remoto a la máquina virtual de recuperación.
5. Abra una sesión de símbolo del sistema con privilegios elevados (**Ejecutar como administrador**). Ejecute los scripts siguientes. En este script, se supone que la letra de unidad que se asigna al disco del sistema operativo conectado es F. Reemplácela por el valor apropiado para su máquina virtual.

      ```
      reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv 
      reg load HKLM\BROKENSOFTWARE F:\windows\system32\config\SOFTWARE.hiv 
 
      REM Ensure that Terminal Server is enabled 

      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 

      REM Ensure Terminal Service is not set to Drain mode 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 

      REM Ensure Terminal Service has logon enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 

      REM Ensure the RDP Listener is not disabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 

      REM Ensure the RDP Listener accepts logons 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 

      REM RDP component is enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f 

      reg unload HKLM\BROKENSYSTEM 
      reg unload HKLM\BROKENSOFTWARE 
      ```

6. Si la máquina virtual está unida al dominio, compruebe la siguiente clave del registro para ver si hay una directiva de grupo que se deshabilitará RDP. 

      ```
      HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
      ```

      Si el valor de esta clave está establecido en 1, significa que RDP está deshabilitado por la directiva. Para habilitar Escritorio remoto a través de la directiva de GPO, cambie la directiva siguiente del controlador de dominio:

   
      **Configuración del equipo\Directivas\Plantillas administrativas:**

      Definiciones de directiva\Componentes de Windows\Servicios de Escritorio remoto\Host de sesión de Escritorio remoto\Conexiones\Permitir que los usuarios se conecten de forma remota mediante Servicios de Escritorio remoto
  
1. Desasocie el disco de la máquina virtual de rescate.
1. [Cree una máquina virtual desde el disco](../windows/create-vm-specialized.md).

Si sigue produciéndose el problema, vaya al paso 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Paso 2: Habilitar Servicios de Escritorio remoto

Para obtener más información, consulte [Los Servicios de Escritorio remoto no se inician en la máquina virtual de Azure](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Paso 3: Restablecer el agente de escucha de RDP

Para obtener más información, consulte [Escritorio remoto se desconecta con frecuencia en la máquina virtual de Azure](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
