---
title: Preparación de un disco duro virtual de Windows para cargar en Azure
description: Aprenda a preparar un VHD o un VHDX de Windows para cargarlo en Azure
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/11/2019
ms.author: genli
ms.openlocfilehash: 8118ecde698b54213547e717d25613c0c3e0d3fd
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631553"
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Preparación de un VHD o un VHDX de Windows antes de cargarlo en Azure

Antes de cargar una máquina virtual Windows desde un entorno local en Azure, debe preparar el disco duro virtual (VHD o VHDX). Azure admite máquinas virtuales de generación 1 y de generación 2 que estén en el formato de archivo VHD y tengan un disco de tamaño fijo. El tamaño máximo que se permite para los discos duros virtuales es de 2 TB.

En una máquina virtual de generación 1, un sistema de archivos VHDX se puede convertir a VHD. También se puede convertir un disco de expansión dinámica en un disco de tamaño fijo. Sin embargo, no puede cambiar la generación de una máquina virtual. Para más información, vea [¿Debería crear una máquina virtual de generación 1 o 2 en Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) y [Compatibilidad de Azure con máquinas virtuales de generación 2 (versión preliminar)](generation-2.md).

Para información sobre la directiva de soporte de software de servidor de Microsoft ejecutado en Azure, vea [Soporte técnico del software de servidor de Microsoft para máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!NOTE]
> Las instrucciones de este artículo se aplican a:
>1. La versión de Windows Server 2008 R2 de 64 bits y versiones posteriores de los sistemas operativos de Windows Server. Para más información sobre cómo ejecutar un sistema operativo de 32 bits en Azure, vea [Compatibilidad de sistemas operativos de 32 bits en máquinas virtuales de Azure](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).
>2. Si se va a usar una herramienta de recuperación ante desastres para migrar la carga de trabajo, como Azure Site Recovery o Azure Migrate, sigue siendo necesario realizar este proceso y seguirlo en el sistema operativo invitado para preparar la imagen antes de la migración.

## <a name="system-file-checker-sfc-command"></a>Comando Comprobador de archivos de sistema (SFC)

### <a name="run-windows-system-file-checker-utility-run-sfc-scannow-on-os-prior-to-generalization-step-of-creating-customer-os-image"></a>Ejecute la utilidad Comprobador de archivos de sistema de Windows (ejecutar sfc/scannow) en el sistema operativo antes del paso de generalización de la creación de la imagen de sistema operativo del cliente.

El comando Comprobador de archivos de sistema (SFC) se usa para comprobar y reemplazar los archivos del sistema de Windows.

Para ejecutar el comando SFC:

1. Abra un símbolo del sistema con privilegios elevados como administrador.
1. Escriba `sfc /scannow` y seleccione **Entrar**.

    ![Comprobador de archivos de sistema](media/prepare-for-upload-vhd-image/system-file-checker.png)


Una vez completado el examen de SFC, intente instalar las actualizaciones de Windows y reinicie el equipo.

## <a name="convert-the-virtual-disk-to-a-fixed-size-and-to-vhd"></a>Conversión de un disco virtual en un disco de tamaño fijo y VHD

Si necesita convertir el disco virtual al formato requerido para Azure, utilice uno de los métodos de esta sección:

1. Cree una copia de seguridad de la máquina virtual antes de ejecutar el proceso de conversión del disco virtual.

1. Asegúrese de que el disco duro virtual de Windows funciona correctamente en el servidor local. Resuelva los errores dentro de la propia máquina virtual antes de intentar la conversión o la carga en Azure.

1. Con respecto al tamaño del disco duro virtual:

   1. En Azure, todos los discos duros virtuales deben tener un tamaño virtual alineado con 1 MB. Al convertir un disco sin procesar en un disco duro virtual, tiene que asegurarse de que su tamaño es un múltiplo de 1 MB antes de la conversión. Las fracciones de un megabyte provocarán errores al crear imágenes a partir del disco duro virtual cargado.

   2. El tamaño máximo permitido del disco duro virtual del SO es de 2 TB.


Después de convertir el disco, cree una máquina virtual que use dicho disco. Inicie la máquina virtual e inicie sesión en ella para prepararla para cargarla.

### <a name="use-hyper-v-manager-to-convert-the-disk"></a>Uso de Administrador de Hyper-V para convertir el disco 
1. Abra el Administrador de Hyper-V y seleccione el equipo local de la izquierda. En el menú situado sobre la lista de equipos, seleccione **Acción** > **Editar disco**.
2. En la página **Localizar disco duro virtual**, localice y seleccione el disco virtual.
3. En la página **Elegir acción**, seleccione **Convertir** > **Siguiente**.
4. Si necesita realizar la conversión desde VHDX, seleccione **VHD** > **Siguiente**.
5. Si necesita realizar la conversión desde un disco de expansión dinámica, seleccione **Tamaño fijo** > **Siguiente**.
6. Localice y seleccione una ruta de acceso para guardar el nuevo archivo VHD en ella.
7. Seleccione **Finalizar**.

> [!NOTE]
> Use una sesión de PowerShell con privilegios elevados para ejecutar los comandos de este artículo.

### <a name="use-powershell-to-convert-the-disk"></a>Uso de PowerShell para convertir el disco 
Puede convertir un disco virtual mediante el comando [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) en Windows PowerShell. Seleccione **Ejecutar como administrador** al iniciar PowerShell. 

Con el siguiente comando de ejemplo, el disco se convierte de VHDX a VHD. Este comando convierte también un disco de expansión dinámica en un disco de tamaño fijo.

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

En este comando, reemplace el valor de `-Path` por la ruta de acceso del disco duro virtual que quiera convertir. Reemplace el valor de `-DestinationPath` por la nueva ruta de acceso y el nombre del disco convertido.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Conversión del formato de disco VMDK de VMware
Si tiene una imagen de máquina virtual de Windows en [formato de archivo VMDK](https://en.wikipedia.org/wiki/VMDK), use [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) para convertirla en un disco duro virtual. Para más información, vea [Cómo convertir un VMDK de VMWare a VHD de Hyper-V](https://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Establecimiento de configuraciones de Windows para Azure

> [!NOTE]
> La plataforma Azure monta un archivo ISO en el DVD-ROM cuando se crea una máquina virtual Windows a partir de una imagen generalizada.
> Por este motivo, el DVD-ROM debe estar habilitado en el sistema operativo de la imagen generalizada. Si está deshabilitado, la máquina virtual Windows se bloqueará en la configuración rápida.

En la máquina virtual que tenga previsto cargar en Azure, ejecute los siguientes comandos en una [ventana del símbolo del sistema con privilegios elevados](https://technet.microsoft.com/library/cc947813.aspx):

1. Quite cualquier ruta estática persistente de la tabla de enrutamiento:
   
   * Para ver la tabla de rutas, ejecute `route print` en el símbolo del sistema.
   * Consulte las secciones `Persistence Routes`. Si hay una ruta persistente, use el comando `route delete` para quitarla.
2. Quite al proxy WinHTTP:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```

    Si es necesario que la máquina virtual funcione con algún proxy concreto, agregue una excepción de proxy a la dirección IP de Azure ([168.63.129.16](https://blogs.msdn.microsoft.com/mast/2015/05/18/what-is-the-ip-address-168-63-129-16/
)), de forma que la máquina virtual tenga conectividad con Azure:
    ```
    $proxyAddress="<your proxy server>"
    $proxyBypassList="<your list of bypasses>;168.63.129.16"

    netsh winhttp set proxy $proxyAddress $proxyBypassList
    ```

3. Establezca la directiva SAN del disco en [`Onlineall`](https://technet.microsoft.com/library/gg252636.aspx):
   
    ```PowerShell
    diskpart 
    ```
    En la ventana del símbolo del sistema abierta, escriba los comandos siguientes:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Establezca la Hora universal coordinada (UTC) para Windows. Establezca también el tipo de inicio del servicio Hora de Windows (`w32time`) en `Automatic`:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -Name "RealTimeIsUniversal" -Value 1 -Type DWord -Force

    Set-Service -Name w32time -StartupType Automatic
    ```
5. Establezca el perfil de energía en alto rendimiento:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```
6. Asegúrese de que las variables de entorno `TEMP` y `TMP` están establecidos en sus valores predeterminados:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name "TEMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name "TMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -Force
    ```

## <a name="check-the-windows-services"></a>Comprobación de los servicios de Windows
Asegúrese de que cada uno de los siguientes servicios de Windows está establecido en los valores predeterminados de Windows. Estos servicios son lo mínimo que debe configurarse para garantizar la conectividad de las máquinas virtuales. Para restablecer la configuración de inicio, ejecute los siguientes comandos:
   
```PowerShell
Get-Service -Name bfe | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name dhcp | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name dnscache | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name IKEEXT | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name iphlpsvc | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name netlogon | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name netman | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name nsi | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name TermService | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name MpsSvc | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name RemoteRegistry | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
```
## <a name="update-remote-desktop-registry-settings"></a>Actualización de la configuración del Registro de Escritorio remoto
Asegúrese de que la siguiente configuración está establecida correctamente para la conexión remota:

>[!NOTE] 
>Puede que reciba un mensaje de error al ejecutar `Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services -Name <object name> -Value <value>`. Puede omitir este mensaje sin problemas. Solo significa que el dominio no obliga a esa configuración a aceptar un objeto de directiva de grupo.

1. Protocolo de escritorio remoto (RDP) está habilitado:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "fDenyTSConnections" -Value 0 -Type DWord -Force
    ```
   
2. El puerto de RDP está configurado correctamente. El puerto predeterminado es 3389:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "PortNumber" -Value 3389 -Type DWord -Force
    ```
    Al implementar una máquina virtual, las reglas predeterminadas se crean en el puerto 3389. Si quiere cambiar el número de puerto, hágalo una vez implementada la máquina virtual en Azure.

3. El agente de escucha está escuchando en todas las interfaces de red:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "LanAdapter" -Value 0 -Type DWord -Force
   ```
4. Configure el modo Autenticación a nivel de red (NLA) para las conexiones RDP:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "UserAuthentication" -Value 1 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "SecurityLayer" -Value 1 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "fAllowSecProtocolNegotiation" -Value 1 -Type DWord -Force
     ```

5. Establezca el valor de conexión persistente:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "KeepAliveEnable" -Value 1  -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "KeepAliveInterval" -Value 1  -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "KeepAliveTimeout" -Value 1 -Type DWord -Force
    ```
6. Vuelva a conectarse:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "fDisableAutoReconnect" -Value 0 -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "fInheritReconnectSame" -Value 1 -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "fReconnectSame" -Value 0 -Type DWord -Force
    ```
7. Limite el número de conexiones simultáneas:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "MaxInstanceCount" -Value 4294967295 -Type DWord -Force
    ```
8. Quite cualquier certificado autofirmado que pueda haber enlazado al agente de escucha del Protocolo de escritorio remoto:
    
    ```PowerShell
    if ((Get-Item -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp').Property -contains "SSLCertificateSHA1Hash")
    {
        Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "SSLCertificateSHA1Hash" -Force
    }
    ```
    Este código garantiza que puede se conectar al principio, al implementar la máquina virtual. Si necesita revisar esto más adelante, puede hacerlo una vez implementada la máquina virtual en Azure.

9. Si la máquina virtual va a formar parte de un dominio, compruebe las siguientes directivas para asegurarse de que la configuración anterior no se revierte. 
    
    | Objetivo                                     | Directiva                                                                                                                                                       | Value                                                                                    |
    |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
    | RDP está habilitado                           | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\ Componentes\Servicios de Escritorio remoto\Host de sesión de Escritorio remoto\Conexiones         | Permitir a los usuarios conectarse de forma remota desde el Escritorio remoto                                  |
    | Directiva de grupo de NLA                         | Configuración\Plantillas administrativas\Componentes\Servicios de Escritorio remoto\Host de sesión de Escritorio remoto\Seguridad                                                    | Requerir la autenticación del usuario para las conexiones remotas mediante Autenticación a nivel de red |
    | Configuración de configuración persistente                      | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\ Componentes Windows\Servicios de Escritorio remoto\Host de sesión de Escritorio remoto\Conexiones | Configurar el intervalo de conexión persistente                                                 |
    | Configuración de reconexión                       | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\ Componentes Windows\Servicios de Escritorio remoto\Host de sesión de Escritorio remoto\Conexiones | Reconectar automáticamente                                                                   |
    | Configuración de número limitado de conexiones | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\ Componentes Windows\Servicios de Escritorio remoto\Host de sesión de Escritorio remoto\Conexiones | Limitar el número de conexiones                                                              |

## <a name="configure-windows-firewall-rules"></a>Configuración de reglas de firewall de Windows
1. Active Firewall de Windows en los tres perfiles (dominio, estándar y público):

   ```PowerShell
    Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
   ```

2. Ejecute el comando siguiente en PowerShell para permitir WinRM mediante los tres perfiles de firewall (dominio, privado y público) y habilitar el servicio remoto de PowerShell:
   
   ```PowerShell
    Enable-PSRemoting -Force

    Set-NetFirewallRule -DisplayName "Windows Remote Management (HTTP-In)" -Enabled True
   ```
3. Habilite las siguientes reglas de firewall para permitir el tráfico RDP:

   ```PowerShell
    Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Enabled True
   ```   
4. Habilite la regla de uso compartido de archivos e impresoras para que la máquina virtual pueda responder a un comando ping dentro de la red virtual:

   ```PowerShell
   Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -Enabled True
   ``` 
5. Cree una regla para la red de la plataforma Azure:

   ```PowerShell
    New-NetFirewallRule -DisplayName "AzurePlatform" -Direction Inbound -RemoteAddress 168.63.129.16 -Profile Any -Action Allow -EdgeTraversalPolicy Allow
    New-NetFirewallRule -DisplayName "AzurePlatform" -Direction Outbound -RemoteAddress 168.63.129.16 -Profile Any -Action Allow
   ``` 
6. Si la máquina virtual va a formar parte de un dominio, compruebe las siguientes directivas de Azure AD para asegurarse de que la configuración anterior no se revierte. 

    | Objetivo                                 | Directiva                                                                                                                                                  | Value                                   |
    |--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
    | Habilitar los perfiles de Firewall de Windows | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\Red\Conexión de red\Firewall de Windows\Perfil de dominio\Firewall de Window   | Proteger todas las conexiones de red         |
    | Habilitar RDP                           | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\Red\Conexión de red\Firewall de Windows\Perfil de dominio\Firewall de Window   | Permitir excepciones de Escritorio remoto entrantes |
    |                                      | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\Red\Conexión de red\Firewall de Windows\Perfil estándar\Firewall de Windows | Permitir excepciones de Escritorio remoto entrantes |
    | Habilitar ICMP-V4                       | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\Red\Conexión de red\Firewall de Windows\Perfil de dominio\Firewall de Window   | Permitir excepciones de ICMP                   |
    |                                      | Configuración del equipo\Directivas\Configuración de Windows\Plantillas administrativas\Red\Conexión de red\Firewall de Windows\Perfil estándar\Firewall de Windows | Permitir excepciones de ICMP                   |

## <a name="verify-the-vm"></a>Comprobación de la máquina virtual 

Asegúrese de que la máquina virtual tiene un estado correcto, es segura y está accesible desde RDP: 

1. Para garantizar que el disco es coherente y está en buen estado, compruebe el disco en el próximo reinicio de máquina virtual:

    ```PowerShell
    Chkdsk /f
    ```
    Asegúrese de que el informe muestra un disco limpio y en buen estado.

2. Establezca la configuración de datos de la configuración de arranque (BCD). 

    > [!NOTE]
    > Use una ventana de PowerShell con privilegios elevados para ejecutar estos comandos.
   
   ```powershell
    bcdedit /set "{bootmgr}" integrityservices enable
    bcdedit /set "{default}" device partition=C:
    bcdedit /set "{default}" integrityservices enable
    bcdedit /set "{default}" recoveryenabled Off
    bcdedit /set "{default}" osdevice partition=C:
    bcdedit /set "{default}" bootstatuspolicy IgnoreAllFailures

    #Enable Serial Console Feature
    bcdedit /set "{bootmgr}" displaybootmenu yes
    bcdedit /set "{bootmgr}" timeout 5
    bcdedit /set "{bootmgr}" bootems yes
    bcdedit /ems "{current}" ON
    bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200
   ```
3. El registro de volcado de memoria puede ser útil para solucionar problemas de bloqueo de Windows. Habilite la colección de registros de volcado de memoria:

    ```powershell
    # Set up the guest OS to collect a kernel dump on an OS crash event
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name CrashDumpEnabled -Type DWord -Force -Value 2
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name DumpFile -Type ExpandString -Force -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name NMICrashDump -Type DWord -Force -Value 1

    # Set up the guest OS to collect user mode dumps on a service crash event
    $key = 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    if ((Test-Path -Path $key) -eq $false) {(New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting' -Name LocalDumps)}
    New-ItemProperty -Path $key -Name DumpFolder -Type ExpandString -Force -Value "c:\CrashDumps"
    New-ItemProperty -Path $key -Name CrashCount -Type DWord -Force -Value 10
    New-ItemProperty -Path $key -Name DumpType -Type DWord -Force -Value 2
    Set-Service -Name WerSvc -StartupType Manual
    ```
4. Compruebe que el repositorio de Instrumental de administración de Windows (WMI) es coherente:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Si el repositorio está dañado, consulte [WMI: daños en el repositorio, o no](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

5. Asegúrese de que ninguna otra aplicación está usando el puerto 3389. Este puerto se usa para el servicio RDP en Azure. Para ver qué puertos se usan en la máquina virtual, ejecute `netstat -anob`:

    ```PowerShell
    netstat -anob
    ```

6. Para cargar un VHD de Windows que sea un controlador de dominio:

   * Siga [estos pasos adicionales](https://support.microsoft.com/kb/2904015) para preparar el disco.

   * Asegúrese de que conoce la contraseña de Modo de restauración de servicios de directorio (DSRM), por si tiene que iniciar la máquina virtual en DSRM en algún momento. Para más información, vea [Establecer una contraseña de DSRM](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

7. Asegúrese de que conoce la cuenta predefinida de administrador y la contraseña. Es posible que quiera restablecer la contraseña de administrador local actual y asegurarse de que puede usar esta cuenta para iniciar sesión en Windows mediante la conexión RDP. Este permiso de acceso se controla mediante el objeto de directiva de grupo "Permitir inicio de sesión a través de Servicios de Escritorio remoto". Puede ver este objeto en el Editor de directivas de grupo local, aquí:

    Configuración del equipo\Configuración de Windows\Configuración de seguridad\Directivas locales\Asignación de derechos de usuario

8. Compruebe las siguientes directivas de Azure AD para asegurarse de que no bloquea el acceso de RDP a través de RDP ni desde la red:

    - Configuración del equipo\Configuración de Windows\Configuración de seguridad\Directivas locales\Asignación de derechos de usuario\Denegar el acceso a este equipo desde la red

    - Configuración del equipo\Configuración de Windows\Configuración de seguridad\Directivas locales\Asignación de derechos de usuario\Denegar inicio de sesión a través de Servicios de Escritorio remoto


9. Compruebe la siguiente directiva de Azure AD para asegurarse de que no quita ninguna de las cuentas de acceso obligatorias:

   - Configuración del equipo\Configuración de Windows\Configuración de seguridad\Directivas locales\Asignación de derechos de usuario\Acceder a este equipo desde la red

   En la directiva deben aparecer los siguientes grupos:

   - Administradores

   - Operadores de copias de seguridad

   - Todos

   - Usuarios

10. Reinicie la máquina virtual para asegurarse de que Windows sigue funcionando correctamente y puede conectarse a ella a través de RDP. Llegado este punto, puede que quiera crear una máquina virtual en la instancia de Hyper-V local para asegurarse de que la máquina virtual se inicia completamente. Tras ello, realice pruebas para asegurarse de que se puede acceder a la máquina virtual a través de RDP.

11. Quite los filtros extra de Interfaz de controlador de transporte (TDI). Por ejemplo, quite el software que analice paquetes TCP o firewalls adicionales. Si necesita revisar esto más adelante, puede hacerlo una vez implementada la máquina virtual en Azure.

12. Desinstale cualquier otro software de terceros o controlador relacionado con componentes físicos, o cualquier otra tecnología de virtualización.

### <a name="install-windows-updates"></a>Instalación de actualizaciones de Windows
Lo ideal es mantener la máquina actualizada en el *nivel de revisión* pero, si no es posible, asegúrese de que las siguientes actualizaciones están instaladas. Para obtener las actualizaciones más recientes, consulte las páginas del historial de actualizaciones de Windows: [Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4000825), [Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/help/4009470), y [Windows 7 SP1 y Windows Server 2008 R2 SP1](https://support.microsoft.com/help/4009469).

| Componente               | Binary         | Windows 7 SP1, Windows Server 2008 R2 SP1 | Windows 8, Windows Server 2012               | Windows 8.1, Windows Server 2012 R2 | Windows 10 v1607, Windows Server 2016 v1607 | Windows 10 v1703    | Windows 10 v1709, Windows Server 2016 v1709 | Windows 10 v1803, Windows Server 2016 v1803 |
|-------------------------|----------------|-------------------------------------------|---------------------------------------------|------------------------------------|---------------------------------------------------------|----------------------------|-------------------------------------------------|-------------------------------------------------|
| Storage                 | disk.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061         | -                                                       | -                          | -                                               | -                                               |
|                         | storport.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.332             | -                                               | -                                               |
|                         | ntfs.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Iologmsg.dll   | 6.1.7601.23403 - KB3125574                | 6.2.9200.16384 - KB2995387                  | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Classpnp.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614         | 10.0.14393.953 - KB4022715                              | -                          | -                                               | -                                               |
|                         | Volsnap.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384         | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | partmgr.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850         | 10.0.14393.953 - KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | volmgr.sys     |                                           |                                             |                                    |                                                         | 10.0.15063.0               | -                                               | -                                               |
|                         | Volmgrx.sys    | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | Msiscsi.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726         | 10.0.14393.1066 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Msdsm.sys      | 6.1.7601.23403 - KB3125574                | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726         | -                                                       | -                          | -                                               | -                                               |
|                         | Mpio.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726         | 10.0.14393.1198 - KB4022715                             | -                          | -                                               | -                                               |
|                         | vmstorfl.sys   | 6.3.9600.18907 - KB4072650                | 6.3.9600.18080 - KB3063109                  | 6.3.9600.18907 - KB4072650         | 10.0.14393.2007 - KB4345418                             | 10.0.15063.850 - KB4345419 | 10.0.16299.371 - KB4345420                      | -                                               |
|                         | Fveapi.dll     | 6.1.7601.23311 - KB3125574                | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614         | 10.0.14393.576 - KB4022715                              | -                          | -                                               | -                                               |
|                         | Fveapibase.dll | 6.1.7601.23403 - KB3125574                | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614         | 10.0.14393.206 - KB4022715                              | -                          | -                                               | -                                               |
| Red                 | netvsc.sys     | -                                         | -                                           | -                                  | 10.0.14393.1198 - KB4022715                             | 10.0.15063.250 - KB4020001 | -                                               | -                                               |
|                         | mrxsmb10.sys   | 6.1.7601.23816 - KB4022722                | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726         | 10.0.14393.479 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb20.sys   | 6.1.7601.23816 - KB4022722                | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726         | 10.0.14393.953 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb.sys     | 6.1.7601.23816 - KB4022722                | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726         | 10.0.14393.953 - KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | tcpip.sys      | 6.1.7601.23761 - KB4022722                | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | http.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726         | 10.0.14393.251 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | vmswitch.sys   | 6.1.7601.23727 - KB4022719                | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.138             | -                                               | -                                               |
| Core                    | ntoskrnl.exe   | 6.1.7601.23807 - KB4022719                | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.483             | -                                               | -                                               |
| Servicios de Escritorio remoto | rdpcorets.dll  | 6.2.9200.21506 - KB4022719                | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.0               | -                                               | -                                               |
|                         | termsrv.dll    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850         | 10.0.14393.0 - KB4022715                                | 10.0.15063.0               | -                                               | -                                               |
|                         | termdd.sys     | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | win32k.sys     | 6.1.7601.23807 - KB4022719                | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726         | 10.0.14393.594 - KB4022715                              | -                          | -                                               | -                                               |
|                         | rdpdd.dll      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | rdpwd.sys      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
| Seguridad                | MS17-010       | KB4012212                                 | KB4012213                                   | KB4012213                          | KB4012606                                               | KB4012606                  | -                                               | -                                               |
|                         |                |                                           | KB4012216                                   |                                    | KB4013198                                               | KB4013198                  | -                                               | -                                               |
|                         |                | KB4012215                                 | KB4012214                                   | KB4012216                          | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         |                |                                           | KB4012217                                   |                                    | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         | CVE-2018-0886  | KB4103718               | KB4103730                | KB4103725       | KB4103723                                               | KB4103731                  | KB4103727                                       | KB4103721                                       |
|                         |                | KB4103712          | KB4103726          | KB4103715|                                                         |                            |                                                 |                                                 |
       
> [!NOTE]
> Para evitar un reinicio accidental durante el aprovisionamiento de la máquina virtual, es aconsejable asegurarse de que todas las instalaciones de Windows Update han finalizado y de que no hay actualizaciones pendientes. Una manera de hacerlo es instalar todas las actualizaciones de Windows posibles y reiniciar una vez antes de ejecutar el comando Sysprep.

### <a name="determine-when-to-use-sysprep"></a>Determinar cuándo usar Sysprep <a id="step23"></a>    

Herramienta de preparación del sistema (Sysprep) es un proceso que se puede ejecutar para restablecer una instalación de Windows. Sysprep proporciona una experiencia original, al eliminar todos los datos personales y restablecer varios componentes. 

Sysprep se suele ejecutar para crear una plantilla desde la que implementar muchas otras máquinas virtuales que tengan una configuración específica. Esta plantilla se conoce como *imagen generalizada*.

Si lo que quiere es crear una sola máquina virtual desde un disco, no tiene que usar Sysprep, sino crear la máquina virtual desde una *imagen especializada*. Para más información sobre cómo crear una máquina virtual desde un disco especializado, vea:

- [Creación de una máquina virtual a partir de un disco especializado](create-vm-specialized.md)
- [Create a VM from a specialized VHD disk](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal?branch=master) (Creación de una máquina virtual a partir de un disco duro virtual especializado)

Si desea crear una imagen generalizada, debe ejecutar Sysprep. Para información, vea [Uso de Sysprep: Introducción](https://technet.microsoft.com/library/bb457073.aspx). 

No todos los roles o aplicaciones instalados en un equipo basado en Windows admiten estas imágenes generalizadas; por lo tanto, antes de ejecutar este procedimiento, asegúrese de que Sysprep admite el rol del equipo. Para más información, consulte [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Compatibilidad de Sysprep con roles de servidor).

### <a name="generalize-a-vhd"></a>Generalización de un VHD

>[!NOTE]
> Después de ejecutar `sysprep.exe` en los pasos siguientes, desactive la máquina virtual. No vuelva a activarla hasta que cree una imagen de ella en Azure.

1. Inicie sesión en la máquina virtual Windows.
1. Ejecute **Símbolo del sistema** como administrador. 
1. Cambie el directorio a `%windir%\system32\sysprep`. A continuación, ejecute `sysprep.exe`.
1. En **Herramienta de preparación del sistema**, seleccione **Iniciar la Configuración rápida (OOBE)** y asegúrese de que la casilla **Generalizar** está activada.

    ![Herramienta de preparación del sistema](media/prepare-for-upload-vhd-image/syspre.png)
1. En **Opciones de apagado**, seleccione **Apagar**.
1. Seleccione **Aceptar**.
1. Cuando Sysprep finalice, apague la máquina virtual. No use **Reiniciar** para apagar la máquina virtual.

Ahora el disco duro virtual está listo para cargarse. Para más información sobre cómo crear una máquina virtual desde un disco generalizado, vea [Carga de un VHD generalizado en Azure para crear una máquina virtual nueva](sa-upload-generalized.md).


>[!NOTE]
> No se admite un archivo *unattend.xml* personalizado, pero sí la propiedad `additionalUnattendContent`, que solo proporciona compatibilidad limitada para agregar las opciones de [microsoft-windows-shell-setup](https://docs.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup) en el archivo *unattend.xml* que utiliza el agente de aprovisionamiento de Azure. Por ejemplo, puede usar [additionalUnattendContent](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.additionalunattendcontent?view=azure-dotnet) para agregar FirstLogonCommands y LogonCommands. Para más información, vea el [ejemplo de FirstLogonCommands de additionalUnattendContent](https://github.com/Azure/azure-quickstart-templates/issues/1407).


## <a name="complete-the-recommended-configurations"></a>Completar las configuraciones recomendadas
Los siguientes valores de configuración no afectan a la carga de discos duros virtuales. Sin embargo, se recomienda firmemente que los configure.

* Instale el [agente de máquina virtual de Azure](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). A continuación, puede habilitar las extensiones de máquina virtual. Las extensiones de máquina virtual implementan la mayor parte de la funcionalidad crítica que es posible que quiera usar con las máquinas virtuales. Necesitará estas extensiones, por ejemplo, para restablecer contraseñas o configurar RDP. Para más información, vea [Información general sobre el agente de la máquina virtual de Azure](../extensions/agent-windows.md).
* Después de crear la máquina virtual en Azure, recomendamos que ponga el archivo de paginación en el *volumen de unidad temporal* para mejorar el rendimiento. Puede configurar la ubicación de archivos del siguiente modo:

   ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -Name "PagingFiles" -Value "D:\pagefile.sys" -Type MultiString -Force
   ```
  Si hay un disco de datos conectado a la máquina virtual, la letra de unidad del volumen de unidad temporal suele ser *D*. Esta designación podría ser diferente, dependiendo de la configuración y del número de unidades disponibles.
  * Se recomienda deshabilitar los bloqueadores de script que pueda incluir el software antivirus, Podrían interferir y bloquear los scripts del agente de aprovisionamiento de Windows que se ejecutan al implementar una nueva máquina virtual desde la imagen.
  
## <a name="next-steps"></a>Pasos siguientes
* [Carga de una imagen de máquina virtual de Windows en Azure para implementaciones de Resource Manager](upload-generalized-managed.md)
* [Solución de problemas de activación de máquinas virtuales Windows de Azure](troubleshoot-activation-problems.md)

