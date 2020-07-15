---
title: Creación del primer flujo de trabajo automatizado
description: 'Inicio rápido: Cree el primer flujo de trabajo automatizado mediante Azure Logic Apps para soluciones de integración de aplicaciones empresariales (EAI) e integración de sistemas'
services: logic-apps
ms.suite: integration
ms.reviewer: logicappspm
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/20/2019
ms.openlocfilehash: 0b3559a27fe9fae6c34b07c648a289d205560bd8
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85321593"
---
# <a name="quickstart-create-your-first-workflow-by-using-azure-logic-apps---azure-portal"></a>Inicio rápido: Creación del primer flujo de trabajo mediante Azure Logic Apps: Azure Portal

En esta guía de inicio rápido se presentan los conceptos generales básicos sobre cómo compilar el primer flujo de trabajo mediante [Azure Logic Apps](../logic-apps/logic-apps-overview.md), como la creación de una aplicación lógica en blanco, la adición de un desencadenador y una acción y, a continuación, la prueba de la aplicación lógica. En esta guía de inicio rápido, creará una aplicación lógica que comprobará periódicamente la fuente RSS de un sitio web para detectar nuevos elementos. Si existe algún elemento nuevo, la aplicación lógica envía un correo electrónico por cada elemento. Cuando haya terminado, la aplicación lógica se parecerá a este flujo de trabajo, en un alto nivel:

![Flujo de trabajo de aplicación lógica de ejemplo de alto nivel](./media/quickstart-create-first-logic-app-workflow/quickstart-workflow-overview.png)

Para este escenario, necesita una suscripción a Azure o [registrarse para una cuenta de Azure gratis](https://azure.microsoft.com/free/), una cuenta de correo electrónico de un servicio compatible con Azure Logic Apps, como Office 365 Outlook, Outlook.com o Gmail. Para otros servicios de correo electrónico compatibles, [revise la lista de conectores aquí](https://docs.microsoft.com/connectors/). En este ejemplo, la aplicación lógica usa cuenta de Office 365 Outlook. Si usa un servicio de correo electrónico diferente, los pasos generales son los mismos, pero la interfaz de usuario podría diferir ligeramente.

> [!IMPORTANT]
> Si quiere usar el conector de Gmail, solo las cuentas empresariales de G-Suite pueden usar este conector sin restricciones en las aplicaciones lógicas. Si tiene una cuenta de consumidor de Gmail, puede usar este conector solo con servicios específicos aprobados por Google o puede [crear una aplicación cliente de Google para usarla en la autenticación con el conector de Gmail](https://docs.microsoft.com/connectors/gmail/#authentication-and-bring-your-own-application). Para más información, consulte [Directivas de privacidad y seguridad de datos de los conectores de Google en Azure Logic Apps](../connectors/connectors-google-data-security-privacy-policy.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com) con sus credenciales de su cuenta de Azure.

## <a name="create-your-logic-app"></a>Creación de una aplicación lógica

1. En la página principal de Azure, en el cuadro de búsqueda, busque y seleccione **Logic Apps**.

   ![Buscar y seleccionar "Logic Apps"](./media/quickstart-create-first-logic-app-workflow/find-select-logic-apps.png)

1. En la página **Logic Apps**, seleccione en **Agregar**.

   ![Incorporación de una nueva aplicación lógica](./media/quickstart-create-first-logic-app-workflow/add-new-logic-app.png)

1. En el panel **Aplicación lógica**, proporcione los detalles sobre la aplicación lógica, como se muestra a continuación. Seleccione **Crear** cuando haya terminado.

   ![Especificar los detalles de la nueva aplicación lógica](./media/quickstart-create-first-logic-app-workflow/create-logic-app-settings.png)

   | Propiedad | Value | Descripción |
   |----------|-------|-------------|
   | **Nombre** | <*nombre-de-la-aplicación-lógica*> | El nombre de la aplicación lógica, que solo puede contener letras, números,guiones (`-`), caracteres de subrayado (`_`), paréntesis (`(`,`)`) y puntos (`.`). En este ejemplo se usa "My-First-Logic-App". <p><p>**Nota**: Los nombres de las aplicaciones lógicas deben ser únicos en todas las regiones. |
   | **Suscripción** | <*Azure-subscription-name*> | El nombre de la suscripción de Azure |
   | **Grupos de recursos** | <*nombre del grupo de recursos de Azure*> | Nombre del [grupo de recursos de Azure](../azure-resource-manager/management/overview.md) que se utiliza para organizar recursos relacionados. En este ejemplo se usa "My-First-LA-RG". <p><p>**Nota**: Los nombres de los grupos de recursos deben ser exclusivos entre regiones. |
   | **Ubicación** | <*Azure-region*> | La región en la que desea almacenar la información de la aplicación lógica. En este ejemplo se utiliza "Oeste de EE. UU.". |
   | **Log Analytics** | Off | Mantenga el valor **Off** para el registro de diagnóstico. |
   ||||

1. Una vez que Azure implemente la aplicación, en la barra de herramientas de Azure, seleccione **Notificaciones** > **Ir al recurso** para la aplicación lógica implementada.

   ![Ir al recurso de aplicación lógica recién creado](./media/quickstart-create-first-logic-app-workflow/go-to-new-logic-app-resource.png)

   O también puede buscar y seleccionar la aplicación lógica escribiendo el nombre en el cuadro de búsqueda.

   El Diseñador de aplicación lógica se abre y muestra una página con un vídeo de introducción y desencadenadores utilizados frecuentemente. En **Plantillas**, elija **Blank Logic App**.

   ![Seleccionar una plantilla en blanco para la aplicación lógica](./media/quickstart-create-first-logic-app-workflow/choose-logic-app-template.png)

A continuación, añada un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts) que se active cuando aparezca un nuevo elemento en la fuente RSS. Cada aplicación lógica debe comenzar con un desencadenador, que se activa cuando sucede un evento específico o cuando se cumple una condición determinada. Cada vez que el desencadenador se activa, el motor de Azure Logic Apps crea una instancia de aplicación lógica que inicia y ejecuta el flujo de trabajo.

<a name="add-rss-trigger"></a>

## <a name="add-the-rss-trigger"></a>Agregar el desencadenador de RSS

1. En el **Diseñador de aplicación lógica**, debajo del cuadro de búsqueda, seleccione **Todo**.

1. En el cuadro de búsqueda, escriba `rss` para buscar el conector de RSS. En la lista de desencadenadores, seleccione el desencadenador**Cuando se publica un elemento de fuente**.

   ![Seleccionar el desencadenador "Cuando se publica un elemento de fuente"](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-new-feed-item.png)

1. Proporcione la información del desencadenador tal y como se describe en la tabla:

   ![Configuración del desencadenador con la fuente RSS, la frecuencia y el intervalo](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-settings.png)

   | Propiedad | Value | Descripción |
   |----------|-------|-------------|
   | **URL de fuente RSS** | <*URL-de-fuente-RSS*> | Vínculo de la fuente RSS que desea supervisar En este ejemplo se usa `http://feeds.reuters.com/reuters/topNews`. |
   | **Intervalo** | 1 | Número de intervalos que se espera entre comprobaciones |
   | **Frecuencia** | Minute | La unidad de tiempo de cada intervalo entre comprobaciones  |
   ||||

   Juntos, el intervalo y la frecuencia definen la programación para el desencadenador de la aplicación lógica. Esta aplicación lógica comprueba la fuente cada minuto.

1. Para contraer los detalles del desencadenador por ahora, haga clic dentro de la barra de título del desencadenador.

   ![Contraer la forma de la aplicación lógica para ocultar los detalles](./media/quickstart-create-first-logic-app-workflow/collapse-trigger-shape.png)

1. Guarde la aplicación lógica. En la barra de herramientas del diseñador, seleccione **Save** (Guardar).

La aplicación lógica estará activa, pero no hace más que comprobar la fuente RSS. Por lo tanto, agregue una acción que responda cuando se active el desencadenador.

## <a name="add-the-send-email-action"></a>Adición de la acción "enviar correo electrónico"

Ahora, agregue una [acción](../logic-apps/logic-apps-overview.md#logic-app-concepts) que envíe un correo electrónico cuando aparezca un nuevo elemento en la fuente RSS.

1. En el desencadenador **Cuando se publica un elemento de fuente**, elija **Nuevo paso**.

   ![En el desencadenador, seleccione "Nuevo paso"](./media/quickstart-create-first-logic-app-workflow/add-new-step-under-trigger.png)

1. En **Elegir una acción** y en el cuadro de búsqueda, seleccione **Todas**.

1. En el cuadro de búsqueda, escriba `send an email` para buscar conectores que ofrezcan esta acción. En la lista de acciones, seleccione la acción "enviar un correo electrónico" del servicio de correo electrónico que quiera usar. En este ejemplo se usa el conector de Office 365 Outlook, que tiene la acción **Enviar un correo electrónico**.

   ![Seleccionar la acción "enviar un correo electrónico" para Office 365 Outlook](./media/quickstart-create-first-logic-app-workflow/add-action-send-email.png)

   Para filtrar la lista de acciones de una aplicación o un servicio específicos, puede seleccionar la aplicación o servicio primero:

   * Para las cuentas profesionales o educativas de Azure, seleccione Office 365 Outlook.
   * Para las cuentas de Microsoft personales, seleccione Outlook.com.

1. Si el conector de correo electrónico seleccionado le pide que autentique su identidad, complete ese paso ahora para crear una conexión entre la aplicación lógica y el servicio de correo electrónico.

   > [!NOTE]
   > En este ejemplo en concreto, la identidad se autentica manualmente. Sin embargo, los conectores que requieren autenticación difieren en los tipos de autenticación que admiten. También tiene opciones para configurar la manera en que desea controlar la autenticación. Por ejemplo, si se usan plantillas de Azure Resource Manager para la implementación, es posible parametrizar y mejorar la seguridad de las entradas que se cambian con frecuencia o con facilidad, como la información de la conexión. Para más información, consulte los temas siguientes:
   >
   > * [Parámetros de plantilla para la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#template-parameters)
   > * [Autorización de conexiones de OAuth](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md#authorize-oauth-connections)
   > * [Autenticación y acceso con identidades administradas](../logic-apps/create-managed-service-identity.md)
   > * [Autenticación de conexiones para la implementación de aplicaciones lógicas](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections)

1. En la acción **Enviar correo electrónico**, especifique los datos que desea que el correo electrónico incluya.

   1. En el cuadro **Para**, escriba la dirección de correo electrónico del destinatario. Para realizar pruebas, puede usar su dirección de correo electrónico.

      Por ahora, ignore la lista **Agregar contenido dinámico** que aparece. Al hacer clic en algunos cuadros de edición, esta lista aparece y muestra los parámetros disponibles del paso anterior que se pueden incluir como entradas en el flujo de trabajo.

   1. En el cuadro **Asunto**, escriba este texto con un espacio en blanco final: `New RSS item: `

      ![En la propiedad "Asunto", escriba el asunto del correo electrónico](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-subject.png)

   1. En la lista **Agregar contenido dinámico**, seleccione **Título de fuente** para incluir el título del elemento RSS.

      ![Seleccione de "Título de fuente" en la lista de contenido dinámico](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-subject-dynamic-content.png)

      Cuando haya terminado, el asunto del correo electrónico será similar a este ejemplo:

      ![Ejemplo de asunto de correo electrónico finalizado para el título de fuente agregado](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-feed-title.png)

      Si un bucle "Para cada uno" aparece en el diseñador, habrá seleccionado un token de una matriz, como el token **categorías-elemento**. Para este tipo de token, el diseñador añade automáticamente este bucle alrededor de la acción que hace referencia a ese token. De este modo, la aplicación lógica realiza la misma acción en cada elemento de la matriz. Para quitar el bucle, elija el botón de  **puntos suspensivos** ( **...** ) de la barra de título del bucle y luego **Eliminar**.

   1. En el cuadro **Cuerpo**, escriba este texto y seleccione estos tokens para el cuerpo del correo electrónico. Para agregar líneas en blanco en el cuadro de edición, presione Mayús + Entrar.

      ![Seleccionar las propiedades del contenido del cuerpo del correo electrónico](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-body.png)

      | Propiedad | Descripción |
      |----------|-------------|
      | **Título de fuente** | Título del elemento |
      | **Fuente publicada el** | Fecha y hora de publicación del elemento |
      | **Vínculo de fuente principal** | Dirección URL del elemento |
      |||

1. Guarde la aplicación lógica.

A continuación, pruebe la aplicación lógica.

## <a name="run-your-logic-app"></a>Ejecución de la aplicación lógica

Para iniciar manualmente la aplicación lógica, en la barra de herramientas del diseñador, elija **Ejecutar**. También puede esperar a que la aplicación lógica compruebe la fuente RSS basada en la programación especificada (cada minuto). Si la fuente RSS tiene nuevos elementos, la aplicación lógica envía un correo electrónico para cada uno de ellos. En caso contrario, la aplicación lógica espera hasta el siguiente intervalo antes de volver a comprobar. Si no obtiene los mensajes de correo electrónico, compruebe la carpeta de correo electrónico no deseado.

Por ejemplo, este es un correo electrónico de ejemplo que envía esta aplicación lógica.

![Correo electrónico de ejemplo que se envía cuando aparece un elemento de canal RSS nuevo](./media/quickstart-create-first-logic-app-workflow/monitor-rss-feed-email.png)

Técnicamente, cuando el desencadenador comprueba la fuente RSS y encuentra nuevos elementos, el desencadenador se activa y el motor de Azure Logic Apps crea una instancia de flujo de trabajo de la aplicación lógica que ejecuta las acciones en el flujo de trabajo. Si el desencadenador no encuentra nuevos elementos, no se activa y "omite" crear instancias de flujo de trabajo.

Enhorabuena, acaba de compilar y ejecutar correctamente su primera aplicación lógica con Azure Portal.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite este ejemplo, elimine el grupo de recursos que contiene la aplicación lógica y los recursos relacionados.

1. En el menú principal de Azure, seleccione **Grupos de recursos** y después el grupo de recursos de la aplicación lógica. En el panel **Información general**, elija **Eliminar grupo de recursos**.

   ![Buscar, seleccionar y eliminar un grupo de recursos](./media/quickstart-create-first-logic-app-workflow/delete-resource-group.png)

1. Cuando aparezca el panel de confirmación, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

   ![Para confirmar la eliminación, seleccione "Eliminar"](./media/quickstart-create-first-logic-app-workflow/delete-resource-group-2.png)

> [!NOTE]
> Cuando se elimina una aplicación lógica, no se crean instancias de nuevas ejecuciones. Todas las ejecuciones nuevas y pendientes se cancelan. Si tiene miles de ejecuciones, la cancelación puede tardar bastante tiempo en completarse.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado su primera aplicación lógica que comprueba si hay actualizaciones de RSS según la programación especificada (cada minuto) y realiza una acción (envía un correo electrónico) cuando hay actualizaciones. Para más información, continúe con este tutorial para crear flujos de trabajo basados en programación más avanzados:

> [!div class="nextstepaction"]
> [Comprobación del tráfico con una aplicación lógica basada en una programación](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
