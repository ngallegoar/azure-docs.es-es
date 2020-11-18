---
title: La máquina virtual no responde mientras espera al administrador de sesión local.
description: En este artículo se proporcionan los pasos para resolver problemas en los que el sistema operativo invitado está atascado a la espera que el administrador de sesión local termine el procesamiento mientras se inicia una máquina virtual de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/22/2020
ms.author: v-mibufo
ms.openlocfilehash: ae3d35bea1c75c797447100316ceab9a27fcbe48
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423801"
---
# <a name="vm-is-unresponsive-while-waiting-for-the-local-session-manager"></a>La máquina virtual no responde mientras espera al administrador de sesión local.

En este artículo se proporcionan los pasos para resolver problemas en los que el sistema operativo invitado está atascado a la espera que el administrador de sesión local termine el procesamiento mientras se inicia una máquina virtual de Azure.

## <a name="symptoms"></a>Síntomas

Cuando use los [diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la máquina virtual, verá que esta captura de pantalla muestra el mensaje siguiente: "*Please wait for the Local Session Manager*" (Espere al administrador de sesión local)

![La captura de pantalla muestra el sistema operativo invitado atascado con el mensaje "Please wait for the Local Session Manager" (Espere al administrador de sesión local) en Windows Server 2012 R2.](media/vm-unresponsive-wait-local-session-manager/vm-unresponsive-wait-local-session-manager-1.png)

## <a name="cause"></a>Causa

Podría haber varios motivos por los que una máquina virtual está atascada en espera del administrador de sesión local. Si espera que el administrador de sesión local sea un problema persistente, deberá recopilar un volcado de memoria para su análisis.

## <a name="solution"></a>Solución

En algunos casos, basta con esperar el tiempo suficiente para que se complete el proceso. Si la máquina virtual se bloquea en la pantalla de espera durante más de una hora, debe recopilar un volcado de memoria y, después, ponerse en contacto con el soporte técnico de Microsoft.

### <a name="collect-the-memory-dump-file"></a>Recopilación de un archivo de volcado de memoria

Para resolver este problema, primero tiene que recopilar el archivo de volcado de memoria del bloqueo y ponerse en contacto con el soporte técnico en relación a ese archivo. Para recopilar el archivo de volcado de memoria, siga estos pasos:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Conecte el disco del sistema operativo a una nueva VM de recuperación

1. Para preparar una máquina virtual de reparación, siga los [pasos 1 a 3 de los comandos de reparación de máquinas virtuales](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md).
2. Conéctese a la máquina virtual de reparación mediante Conexión a Escritorio remoto.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico

1. En la máquina virtual de reparación, vaya a la carpeta Windows en el disco de sistema operativo conectado. Si la letra de unidad que se asigna al disco del sistema operativo conectado es *F*, deberá ir a `F:\Windows`.
2. Busque el archivo **memory.dmp** y, a continuación, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) con el archivo de volcado de memoria.
3. Si tiene problemas para encontrar el archivo **memory.dmp**, siga la guía para [generar un archivo de volcado de memoria con llamadas de interrupción no enmascarable (NMI)](/windows/client-management/generate-kernel-or-complete-crash-dump).

Para más información sobre las llamadas a NMI, consulte la guía de usuario de las [llamadas de interrupción no enmascarable (NMI) en la consola en serie](./serial-console-windows.md#use-the-serial-console-for-nmi-calls).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Solución de problemas de arranque en máquinas virtuales de Azure](boot-error-troubleshoot.md)