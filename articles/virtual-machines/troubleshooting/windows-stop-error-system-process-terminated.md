---
title: 'Error de detención de Windows: proceso del sistema terminado'
description: Este artículo proporciona los pasos para resolver problemas en los que el sistema operativo encuentra el error de detención 0xC000021A, que impide el arranque de una máquina virtual de Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 037d0858-4611-4707-bd46-cc8085d011ed
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/21/2020
ms.author: v-mibufo
ms.openlocfilehash: b07033f96402edc24edd51de57661603e57472bc
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91340730"
---
# <a name="windows-stop-error---0xc000021a-status-system-process-terminated"></a>Error de detención Windows 0xC000021A: proceso del sistema de estado finalizado

Este artículo proporciona los pasos para resolver los problemas en los que el sistema operativo encuentra el error de detención 0xC000021A, que impide el arranque de una máquina virtual de Azure.

## <a name="symptom"></a>Síntoma

Al usar [diagnósticos de arranque](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) para ver la captura de pantalla de la máquina virtual, la captura muestra un mensaje que indica que el sistema operativo encontró un error durante el arranque y dice lo siguiente:

**Se produjo un problema con su equipo y se debe reiniciar. Solo vamos a recopilar alguna información de error y, a continuación, podrá reiniciar. (# #% completo) Si desea obtener más información, busque en línea este error más adelante: 0xC000021a**.

  ![La figura 1 muestra el código de error #0xC000021A con el mensaje "Se produjo un problema con su equipo y se debe reiniciar. Solo vamos a recopilar alguna información de error y, a continuación, podrá reiniciar".](./media/windows-stop-error-system-process-terminated/1-pc-problem-restart.png)

## <a name="cause"></a>Causa

El error 0xC000021A indica **STATUS_SYSTEM_PROCESS_TERMINATED**.

Este error se produce cuando un proceso crítico, como WinLogon (winlogon.exe) o el subsistema cliente-servidor en tiempo de ejecución (csrss.exe), genera error. Una vez que el kernel detecta que uno de esos servicios se ha detenido, genera el error **STOP 0xC000021A**. Este error puede deberse a varias causas, incluidas las siguientes:

- Se han instalado archivos del sistema no coincidentes.
- Se ha producido un error en la instalación de una actualización de Service Pack o de KB.
- Un programa de copia de seguridad que se ha usado para restaurar un disco duro no ha restaurado correctamente los archivos que estaban en uso.
- Se ha instalado un programa de terceros que no es compatible.

## <a name="solution"></a>Solución

### <a name="collect-the-memory-dump-file"></a>Recopilación de un archivo de volcado de memoria

Para resolver este problema, se debe analizar el volcado de memoria. Recopile el archivo de volcado de memoria para el bloqueo y póngase en contacto con el servicio de soporte técnico. Para recopilar el archivo de volcado de memoria, siga estos pasos:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Conecte el disco del sistema operativo a una nueva VM de recuperación

1.  Siga los pasos 1 a 3 de los [comandos de reparación de VM](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) para preparar una VM de reparación.
2.  Use **Conexión a Escritorio remoto** para conectarse a la máquina virtual de reparación.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico

1.  En la VM de reparación, vaya a la carpeta de Windows en el disco del sistema operativo conectado. Si la letra de unidad que se asigna al disco del sistema operativo conectado es F, vaya a F:\Windows.
2.  Busque el archivo memory.dmp y, luego, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) con el archivo de volcado de memoria.
3.  Si tiene problemas para encontrar el archivo memory.dmp, puede que quiera usar en su lugar [llamadas de interrupción no enmascarable (NMI) en la consola serie](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows#use-the-serial-console-for-nmi-calls). Puede seguir la guía para [generar aquí un archivo de volcado de memoria completo mediante llamadas NMI](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre la solución de problemas, consulte [Solución de errores de arranque en máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-error-troubleshoot) o [Solución de problemas de una máquina virtual Windows mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-recovery-disks-windows). También debe familiarizarse con el [uso de diagnósticos de arranque para solucionar problemas de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics).
- Para más información sobre el uso de Resource Manager, consulte [Información general de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview).
- Si no puede conectarse a la máquina virtual, consulte [Solución de problemas de conexiones del Escritorio remoto a una máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-rdp-connection).
