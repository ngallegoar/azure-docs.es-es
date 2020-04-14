---
title: Administración de aplicaciones lógicas en Azure Portal
description: Edite, habilite, deshabilite o elimine aplicaciones lógicas mediante Azure Portal.
services: logic-apps
ms.suite: integration
author: lauradolan
ms.author: ladolan
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.custom: mvc
ms.date: 04/06/2020
ms.openlocfilehash: a35f34ed27299077dca78630511ea3c291caff60
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "80805214"
---
# <a name="manage-logic-apps-in-the-azure-portal"></a>Administración de aplicaciones lógicas en Azure Portal

Puede administrar aplicaciones lógicas mediante [Azure Portal](https://portal.azure.com) o [Visual Studio](manage-logic-apps-with-visual-studio.md). En este artículo se muestra cómo editar, deshabilitar, habilitar o eliminar aplicaciones lógicas en Azure Portal. Si no está familiarizado con Azure Logic Apps, consulte [¿Qué es Azure Logic Apps?](logic-apps-overview.md)

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Elija una aplicación lógica existente. Para obtener información sobre cómo crear una aplicación lógica mediante Azure Portal, consulte [Inicio rápido: Creación del primer flujo de trabajo mediante Azure Logic Apps: Azure Portal](quickstart-create-first-logic-app-workflow.md).

<a name="find-logic-app"></a>

## <a name="find-your-logic-apps"></a>Búsqueda de las aplicaciones lógicas

Para buscar y abrir la aplicación lógica, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de Azure.

1. En la barra de búsqueda de Azure, escriba `logic apps` y seleccione **Logic Apps**.

   ![Buscar y seleccionar "Logic Apps"](./media/manage-logic-apps-with-azure-portal/find-select-logic-apps.png)

1. En la página **Logic Apps**, busque y seleccione la aplicación lógica que desea administrar.

   Una vez que se abra el panel **Información general**, puede filtrar la lista que aparece en la página **Logic Apps** de las siguientes maneras:

   * Buscar aplicaciones lógicas por nombre
   * Filtrar las aplicaciones lógicas por suscripción, grupo de recursos, ubicación y etiquetas
   * Agrupar las aplicaciones lógicas por grupo de recursos, tipo, suscripción y ubicación

## <a name="view-logic-app-properties"></a>Visualización de las propiedades de la aplicación lógica

1. En Azure Portal, [busque y abra la aplicación lógica](#find-logic-app).

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Propiedades**.

1. En el panel **Propiedades**, puede ver y copiar la siguiente información sobre la aplicación lógica:

   * **Nombre**
   * **Identificador del recurso**
   * **Grupos de recursos**
   * **Ubicación**
   * **Tipo** 
   * **Subscription Name**
   * **Subscription ID**
   * **Punto de conexión de acceso**
   * **Direcciones IP salientes en tiempo de ejecución**
   * **Direcciones IP de puntos de conexión de acceso**
   * **Direcciones IP salientes del conector**

## <a name="disable-or-enable-logic-apps"></a>Deshabilitación o habilitación de la aplicaciones lógicas

Puede habilitar o deshabilitar una [sola aplicación lógica](#disable-enable-single-logic-app) o [varias aplicaciones lógicas a la vez](#disable-or-enable-multiple-logic-apps) en el Azure Portal. También puede [habilitar o deshabilitar aplicaciones lógicas en Visual Studio](manage-logic-apps-with-visual-studio.md#disable-or-enable-logic-app).

La deshabilitación de la aplicación lógica afecta a las instancias de flujo de trabajo y se ejecuta de estas maneras:

* Todas las ejecuciones en curso y pendientes continuarán hasta que finalicen. En función del número de ejecuciones, este proceso puede tardar algún tiempo.

* El motor de Logic Apps no creará ni ejecutará nuevas instancias de flujo de trabajo.

* El desencadenador no se activará la próxima vez que se cumplan sus condiciones.

* El estado del desencadenador recuerda el punto en el que se detuvo la aplicación lógica. Por lo tanto, si vuelve a habilitar la aplicación lógica, el desencadenador se activará para todos los elementos no procesados desde la última ejecución.

  Para que la aplicación lógica no se active para los elementos no procesados desde la última ejecución, borre el estado del desencadenador antes de volver a habilitar la aplicación lógica:

  1. En Azure Portal, [busque y abra la aplicación lógica](#find-logic-app).

  1. Edite cualquier parte del desencadenador de la aplicación lógica.

  1. Guarde los cambios. Este paso restablece el estado actual del desencadenador.

  1. [Vuelva a habilitar la aplicación lógica](#disable-enable-single-logic-app).

<a name="disable-enable-single-logic-app"></a>

### <a name="disable-or-enable-single-logic-app"></a>Deshabilitación o habilitación de una sola aplicación lógica

1. En Azure Portal, [busque y abra la aplicación lógica](#find-logic-app).

1. En el menú de la aplicación lógica, seleccione **Información general**. Elija una de estas opciones:

   * En la barra de herramientas, seleccione **Deshabilitar**.

     ![Deshabilitación de una sola aplicación lógica en Azure Portal](./media/manage-logic-apps-with-azure-portal/disable-single-logic-app.png)

     Si la aplicación lógica ya está deshabilitada, solo verá la opción **Habilitar**.

   * En la barra de herramientas, seleccione **Enable** (Habilitar).

     ![Habilitación de una sola aplicación lógica en Azure Portal](./media/manage-logic-apps-with-azure-portal/enable-single-logic-app.png)

     Si la aplicación lógica ya está habilitada, solo verá la opción **Deshabilitar**. 

   En Azure Portal se muestra una notificación en la barra de herramientas principal de Azure que confirma si la operación se realizó correctamente o no.

   ![Notificación para confirmar el estado de la operación](./media/manage-logic-apps-with-azure-portal/operation-confirmation-notification.png)

<a name="disable-or-enable-multiple-logic-apps"></a>

### <a name="disable-or-enable-multiple-logic-apps"></a>Deshabilitación o habilitación de varias aplicaciones lógicas

1. En el Azure Portal, [busque las aplicaciones lógicas](#find-logic-app) que desea deshabilitar o habilitar.

1. Para comprobar si una aplicación lógica está habilitada o deshabilitada actualmente, en la página **Logic Apps**, consulte la columna **Estado** de esa aplicación lógica. 

   ![Columna de estado de Logic Apps](./media/manage-logic-apps-with-azure-portal/view-logic-app-status.png)

   Si la columna **Estado** no está visible, en la barra de herramientas de **Logic Apps**, seleccione **Probar la versión preliminar**.

   ![Activación de la versión preliminar](./media/manage-logic-apps-with-azure-portal/select-try-preview.png)

1. En la columna de casillas, seleccione las aplicaciones lógicas que quiere deshabilitar o habilitar. En la barra de herramientas, seleccione **Deshabilitar** o **Habilitar**.

   ![Habilitación o deshabilitación de varias aplicaciones lógicas en Azure Portal](./media/manage-logic-apps-with-azure-portal/enable-disable-multiple-logic-apps.png)

1. Cuando aparezca el cuadro de confirmación, seleccione **Sí** para continuar.

   En Azure Portal se muestra una notificación en la barra de herramientas principal de Azure que confirma si la operación se realizó correctamente o no.

## <a name="delete-logic-apps"></a>Eliminación de aplicaciones lógicas

Puede [eliminar una sola aplicación lógica](#delete-single-logic-app) o [eliminar varias aplicaciones lógicas a la vez](#delete-multiple-logic-apps) en el Azure Portal. También puede [eliminar la aplicación lógica en Visual Studio](manage-logic-apps-with-visual-studio.md#delete-your-logic-app).

La eliminación de la aplicación lógica afecta a las instancias de flujo de trabajo de estas maneras:

* Todas las ejecuciones en curso y pendientes continuarán hasta que finalicen. En función del número de ejecuciones, este proceso puede tardar algún tiempo.

* El motor de Logic Apps no creará ni ejecutará nuevas instancias de flujo de trabajo.

<a name="delete-single-logic-app"></a>

### <a name="delete-single-logic-app"></a>Eliminación de una sola aplicación lógica

1. En Azure Portal, [busque y abra la aplicación lógica](#find-logic-app).

1. En el menú de la aplicación lógica, seleccione **Información general**. En la barra de herramientas de la aplicación lógica, seleccione **Eliminar**.

   ![En la barra de herramientas de la aplicación lógica, seleccione "Eliminar"](./media/manage-logic-apps-with-azure-portal/delete-single-logic-app.png)

1. Cuando aparezca el cuadro de confirmación, escriba el nombre de la aplicación lógica y seleccione **Eliminar**.

   ![Confirmación de la eliminación de la aplicación lógica](./media/manage-logic-apps-with-azure-portal/delete-confirmation-single-logic-app.png)

   En Azure Portal se muestra una notificación en la barra de herramientas principal de Azure que confirma si la operación se realizó correctamente o no.

<a name="delete-multiple-logic-apps"></a>

### <a name="delete-multiple-logic-apps"></a>Eliminación de varias aplicaciones lógicas

1. En el Azure Portal, [busque las aplicaciones lógicas que desea eliminar](#find-logic-app).

1. En la columna de casillas, seleccione las aplicaciones lógicas que quiere eliminar. En la barra de herramientas, seleccione **Eliminar**.

   ![Eliminación de varias aplicaciones lógicas](./media/manage-logic-apps-with-azure-portal/delete-multiple-logic-apps.png)

1. Cuando aparezca el cuadro de confirmación, escriba `yes` y seleccione **Eliminar**.

   ![Confirmación de la eliminación de las aplicaciones lógicas](./media/manage-logic-apps-with-azure-portal/delete-confirmation-multiple-logic-apps.png)

   En Azure Portal se muestra una notificación en la barra de herramientas principal de Azure que confirma si la operación se realizó correctamente o no.

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión de aplicaciones lógicas](monitor-logic-apps.md)
