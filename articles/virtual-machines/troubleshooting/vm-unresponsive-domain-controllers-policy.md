---
title: La máquina virtual no responde cuando se aplica una directiva de controladores de dominio predeterminada
titlesuffix: Azure Virtual Machines
description: En este artículo se proporcionan los pasos para resolver problemas en los que la directiva de controladores de dominio predeterminados impiden el arranque de una VM de Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 34e6b765-3496-46a1-b7d7-6def00884394
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/08/2020
ms.author: v-miegge
ms.openlocfilehash: 4f181a6dee7ed182150bd0cad2b51690b6c77d7a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91963132"
---
# <a name="vm-is-unresponsive-while-applying-default-domain-controllers-policy"></a>La máquina virtual no responde cuando se aplica una directiva de controladores de dominio predeterminada

En este artículo se proporcionan los pasos para resolver problemas en los que la directiva predeterminada de controladores de dominio impiden el arranque de una máquina virtual (VM) de Azure.

## <a name="symptom"></a>Síntoma

Cuando use los [diagnósticos de arranque](./boot-diagnostics.md) para ver la captura de pantalla de la máquina virtual, verá que dicha captura de pantalla muestra cómo el sistema operativo deja de responder mientras arranca con el mensaje **Directiva predeterminada de controladores de dominio**.

  ![La imagen 1 muestra al sistema operativo bloqueado con el mensaje "Directiva predeterminada de controladores de dominio".](./media/vm-unresponsive-domain-controllers-policy/1-default-domain-controllers-policy.png)

## <a name="cause"></a>Causa

Este problema puede deberse a los cambios que se realizaron recientemente en la directiva predeterminada de controladores de dominio. De lo contrario, deberá realizar un análisis de volcado de memoria para determinar la causa principal.

## <a name="solution"></a>Solución

Si ha realizado cambios recientemente en la directiva predeterminada de controladores de dominio, quizá quiera deshacerlos para solucionar el problema. Si no está seguro de qué está causando el problema, recopile un volcado de memoria y envíe una incidencia de soporte técnico.

### <a name="collect-the-memory-dump-file"></a>Recopilación de un archivo de volcado de memoria

Para resolver este problema, primero debería recopilar el archivo de volcado de memoria del bloqueo y ponerse en contacto con el soporte técnico en relación a ese archivo. Para recopilar el archivo de volcado de memoria, siga estos pasos:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Conecte el disco del sistema operativo a una nueva VM de recuperación

1. Siga los pasos 1 a 3 de los [comandos de reparación de VM](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) para preparar una VM de reparación.

1. Use Conexión a Escritorio remoto para conectarse a la VM de reparación.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Busque el archivo de volcado de memoria y envíe una incidencia de soporte técnico

1. En la máquina virtual de reparación, vaya a la carpeta Windows en el disco de sistema operativo conectado. Si la letra de unidad que se asigna al disco del sistema operativo conectado es `F`, debería ir a `F:\Windows`.

1. Busque el archivo memory.dmp y, luego, [envíe una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) con el archivo de volcado de memoria.

1. Si tiene problemas para encontrar el archivo memory.dmp, puede que quiera usar en su lugar [llamadas de interrupción no enmascarable (NMI) en la consola serie](./serial-console-windows.md#use-the-serial-console-for-nmi-calls). Siga la guía para [generar un archivo de volcado de memoria completo mediante llamadas NMI](/windows/client-management/generate-kernel-or-complete-crash-dump).