---
title: 'Error de detención de Windows: excepción de subproceso del sistema no controlada 0x0000007E'
description: En este artículo se proporcionan los pasos para resolver los problemas en los que el sistema operativo invitado detecta un problema y desea reiniciar la máquina virtual de Azure. El mensaje indicará que no se controló una excepción de subproceso del sistema.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 11/04/2020
ms.author: v-mibufo
ms.openlocfilehash: dd18d69009b9c150c4c12a755e9060cd5dfaccae
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423797"
---
# <a name="windows-stop-error---0x0000007e-system-thread-exception-not-handled"></a>Error de detención de Windows: excepción de subproceso del sistema no controlada 0x0000007E

En este artículo se proporcionan los pasos para resolver los problemas en los que el sistema operativo invitado detecta un problema y desea reiniciar la máquina virtual de Azure. El mensaje indicará que no se controló una excepción de subproceso del sistema.

## <a name="symptoms"></a>Síntomas

Cuando use los [diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la máquina virtual, verá que en la captura de pantalla se muestran las ventanas que necesitan reiniciarse con el código de detención **SYSTEM THREAD EXCEPTION NOT HANDLED** (Excepción de subproceso de sistema no controlada) o el código de error **0x0000007E**.

![La captura de pantalla muestra Windows atascado durante el arranque con el mensaje: "Se produjo un problema con su equipo y se debe reiniciar. Se reiniciará automáticamente". Código de detención: SYSTEM THREAD EXCEPTION NOT HANDLED (Excepción de subproceso del sistema no controlada)](media/windows-stop-error-system-thread-exception-not-handled/windows-stop-error-system-thread-exception-not-handled-1.png)

## <a name="cause"></a>Causa

La causa no se puede determinar hasta que se analiza un archivo de volcado de memoria. Continúe con la recopilación del archivo de volcado de memoria.

## <a name="solution"></a>Solución

### <a name="collect-the-memory-dump-file"></a>Recopilación de un archivo de volcado de memoria

Para resolver este problema, primero tiene que recopilar el archivo de volcado de memoria del bloqueo y ponerse en contacto con el soporte técnico en relación a ese archivo. Para recopilar el archivo de volcado de memoria, siga estos pasos:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Conecte el disco del sistema operativo a una nueva VM de recuperación

1. Para preparar una máquina virtual de reparación, siga los [pasos 1 a 3 de los comandos de reparación de máquinas virtuales](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md).
2. Conéctese a la máquina virtual de reparación mediante Conexión a Escritorio remoto.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico

1. En la máquina virtual de reparación, vaya a la carpeta Windows en el disco de sistema operativo conectado. Si la letra de unidad que se asigna al disco del sistema operativo conectado es *F*, deberá ir a `F:\Windows`.
2. Busque el archivo **memory.dmp** y, a continuación, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) con el archivo de volcado de memoria.
3. Si tiene problemas para encontrar el archivo **memory.dmp**, siga la guía para [generar un archivo de volcado de memoria con llamadas de interrupción no enmascarable (NMI)](/windows/client-management/generate-kernel-or-complete-crash-dump).

Para más información sobre las llamadas a NMI, consulte la guía de usuario de las [llamadas de interrupción no enmascarable (NMI) en la consola serie](./serial-console-windows.md#use-the-serial-console-for-nmi-calls).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Solución de problemas de arranque en máquinas virtuales de Azure](./boot-error-troubleshoot.md)