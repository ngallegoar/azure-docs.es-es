---
title: Desvinculación de un área de trabajo de una cuenta de Automation para Change Tracking e Inventario
description: En este artículo se describe cómo desvincular un área de trabajo de Log Analytics de la cuenta de Automation para Change Tracking e Inventario.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 2be702ec6e820fe71dd8d2da7aa4cf831b52402e
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83828260"
---
# <a name="unlink-workspace-from-automation-account"></a>Desvinculación de un área de trabajo de una cuenta de Automation

Puede decidir no integrar la cuenta de Automation con un área de trabajo Log Analytics al habilitar las operaciones [Change Tracking e Inventario](change-tracking.md). En este artículo se explica cómo desvincular el área de trabajo de la cuenta.

1. Inicie sesión en Azure en https://portal.azure.com.

2. Eliminación de Update Management para las máquinas virtuales. Vea [Eliminación de máquinas virtuales de Change Tracking e Inventario](automation-remove-vms-from-change-tracking.md).

3. Si Change Tracking e Inventario incluye versiones anteriores de la supervisión SQL de Azure, es posible que el programa de instalación de la característica haya creado recursos de Automation que se deberían quitar. Busque estos recursos y quítelos.

4. Abra la cuenta de Automation y seleccione **Área de trabajo vinculada** en **Recursos relacionados** a la izquierda.

5. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo** y responda a los mensajes.

   ![Página Desvincular área de trabajo](media/automation-unlink-workspace-change-tracking/automation-unlink-workspace-blade.png).

6. Mientras Azure Automation trata de desvincular el área de trabajo de Log Analytics, puede realizar el seguimiento del progreso en la opción **Notificaciones** del menú.

También puede desvincular el área de trabajo de Log Analytics de la cuenta de Automation desde el área de trabajo:

1. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados**. 
2. En la página Cuenta de Automation, seleccione **Desvincular cuenta**.

## <a name="next-steps"></a>Pasos siguientes

* Para trabajar con Change Tracking e Inventario, vea [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para solucionar problemas generales de la característica, vea [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).
