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
ms.openlocfilehash: 1ce594d9e3ffddf781c61717ae4534f0c7bd40f8
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94681897"
---
# <a name="windows-stop-error---0x0000007e-system-thread-exception-not-handled"></a>Error de detención de Windows: excepción de subproceso del sistema no controlada 0x0000007E

En este artículo se proporcionan los pasos para resolver los problemas en los que el sistema operativo invitado (SO invitado) detecta un problema e intenta reiniciar la máquina virtual (VM) de Azure. El mensaje de error que se muestra indica "A system thread exception wasn’t handled" (No se ha controlado una excepción de subproceso del sistema).

## <a name="symptoms"></a>Síntomas

Cuando use los [diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la salida de la máquina virtual, verá que Windows necesita reiniciarse con el código de detención "SYSTEM THREAD EXCEPTION NOT HANDLED" (Excepción de subproceso del sistema no controlada) o el código de error "0x0000007E".

![Captura de pantalla que muestra a Windows bloqueado durante el arranque con un mensaje de error "Se ha producido un problema en su PC y necesita reiniciarse. Se reiniciará automáticamente". y un código de detención "SYSTEM THREAD EXCEPTION NOT HANDLED" (Excepción de subproceso del sistema no controlada).](media/windows-stop-error-system-thread-exception-not-handled/windows-stop-error-system-thread-exception-not-handled-1.png)

## <a name="cause"></a>Causa

La causa no se puede determinar hasta que se analiza un archivo de volcado de memoria. Continúe con la recopilación del archivo de volcado de memoria.

## <a name="solution"></a>Solución

Para resolver este problema, primero tiene que recopilar el archivo de volcado de memoria del bloqueo y enviarlo al soporte técnico de Microsoft. Para recopilar el archivo de volcado, siga las instrucciones de las dos secciones siguientes.

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Conexión del disco del sistema operativo a una nueva VM de reparación

1. Para preparar una máquina virtual de reparación, siga los pasos 1 a 3 de los [comandos de reparación de máquinas virtuales](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md).
1. Conéctese a la máquina virtual de reparación mediante la conexión a Escritorio remoto.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico

1. En la máquina virtual de reparación, vaya a la carpeta Windows en el disco de sistema operativo conectado. Por ejemplo, si la letra de unidad asignada al disco del sistema operativo conectado tiene la etiqueta *F*, vaya a `F:\Windows`.
1. Busque el archivo *memory.dmp* y, luego, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) y adjunte el archivo de volcado de memoria.
1. Si tiene problemas para encontrar el archivo *memory.dmp*, siga la guía para [generar un archivo de volcado de memoria con llamadas de interrupción no enmascarable (NMI)](/windows/client-management/generate-kernel-or-complete-crash-dump).

Para más información sobre las llamadas a NMI, consulte la guía del usuario de las [llamadas NMI en la consola serie de Azure](./serial-console-windows.md#use-the-serial-console-for-nmi-calls).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Solución de problemas de arranque en máquinas virtuales de Azure](./boot-error-troubleshoot.md)
