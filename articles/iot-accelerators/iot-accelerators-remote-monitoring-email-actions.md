---
title: 'Acción de correo electrónico desde supervisión remota: Azure | Microsoft Docs'
description: Esta guía paso a paso muestra cómo agregar una acción de correo electrónico a una regla nueva o existente.
author: dominicbetts
manager: hegate
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/12/2018
ms.topic: conceptual
ms.openlocfilehash: 2776b18dc3faded103b49e8ff58a8a0a6dccf144
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92074471"
---
# <a name="add-an-email-action"></a>Adición de una acción de correo electrónico

Las acciones de correo electrónico ayudan a asegurarse de que no se pierda las alertas. Puede agregar una acción de correo electrónico a una regla existente o durante la creación de una nueva regla.

Para completar los pasos en esta guía paso a paso, necesita una instancia implementada de la solución de supervisión remota en la suscripción de Azure.

Para crear o modificar una regla, debe ser un [**administrador** o tener los permisos correctos](iot-accelerators-remote-monitoring-rbac.md).

## <a name="edit-an-existing-rule"></a>Editar una regla existente

Siga los pasos a continuación para agregar una acción de correo electrónico a una regla existente:

1. Vaya a la solución de supervisión remota.

1. Desde el **Panel**, navegue hasta la página **Reglas**:

    ![Página de Reglas](./media/iot-accelerators-remote-monitoring-email-actions/rules-email.png)

1. Haga clic en la casilla de verificación junto a la regla existente que va a modificar y, después, haga clic en **Editar** en la parte superior. Se muestra un panel editable titulado **Regla**.

1. En la sección **Acción**, cambie el control **Email enabled** (Habilitado para correo electrónico) a **Activado**.

1. La primera vez que habilite una acción de correo electrónico en el acelerador de soluciones, debe [iniciar sesión con Outlook](#outlook).

1. Escriba una dirección de correo electrónico en el cuadro de destinatario y presione la tecla **ENTRAR** en cada una de las direcciones de correo electrónico que va a agregar:

    ![Entrada de dirección](./media/iot-accelerators-remote-monitoring-email-actions/address-email.png)

1. Escriba un asunto para el correo electrónico.

1. Escriba las notas adicionales para los destinatarios de correo electrónico como texto sin formato. Puede usar el formato HTML si [edita la plantilla de correo electrónico](#htmledit).

1. Asegúrese de que el **Estado de la regla** se establece en **Habilitado**.

1. Haga clic en **Aplicar**.

## <a name="create-a-new-rule"></a>Crear una nueva regla

Siga estos pasos para agregar una acción de correo electrónico cuando cree una nueva regla:

1. Vaya a la solución de supervisión remota.

1. Desde el **Panel**, navegue hasta la página **Reglas**:

    ![Página de Reglas](./media/iot-accelerators-remote-monitoring-email-actions/rules-email.png)

1. Siga los pasos descritos en la [sección Crear una regla](iot-accelerators-remote-monitoring-automate.md#create-a-rule). Después, siga los pasos en la sección de [creación de una regla avanzada](iot-accelerators-remote-monitoring-automate.md#create-an-advanced-rule) hasta el punto donde se establece un **nivel de gravedad**. No haga clic en **Aplicar** todavía.

1. En la sección **Acción**, cambie el control **Email enabled** (Habilitado para correo electrónico) a **Activado**.

1. La primera vez que habilite una acción de correo electrónico en el acelerador de soluciones, debe [iniciar sesión con Outlook](#outlook).

1. Escriba una dirección de correo electrónico en el cuadro de destinatario y presione la tecla **ENTRAR** en cada una de las direcciones de correo electrónico que va a agregar:

    ![Entrada de dirección](./media/iot-accelerators-remote-monitoring-email-actions/address-email.png)

1. Escriba un asunto para el correo electrónico.

1. Escriba las notas adicionales para los destinatarios de correo electrónico como texto sin formato. Puede usar el formato HTML si [edita la plantilla de correo electrónico](#htmledit).

1. Asegúrese de que el **Estado de la regla** se establece en **Habilitado**.

1. Haga clic en **Aplicar**.

Ahora está habilitada la regla con una acción de correo electrónico. Cada vez que se desencadena la acción, se envía un nuevo correo electrónico a los destinatarios.

## <a name="sign-in-to-outlook"></a>Inicio de sesión en Outlook <a name="outlook"></a>

La primera vez que habilite una acción de correo electrónico en el acelerador de soluciones, debe iniciar sesión con Outlook. Esta acción configura la cuenta de correo electrónico que envía las notificaciones por correo.

> [!NOTE]
> Debe crear una cuenta de Outlook específica solo para las notificaciones del acelerador de soluciones y usar esa cuenta cuando habilite la primera acción de correo electrónico.

### <a name="contributor-role-outlook-setup"></a>Configuración de Outlook para el rol de colaborador

Si una persona con el rol de **colaborador** en la suscripción implementó el acelerador de soluciones, la aplicación no tiene permisos suficientes para configurar y verificar acciones de correo electrónico a través de la interfaz de usuario web.

Antes de empezar, cree una cuenta de Outlook para usarla en el envío de notificaciones por correo electrónico desde su acelerador de soluciones.

En los pasos siguientes se muestra cómo configurar y comprobar manualmente las acciones de correo electrónico:

1. Acceda a [Azure Portal](https://portal.azure.com).

1. Vaya al grupo de recursos de su acelerador de soluciones.

1. Haga clic en el conector **office365-connector**:

    ![API Connection](./media/iot-accelerators-remote-monitoring-email-actions/apiconnector1.png)

1. Haga clic en el banner para comenzar el proceso de autorización:

    ![autorización](./media/iot-accelerators-remote-monitoring-email-actions/connector1.png)

1. Haga clic en **Autorizar**. Se le pedirá que inicie sesión. La cuenta que utilice para iniciar sesión debe ser la misma que la que utilice la aplicación para enviar las notificaciones por correo electrónico:

    ![Botón Autorizar](./media/iot-accelerators-remote-monitoring-email-actions/authorize.png)

1. Haga clic en **Guardar** en la parte inferior. Si el banner desaparece, la autorización se realizó correctamente.

1. Para cambiar la dirección de correo electrónico desde la que se envían las notificaciones, haga clic en **Editar la conexión de API**.

    ![cambiar correo electrónico](./media/iot-accelerators-remote-monitoring-email-actions/editemail1.png)

### <a name="owner-role-outlook-setup"></a>Configuración de Outlook para el rol de propietario

Si una persona con el rol de **propietario** en la suscripción implementó el acelerador de soluciones, la aplicación puede verificar las acciones de correo electrónico que se hayan configurado correctamente a través de la interfaz de usuario web.

Antes de empezar, cree una cuenta de Outlook para usarla en el envío de notificaciones por correo electrónico desde su acelerador de soluciones.

Los pasos siguientes le ayudarán a iniciar sesión y a configurar las acciones de correo electrónico:

1. Haga clic en iniciar sesión en Outlook. Se lo redirige a Azure Portal:

   ![Inicio de sesión en Outlook](./media/iot-accelerators-remote-monitoring-email-actions/owneroutlook-email.png)

1. Haga clic en **Autorizar**. Se le pedirá que inicie sesión. La cuenta que utilice para iniciar sesión debe ser la misma que la que utilice la aplicación para enviar las notificaciones por correo electrónico:

1. Haga clic en **Save**(Guardar). Vuelva al acelerador de soluciones y actualice la página.

1. Si ha configurado correctamente la notificación por correo electrónico, verá este mensaje:

   ![Inicio de sesión de Outlook correcto](./media/iot-accelerators-remote-monitoring-email-actions/success-email.png)

## <a name="customize-the-email-html"></a>Personalización del HTML del correo electrónico <a name="htmledit"></a>

El acelerador de soluciones de supervisión remota incluye de fábrica una plantilla HTML básica para los correos electrónicos de las acciones. La plantilla de correo electrónico utiliza los valores de la configuración de las acciones de correo electrónico. Este es un ejemplo de correo electrónico:

![ejemplo de correo electrónico](./media/iot-accelerators-remote-monitoring-email-actions/emailtemplate1.png)

En los pasos siguientes se muestra cómo editar la plantilla HTML de correo electrónico. Por ejemplo, puede incluir más información o agregar imágenes personalizadas:

1. Clone el repositorio de GitHub de supervisión remota para Java o .NET:

1. Desplácese hasta la ubicación de la plantilla de correo electrónico:
  
    `Dotnet: device-telemetry\ActionsAgent\data\EmailTemplate.html`
  
    `Java device-telemetry/app/resources/data/EmailTemplate.html`

1. Puede agregar o quitar los parámetros de esta plantilla para personalizar el mensaje. También puede agregar, quitar o reemplazar las llamadas según sea necesario:

    Por ejemplo, en el código .NET: `emailTemplate = emailTemplate.Replace("${subject}", emailAction.GetSubject());`

    Por ejemplo, en el código Java: `this.emailTemplate.replace("${subject}", emailAction.GetSubject());`

1. Los parámetros de la plantilla adoptan la forma `${...}`. Para eliminar un parámetro, elimine la línea necesaria. Para agregar un parámetro, agregue una línea con el valor que insertará.

1. Para agregar imágenes o texto personalizado, actualice el archivo EmailTemplate.HTML directamente.

## <a name="throttling"></a>Limitaciones

El acelerador de soluciones de supervisión remota utiliza Outlook para enviar la notificaciones por correo electrónico. Outlook limita el número de correos electrónicos enviados a [30 correos electrónicos por minuto](/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#receiving-and-sending-limits). Los clientes de correo electrónico que reciben los correos electrónicos también pueden limitar al número de correos recibidos por minuto. Póngase en contacto con el cliente de correo electrónico específico para comprobar las limitaciones. Al configurar las notificaciones por correo electrónico para una regla, la regla debe calcular valores promedio durante un período de al menos un minuto y no usar valores instantáneos:

![Cálculo promedio](./media/iot-accelerators-remote-monitoring-email-actions/calculation-email.png)

## <a name="next-steps"></a>Pasos siguientes

En esta guía se ha explicado cómo agregar una acción de correo electrónico a una regla nueva o existente en una solución de supervisión remota. En la guía también se le ha mostrado y explicado cómo editar el HTML que define el formato del mensaje.

El siguiente paso sugerido es aprender a [usar las alertas y solucionar problemas del dispositivo](iot-accelerators-remote-monitoring-maintain.md).