---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 00b0c1b1a40ad16db177916c57dba6e9d5a187a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "67186843"
---
Siga los pasos para sus circunstancias específicas.

### <a name="unregister-a-connected-process-server"></a>Anulación del registro de un servidor de procesos conectado

1. Establezca una conexión remota al servidor de procesos como administrador.
2. En el **Panel de Control**, abra **Programas > Desinstalar un programa**.
3. Desinstale el programa **Microsoft Azure Site Recovery Mobility Service/Servidor de destino maestro**.
4. Desinstale el programa **Configuración de Microsoft Azure Site Recovery/Servidor de procesos**.
5. Después de desinstalan los programas en los pasos 3 y 4, desinstale **Configuración de Microsoft Azure Site Recovery/Dependencias del servidor de procesos**.

### <a name="unregister-a-disconnected-process-server"></a>Anulación del registro de un servidor de procesos desconectado

Use solamente estos pasos si no hay ninguna manera de reactivar la máquina donde está instalado el servidor de procesos.

1. Inicie sesión como administrador en el servidor de configuración.
2. Abra un símbolo del sistema administrativo y vaya a `%ProgramData%\ASR\home\svsystems\bin`.
3. Ejecute este comando para obtener una lista de uno o más servidores de procesos.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
    - S. No: el número de serie del servidor de procesos.
    - IP/Nombre: la dirección IP y el nombre de la máquina que ejecuta el servidor de procesos.
    - Heartbeat: último latido de la máquina del servidor de procesos.
    ![Unregister-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)

4. Especifique el número de serie del servidor de procesos cuyo registro quiera anular.
5. Al anular el registro de un servidor de procesos, se quitan todos sus detalles del sistema y se muestra el mensaje: **Se eliminó a <nombre del servidor> del registro correctamente (dirección IP del servidor)**

