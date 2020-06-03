---
title: Quitar máquinas virtuales de Azure Automation Change Tracking e inventario
description: En este artículo se explica cómo quitar las máquinas virtuales de Change Tracking e inventario.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 22cb49c414e21e5c47330f2c67fc2cf30e3364b2
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836675"
---
# <a name="remove-vms-from-change-tracking-and-inventory"></a>Eliminación de máquinas virtuales de Change Tracking e Inventario

Cuando haya terminado de implementar los cambios en las máquinas virtuales de su entorno, puede quitarlos de la característica [Change Tracking e inventario](change-tracking.md).

1. Desde la cuenta de Automation, seleccione **Change Tracking** o **Inventario** en **Administración de configuración**.

2. Use el siguiente comando para identificar el UUID de una máquina virtual que desea quitar de la administración.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. En el área de trabajo de Log Analytics en **General**, acceda a las búsquedas guardadas.

4. Para la búsqueda guardada `MicrosoftDefaultComputerGroup`, haga clic en la elipse a la derecha y seleccione **Editar**. 

5. Quite el UUID de la máquina virtual.

6. Repita los pasos para las demás máquinas virtuales que se van a quitar.

7. Guarde la búsqueda guardada cuando haya terminado de modificarla. 

## <a name="next-steps"></a>Pasos siguientes

* Para seguir trabajando con Change Tracking e Inventario, vea [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para resolver problemas generales de la característica, vea [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).