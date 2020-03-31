---
title: Opciones de energía de la consola serie de Azure | Microsoft Docs
description: Opciones de energía de máquina virtual disponibles en la consola serie de Azure
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/6/2019
ms.author: alsin
ms.openlocfilehash: 11c2549d7282bae5654ede1ac34d63a43cd0f059
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451197"
---
# <a name="power-options-available-from-the-azure-serial-console"></a>Opciones de energía disponibles en la consola serie de Azure

La consola serie de Azure proporciona varias herramientas eficaces para la administración de energía en la máquina virtual o en el conjunto de escalado de máquinas virtuales. Estas opciones de administración de energía pueden resultar confusas, por lo que aquí se ofrece información general sobre cada herramienta y su caso de uso previsto.

Característica de la consola serie | Descripción | Caso de uso
:----------------------|:------------|:---------
Reinicio de la máquina virtual | Un reinicio estable de la máquina virtual o la instancia de conjunto de escalado de máquinas virtuales. Esta operación es igual que llamar a la característica de reinicio disponible en la página de información general. | En la mayoría de los casos, esta opción debe ser la primera herramienta para intentar reiniciar la máquina virtual. La conexión a la consola serie experimentará una breve interrupción y se reanudará automáticamente en cuanto se reinicie la máquina virtual.
Restablecimiento de la máquina virtual | Un ciclo de energía forzado de la máquina virtual o del conjunto de escalado de máquinas virtuales por parte de la plataforma Azure. | Esta opción se usa para reiniciar inmediatamente el sistema operativo, independientemente de su estado actual. Dado que esta operación no es estable, existe el riesgo de que se pierdan los datos o se dañen. No hay interrupción en la conexión de la consola serie, lo que puede ser útil para enviar comandos al principio del arranque (por ejemplo, llegar a GRUB en una máquina virtual Linux o en modo seguro en una máquina virtual Windows).
SysRq - Reboot (b) | Una solicitud del sistema para forzar un reinicio de invitado. | Esta característica solo se aplica a los sistemas operativos Linux y requiere que [SysRq esté habilitado](./serial-console-nmi-sysrq.md#system-request-sysrq) en el sistema operativo. Si el sistema operativo está configurado correctamente para SysRq, este comando hará que se reinicie el sistema operativo.
NMI (Interrupción no enmascarable) | Un comando de interrupción, que se entregará al sistema operativo | Esta operación está disponible para máquinas virtuales [Windows](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) y [Linux](./serial-console-nmi-sysrq.md#non-maskable-interrupt-nmi), y requiere que NMI esté habilitada. El envío de NMI normalmente hará que el sistema operativo se bloquee. Puede configurar el sistema operativo para crear un archivo de volcado de memoria y reiniciarlo después de recibir NMI, lo que puede resultar útil en la depuración de bajo nivel.

## <a name="next-steps"></a>Pasos siguientes
* Más información sobre la [consola serie de Azure para máquinas virtuales Linux](./serial-console-linux.md)
* Más información sobre la [consola serie de Azure para máquinas virtuales Windows](./serial-console-windows.md)