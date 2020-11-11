---
title: Comprobación de aplicaciones lógicas con datos simulados
description: Configure resultados estáticos para comprobar aplicaciones lógicas con datos simulados, sin que ello afecte a los entornos de producción.
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 05/13/2019
ms.openlocfilehash: 711d753203aeaeba50cea692053a37fcab2e9c7b
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "93027710"
---
# <a name="test-logic-apps-with-mock-data-by-setting-up-static-results"></a>Probar aplicaciones lógicas con datos simulados mediante la configuración de resultados estáticos

Al probar sus aplicaciones lógicas, podría no estar listo para llamar o tener acceso de facto a las aplicaciones, servicios y sistemas por diversos motivos. Normalmente, en estos casos puede que deba ejecutar rutas de acceso de distinta condición, forzar errores, especificar cuerpos de respuesta de mensaje específicos o, incluso, intentar omitir algunos pasos. Si se configuran resultados estáticos de una acción en la aplicación lógica, se pueden simular datos de salida de esa acción. Habilitar los resultados de estático de una acción no hace que la acción se ejecute, sino que devuelve datos simulados.

Por ejemplo, si se configuran resultados estáticos de la acción de envío de Outlook 365, el motor de Logic Apps devolverá únicamente los datos ficticios que se hayan especificado como resultados estáticos; no llamará a Outlook ni enviará un correo electrónico.

## <a name="prerequisites"></a>Prerequisites

* Suscripción a Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica donde quiera configurar resultados estáticos.

<a name="set-up-static-results"></a>

## <a name="set-up-static-results"></a>Configurar resultados estáticos

1. Si aún no lo ha hecho, en [Azure Portal](https://portal.azure.com), abra la aplicación lógica en el diseñador de Logic Apps.

1. Haga lo siguiente en la acción donde quiera configurar resultados estáticos: 

   1. En la esquina superior derecha de la acción, elija el botón de puntos suspensivos ( *...* ) y, después, seleccione **Resultado estático**.

      ![Selección de "Resultado estático" > "Habilitar el resultado estático"](./media/test-logic-apps-mock-data-static-results/select-static-result.png)

   1. Seleccione **Habilitar el resultado estático**. En las propiedades obligatorias (*), especifique los valores de salida ficticios que quiera devolver en la respuesta de la acción.

      Por ejemplo, estas son las propiedades obligatorias en la acción HTTP:

      | Propiedad | Descripción |
      |----------|-------------|
      | **Estado** | Estado de la acción que se va a devolver. |
      | **Código de estado** | Código de estado específico que se va a devolver. |
      | **Encabezados** | Contenido de encabezado que se va a devolver. |
      |||

      ![Selección de "Habilitar el resultado estático"](./media/test-logic-apps-mock-data-static-results/enable-static-result.png)

      Para escribir los datos simulados con formato de notación de objetos JavaScript (JSON), elija **Cambiar al modo JSON** (![Selección de "Cambiar al modo JSON"](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)).

   1. En las propiedades opcionales, abra la lista **Seleccionar campos opcionales** y elija las propiedades que quiera simular.

      ![Seleccionar propiedades opcionales](./media/test-logic-apps-mock-data-static-results/optional-properties.png)

1. Cuando esté listo para guardar, elija **Listo**.

   En la esquina superior derecha de la acción, la barra de título ahora muestra un icono de vaso de precipitación de prueba (![Icono de resultados estáticos](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)), lo que indica que los resultados estáticos se han habilitado.

   ![Icono que muestra los resultados estáticos habilitados](./media/test-logic-apps-mock-data-static-results/static-results-enabled.png)

   Para buscar ejecuciones anteriores donde se hayan usado datos simulados, vea [Buscar ejecuciones donde se usan resultados estáticos](#find-runs-mock-data) más adelante en este tema.

<a name="reuse-sample-outputs"></a>

## <a name="reuse-previous-outputs"></a>Reutilizar salidas anteriores

Si la aplicación lógica tiene una ejecución anterior con salidas que se puede volver a usar como salidas simuladas, puede copiar y pegar los resultados de esa ejecución.

1. Si aún no lo ha hecho, en [Azure Portal](https://portal.azure.com), abra la aplicación lógica en el diseñador de Logic Apps.

1. En el menú principal de la aplicación lógica, seleccione **Introducción**.

1. En la sección **Historial de ejecuciones** , seleccione la ejecución de aplicación lógica que quiera.

1. En el flujo de trabajo de la aplicación lógica, busque y expanda la acción que tiene los resultados que quiera.

1. Elija el vínculo **Mostrar salidas sin procesar**.

1. Copiar el objeto de notación de objetos JavaScript (JSON) completo o la subsección específica que quiera usar, por ejemplo, la sección de salidas o, incluso, únicamente la sección de encabezados.

1. Realice el procedimiento para abrir el cuadro **Resultado estático** de la acción descrito en la sección [Configurar resultados estáticos](#set-up-static-results).

1. Cuando se abra el cuadro **Resultado estático** , elija uno de estos dos pasos:

   * Para pegar un objeto JSON completo, elija **Cambiar al modo JSON** (![Selección de "Cambiar al modo JSON"](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)):

     ![Selección de "Cambiar al modo JSON" de un objeto completo](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-complete.png)

   * Para pegar simplemente una sección JSON, elija **Cambiar al modo JSON** junto a la etiqueta de la sección en cuestión, por ejemplo:

     ![Selección de "Cambiar al modo JSON" de salidas](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-outputs.png)

1. En el editor de JSON, pegue el JSON copiado previamente.

   ![Modo JSON](./media/test-logic-apps-mock-data-static-results/json-editing-mode.png)

1. Cuando haya finalizado, elija **Listo**. Si lo que quiere es volver al diseñador, elija **Cambiar al modo de editor** (![Selección de "Cambiar al modo de editor"](./media/test-logic-apps-mock-data-static-results/switch-editor-mode-button.png)).

<a name="find-runs-mock-data"></a>

## <a name="find-runs-that-use-static-results"></a>Buscar ejecuciones donde se usan resultados estáticos

En el historial de ejecuciones de la aplicación lógica se identifican las ejecuciones donde las acciones usan resultados estáticos. Haga lo siguiente para encontrar esas ejecuciones:

1. En el menú principal de la aplicación lógica, seleccione **Introducción**. 

1. En el panel derecho, bajo **Historial de ejecuciones** , busque la columna **Resultados estáticos**. 

   Las ejecuciones que incluyan acciones con resultados tienen la columna **Resultados estáticos** establecida en **Habilitado** , por ejemplo:

   ![Historial de ejecuciones: columna Resultados estáticos](./media/test-logic-apps-mock-data-static-results/run-history.png)

1. Para ver las acciones que usan resultados estáticos, seleccione la ejecución que quiera donde la columna **Resultados estáticos** esté establecida en **Habilitado**.

   Las acciones que usan resultados estáticos van acompañadas del icono de vaso de precipitación de prueba (![Icono de resultados estáticos](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)), por ejemplo:

   ![Historial de ejecuciones: acciones que usan resultados estáticos](./media/test-logic-apps-mock-data-static-results/static-results-enabled-run-details.png)

## <a name="disable-static-results"></a>Deshabilitar resultados estáticos

Si los resultados estáticos se desactivan, los valores de la última configuración no se pierden; por lo tanto, la próxima vez que se activen los resultados estáticos, podrá seguir usando los valores anteriores.

1. Busque la acción donde quiera deshabilitar las salidas estáticas. En la esquina superior derecha de la acción, seleccione el icono de vaso de precipitación de prueba (![Icono de resultados estáticos](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)).

   ![Captura de pantalla en la que se muestra una acción HTTP donde puede seleccionar el icono de vaso de precipitación.](./media/test-logic-apps-mock-data-static-results/disable-static-results.png)

1. Seleccione **Deshabilitar el resultado estático** > **Listo**.

   ![Captura de pantalla en la que se muestra la opción Deshabilitar el resultado estático que se puede seleccionar.](./media/test-logic-apps-mock-data-static-results/disable-static-results-button.png)

## <a name="reference"></a>Referencia

Para obtener más información sobre esta configuración en las definiciones de flujo de trabajo subyacentes, vea [Resultados estáticos: referencia de esquema de lenguaje de definición de flujo de trabajo](../logic-apps/logic-apps-workflow-definition-language.md#static-results) y [runtimeConfiguration.staticResult: opciones de configuración en tiempo de ejecución](../logic-apps/logic-apps-workflow-actions-triggers.md#runtime-configuration-settings).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [Azure Logic Apps](../logic-apps/logic-apps-overview.md).