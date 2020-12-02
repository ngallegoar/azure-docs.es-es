---
title: Solución de problemas del agente invitado de Linux para Azure
description: Solución de problemas del funcionamiento del agente invitado de Linux para Azure
services: virtual-machines-linux
ms.service: virtual-machines-linux
author: axelg
manager: dcscontentpm
editor: ''
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2020
ms.author: axelg
ms.openlocfilehash: 9e3b376cfaf5379acaf92713c42509471200d066
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95547944"
---
# <a name="troubleshooting-azure-linux-guest-agent"></a>Solución de problemas del agente invitado de Linux para Azure

El [agente invitado de Linux para Azure](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) es un agente de máquina virtual (VM). Permite que la máquina virtual se comunique con el controlador de tejido (el servidor físico subyacente en el que se hospeda la máquina virtual) en la dirección IP 168.63.129.16. Esta dirección IP es una dirección IP pública virtual que facilita la comunicación. Para más información, vea [¿Qué es la dirección IP 168.63.129.16?](../../virtual-network/what-is-ip-address-168-63-129-16.md)

## <a name="checking-agent-status-and-version"></a>Comprobación de la versión y el estado del agente

Consulta https://github.com/Azure/WALinuxAgent/wiki/FAQ#what-does-goal-state-agent-mean-in-waagent---version-output.

## <a name="troubleshooting-vm-agent-that-is-in-not-ready-status"></a>Solución de problemas del agente de máquina virtual con el estado No está listo

### <a name="step-1-check-whether-the-azure-linux-guest-agent-service-is-running"></a>Paso 1: Comprobar si el servicio del agente invitado de Linux para Azure está en ejecución

En función de la distribución, el nombre del servicio puede ser walinuxagent o waagent:

```
root@nam-u18:/home/nam# service walinuxagent status
● walinuxagent.service - Azure Linux Agent
   Loaded: loaded (/lib/systemd/system/walinuxagent.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-10-08 17:10:29 UTC; 3min 9s ago
 Main PID: 1036 (python3)
    Tasks: 4 (limit: 4915)
   CGroup: /system.slice/walinuxagent.service
           ├─1036 /usr/bin/python3 -u /usr/sbin/waagent -daemon
           └─1156 python3 -u bin/WALinuxAgent-2.2.51-py2.7.egg -run-exthandlers

Oct 08 17:10:33 nam-u18 python3[1036]: 2020-10-08T17:10:33.129375Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.OSTCExtensions.VMAccessForLinux-1.5.10, path: /sys/fs/cgroup/memory/sys
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.189020Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Target handler state: enabled [incarnation 2]
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.197932Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] [Enable] current handler state is: enabled
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.212316Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Update settings file: 0.settings
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.224062Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Enable extension [bin/run-command-shim enable]
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.236993Z INFO ExtHandler ExtHandler Started extension in unit 'Microsoft.CPlat.Core.RunCommandLinux_1.0.1_db014406-294a-49ed-b112-c7912a86ae9e
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.263572Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.CPlat.Core.RunCommandLinux-1.0.1, path: /sys/fs/cgroup/cpu,cpuacct/syst
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.280691Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.CPlat.Core.RunCommandLinux-1.0.1, path: /sys/fs/cgroup/memory/system.sl
Oct 08 17:10:37 nam-u18 python3[1036]: 2020-10-08T17:10:37.349090Z INFO ExtHandler ExtHandler ProcessGoalState completed [incarnation 2; 4496 ms]
Oct 08 17:10:37 nam-u18 python3[1036]: 2020-10-08T17:10:37.365590Z INFO ExtHandler ExtHandler [HEARTBEAT] Agent WALinuxAgent-2.2.51 is running as the goal state agent [DEBUG HeartbeatCounter: 1;Heartb
root@nam-u18:/home/nam#
```


Si puede ver los servicios y se están ejecutando, reinicie el servicio para ver si el problema se ha resuelto. Si los servicios se detienen, inícielos y espere unos minutos. Después, compruebe si el **Estado del agente** se muestra como **Listo**. Para solucionar estos problemas, póngase en contacto con [Soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="step-2-check-whether-auto-update-is-enabled"></a>Paso 2: Comprobar si la actualización automática está habilitada

Compruebe esta opción en/etc/waagent.conf:

```
AutoUpdate.Enabled=y
```

Para obtener más información sobre cómo actualizar el agente de Linux para Azure, consulte https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent 
    

### <a name="step-3-check-whether-the-vm-can-connect-to-the-fabric-controller"></a>Paso 3: Comprobar si la máquina virtual se puede conectar al controlador de tejido

Use una herramienta como cURL para probar si la máquina virtual puede conectarse a 168.63.129.16 en los puertos 80, 32526 y 443. Si la máquina virtual no se conecta según lo previsto, compruebe si la comunicación saliente a través de los puertos 80, 443 y 32526 está abierta en el firewall local de la máquina virtual. Si esta dirección IP se bloquea, el agente de máquina virtual puede mostrar un comportamiento inesperado en una variedad de escenarios.

## <a name="advanced-troubleshooting"></a>Pasos detallados para solucionar problemas de conexión a Escritorio remoto a máquinas virtuales Windows en Azure

Los eventos para solucionar problemas del agente invitado de Linux para Azure se registran en los siguientes archivos de registro:

- /var/log/waagent.log


  
### <a name="unable-to-connect-to-wireserver-ip-host-ip"></a>No se puede conectar con la dirección IP de WireServer (IP de host) 

Se observan las siguientes entradas de error en /var/log/waagent.log:

```
2020-10-02T18:11:13.148998Z WARNING ExtHandler ExtHandler An error occurred while retrieving the goal state:
```

**Análisis**

La máquina virtual no puede acceder a la IP de WireServer del servidor de host.

**Solución**

1. Dado que no se puede acceder a la IP de WireServer, conéctese a la máquina virtual mediante SSH y, a continuación, intente acceder a la siguiente dirección URL desde cURL: http://168.63.129.16/?comp=versions. 
1. Compruebe si hay algún problema provocado por un firewall, un proxy u otro origen que pueda estar bloqueando el acceso a la dirección IP 168.63.129.16.
1. Compruebe si Linux IPTables o un firewall de terceros está bloqueando el acceso a los puertos 80, 443 y 32526. Para más información sobre por qué no se debe bloquear esta dirección, vea [¿Qué es la dirección IP 168.63.129.16?](../../virtual-network/what-is-ip-address-168-63-129-16.md)


## <a name="next-steps"></a>Pasos siguientes

Para solucionar el problema "El agente invitado de Microsoft Azure no funciona", [póngase en contacto con Soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
