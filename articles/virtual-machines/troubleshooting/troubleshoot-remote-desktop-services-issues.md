---
title: Los Servicios de escritorio remoto no se inician en una máquina virtual de Azure | Microsoft Docs
description: Obtenga información sobre cómo solucionar problemas con los Servicios de Escritorio remoto al conectarse a una máquina virtual | Microsoft Docs
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
ms.openlocfilehash: 4b314fbdb9cbc0c0b797cbee8e92ee4702bbea81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919471"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Los Servicios de Escritorio remoto no se inician en la máquina virtual de Azure

En este artículo se describe cómo solucionar problemas de conexión a una máquina virtual (VM) de Azure cuando los Servicios de escritorio remoto (TermService) no se inician o devuelven un error.


## <a name="symptoms"></a>Síntomas

Cuando intenta conectarse a una máquina virtual, se encuentra con los siguientes escenarios:

- En la captura de pantalla de la máquina virtual se muestra que el sistema operativo está totalmente cargado y esperando las credenciales.

    ![Captura de pantalla del estado de la máquina virtual](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- Puede ver de forma remota los registros de eventos de la máquina virtual con el Visor de eventos. Verá que los Servicios de escritorio remoto, TermService, no se inician o devuelven un error. A continuación se muestra un registro ejemplo:

    **Nombre de registro**:      Sistema </br>
    **Origen**:        Administrador de control de servicios </br>
    **Fecha**:          16/12/2017 11:19:36 a. m.</br>
    **Id. de evento**:      7022</br>
    **Categoría de tarea**: None</br>
    **Nivel**:         Error</br>
    **Palabras clave**:      Clásico</br>
    **Usuario**:          N/D</br>
    **Equipo**:      vm.contoso.com</br>
    **Descripción**: el servicio Servicios de escritorio remoto no responde al iniciarse. 

    También puede usar la característica Consola de acceso serie para buscar estos errores con la ejecución de la consulta siguiente: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Causa
 
Este problema se produce porque los Servicios de Escritorio remoto no están ejecutándose en la máquina virtual. La causa dependerá de los siguientes escenarios: 

- El servicio TermService se ha establecido en **Deshabilitado**. 
- El servicio TermService se bloquea o no responde. 
- El servicio TermService no se inicia debido a una configuración incorrecta.

## <a name="solution"></a>Solución

Para solucionar este problema, utilice la consola serie. O bien, [repare la máquina virtual sin conexión](#repair-the-vm-offline) mediante la conexión del disco del sistema operativo de la máquina virtual a una máquina virtual de recuperación.

### <a name="use-serial-console"></a>Uso de Serial Console

1. Acceda a [Serial Console](serial-console-windows.md) seleccionando **Soporte técnico y solución de problemas** > **Serial Console**. Si la característica está habilitada en la máquina virtual, puede conectar la máquina virtual correctamente.

2. Creación de un nuevo canal para una instancia CMD. Escriba **CMD** para iniciar el canal y obtener el nombre del canal.

3. Cambie al canal que ejecuta la instancia de CMD. En este caso, debe ser el canal 1:

   ```
   ch -si 1
   ```

4. Presione **Entrar** otra vez y escriba un nombre de usuario y una contraseña válidos y un identificador de dominio o local para la máquina virtual.

5. Consulte el estado del servicio TermService:

   ```
   sc query TermService
   ```

6. Si el estado del servicio se muestra como **Detenido**, intente iniciar el servicio:

    ```
    sc start TermService
     ``` 

7. Consulte el servicio de nuevo para asegurarse de que el servicio se ha iniciado correctamente:

   ```
   sc query TermService
   ```
8. Si no se puede iniciar el servicio, siga la solución basada en el error recibido:

    |  Error |  Sugerencia |
    |---|---|
    |5- ACCESS DENIED |Consulte [Servicio TermService detenido debido a error de acceso denegado](#termservice-service-is-stopped-because-of-an-access-denied-problem). |
    |1053 - ERROR_SERVICE_REQUEST_TIMEOUT  |Consulte [El servicio TermService está deshabilitado](#termservice-service-is-disabled).  |  
    |1058 - ERROR_SERVICE_DISABLED  |Consulte [El servicio TermService se bloquea o deja de responder](#termservice-service-crashes-or-hangs).  |
    |1059 - ERROR_CIRCULAR_DEPENDENCY |[Póngase en contacto con el equipo de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.|
    |1067 - ERROR_PROCESS_ABORTED  |Consulte [El servicio TermService se bloquea o deja de responder](#termservice-service-crashes-or-hangs).  |
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL|[Póngase en contacto con el equipo de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.|
    |1069 - ERROR_SERVICE_LOGON_FAILED  |Consulte [Se produce un error en el servicio TermService debido a un error de inicio de sesión](#termservice-service-fails-because-of-logon-failure) |
    |1070 - ERROR_SERVICE_START_HANG   | Consulte [El servicio TermService se bloquea o deja de responder](#termservice-service-crashes-or-hangs). |
    |1077 - ERROR_SERVICE_NEVER_STARTED   | Consulte [El servicio TermService está deshabilitado](#termservice-service-is-disabled).  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   |[Póngase en contacto con el equipo de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente. |
    |1753   |[Póngase en contacto con el equipo de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.   |
    
#### <a name="termservice-service-is-stopped-because-of-an-access-denied-problem"></a>El servicio TermService está detenido debido a problema de acceso denegado

1. Conéctese a [Serial Console](serial-console-windows.md) y abra una instancia de PowerShell.
2. Descargue la herramienta Monitor de procesos ejecutando el siguiente script:

   ```
   remove-module psreadline  
   $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
   $destination = "c:\temp\ProcessMonitor.zip" 
   $wc = New-Object System.Net.WebClient 
   $wc.DownloadFile($source,$destination) 
   ```

3. Ahora inicie un seguimiento de **procmon**:

   ```
   procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
   ```

4. Reproduzca el problema iniciando el servicio que está produciendo un error de **Acceso denegado**: 

   ```
   sc start TermService 
   ```

   Cuando se produzca un error, finalice el seguimiento de Monitor de procesos:

   ```   
   procmon /Terminate 
   ```

5. Recopile el archivo **c:\temp\ProcMonTrace.PML**:

    1. [Conecte un disco de datos a la máquina virtual](../windows/attach-managed-disk-portal.md
).
    2. Use la consola serie para copiar el archivo a la nueva unidad. Por ejemplo, `copy C:\temp\ProcMonTrace.PML F:\`. En este comando, F es la letra de unidad del disco de datos conectado.
    3. Desconecte la unidad de datos y conéctela en una máquina virtual en funcionamiento que tenga instalado el Monitor de procesos.

6. Abra **ProcMonTrace.PML** con el Monitor de procesos de la máquina virtual en funcionamiento. A continuación, filtre por **El resultado es Acceso denegado**, tal y como se muestra en la captura de pantalla siguiente:

    ![Filtro por resultado en el Monitor de procesos](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Corrija las claves del registro, carpetas o archivos que están en la salida. Normalmente, este problema se produce cuando la cuenta de inicio de sesión usada en el servicio no tiene permisos de ACL para acceder a estos objetos. Para conocer los permisos de ACL correctos para la cuenta de inicio de sesión, puede comprobarlo una máquina virtual correcta. 

#### <a name="termservice-service-is-disabled"></a>El servicio TermService está deshabilitado

1. Restaure el servicio a su valor de inicio predeterminado:

   ```
   sc config TermService start= demand 
   ```

2. Inicie el servicio:

   ```
   sc start TermService
   ```

3. Consulte su estado de nuevo para asegurarse de que el servicio se está ejecutando:

   ```
   sc query TermService 
   ```

4. Trate de conectarse a la máquina virtual mediante Escritorio remoto.

#### <a name="termservice-service-fails-because-of-logon-failure"></a>Se produce un error en el servicio TermService debido a un error de inicio de sesión

1. Este problema se produce si se ha cambiado la cuenta de inicio de este servicio. Cámbiela a su valor predeterminado: 

        sc config TermService obj= 'NT Authority\NetworkService'
2. Inicie el servicio:

        sc start TermService
3. Trate de conectarse a la máquina virtual mediante Escritorio remoto.

#### <a name="termservice-service-crashes-or-hangs"></a>El servicio TermService se bloquea o deja de responder
1. Si el estado del servicio está atascado en **Iniciando** o **Deteniendo**, intente detener el servicio: 

        sc stop TermService
2. Aisle el servicio en su propio contenedor "svchost":

        sc config TermService type= own
3. Inicie el servicio:

        sc start TermService
4. Si el servicio todavía no se inicia, [póngase en contacto con el departamento de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="repair-the-vm-offline"></a>Reparación de la máquina virtual sin conexión

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Conecte el disco del sistema operativo a una máquina virtual de recuperación.

1. [Conecte el disco del sistema operativo a una máquina virtual de recuperación](../windows/troubleshoot-recovery-disks-portal.md).
2. Inicie una conexión mediante el Escritorio remoto a la máquina virtual de recuperación. Asegúrese de que el disco asociado aparece marcado como **En línea** en la consola de Administración de discos. Anote la letra de unidad que está asignada al disco del sistema operativo conectado.
3. Abra una instancia del símbolo del sistema con privilegios elevados (**Ejecutar como administrador**). A continuación, ejecute el siguiente script. Se supone que la letra de unidad que está asignada al disco del sistema operativo conectado es **F**. Reemplácela por el valor adecuado en la máquina virtual. 

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
   REM Set default values back on the broken service 
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
   ```

4. [Desconecte el disco del sistema operativo y vuelva a crear la máquina virtual](../windows/troubleshoot-recovery-disks-portal.md). A continuación, compruebe si se ha resuelto el problema.

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema.
