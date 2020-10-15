---
title: Quitar máquinas virtuales de Azure Automation Change Tracking e inventario
description: En este artículo se explica cómo quitar las máquinas virtuales de Change Tracking e inventario.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 8e9b80c60c098e4daf247db07fa48baa322aa7f0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84169461"
---
# <a name="remove-vms-from-change-tracking-and-inventory"></a>Eliminación de máquinas virtuales de Change Tracking e Inventario

Cuando haya terminado de implementar los cambios en las máquinas virtuales de su entorno, puede quitarlos de la característica [Change Tracking e inventario](change-tracking.md).

## <a name="to-remove-your-vms"></a>Para quitar las máquinas virtuales

1. Desde la cuenta de Automation, seleccione **Change Tracking** o **Inventario** en **Administración de configuración**.

2. Use el siguiente comando para identificar el UUID de una máquina virtual que desea quitar de la administración.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. En el área de trabajo de Log Analytics, debajo de **General**, acceda a las búsquedas guardadas para la configuración de ámbito `MicrosoftDefaultScopeConfig-ChangeTracking`.

4. Para la búsqueda guardada `MicrosoftDefaultComputerGroup`, haga clic en la elipse a la derecha y seleccione **Editar**. 

5. Quite el UUID de la máquina virtual.

6. Repita los pasos para las demás máquinas virtuales que se van a quitar.

7. Guarde la búsqueda guardada cuando haya terminado de modificarla. 

## <a name="next-steps"></a>Pasos siguientes

* Para seguir trabajando con Change Tracking e Inventario, vea [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para resolver problemas generales de la característica, vea [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).