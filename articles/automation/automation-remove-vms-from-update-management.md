---
title: Quitar máquinas virtuales de Azure Automation Update Management
description: En este artículo se explica cómo quitar máquinas administradas con Update Management.
services: automation
ms.topic: conceptual
ms.date: 06/30/2020
ms.custom: mvc
ms.openlocfilehash: 9745ddea1035f239a9ca65a073fb698a8f42c01f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610054"
---
# <a name="remove-vms-from-update-management"></a>Eliminación de una máquina virtual desde Update Management

Cuando haya terminado de implementar las actualizaciones en las máquinas virtuales de su entorno, puede quitarlas de la característica [Update Management](automation-update-management.md).

## <a name="to-remove-your-vms"></a>Para quitar las máquinas virtuales

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

2. Use el siguiente comando para identificar el UUID de un equipo que desea quitar de la administración.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. En el área de trabajo de Log Analytics, debajo de **General**, acceda a las búsquedas guardadas para la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates`.

4. Para la búsqueda guardada `MicrosoftDefaultComputerGroup`, haga clic en la elipse a la derecha y seleccione **Editar**.

5. Quite el UUID de la máquina virtual.

6. Repita los pasos para las demás máquinas virtuales que se van a quitar.

7. Guarde la búsqueda guardada cuando haya terminado de modificarla.

>[!NOTE]
>Las máquinas todavía se muestran después de anular la inscripción, ya que se informa de todas las máquinas que se han evaluado en las últimas 24 horas. Después de desconectar la máquina, debe esperar 24 horas antes de que ya no aparezcan en la lista.

## <a name="next-steps"></a>Pasos siguientes

* Para seguir trabajando con Update Management, vea [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
* Para resolver problemas de características generales, vea [Solución de problemas de Update Management](troubleshoot/update-management.md).
* Para problemas con el agente de Windows Update, vea [Solución de problemas del agente de actualización de Windows](troubleshoot/update-agent-issues.md).
* Para problemas con el agente de actualización de Linux, vea [Solución de problemas del agente de actualización de Linux](troubleshoot/update-agent-issues-linux.md).