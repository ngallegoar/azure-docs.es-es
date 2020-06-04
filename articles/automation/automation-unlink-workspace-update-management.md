---
title: Desvinculación de un área de trabajo de una cuenta de Automation para Update Management
description: En este artículo se describe cómo desvincular un área de trabajo de Log Analytics de la cuenta de Automation para Update Management.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 9129d10071a4c8da0376cbad3d64c10cbaceb8b9
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835791"
---
# <a name="unlink-workspace-from-automation-account-for-update-management"></a>Desvinculación de un área de trabajo de una cuenta de Automation para Update Management

Puede decidir no integrar la cuenta de Automation con un área de trabajo Log Analytics durante las operaciones de [Update Management](automation-update-management.md). En este artículo se explica cómo desvincular el área de trabajo de la cuenta.

1. Inicie sesión en Azure en https://portal.azure.com.

2. Elimine Update Management para las máquinas virtuales. Consulte [Eliminación de máquinas virtuales de Update Management](automation-remove-vms-from-update-management.md).

3. Si Update Management incluye versiones anteriores de la supervisión SQL de Azure, es posible que el programa de instalación de la característica haya creado recursos de Automation que se deberían eliminar. En el caso de Update Management, es posible que desee eliminar los siguientes elementos que ya no son necesarios:

   * Programaciones de actualizaciones: cada una tiene un nombre que coincide con la implementación de actualizaciones que ha creado.
   * Grupos de Hybrid Worker que se han creado para Update Management: cada uno de ellos tiene un nombre similar a machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8.

4. Abra la cuenta de Automation y seleccione **Área de trabajo vinculada** en **Recursos relacionados** a la izquierda.

5. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo** y responda a los mensajes.

   ![Página Desvincular área de trabajo](media/automation-unlink-workspace-update-management/automation-unlink-workspace-blade.png).

6. Mientras Azure Automation trata de desvincular el área de trabajo de Log Analytics, puede realizar el seguimiento del progreso en la opción **Notificaciones** del menú.

También puede desvincular el área de trabajo de Log Analytics de la cuenta de Automation desde el área de trabajo:

1. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados**. 
2. En la página Cuenta de Automation, seleccione **Desvincular cuenta**.

## <a name="next-steps"></a>Pasos siguientes

* Para trabajar con la característica, consulte [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
* Para solucionar errores de la característica, consulte [Solución de problemas de Update Management](troubleshoot/update-management.md).
* Para solucionar errores del agente de Windows Update, consulte [Solución de problemas del agente de Windows Update](troubleshoot/update-agent-issues.md).
* Para solucionar errores del agente de actualización de Linux, consulte [Solución de problemas del agente de actualización de Linux](troubleshoot/update-agent-issues-linux.md).
