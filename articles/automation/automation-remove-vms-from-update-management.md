---
title: Quitar máquinas virtuales de Azure Automation Update Management
description: En este artículo se indica cómo quitar una máquina virtual desde Update Management.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 26a38c0851643fbd6446acddd99a05abf8d5b26c
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836658"
---
# <a name="remove-vms-from-update-management"></a>Eliminación de una máquina virtual desde Update Management

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

Cuando haya terminado de implementar las actualizaciones en las máquinas virtuales de su entorno, puede quitarlas de la característica [Update Management](automation-update-management.md).

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

2. Use el siguiente comando para identificar el UUID de una máquina virtual que desea quitar de la administración.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. En el área de trabajo de Log Analytics en **general**, acceda a las búsquedas guardadas.

4. Para la búsqueda guardada `MicrosoftDefaultComputerGroup`, haga clic en la elipse a la derecha y seleccione **Editar**. 

5. Quite el UUID de la máquina virtual.

6. Repita los pasos para las demás máquinas virtuales que se van a quitar.

7. Guarde la búsqueda guardada cuando haya terminado de modificarla. 

## <a name="next-steps"></a>Pasos siguientes

* Para seguir trabajando con Update Management, vea [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
* Para resolver problemas de características generales, vea [Solución de problemas de Update Management](troubleshoot/update-management.md).
* Para problemas con el agente de Windows Update, vea [Solución de problemas del agente de actualización de Windows](troubleshoot/update-agent-issues.md).
* Para problemas con el agente de actualización de Linux, vea [Solución de problemas del agente de actualización de Linux](troubleshoot/update-agent-issues-linux.md).