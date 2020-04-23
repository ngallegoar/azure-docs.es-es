---
title: 'Inicio rápido de Azure: Creación de una cuenta de Azure Automation | Microsoft Docs'
description: Información sobre cómo crear una cuenta de Azure Automation y ejecutar un runbook
services: automation
ms.date: 04/04/2019
ms.topic: quickstart
ms.subservice: process-automation
ms.custom: mvc
ms.openlocfilehash: 7704f080b7c1878f2fa2b079a1f242c8c2cc87a9
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536988"
---
# <a name="create-an-azure-automation-account"></a>Creación de una cuenta de Azure Automation

Puede crear una cuenta de Azure Automation mediante Azure, desde Azure Portal, una interfaz de usuario basada en explorador que permite el acceso a varios recursos. Una cuenta de Automation puede administrar los recursos de todas las regiones y las suscripciones de un inquilino determinado. 

Este inicio rápido le guía en la creación de una cuenta de Automation y en la ejecución de un runbook en esa cuenta. Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[Inicie de sesión en Azure](https://portal.azure.com).

## <a name="create-automation-account"></a>Creación de una cuenta de Automation

1. Elija un nombre para la cuenta de Azure. Los nombres de la cuenta de Automation son únicos en cada región y grupo de recursos. Es posible que los nombres de las cuentas de Automation que se hayan eliminado no estén disponibles de inmediato.

    > [!NOTE]
    > No se puede cambiar el nombre de la cuenta una vez que se ha escrito en la interfaz de usuario. 

2. Haga clic en el botón **Crear un recurso** de la esquina superior izquierda de Azure Portal.

3. Seleccione **Herramientas de administración y TI** y, después, **Automation**.

4. Escriba la información de la cuenta, incluido el nombre de cuenta seleccionado. En **Crear cuenta de ejecución de Azure**, elija **Sí** para que los artefactos destinados a simplificar la autenticación en Azure se habiliten automáticamente. Cuando la información esté completa, haga clic en **Crear** para iniciar la implementación de la cuenta de Automation.

    ![Escriba la información sobre su cuenta de Automation en la página](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

    > [!NOTE]
    > Para obtener una lista actualizada de ubicaciones en las que se puede implementar una cuenta de Automation, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=automation&regions=all).

5. Cuando haya finalizado la implementación, haga clic en **Todos los servicios**.

6. Seleccione **Cuentas de Automation** y, después, elija la cuenta de Automation que ha creado.

    ![Información general de la cuenta de Automation](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Ejecutar un runbook

Ejecute uno de los runbooks de tutorial.

1. Haga clic en **Runbooks** en **Automatización de procesos**. Se muestra la lista de runbooks. De forma predeterminada, se habilitan varios runbooks del tutorial en la cuenta.

    ![Lista de runbooks de la cuenta de Automation](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. Seleccione el runbook **AzureAutomationTutorialScript**. Esta acción abre la página de información general del runbook.

    ![Información general del runbook](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. Haga clic en **Iniciar** y, en la página Iniciar runbook, haga clic en **Aceptar** para iniciar el runbook.

    ![Página del trabajo de runbook](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. Cuando el estado del trabajo sea `Running`, haga clic en **Salida** o en **Todos los registros** para ver la salida del trabajo del runbook. Para este runbook de tutorial, la salida es una lista de sus recursos de Azure.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha implementado una cuenta de Automation, ha iniciado un trabajo de runbook y ha visto los resultados del trabajo. Para obtener más información sobre Azure Automation, continúe en la guía de inicio rápido sobre la creación del primer runbook.

> [!div class="nextstepaction"]
> [Inicio rápido de Automation: creación de un runbook](./automation-quickstart-create-runbook.md)

