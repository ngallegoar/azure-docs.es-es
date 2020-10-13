---
author: georgewallace
ms.service: multiple
ms.topic: include
ms.date: 11/25/2018
ms.author: gwallace
ms.openlocfilehash: 4eb1a6f351cdf129611949049f762fe51cac4b16
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376287"
---
Los clientes de Azure pueden desbloquear 25.000 correos electrónicos gratuitos cada mes. Estos 25 000 correos electrónicos mensuales gratuitos le darán acceso a funciones avanzadas de informes y análisis, así como a [todas las API][all APIs] (Web, SMTP, Event, Parse, etc.). Para obtener información sobre los servicios adicionales que ofrece SendGrid, visite la página [Soluciones de SendGrid][SendGrid Solutions].

### <a name="to-sign-up-for-a-sendgrid-account"></a>Para registrarse y obtener una cuenta SendGrid
1. Inicie sesión en [Azure Portal][Azure portal].
2. En el menú de Azure Portal o en la página principal, seleccione **Crear un recurso**.

    ![Captura de pantalla del menú de Azure Portal con la opción Crear un recurso seleccionada.][command-bar-new]
3. Busque y seleccione **SendGrid**.

    ![Captura de pantalla de la pantalla de Marketplace de Azure Portal que muestra "SendGr" en el cuadro de búsqueda y SendGrid seleccionado en los resultados de la búsqueda.][sendgrid-store]
4. Complete el formulario de registro y seleccione **Crear**.

    ![Captura de pantalla del cuadro de diálogo Crear una nueva cuenta de SendGrid con los campos Nombre, Contraseña, Suscripción y Grupo de recursos rellenados.][sendgrid-create]
5. Escriba un **nombre** para identificar el servicio SendGrid en la configuración de Azure. Los nombres deben tener de 1 a 100 caracteres y contener únicamente caracteres alfanuméricos, guiones, puntos y caracteres de subrayado. El nombre debe ser único en la lista de elementos de la Tienda de Azure.
6. Escriba y confirme la **contraseña**.
7. Elija la **suscripción**.
8. Cree un **grupo de recursos**  o use uno existente.
9. En la sección **Plan de tarifa**, seleccione el plan de SendGrid en el que desea registrarse.

    ![Captura de pantalla del cuadro de diálogo Crear una nueva cuenta de SendGrid con la sección Elija su plan de tarifa abierto y el plan de tarifa Gratis seleccionado.][sendgrid-pricing]
10. Escriba un **código de promoción**, si tiene.
11. Escriba la **información de contacto**.
12. Revise y acepte los **términos legales**.
13. Después de confirmar la compra, verá una ventana emergente que indica que la **implementación se realizó correctamente** y se mostrará la cuenta.

    ![Captura de pantalla de la página Cuentas de SendGrid en la que aparece la nueva cuenta ContosoSendGrid.][all-resources]

    Una vez que complete la compra y haga clic en el botón **Administrar** para iniciar el proceso de comprobación de correo electrónico, recibirá un correo electrónico de SendGrid en el que se le pedirá que compruebe la cuenta. Si no recibe este correo electrónico o tiene problemas para verificar la cuenta, consulte nuestras Preguntas más frecuentes.

    ![Captura de pantalla de la página de la cuenta ContosoSendGrid con el botón Administrar resaltado.][manage]

    **Solo podrá enviar hasta 100 correos electrónicos al día hasta que compruebe la cuenta.**

    Para modificar el plan de suscripción o ver la configuración de contacto de SendGrid, haga clic en el nombre de su servicio SendGrid para abrir el panel del Marketplace de SendGrid.

    ![Captura de pantalla que muestra que la página Configuración de la cuenta ContosoSendGrid se abre seleccionando Toda la configuración en la página de cuenta de ContosoSendGrid.][settings]

    Para enviar un correo electrónico con SendGrid, debe proporcionar su clave de API.

### <a name="to-find-your-sendgrid-api-key"></a>Para encontrar su clave de API de SendGrid
1. Haga clic en **Administrar**.

    ![Captura de pantalla de la página de la cuenta ContosoSendGrid con el botón Administrar resaltado.][manage]
2. En el panel de SendGrid, seleccione **Configuración** y, luego, **Claves de API** en el menú de la izquierda.

    ![Captura de pantalla del panel de SendGrid con la lista desplegable Configuración abierta y Claves de API seleccionado.][api-keys]

3. Haga clic en **Crear clave de API**.

    ![Captura de pantalla de la pantalla Claves de API con el botón Crear clave de API seleccionado.][general-api-key]
4. Como mínimo, debe proporcionar el **nombre de esta clave** y otorgar acceso completo a **Envío de correo** y seleccione **Guardar**.

    ![Captura de pantalla de Agregar nueva clave API general con Envío de correo configurado en Acceso completo, Envíos programados configurado como Sin acceso y el botón Guardar resaltado.][access]
5. En este punto, la API aparecerá una vez. Asegúrese de almacenarla de forma segura.

### <a name="to-find-your-sendgrid-credentials"></a>Para encontrar las credenciales de SendGrid
1. Haga clic en el icono de llave para buscar el **nombre de usuario**.

    [Captura de pantalla de la página de la cuenta ContosoSendGrid con el icono Clave resaltado.][clave]
2. La contraseña es la que eligió en el momento de la configuración. Si desea hacer algún cambio, puede seleccionar **Cambiar contraseña** o **Restablecer contraseña**.

Para administrar la configuración de las opciones de entrega de correo electrónico, haga clic en el botón **Administrar**. De esta forma, se le redirigirá al panel de SendGrid.

![Captura de pantalla de la página de la cuenta ContosoSendGrid con el botón Administrar resaltado.][manage]

Para obtener más información sobre el envío de correos electrónicos a través de SendGrid, visite el artículo [Introducción a Email API][Email API Overview].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/new-addon.png
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid-store.png
[sendgrid-create]: ./media/sendgrid-sign-up/sendgrid-create.png
[sendgrid-pricing]: ./media/sendgrid-sign-up/sendgrid-pricing.png
[all-resources]: ./media/sendgrid-sign-up/all-resources.png
[manage]: ./media/sendgrid-sign-up/manage.png
[settings]: ./media/sendgrid-sign-up/settings.png
[api-keys]: ./media/sendgrid-sign-up/api-keys.png
[general-api-key]: ./media/sendgrid-sign-up/general-api-key.png
[access]: ./media/sendgrid-sign-up/access.png
[key]: ./media/sendgrid-sign-up/key.png

<!--Links-->

[SendGrid Solutions]: https://sendgrid.com/solutions
[Azure portal]: https://portal.azure.com
[SendGrid Getting Started]: http://sendgrid.com/docs
[SendGrid Provisioning Process]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[all APIs]: https://sendgrid.com/docs/API_Reference/index.html
[Email API Overview]: https://sendgrid.com/docs/API_Reference/Web_API_v3/Mail/index.html
