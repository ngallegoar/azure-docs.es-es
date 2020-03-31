---
title: Creación de una cuenta independiente de Azure Automation
description: En este artículo se le guía en la creación, la prueba y el uso de la autenticación de la entidad de seguridad en Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 01/15/2019
ms.topic: conceptual
ms.openlocfilehash: 22efb5e94049b975780c6f6ea69aa94a71cc9992
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235636"
---
# <a name="create-a-standalone-azure-automation-account"></a>Creación de una cuenta independiente de Azure Automation

En este artículo se muestra cómo crear una cuenta de Azure Automation en el portal de Azure. Puede usar la cuenta de Automation del portal para evaluar y obtener información acerca de Automation sin usar soluciones de administración o integración adicionales con registros de Azure Monitor. Puede agregar estas soluciones de administración o realizar la integración con registros de Azure Monitor para disfrutar de una supervisión avanzada de los trabajos de runbook en cualquier momento en el futuro.

Con una cuenta de Automation, podrá autenticar runbooks mediante la administración de recursos en Azure Resource Manager o en el modelo de implementación clásica. Una cuenta de Automation puede administrar los recursos de todas las regiones y suscripciones de un inquilino determinado.

Al crear una cuenta de Automation en Azure Portal, estas cuentas se crean automáticamente:

* **Cuenta de ejecución**. Esta cuenta hace las siguientes tareas:
  * Creación de una entidad de servicio en Azure Active Directory (Azure AD).
  * Crea un certificado.
  * Asigna el control de acceso basado en rol (RBAC) del colaborador, que administra recursos de Azure Resource Manager mediante runbooks.

Con estas cuentas que cree, podrá generar e implementar Runbooks que den soporte técnico a sus necesidades de automatización.

## <a name="permissions-required-to-create-an-automation-account"></a>Permisos necesarios para crear una cuenta de Automation

Para crear o actualizar una cuenta de Automation y completar las tareas que se describen en este artículo, debe tener los siguientes privilegios y permisos:

* Para crear una cuenta de Automation, la cuenta de usuario de Azure AD debe agregarse a un rol con permisos equivalentes al rol de propietario para **Microsoft. Recursos de Automation**. Para más información, consulte [Control de acceso basado en rol en Azure Automation](automation-role-based-access-control.md).
* En Azure Portal, en **Azure Active Directory** > **ADMINISTRAR** > **Configuración de usuario**, si **Registros de aplicaciones** está establecido en **Sí**, los usuarios sin derechos administrativos del inquilino de Azure AD pueden [registrar aplicaciones de Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#check-azure-subscription-permissions). Si **Registros de aplicaciones** está establecido en **No**, el usuario que realice esta acción debe ser administrador global de Azure AD.

Si no es miembro de la instancia de Active Directory de la suscripción antes de que le agreguen al rol de administrador global/coadministrador de esta última, se le agregará a Active Directory como invitado. En este escenario, verá este mensaje en la página **Agregar cuenta de Automation**: "No tiene permisos para crear".

Si un usuario se agrega primero a los roles de administrador global o coadministrador, se puede quitar de la instancia de Active Directory de la suscripción y volver a agregarlo al rol de usuario completo en Active Directory.

Para comprobar los roles de usuario:

1. En Azure Portal, vaya al panel **Azure Active Directory**.
1. Seleccione **Usuarios y grupos**.
1. Seleccione **Todos los usuarios**.
1. Después de seleccionar un usuario específico, seleccione **Perfil**. El valor del atributo **Tipo de usuario** del perfil de los usuarios no debería ser **Invitado**.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Creación de una nueva cuenta de Automation en Azure Portal

Para crear una cuenta de Azure Automation en Azure Portal, complete los pasos siguientes:

1. Inicie sesión en Azure Portal con una cuenta que sea miembro del rol Administradores de la suscripción y coadministrador de la suscripción.
1. Seleccione **+ Crear un recurso**.
1. Busque **Automation**. En los resultados de la búsqueda, haga clic en **Automation**.

   ![Busque y seleccione Automation & Control en Azure Marketplace.](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. En la pantalla siguiente, seleccione **Crear**.

   ![Agregar cuenta de Automation](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

   > [!NOTE]
   > Si ve el mensaje siguiente en el panel **Agregar cuenta de Automation**, significa que su cuenta no es miembro del rol Administradores de la suscripción ni del rol de coadministrador de la suscripción.
   >
   > ![Advertencia de Agregar cuenta de Automation](media/automation-create-standalone-account/create-account-without-perms.png)

1. En el panel **Agregar cuenta de Automation**, en el cuadro **Nombre**, escriba el nombre de la nueva cuenta de Automation. Este nombre no puede cambiarse una vez que se elige. *Los nombres de la cuenta de Automation son únicos en cada región y grupo de recursos. Los nombres de las cuentas de Automation que se han eliminado es posible que no estén disponibles de inmediato.*
1. Si tiene más de una suscripción, en el cuadro **Suscripción**, especifique qué suscripción quiere usar para la cuenta nueva.
1. Para **Grupo de recursos**, escriba o seleccione un grupo de recursos nuevo o existente.
1. Para **Ubicación**, seleccione una ubicación del centro de datos de Azure.
1. Para la opción **Crear cuenta de ejecución de Azure**, asegúrese de que la opción **Sí** está seleccionada y, después, seleccione **Crear**.

   > [!NOTE]
   > Si decide no crear la cuenta de ejecución, al seleccionar **No** para **Crear cuenta de ejecución de Azure**, aparecerá un mensaje en el panel **Agregar cuenta de Automation**. Aunque la cuenta se crea en Azure Portal, no tiene la identidad de autenticación correspondiente en la suscripción del modelo de implementación clásico ni en el servicio de directorio de suscripciones de Resource Manager. Por lo tanto, la cuenta de Automation no tiene acceso a los recursos en su suscripción. Esto evitará que los runbooks que hacen referencia a esta cuenta puedan autenticarse y realizar tareas con los recursos en dichos modelos de suscripción.
   >
   > ![Advertencia de Agregar cuenta de Automation](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
   >
   > Si no se crea la entidad de servicio, no se asigna el rol Colaborador.
   >

1. Para realizar un seguimiento del progreso de la creación de la cuenta de Automation, en el menú, seleccione **Notificaciones**.

### <a name="resources-included"></a>Recursos incluidos

Una vez que se crea la cuenta de Automation, se también varios recursos automáticamente. Una vez creados, estos runbooks se pueden eliminar de forma segura, en caso de que no se desee conservarlos. Las cuentas de ejecución pueden utilizarse para autenticarse con la cuenta en un runbook y deben conservarse a menos que se cree otra o que ya no sean necesarias. La siguiente tabla resume los recursos de la cuenta de ejecución.

| Resource | Descripción |
| --- | --- |
| Runbook AzureAutomationTutorial |Runbook gráfico de ejemplo que muestra cómo realizar la autenticación mediante la cuenta de ejecución. El runbook obtiene todos los recursos de Resource Manager. |
| Runbook AzureAutomationTutorialScript |Runbook de PowerShell de ejemplo que muestra cómo realizar la autenticación mediante la cuenta de ejecución. El runbook obtiene todos los recursos de Resource Manager. |
| Runbook AzureAutomationTutorialPython2 |Runbook de Python de ejemplo que muestra cómo realizar la autenticación mediante la cuenta de ejecución. El runbook enumera todos los grupos de recursos presentes en la suscripción. |
| AzureRunAsCertificate |Recurso de certificado que se crea automáticamente al crear una cuenta de Automation, o mediante el uso del siguiente script de PowerShell para una cuenta existente. El certificado realiza la autenticación en Azure, de modo que puede administrar los recursos de Azure Resource Manager desde los runbooks. Este certificado tiene una duración de un año. |
| AzureRunAsConnection |Recurso de conexión que se crea automáticamente al crear una cuenta de Automation, o mediante el uso del siguiente script de PowerShell para una cuenta existente. |

## <a name="classic-run-as-accounts"></a>Cuentas de ejecución clásicas

Las cuentas de ejecución clásicas ya no se crean, de forma predeterminada, cuando se crea una cuenta de Azure Automation. Si todavía necesita una cuenta de ejecución clásica, siga los pasos que se describen a continuación.

1. Desde la **Cuenta de Automation**, seleccione **Cuentas de ejecución** en **Configuración de la cuenta**.
2. Seleccione **Cuenta de ejecución de Azure clásico**.
3. Haga clic en **Crear** para continuar con la creación de la cuenta de ejecución clásica.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información acerca de la creación de gráficos, consulte [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md).
* Para empezar a trabajar con runbooks de PowerShell, consulte [Mi primer runbook de PowerShell](automation-first-runbook-textual-powershell.md).
* Para empezar a trabajar con Runbooks de flujo de trabajo de PowerShell, consulte [Mi primer Runbook de flujo de trabajo de PowerShell](automation-first-runbook-textual.md).
* Para empezar a trabajar con runbooks Python2, consulte [Mi primer runbook Python2](automation-first-runbook-textual-python2.md).

