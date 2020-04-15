---
title: La máquina virtual no responde mientras se aplica la directiva de usuarios y grupos locales de directiva de grupo
description: En este artículo se proporcionan los pasos para resolver los problemas en los que la pantalla de carga se bloquea al aplicar una directiva durante el arranque en una máquina virtual (VM) de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: ff113268-f5bf-4e6a-986e-63b9b0ceff20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 04/02/2020
ms.author: v-mibufo
ms.openlocfilehash: 085880122e9a80e976cfe59686748b58aeba1922
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80620812"
---
# <a name="virtual-machine-is-unresponsive-while-applying-group-policy-local-users--groups-policy"></a>La máquina virtual no responde mientras se aplica la directiva de usuarios y grupos locales de directiva de grupo

En este artículo se proporcionan los pasos para resolver los problemas en los que la pantalla de carga se bloquea al aplicar una directiva durante el arranque en una máquina virtual (VM) de Azure.

## <a name="symptom"></a>Síntoma

Cuando use los [diagnósticos de arranque](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) para ver la captura de pantalla de la máquina virtual, verá que la pantalla se bloquea durante la carga con el mensaje: *Applying Group Policy Local Users and Groups policy* (Aplicando la directiva de usuarios y grupos locales de directiva de grupo).

![Texto alternativo: Pantalla que muestra Applying Group Policy Local Users and Groups policy (Aplicando la directiva de usuarios y grupos locales de directiva de grupo) cargándose (Windows Server 2012).](media/troubleshoot-vm-unresponsive-group-policy-local-users/1.png)

Windows Server 2012

![Texto alternativo: Pantalla que muestra Applying Group Policy Local Users and Groups policy (Aplicando la directiva de usuarios y grupos locales de directiva de grupo) cargándose (Windows Server 2012 R2).](media/troubleshoot-vm-unresponsive-group-policy-local-users/2.png)

Windows Server 2012 R2

## <a name="cause"></a>Causa

Los síntomas de este bloqueo están causados por un defecto en el código de la biblioteca de vínculos dinámicos del servicio de perfil de Windows (*profSvc.dll*).

> [!NOTE]
> Este defecto solo se aplica en Windows Server 2012 y Windows Server 2012 R2.

### <a name="the-policy-in-question"></a>La directiva en cuestión

La directiva que se aplica que no finaliza sus procesos es:

* *Configuración del equipo\Directivas\Plantillas administrativas\Sistema/Perfiles de usuario\Delete user profiles older than a specified number of days on system restart (Eliminar perfiles de usuario anteriores al número de días especificado en el reinicio del sistema)*

Esta directiva solo se bloqueará si se cumplen las seis condiciones siguientes:

* La directiva *Delete user profiles older than a specified number of days on system restart* (Eliminar perfiles de usuario anteriores al número de días especificado en el reinicio del sistema) está habilitada.
* Tiene perfiles que han cumplido los requisitos de antigüedad para requerir la limpieza.
* Tiene componentes que se han registrado para la notificación de eliminación de los perfiles.
* Los componentes realizan llamadas (directas o indirectas) que necesitan para adquirir datos de los componentes del Administrador de control de servicios (SCM) de Windows, como iniciar, detener o consultar información sobre un servicio.
* Tiene un servicio configurado para iniciarse como *automático*.
* Este servicio se establece para ejecutarse en el contexto de una cuenta de dominio (por oposición a usar una cuenta integrada, como un sistema local).

### <a name="the-code-defect"></a>El defecto en el código

El defecto en el código se debe al intento del Administrador de control de servicios (SCM) y de los servicios de perfil de aplicarse bloqueos mutuamente de forma simultánea. Los bloqueos están para evitar que varios servicios realicen cambios en los mismos datos al mismo tiempo, lo que provocaría daños. Normalmente, varias solicitudes de bloqueo provocarían un problema. Sin embargo, puesto que esto ocurre durante el arranque, ninguno de los servicios puede finalizar sus procesos, ya que se quedan bloqueados esperando al otro.

### <a name="os-bug-5880648---service-control-manager-deadlocks-with-the-delete-user-profiles-on-restart-policy"></a>Error del sistema operativo 5880648: Service Control Manager deadlocks with the "Delete user profiles on restart" policy (El Administrador de control de servicios se interbloquea con la directiva "Eliminar perfiles de usuario al reiniciar")

Hay dos acciones que se superponen de modo que:

* La acción 1 adquiere el bloqueo del perfil, pero aún no ha adquirido el bloqueo de SCM.

  **AND**

* La acción 2 adquiere el bloqueo de SCM, pero aún no ha adquirido el bloqueo del perfil.

Una vez que se produce este interbloqueo, el intento de adquirir el segundo bloqueo necesario bloquea la acción.

### <a name="action-1---old-profile-deletion-notification-has-profile-lock-needs-scm-lock"></a>Acción 1: la notificación de eliminación del perfil anterior (tiene el **bloqueo del perfil**, necesita el **bloqueo de SCM**)

1. En primer lugar, la directiva establecida para eliminar perfiles antiguos adquiere un bloqueo del servicio de perfil interno.

   * Este bloqueo está ahí para evitar que dos subprocesos interactúen con los perfiles mientras la *operación de eliminación* está en curso.

2. La directiva busca los perfiles que son lo suficientemente antiguos como para que se eliminen.
3. Como parte de la eliminación del perfil, un componente que se ha registrado recibir las notificaciones de las eliminaciones de un perfil intenta **iniciar un servicio**.
4. Antes de iniciar el servicio, el Administrador de control de servicios (SCM) debe adquirir un **bloqueo de SCM interno** mantenido por los subprocesos en la **acción 2**.

### <a name="action-2---profile-loadcreation-for-user-specific-data-has-scm-lock-needs-profile-lock"></a>Acción 2: carga o creación del perfil para datos específicos del usuario (tiene el **bloqueo de SCM**, necesita el **bloqueo del perfil**)

1. Al arrancar, SCM necesita ordenar todos los servicios de *inicio automático* por su grupo, así como los servicios de los que dependen estos.

2. **SCM adquiere un bloqueo de SCM interno** que se usa para controlar el acceso al inicio, la detención o la configuración de los servicios a medida que estos se ordenan.

3. Una vez que los servicios están en orden, el SCM recorre en bucle cada servicio y lo inicia.

4. Si el servicio se ejecuta en el contexto de una cuenta de dominio, es necesario cargar o crear un perfil para la cuenta de dominio, de modo que pueda almacenar datos específicos del usuario.

5. Esta solicitud se envía al **servicio de perfil**.

6. El servicio de perfil necesita tener acceso al **bloqueo interno** adquirido en la **acción 1**.

## <a name="solution"></a>Solución

### <a name="process-overview"></a>Información general del proceso

1. Creación de una VM de reparación y acceso a ella
2. Habilitación de la consola serie y recopilación del volcado de memoria
3. Recompilación de la máquina virtual
4. Recopilación de un archivo de volcado de memoria

   > [!NOTE]
   > Este error de arranque indica que el sistema operativo invitado no está operativo. Ahora solucionará el problema en el modo sin conexión.

### <a name="create-and-access-a-repair-vm"></a>Creación de una VM de reparación y acceso a ella

1. Siga los [pasos 1 a 3 de los comandos de reparación de VM](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) para preparar una VM de reparación.
2. Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Habilitación de la consola serie y recopilación del volcado de memoria

Para habilitar la recopilación del volcado de memoria y la consola serie, ejecute el siguiente script:

1. Abra una sesión de símbolo del sistema con privilegios elevados (Ejecutar como administrador).
2. Ejecute los comandos siguientes:

   * Habilitación de la consola serie:

     `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

     `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

3. Verifique que el espacio disponible en el disco del sistema operativo sea igual al tamaño de memoria (RAM) de la VM.

   * Si no hay suficiente espacio en el disco del sistema operativo, debe cambiar la ubicación donde se creará el archivo de volcado de memoria y remitirlo a los discos de datos conectados a la VM que tengan espacio libre suficiente. Para cambiar la ubicación, reemplace `%SystemRoot%` por la letra de unidad (por ejemplo, "F:") del disco de datos en los comandos siguientes.

#### <a name="suggested-configuration-to-enable-os-dump"></a>Configuración sugerida para habilitar el volcado del sistema operativo

**Cargar disco del sistema operativo roto:**

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**Habilitar en ControlSet001:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Habilitar en ControlSet002:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

### <a name="rebuild-the-vm"></a>Recompilación de la máquina virtual

Siga el [paso 5 de los comandos de reparación de la VM](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) para volver a ensamblar la VM.

### <a name="collect-the-memory-dump-file"></a>Recopilación de un archivo de volcado de memoria

Para resolver este problema, primero debe recopilar el archivo de volcado de memoria del bloqueo y ponerse en contacto con el soporte técnico en relación a dicho archivo. Para recopilar el archivo de volcado de memoria, siga estos pasos:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Conecte el disco del sistema operativo a una nueva VM de recuperación

1. Siga los pasos 1 a 3 de los [comandos de reparación de la máquina virtual](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) para preparar una nueva máquina virtual de reparación.

2. Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico

1. En la VM de reparación, vaya a la carpeta de Windows en el disco del sistema operativo conectado. Si la letra de unidad que se asigna al disco del sistema operativo conectado es F, deberá ir a F:\Windows.

2. Busque el archivo memory.dmp y, luego, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) con el archivo de volcado de memoria.

3. Si tiene problemas para encontrar el archivo memory.dmp, puede que quiera usar en su lugar [llamadas de interrupción no enmascarable (NMI) en la consola serie](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows#use-the-serial-console-for-nmi-calls). Puede seguir la guía para [generar un kernel o un archivo de volcado de memoria completo](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump) mediante llamadas NMI.
