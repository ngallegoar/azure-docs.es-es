---
title: Habilitación de la extensión de VM en Azure Portal
description: En este artículo se describe cómo implementar extensiones de máquina virtual en servidores habilitados para Azure Arc que se ejecutan en entornos de nube híbrida desde Azure Portal.
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: dcab7cb441c329a60b2c6fa3256aeedb2bb5b33d
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92462836"
---
# <a name="enable-azure-vm-extensions-from-the-azure-portal"></a>Habilitación de las extensiones de máquina virtual de Azure desde Azure Portal

En este artículo se muestra cómo implementar y desinstalar extensiones de VM de Azure, compatibles con servidores habilitados para Azure Arc, en una máquina híbrida Linux o Windows mediante Azure Portal.

## <a name="enable-extensions-from-the-portal"></a>Habilitación de extensiones desde el portal

Las extensiones de máquina virtual se pueden aplicar a la máquina administrada de Arc para servidores mediante Azure Portal.

1. En el explorador, vaya a [Azure Portal](https://portal.azure.com).

2. En el portal, vaya a **Servers - Azure Arc** (Servidores: Azure Arc) y seleccione la máquina híbrida de la lista.

3. Elija **Extensiones** y, después, seleccione **Agregar**. Elija la extensión que quiera en la lista de extensiones disponibles y siga las instrucciones del asistente. En este ejemplo, se implementará la extensión de máquina virtual de Log Analytics.

    ![Selección de la extensión de máquina virtual para la máquina seleccionada](./media/manage-vm-extensions/add-vm-extensions.png)

    En el ejemplo siguiente se muestra la instalación de la extensión de máquina virtual de Log Analytics desde Azure Portal:

    ![Instalación de la extensión de máquina virtual de Log Analytics](./media/manage-vm-extensions/mma-extension-config.png)

    Para completar la instalación, debe proporcionar el id. del área de trabajo y la clave principal. Si no está familiarizado con el método para encontrar esta información, consulte [Obtención del id. y la clave del área de trabajo](../../azure-monitor/platform/log-analytics-agent.md#workspace-id-and-key).

4. Después de confirmar la información necesaria que proporcionó, seleccione **Crear**. Se muestra un resumen de la implementación y puede revisar el estado de esta.

>[!NOTE]
>Aunque puede procesar por lotes varias extensiones, estas se instalan en serie. Una vez completada la instalación de la primera extensión, se intenta la instalación de la siguiente.

## <a name="uninstall-extension"></a>Desinstalación de una extensión

Puede quitar una o más extensiones de un servidor habilitado para Arc desde Azure Portal. Siga los pasos siguientes para quitar una extensión.

1. En el explorador, vaya a [Azure Portal](https://portal.azure.com).

2. En el portal, vaya a **Servers - Azure Arc** (Servidores: Azure Arc) y seleccione la máquina híbrida de la lista.

3. Elija **Extensiones** y, después, seleccione una de la lista de extensiones instaladas.

4. Seleccione **Desinstalar** y, cuando se le pida que realice la comprobación, seleccione **Sí** para continuar.

## <a name="next-steps"></a>Pasos siguientes

- Puede implementar, administrar y quitar extensiones de VM con la [CLI de Azure](manage-vm-extensions-cli.md), [PowerShell](manage-vm-extensions-powershell.md) o [plantillas de Azure Resource Manager](manage-vm-extensions-template.md).

- Puede encontrar información de solución de problemas en la [guía de solución de problemas de las extensiones de máquina virtual](troubleshoot-vm-extensions.md).