---
title: Integración con Office 365 Outlook
description: Automatización de tareas y flujos de trabajo que administran el correo electrónico, los contactos y los calendarios en Office 365 Outlook con Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: logicappspm
ms.topic: article
ms.date: 11/13/2020
tags: connectors
ms.openlocfilehash: 9caf69a7f78c7872f0a5f8a2ed07bdc749a29023
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94683002"
---
# <a name="manage-email-contacts-and-calendars-in-office-365-outlook-by-using-azure-logic-apps"></a>Administración del correo electrónico, los contactos y los calendarios en Office 365 Outlook con Azure Logic Apps

Con [Azure Logic Apps](../logic-apps/logic-apps-overview.md) y el [conector de Office 365 Outlook](/connectors/office365connector/), puede crear tareas y flujos de trabajo automatizados que administren su cuenta profesional o educativa mediante la compilación de aplicaciones lógicas. Por ejemplo, puede automatizar estas tareas:

* Recibir, enviar y responder mensajes de correo electrónico.
* Programar reuniones en el calendario.
* Agregar y editar contactos.

Puede usar cualquier desencadenador para iniciar el flujo de trabajo (por ejemplo, cuando llega un nuevo correo electrónico, cuando se actualiza un elemento del calendario o cuando se produce un evento en un servicio diferencial, como Salesforce). Puede usar acciones que respondan al evento desencadenador, como enviar un correo electrónico o crear un nuevo evento de calendario.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Outlook donde inicie sesión con una [cuenta profesional o educativa](https://www.office.com/). Si tiene una cuenta @outlook.com o @hotmail.com, use mejor el conector [Outlook.com](../connectors/connectors-create-api-outlook.md). Para conectarse a Outlook con una cuenta de usuario diferente, por ejemplo, una cuenta de servicio, consulte [Conexión con otras cuentas](#connect-using-other-accounts).

* Una cuenta y una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* La aplicación lógica donde quiere acceder a su cuenta de Outlook. Para iniciar el flujo de trabajo con un desencadenador de Office 365 Outlook, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para agregar una acción de Office 365 Outlook al flujo de trabajo, la aplicación lógica ya debe tener un desencadenador.

## <a name="add-a-trigger"></a>Incorporación de un desencadenador

Un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts) es un evento que inicia el flujo de trabajo en la aplicación lógica. Esta aplicación lógica de ejemplo usa un desencadenador de "sondeo" que comprueba cualquier evento de calendario actualizado en la cuenta de correo electrónico, según el intervalo y la frecuencia especificados.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en blanco en el Diseñador de aplicación lógica.

1. En el cuadro de búsqueda, escriba `office 365 outlook` como filtro. En este ejemplo se selecciona **Cuando un evento próximo va a comenzar pronto**.
   
   ![Seleccionar el desencadenador para iniciar la aplicación lógica](./media/connectors-create-api-office365-outlook/office365-trigger.png)

1. Si no tiene una conexión activa a su cuenta de Outlook, se le pedirá que inicie sesión y la cree. Para conectarse a Outlook con una cuenta de usuario diferente, por ejemplo, una cuenta de servicio, consulte [Conexión con otras cuentas](#connect-using-other-accounts). De lo contrario, proporcione la información de las propiedades del desencadenador.

   > [!NOTE]
   > La conexión no expira hasta que se revoca, incluso si cambia las credenciales de inicio de sesión. Para obtener más información, consulte [Vigencia de tokens configurables de Azure Active Directory (versión preliminar pública)](../active-directory/develop/active-directory-configurable-token-lifetimes.md).

   En este ejemplo se selecciona el calendario que comprueba el desencadenador, por ejemplo:

   ![Configurar las propiedades del desencadenador](./media/connectors-create-api-office365-outlook/select-calendar.png)

1. En el desencadenador, establezca los valores de **Frecuencia** e **Intervalo**. Para agregar otras propiedades del desencadenador disponibles, como **Zona horaria**, selecciónelas en la lista **Agregar nuevo parámetro**.

   Por ejemplo, si desea que el desencadenador compruebe el calendario cada 15 minutos, establezca el valor de **Frecuencia** en **Minuto** y el de **Intervalo** en `15`. 

   ![Establecer la frecuencia y el intervalo del desencadenador](./media/connectors-create-api-office365-outlook/calendar-settings.png)

1. En la barra de herramientas del diseñador, seleccione **Save** (Guardar).

Ahora agregue una acción que se ejecute cuando se active el desencadenador. Por ejemplo, puede agregar la acción **Enviar mensaje** de Twilio, que envía un mensaje de texto cuando faltan 15 minutos para que comience un evento de calendario.

## <a name="add-an-action"></a>Agregar una acción

Una [acción](../logic-apps/logic-apps-overview.md#logic-app-concepts) es una operación que se ejecuta mediante el flujo de trabajo de la aplicación lógica. Esta aplicación lógica de ejemplo crea un nuevo contacto en Office 365 Outlook. Para crear el contacto puede utilizar la salida de otro desencadenador u otra acción. Por ejemplo, supongamos que la aplicación lógica usa el desencadenador Dynamics 365, **Al crear un registro**. Después, agregue la acción **Crear contacto** de Office 365 Outlook y utilice las salidas del desencadenador de SalesForce para crear el contacto nuevo.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. Para agregar una acción como último paso del flujo de trabajo, seleccione **Nuevo paso**. 

   Para agregar una acción entre un paso y otro, mueva el puntero por encima de la flecha entre ellos. Seleccione el signo más ( **+** ) que aparece y, luego, seleccione **Agregar una acción**.

1. En el cuadro de búsqueda, escriba `office 365 outlook` como filtro. En este ejemplo se selecciona **Crear contacto**.

   ![Seleccione la acción que se ejecutará en la aplicación lógica](./media/connectors-create-api-office365-outlook/office365-actions.png) 

1. Si no tiene una conexión activa a su cuenta de Outlook, se le pedirá que inicie sesión y la cree. Para conectarse a Outlook con una cuenta de usuario diferente, por ejemplo, una cuenta de servicio, consulte [Conexión con otras cuentas](#connect-using-other-accounts). De lo contrario, proporcione la información de las propiedades de la acción.

   > [!NOTE]
   > La conexión no expira hasta que se revoca, incluso si cambia las credenciales de inicio de sesión. Para obtener más información, consulte [Vigencia de tokens configurables de Azure Active Directory (versión preliminar pública)](../active-directory/develop/active-directory-configurable-token-lifetimes.md).

   En este ejemplo se selecciona la carpeta de contactos en la que la acción crea el nuevo contacto, por ejemplo:

   ![Configurar las propiedades de la acción](./media/connectors-create-api-office365-outlook/select-contacts-folder.png)

   Para agregar otras propiedades de la acción disponibles, selecciónelas en la lista **Agregar nuevo parámetro**.

1. En la barra de herramientas del diseñador, seleccione **Save** (Guardar).

<a name="connect-using-other-accounts"></a>

## <a name="connect-using-other-accounts"></a>Conexión con otras cuentas

Si intenta conectarse a Outlook con una cuenta diferente de aquella con la que ha iniciado sesión actualmente en Azure, es posible que obtenga errores de [inicio de sesión único (SSO)](../active-directory/manage-apps/what-is-single-sign-on.md). Este problema se produce cuando inicia sesión en Azure Portal con una cuenta, pero usa otra para crear la conexión. El Diseñador de aplicación lógica espera usar la cuenta con la que se inició sesión en Azure. Para resolver este problema, tiene estas opciones:

* Configure la otra cuenta como **colaborador** del grupo de recursos de la aplicación lógica.

  1. En el menú del grupo de recursos de la aplicación lógica, haga clic en **Control de acceso (IAM)** . Configure la otra cuenta con el rol **Colaborador**. Para más información, consulte [Incorporación o eliminación de asignaciones de roles mediante Azure Portal](../role-based-access-control/role-assignments-portal.md).

  1. Si ha iniciado sesión en Azure Portal con su cuenta profesional o educativa, cierre la sesión y vuelva a iniciarla con su otra cuenta. Ahora puede crear una conexión a Outlook mediante la otra cuenta.

* Configure la otra cuenta para que su cuenta profesional o educativa tenga permisos para "enviar como".

   Si tiene permisos de administrador, en el buzón de la cuenta de servicio, configure su cuenta profesional o educativa con los permisos **Enviar como** o **Enviar en nombre de**. Para más información, consulte [Conceder permisos de buzón a otro usuario: Ayuda para administradores](/microsoft-365/admin/add-users/give-mailbox-permissions-to-another-user). Después, puede crear la conexión con su cuenta profesional o educativa. Ahora, en los desencadenadores o acciones donde puede especificar el remitente, puede usar la dirección de correo electrónico de la cuenta de servicio.

   Por ejemplo, la acción **Enviar un correo electrónico** tiene un parámetro opcional, **De (enviar como)** , que puede agregar a la acción y usar la dirección de correo electrónico de la cuenta de servicio como remitente. Para agregar este parámetro, siga estos pasos:

   1. En la acción **Enviar un correo electrónico**, abra la lista **Agregar un parámetro** y seleccione el parámetro **De (enviar como)** .

   1. Cuando el parámetro aparezca en la acción, escriba la dirección de correo electrónico de la cuenta de servicio.

## <a name="connector-reference"></a>Referencia de conectores

Si necesita detalles técnicos sobre este conector, como los desencadenadores, las acciones y los límites que se describen en el archivo de Swagger del conector, consulte la [página de referencia del conector](/connectors/office365/). 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)