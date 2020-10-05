---
title: Quitar máquinas virtuales de Azure Automation Update Management
description: En este artículo se explica cómo quitar máquinas administradas con Update Management.
services: automation
ms.topic: conceptual
ms.date: 09/09/2020
ms.custom: mvc
ms.openlocfilehash: 66631adbb56a98431e70f956f3e860b16e8f7ea2
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89648641"
---
# <a name="remove-vms-from-update-management"></a>Eliminación de una máquina virtual desde Update Management

Cuando haya terminado de administrar las actualizaciones en las máquinas virtuales de su entorno, puede dejar de administrar las máquinas virtuales con la característica [Update Management](update-mgmt-overview.md). Para dejar de administrarlos, modificará la consulta de búsqueda guardada `MicrosoftDefaultComputerGroup` en el área de trabajo de Log Analytics que está vinculada a su cuenta de Automation.

## <a name="sign-into-the-azure-portal"></a>Inicie sesión en Azure Portal.

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="to-remove-your-vms"></a>Para quitar las máquinas virtuales

1. En Azure Portal, inicie **Cloud Shell** en la navegación superior de Azure Portal. Si no está familiarizado con Azure Cloud Shell, consulte [Introducción a Azure Cloud Shell](../../cloud-shell/overview.md).

2. Use el siguiente comando para identificar el UUID de un equipo que desea quitar de la administración.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. En Azure Portal, vaya a **Areas de trabajo de Log Analytics**. Seleccione el área de trabajo de la lista.

4. En el área de trabajo de Log Analytics, seleccione **Registros** y luego elija **Explorador de consultas** en el menú acciones superiores.

5. En **Explorador de consultas** en el panel de la derecha, expanda **Consultas guardadas\Actualizaciones** y seleccione la consulta de búsqueda guardada `MicrosoftDefaultComputerGroup` para modificarla.

6. En el editor de consultas, revise la consulta y busque el UUID de la máquina virtual. Quite el UUID de la máquina virtual y repita los pasos para las demás máquinas virtuales que desee quitar.

7. Guarde la búsqueda guardada cuando haya terminado de modificarla; para ello, seleccione **Guardar** en la barra superior.

>[!NOTE]
>Las máquinas todavía se muestran después de anular la inscripción, ya que se informa de todas las máquinas que se han evaluado en las últimas 24 horas. Después de quitar la máquina, debe esperar 24 horas antes de que ya no aparezcan en la lista.

## <a name="next-steps"></a>Pasos siguientes

Para volver a habilitar la administración de la máquina virtual, consulte [Examinen de Azure Portal para habilitar Update Management](update-mgmt-enable-portal.md) o [Habilitación de Update Management desde una máquina virtual de Azure](update-mgmt-enable-vm.md).