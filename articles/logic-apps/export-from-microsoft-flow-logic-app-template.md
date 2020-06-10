---
title: Exportación de flujos desde Power Automate en Azure Logic Apps
description: Migración de flujos desde Power Automate a Azure Logic Apps mediante su exportación como plantillas de Azure Resource Manager
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: b30a2ae8d90a193e23229dc6743c7e92ebf83b52
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84298727"
---
# <a name="export-flows-from-power-automate-and-deploy-to-azure-logic-apps"></a>Exportación de flujos desde Power Automate y su implementación en Azure Logic Apps

Para ampliar y expandir las funcionalidades de un flujo, puede migrarlo desde [Power Automate](https://flow.microsoft.com) a [Azure Logic Apps](../logic-apps/logic-apps-overview.md). Puede exportar el flujo en forma de plantilla de Azure Resource Manager para una aplicación lógica, implementar esa plantilla en un grupo de recursos de Azure y, después, abrir la aplicación en el Diseñador de aplicación lógica.

> [!NOTE]
> No todos los conectores de Power Automate están disponibles en Azure Logic Apps. Solo se pueden migrar flujos de Power Automate que tengan conectores equivalentes en Azure Logic Apps. Por ejemplo, el desencadenador Button, el conector Approvals y el conector Notification son específicos de Power Automate. En la actualidad, los flujos basados en OpenAPI de Power Automate no se admiten para la exportación e implementación como plantillas de aplicación lógica.
>
> * Para saber qué conectores de Power Automate carecen de equivalente en Logic Apps, vea [Conectores de Power Automate](https://docs.microsoft.com/connectors/connector-reference/connector-reference-powerautomate-connectors).
>
> * Para saber qué conectores de Logic Apps carecen de equivalente en Power Automate, vea [Conectores de Logic Apps](https://docs.microsoft.com/connectors/connector-reference/connector-reference-powerautomate-connectors).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* El flujo que desea exportar desde Power Automate

## <a name="export-a-flow"></a>Exportación de un flujo

1. Inicie sesión en [Power Automate](https://flow.microsoft.com) y seleccione **Mis flujos**. Busque y seleccione el flujo. En la barra de herramientas, seleccione el botón de los puntos suspensivos ( **...** ). Seleccione **Exportar** > **Plantilla de Logic Apps (.json)** .

   ![Exportar flujo](./media/export-from-microsoft-flow-logic-app-template/export-flow.png)

1. Guarde la plantilla en la ubicación que desee.

Para más información, consulte [Crecer hasta Azure Logic Apps](https://flow.microsoft.com/blog/grow-up-to-logic-apps/).

## <a name="deploy-template-by-using-the-azure-portal"></a>Implementación de una plantilla mediante Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de Azure.

1. En el menú principal de Azure, seleccione **Crear un recurso**. En el cuadro de búsqueda, escriba "implementación de plantillas". Seleccione **Implementación de plantillas (implementar mediante plantillas personalizadas)** y, después, seleccione **Crear**.

   ![Seleccionar "Implementación de plantillas"](./media/export-from-microsoft-flow-logic-app-template/select-template-deployment.png)

1. En **Implementación personalizada**, seleccione **Cree su propia plantilla en el editor**.

   ![Seleccionar "Cree su propia plantilla en el editor"](./media/export-from-microsoft-flow-logic-app-template/build-template-in-editor.png)

1. En la barra de herramientas **Editar plantilla**, seleccione **Cargar archivo**. Busque y seleccione la plantilla JSON que exportó desde Power Automate y seleccione **Abrir**.

   ![Seleccionar "Cargar archivo"](./media/export-from-microsoft-flow-logic-app-template/load-file.png)

1. Después de que el editor muestre el archivo JSON, los parámetros y los recursos de la plantilla, seleccione**Guardar**.
  
   ![Guardar plantilla](./media/export-from-microsoft-flow-logic-app-template/save-template.png)

1. Ahora, especifique estos parámetros de entrada para la plantilla:

   * Suscripción de Azure que se usa para la facturación
   * Grupo de recursos de Azure
   * Ubicación del grupo de recursos de Azure
   * Nombre del recurso de la aplicación lógica
   * Ubicación del recurso de la aplicación lógica, en caso de que sea diferente del grupo de recursos de Azure
   * El nombre de cualquier conexión creada previamente que la aplicación lógica pueda volver a usar

      Si va a crear su primera aplicación lógica, todas las conexiones se crean como nuevas, por lo que puede aceptar los nombres predeterminados. Si no es así, puede especificar los nombres de las conexiones creadas anteriormente, que puede usar en varias aplicaciones lógicas.

   Después de proporcionar esta información para la plantilla, revise y acepte los términos y condiciones de Azure Marketplace para crear los recursos necesarios de Azure y facturar su suscripción de Azure en consecuencia y, después, seleccione **Comprar**.
  
   ![Especificar los parámetros de entrada de la plantilla](./media/export-from-microsoft-flow-logic-app-template/template-input-parameters.png)

   Azure implementa la plantilla como una aplicación lógica en el grupo de recursos que especifique. Todas las aplicaciones lógicas que migre desde Power Automate se implementan en estado deshabilitado.

1. Antes de activar la aplicación lógica, autorice las nuevas conexiones, para lo que debe seguir estos pasos:

   1. Abra la aplicación lógica que creó. En el menú de la aplicación lógica, seleccione **Diseñador de aplicación lógica**.

      Todas las conexiones que requieren autorización muestran un icono de advertencia:

      ![Icono de advertencia](./media/export-from-microsoft-flow-logic-app-template/authorize-connections.png)

   1. En todos los pasos que requieran una conexión autorizada, expanda el paso y seleccione **Agregar nueva**.

      ![Agregar nueva conexión](./media/export-from-microsoft-flow-logic-app-template/add-new-connection.png)

   1. Inicie sesión en cada servicio o proporcione las credenciales necesarias para autorizar la conexión.

1. Guarde la aplicación lógica. Cuando esté listo para activar la aplicación lógica, en el menú de la aplicación lógica, seleccione **Información general** y, después, seleccione **Habilitar**.

   ![Habilitar aplicación lógica](./media/export-from-microsoft-flow-logic-app-template/enable-logic-app.png)

1. Para evitar que se ejecuten flujos de trabajo duplicados, asegúrese de desactivar o eliminar el flujo original.

## <a name="deploy-template-by-using-visual-studio"></a>Implementación de una plantilla mediante Visual Studio

Si ha configurado Visual Studio con los [requisitos previos](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites) para la creación de aplicaciones lógicas, puede implementar la plantilla exportada desde Visual Studio en Azure Logic Apps.

1. En Visual Studio, abra el archivo de plantilla que exportó desde Power Automate.

1. En Visual Studio, cree el proyecto Azure Resource Group y seleccione la plantilla **Logic App** siguiendo los pasos de [Inicio rápido: Creación y automatización de tareas, procesos y flujos de trabajo con Azure Logic Apps en Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md), por ejemplo:

   ![Creación de un proyecto de grupo de recursos de Azure](./media/export-from-microsoft-flow-logic-app-template/create-azure-resource-group-project.png)

1. En el Explorador de soluciones, abra el **archivo LogicApp.json**, en caso de que no esté abierto aún.

1. Copie el contenido de la plantilla exportada y sobrescriba el contenido del archivo **LogicApp.json.**

1. Antes de implementar la aplicación lógica, autorice las nuevas conexiones, para lo que debe seguir estos pasos:

   1. Abra el menú contextual de **LogicApp.json** y, después, seleccione **Abrir con el Diseñador de aplicación lógica**.

      ![Abrir plantilla con el Diseñador de aplicación lógica](./media/export-from-microsoft-flow-logic-app-template/open-logic-app-designer.png)

   1. Si se solicita, seleccione la suscripción de Azure y el grupo de recursos que desea usar para implementar la aplicación lógica.

      ![Seleccionar una suscripción de Azure y un grupo de recursos](./media/export-from-microsoft-flow-logic-app-template/select-azure-subscription-resource-group-deployment.png)

      Después de que la aplicación lógica aparezca en el diseñador, todas las conexiones que requieran autorización mostrarán iconos de advertencia:

      ![Conexiones con iconos de advertencia](./media/export-from-microsoft-flow-logic-app-template/authorize-connections-vs.png)

   1. En todos los pasos que requieran una conexión autorizada, expanda el paso y seleccione **Agregar nueva**.

      ![Agregar nueva conexión](./media/export-from-microsoft-flow-logic-app-template/add-new-connection-vs.png)

   1. Inicie sesión en cada servicio o proporcione las credenciales necesarias para autorizar la conexión.

   1. Guarde la solución antes de implementar la aplicación lógica.

1. En el Explorador de soluciones, abra el menú contextual del proyecto y seleccione **Implementar** > **Nuevo**. Si se le pide, inicie sesión en su cuenta de Azure.

1. Cuando se le solicite, confirme la suscripción de Azure, el grupo de recursos de Azure y cualquier otro valor que desee usar para la implementación, como el [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md) que se usará para pasar los valores de los parámetros de la plantilla y, después, seleccione **Implementar**.

   ![Confirmar valores de implementación](./media/export-from-microsoft-flow-logic-app-template/confirm-azure-subscription-resource-group-deployment.png)

1. Si el cuadro **Editar parámetros** aparece, especifique el nombre del recurso de la aplicación lógica en Azure y seleccione **Guardar**.  

   ![Editar parámetros de implementación](./media/export-from-microsoft-flow-logic-app-template/edit-parameters-deployment.png)

   Cuando se inicia la implementación, aparece el estado de la implementación de la aplicación en la ventana **Salida** de Visual Studio. Si no aparece el estado, abra la lista **Mostrar salida de** y seleccione el grupo de recursos de Azure. Por ejemplo:

   ![Ventana de salida](./media/export-from-microsoft-flow-logic-app-template/output-window.png)

   Si las conexiones de la aplicación lógica necesitan que el usuario escriba datos, se abrirá una ventana de PowerShell en segundo plano y le solicitará las contraseñas o claves secretas necesarias. Después de escribir esta información, la implementación continúa.

   ![Autenticar conexiones](./media/export-from-microsoft-flow-logic-app-template/logic-apps-powershell-window.png)

   Una vez finalizada la implementación, la aplicación lógica se publica, pero no está activada en Azure Portal.

1. Cuando esté listo para activar la aplicación lógica en Azure Portal, busque la aplicación lógica y ábrala en el Diseñador de aplicación lógica. En el menú de la aplicación lógica, seleccione **Información general** y, después, seleccione **Habilitar**.

1. Para evitar que se ejecuten flujos de trabajo duplicados, asegúrese de desactivar o eliminar el flujo original.

Para más información acerca de estos pasos de implementación, consulte [Inicio rápido: Creación y automatización de tareas, procesos y flujos de trabajo con Azure Logic Apps en Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-to-Azure)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre [conectores para Azure Logic Apps](../connectors/apis-list.md)
* Obtenga más información sobre [Azure Logic Apps](../logic-apps/logic-apps-overview.md).
