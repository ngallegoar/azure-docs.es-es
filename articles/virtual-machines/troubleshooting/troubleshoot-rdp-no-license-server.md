---
title: El servidor de licencias de Escritorio remoto no está disponible cuando se conecta a una máquina virtual de Azure | Microsoft Docs
description: Aprenda a solucionar problemas de errores de RDP porque no está disponible ningún servidor de licencias de Escritorio remoto | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 276be92ea4c03b9ebeb3e13df69ce1b10328dcaf
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86526476"
---
# <a name="remote-desktop-license-server-isnt-available-when-you-connect-to-an-azure-vm"></a>El servidor de licencias de Escritorio remoto no está disponible cuando se conecta a una máquina virtual de Azure.

En este artículo se le ayuda a resolver el problema en el que no se puede conectarse a una máquina virtual (VM) de Azure porque no hay ningún servidor de licencias de Escritorio remoto disponible para proporcionar una licencia.

## <a name="symptoms"></a>Síntomas

Cuando intenta conectarse a una máquina virtual (VM), se encuentra con los siguientes escenarios:

- En la captura de pantalla de la máquina virtual se muestra que el sistema operativo está totalmente cargado y esperando las credenciales.
- Recibe los siguientes mensajes de error cuando intenta realizar una conexión del Protocolo de escritorio remoto de Microsoft (RDP):

  - Se desconectó la sesión remota porque no hay servidores de licencias de Escritorio remoto disponibles para proporcionar una licencia.

  - No hay ningún servidor de licencias de Escritorio remoto disponible. Los Servicios de Escritorio remoto dejarán de funcionar porque el período de gracia de este equipo terminó y no ha establecido contacto con un servidor de licencias de Windows Server 2008 válido como mínimo. Seleccione este mensaje para abrir la configuración del servidor host de sesión de Escritorio remoto a fin de usar el diagnóstico de licencias.

Sin embargo, puede conectarse a la máquina virtual normalmente mediante una sesión administrativa:

```
mstsc /v:<Server>[:<Port>] /admin
```

## <a name="cause"></a>Causa

Este problema se produce si no hay ningún servidor de licencias de Escritorio remoto disponible para proporcionar una licencia que permita iniciar una sesión remota. Puede ser debido a varios escenarios, incluso si en la máquina virtual se configuró un rol de host de sesión de Escritorio remoto:

- Nunca hubo ningún rol de administración de licencias de Escritorio remoto en el entorno y se terminó el período de gracia (180 días).
- Una licencia de Escritorio remoto se instaló en el entorno, pero nunca se activó.
- Una licencia de Escritorio remoto en el entorno no tiene insertadas licencias de acceso de cliente (CAL) para establecer la conexión.
- Se instaló una licencia de Escritorio remoto en el entorno. Hay CAL disponibles, pero no se han configurado correctamente.
- Una licencia de Escritorio remoto cuenta con CAL y se ha activado. Sin embargo, algunos otros problemas en el servidor de licencias de Escritorio remoto impiden que se proporcionen las licencias en el entorno.

## <a name="solution"></a>Solución

Para resolver este problema, [realice una copia de seguridad del disco del sistema operativo](../windows/snapshot-copy-managed-disk.md) y siga estos pasos:

1. Conéctese a la máquina virtual mediante una sesión administrativa:

   ```
   mstsc /v:<Server>[:<Port>] /admin
   ```

    Si no puede conectarse a la máquina virtual mediante una sesión administrativa, puede usar la [consola serie de máquina virtual de Azure](serial-console-windows.md) para acceder a la máquina virtual de la manera siguiente:

    1. Acceda a Serial Console seleccionando **Soporte técnico y solución de problemas** > **Serial Console (versión preliminar)** . Si la característica está habilitada en la máquina virtual, puede conectar la máquina virtual correctamente.

    2. Creación de un nuevo canal para una instancia CMD. Escriba **CMD** para iniciar el canal y obtener el nombre del canal.

    3. Cambie al canal que ejecuta la instancia de CMD. En este caso, debe ser el canal 1:

       ```
       ch -si 1
       ```

    4. Presione **Entrar** otra vez y escriba un nombre de usuario y una contraseña válidos y un identificador de dominio o local para la máquina virtual.

2. Compruebe si la máquina virtual tiene habilitado un rol de host de sesión de Escritorio remoto. Si el rol está habilitado, asegúrese de que funciona correctamente. Abra una instancia CMD con privilegios elevados y siga estos pasos:

    1. Use el siguiente comando para comprobar el estado del rol de host de sesión de Escritorio remoto:

       ```
        reg query "HKLM\SOFTWARE\Microsoft\ServerManager\ServicingStorage\ServerComponentCache\RDS-RD-Server" /v InstallState
        ```

        Si este comando devuelve un valor de 0, significa que el rol está deshabilitado, y puede continuar en el paso 3.

    2. Use el siguiente comando para comprobar las directivas y volver a configurarlas según sea necesario:

       ```
        reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode reg query "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers
       ```

        Si el valor de **LicensingMode** es distinto de 4 (por usuario), establezca el valor en 4:

         ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode /t REG_DWORD /d 4
        ```

       Si el valor de **SpecifiedLicenseServers** no existe o tiene información incorrecta del servidor de licencias, cámbielo como se muestra a continuación:

       ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers /t REG_MULTI_SZ /d "<FQDN / IP License server>"
       ```

    3. Después de realizar cualquier cambio en el registro, reinicie la máquina virtual.

    4. Si no dispone de ninguna CAL, quite el rol de host de sesión de Escritorio remoto. A continuación, el RDP se establecerá de vuelta a la normalidad. Solo permite dos conexiones RDP simultáneas a la máquina virtual:

        ```
       dism /ONLINE /Disable-feature /FeatureName:Remote-Desktop-Services
        ```

        Si la máquina virtual tiene el rol de administración de licencias de Escritorio remoto y no se usa, también puede quitar ese rol:

       ```
        dism /ONLINE /Disable-feature /FeatureName:Licensing
       ```

    5. Asegúrese de que la máquina virtual puede conectarse al servidor de licencias de Escritorio remoto. Puede probar la conectividad con el puerto 135 entre la máquina virtual y el servidor de licencias: 

       ```
       telnet <FQDN / IP License Server> 135
       ```

3. Si no hay ningún servidor de licencias de Escritorio remoto en el entorno y quiere uno, puede [instalar un servicio de rol de administración de licencias de Escritorio remoto ](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731765(v=ws.11)). A continuación, [configure la administración de licencias de RDS](https://techcommunity.microsoft.com/t5/Ask-The-Performance-Team/RD-Licensing-Configuration-on-Windows-Server-2012/ba-p/375383).

4. Si un servidor de licencias de Escritorio remoto está configurado y en buen estado, debe asegurarse de activar el servidor de licencias de escritorio remoto con CAL.

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema.
