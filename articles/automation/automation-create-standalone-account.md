---
title: Creación de una cuenta independiente de Azure Automation
description: En este artículo se indica cómo crear una cuenta de Azure Automation independiente y una cuenta de ejecución clásica.
services: automation
ms.subservice: process-automation
ms.date: 01/15/2019
ms.topic: conceptual
ms.openlocfilehash: c78a65c3cc2d913ba0b836947d2184b3a061a648
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91714652"
---
# <a name="create-a-standalone-azure-automation-account"></a>Creación de una cuenta independiente de Azure Automation

En este artículo se muestra cómo crear una cuenta de Azure Automation en el portal de Azure. Puede usar la cuenta de Automation del portal para evaluar y obtener información acerca de Automation sin usar características de administración o integración adicionales con registros de Azure Monitor. Puede agregar características de administración o realizar la integración con registros de Azure Monitor para disfrutar de una supervisión avanzada de los trabajos de runbook en cualquier momento en el futuro.

Con una cuenta de Automation, podrá autenticar runbooks mediante la administración de recursos en Azure Resource Manager o en el modelo de implementación clásica. Una cuenta de Automation puede administrar los recursos de todas las regiones y suscripciones de un inquilino determinado.

Cuando crea una cuenta de Automation en Azure Portal, se crea automáticamente la cuenta de **ejecución**. Esta cuenta hace las siguientes tareas:

* Creación de una entidad de servicio en Azure Active Directory (Azure AD).
* Crea un certificado.
* Asigna el control de Colaborador, que administra recursos de Azure Resource Manager mediante runbooks.

Con esta cuenta que se ha creado automáticamente, podrá generar e implementar rápidamente runbooks que contribuyan a satisfacer sus necesidades de automatización.

## <a name="permissions-required-to-create-an-automation-account"></a>Permisos necesarios para crear una cuenta de Automation

Para crear o actualizar una cuenta de Automation y completar las tareas que se describen en este artículo, debe tener los siguientes privilegios y permisos:

* Para crear una cuenta de Automation, la cuenta de usuario de Azure AD debe agregarse a un rol con permisos equivalentes a los del rol del propietario de los recursos de `Microsoft.Automation`. Para más información, consulte [Control de acceso basado en rol en Azure Automation](automation-role-based-access-control.md).
* En Azure Portal, en **Azure Active Directory** > **ADMINISTRAR** > **Configuración de usuario**, si **Registros de aplicaciones** está establecido en **Sí**, los usuarios sin derechos administrativos del inquilino de Azure AD pueden [registrar aplicaciones de Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#check-azure-subscription-permissions). Si **Registros de aplicaciones** está establecido en **No**, el usuario que realice esta acción debe tener al menos el rol de desarrollador de la aplicación en Azure AD.

Si no es miembro de la instancia de Active Directory de la suscripción antes de que le asignen el rol de administrador global o de coadministrador de dicha suscripción, se le agregará a Active Directory como invitado. En este caso, aparecerá este mensaje en el panel Agregar cuenta de Automation: `You do not have permissions to create.`.

Si a un usuario se le asigna primero el rol de administrador global o de coadministrador, se le puede quitar de la instancia de Active Directory de la suscripción. Si desea asignarle de nuevo el rol de usuario, puede hacerlo en Active Directory. Para comprobar los roles de usuario:

1. En Azure Portal, vaya al panel Azure Active Directory.
1. Seleccione **Usuarios y grupos**.
1. Seleccione **Todos los usuarios**.
1. Después de seleccionar un usuario específico, seleccione **Perfil**. El valor del atributo **Tipo de usuario** del perfil de los usuarios no debería ser **Invitado**.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Creación de una nueva cuenta de Automation en Azure Portal

Para crear una cuenta de Azure Automation en Azure Portal, complete los pasos siguientes:

1. Inicie sesión en Azure Portal con una cuenta que sea miembro del rol Administradores de la suscripción y coadministrador de la suscripción.
1. Seleccione **+ Crear un recurso**.
1. Busque **Automation**. En los resultados de la búsqueda, haga clic en **Automation**.

   ![Busque y seleccione Automation & Control en Azure Marketplace.](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. En la pantalla siguiente, seleccione **Crear nuevo**.

   ![Agregar cuenta de Automation](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

   > [!NOTE]
   > Si ve el mensaje siguiente en el panel Agregar cuenta de Automation, significa que la cuenta no es miembro del rol de administradores ni del rol de coadministrador de la suscripción.
   >
   > :::image type="content" source="media/automation-create-standalone-account/create-account-without-perms.png" alt-text="Captura de pantalla de la solicitud &quot;No tiene permisos para crear una cuenta de ejecución en Azure Active Directory&quot;.":::

1. En el panel Agregar cuenta de Automation, en el cuadro **Nombre**, escriba el nombre de la nueva cuenta de Automation. Una vez elegido el nombre, no puede cambiarse. 

    > [!NOTE]
    > Los nombres de la cuenta de Automation son únicos en cada región y grupo de recursos. Es posible que los nombres de las cuentas de Automation eliminadas no estén disponibles de inmediato.

1. Si tiene varias suscripciones, en el campo **Suscripción** especifique la que va a usar con la nueva cuenta.
1. Para **Grupo de recursos**, escriba o seleccione un grupo de recursos nuevo o existente.
1. Para **Ubicación**, seleccione una ubicación del centro de datos de Azure.
1. En la opción **Crear cuenta de ejecución de Azure**, asegúrese de que la opción **Sí** está seleccionada y haga clic en **Crear**.

   > [!NOTE]
   > Si prefiere no crear la cuenta de ejecución y selecciona **No** en **Crear cuenta de ejecución de Azure**, aparecerá un mensaje en el panel Agregar cuenta de Automation. Aunque la cuenta se crea en Azure Portal, no tiene la identidad de autenticación correspondiente en la suscripción del modelo de implementación clásico ni en el servicio de directorio de suscripciones de Resource Manager. Por lo tanto, la cuenta de Automation no tiene acceso a los recursos en su suscripción. Esto evitará que los runbooks que hacen referencia a esta cuenta puedan autenticarse y realizar tareas con los recursos en dichos modelos de suscripción.
   >
   > :::image type="content" source="media/automation-create-standalone-account/create-account-decline-create-runas-msg.png" alt-text="Captura de pantalla de la solicitud &quot;No tiene permisos para crear una cuenta de ejecución en Azure Active Directory&quot;.":::
   >
   > Si no se crea la entidad de servicio, no se asigna el rol Colaborador.
   >

1. Para seguir el progreso de creación de la cuenta de Automation, en el menú, seleccione **Notificaciones**.

Una vez que se crea la cuenta de Automation, se también varios recursos automáticamente. Una vez creados, estos runbooks se pueden eliminar de forma segura, en caso de que no se desee conservarlos. Las cuentas de ejecución pueden utilizarse para autenticarse con la cuenta en un runbook y deben conservarse a menos que se cree otra o que ya no sean necesarias. La siguiente tabla resume los recursos de la cuenta de ejecución.

| Recurso | Descripción |
| --- | --- |
| Runbook AzureAutomationTutorial |Runbook gráfico de ejemplo que muestra cómo realizar la autenticación mediante la cuenta de ejecución. El runbook obtiene todos los recursos de Resource Manager. |
| Runbook AzureAutomationTutorialScript |Runbook de PowerShell de ejemplo que muestra cómo realizar la autenticación mediante la cuenta de ejecución. El runbook obtiene todos los recursos de Resource Manager. |
| Runbook AzureAutomationTutorialPython2 |Runbook de Python de ejemplo que muestra cómo realizar la autenticación mediante la cuenta de ejecución. El runbook enumera todos los grupos de recursos presentes en la suscripción. |
| AzureRunAsCertificate |Recurso de certificado que se crea automáticamente al crear una cuenta de Automation, o mediante el uso del siguiente script de PowerShell para una cuenta existente. El certificado realiza la autenticación en Azure, de modo que puede administrar los recursos de Azure Resource Manager desde los runbooks. Este certificado tiene una duración de un año. |
| AzureRunAsConnection |Recurso de conexión que se crea automáticamente al crear una cuenta de Automation, o mediante el uso del siguiente script de PowerShell para una cuenta existente. |

## <a name="create-a-classic-run-as-account"></a>Creación de una cuenta de ejecución clásica

Las cuentas de ejecución clásicas ya no se crean de forma predeterminada al crear una cuenta de Azure Automation. Sin embargo, si necesita una cuenta de ejecución clásica:

1. En la cuenta de Automation, en **Configuración de la cuenta**, seleccione **Cuentas de ejecución**.
2. Seleccione **Cuenta de ejecución de Azure clásico**.
3. Haga clic en **Crear** para continuar con la creación de la cuenta de ejecución clásica.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la creación de runbooks gráficos, vea [Creación de runbooks gráficos en Azure Automation](automation-graphical-authoring-intro.md).
* Para empezar a trabajar con runbooks de PowerShell, vea [Tutorial: Creación de un runbook de PowerShell](learn/automation-tutorial-runbook-textual-powershell.md).
* Para empezar a trabajar con runbooks de flujo de trabajo de PowerShell, consulte [Tutorial: Creación de un runbook de flujo de trabajo de PowerShell](learn/automation-tutorial-runbook-textual.md).
* Para empezar a trabajar con runbooks de Python 2, vea [Tutorial: Creación de un runbook de Python 2](learn/automation-tutorial-runbook-textual-python2.md).
* Para ver una referencia de los cmdlets de PowerShell, consulte [Az.Automation](/powershell/module/az.automation/?view=azps-3.7.0&preserve-view=true#automation).
