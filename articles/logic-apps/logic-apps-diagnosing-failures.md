---
title: Solución de problemas y diagnóstico de errores de flujo de trabajo
description: Más información sobre cómo solucionar y diagnosticar problemas y errores en sus flujos de trabajo en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/31/2020
ms.openlocfilehash: 1f83f13564a64a0d9d8a5e0144ca95af6a769d6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76904978"
---
# <a name="troubleshoot-and-diagnose-workflow-failures-in-azure-logic-apps"></a>Solución de problemas y diagnóstico de errores de flujo de trabajo en Azure Logic Apps

La aplicación lógica genera información que puede ayudarle a diagnosticar y depurar los problemas de la aplicación. Puede diagnosticar una aplicación lógica revisando cada paso del flujo de trabajo a través de Azure Portal. O bien, puede agregar algunos pasos a un flujo de trabajo para la depuración en tiempo de ejecución.

<a name="check-trigger-history"></a>

## <a name="check-trigger-history"></a>Comprobación del historial de desencadenadores

Cada ejecución de una aplicación lógica comienza con el intento de un desencadenador. Por lo tanto, si no se activa el desencadenador, siga estos pasos:

1. [Compruebe el historial de desencadenadores](../logic-apps/monitor-logic-apps.md#review-trigger-history) para confirmar el estado del desencadenador. Para ver más información sobre el intento del desencadenador, seleccione ese evento de desencadenador, por ejemplo:

   ![Visualización del estado del desencadenador y el historial de desencadenadores](./media/logic-apps-diagnosing-failures/logic-app-trigger-history.png)

1. Compruebe las entradas del desencadenador para confirmar que aparecen según lo esperado. En **Vínculos de entrada**, seleccione el vínculo, que muestra el panel **Entradas**.

   Las entradas del desencadenador incluyen los datos que el desencadenador espera y necesita para iniciar el flujo de trabajo. Revisar estas entradas puede ayudarle a determinar si son correctas y si se ha cumplido la condición para que el flujo de trabajo pueda continuar.

   Por ejemplo, la propiedad `feedUrl` tiene aquí un valor de fuente RSS incorrecto:

   ![Revisión de errores de las entradas del desencadenador](./media/logic-apps-diagnosing-failures/review-trigger-inputs-for-errors.png)

1. Compruebe las salidas del desencadenador, si las hay, para confirmar que aparecen según lo esperado. En **Vínculo de salidas**, seleccione el vínculo, que muestra el panel **Salidas**.

   Entre las salidas del desencadenador se incluyen los datos que el desencadenador pasa al siguiente paso del flujo de trabajo. Revisar estas salidas puede ayudarle a determinar si los valores correctos o esperados se han pasado al siguiente paso del flujo de trabajo, por ejemplo:

   ![Revisión de errores de las salidas del desencadenador](./media/logic-apps-diagnosing-failures/review-trigger-outputs-for-errors.png)

   > [!TIP]
   > Si encuentra algún contenido que no reconozca, aprenda más sobre los [diferentes tipos de contenido](../logic-apps/logic-apps-content-type.md) en Azure Logic Apps.

<a name="check-runs-history"></a>

## <a name="check-runs-history"></a>Comprobación del historial de ejecuciones

Cada vez que el desencadenador se activa por un elemento o un evento, el motor de Logic Apps crea y ejecuta una instancia distinta del flujo de trabajo para cada uno. Si una ejecución produce errores, siga estos pasos para ver lo que ha ocurrido durante la ejecución, incluido el estado de cada paso del flujo de trabajo, además de las entradas y salidas de cada paso.

1. [Compruebe el historial de ejecuciones](../logic-apps/monitor-logic-apps.md#review-runs-history) para confirmar el estado de ejecución del flujo de trabajo. Para ver más información sobre una ejecución con errores, incluidos todos los pasos de esa ejecución y su estado, selecciónela.

   ![Visualización del historial de ejecuciones y selección de la ejecución con errores](./media/logic-apps-diagnosing-failures/logic-app-runs-history.png)

1. Después de que aparezcan todos los pasos de la ejecución, expanda el primer paso con errores.

   ![Expansión del primer paso con errores](./media/logic-apps-diagnosing-failures/logic-app-run-pane.png)

1. Compruebe las entradas del paso con errores para confirmar que aparecen según lo esperado.

1. Revise los detalles de cada paso de una ejecución específica. En **Historial de ejecuciones**, seleccione la ejecución que desea examinar.

   ![Revisión del historial de ejecuciones](./media/logic-apps-diagnosing-failures/logic-app-runs-history.png)

   ![Visualización de los detalles de una ejecución de aplicación lógica](./media/logic-apps-diagnosing-failures/logic-app-run-details.png)

1. Para examinar las entradas, salidas y los mensajes de error de un paso específico, seleccione ese paso para que la forma se expanda y muestre los detalles. Por ejemplo:

   ![Visualización de los detalles del paso](./media/logic-apps-diagnosing-failures/logic-app-run-details-expanded.png)

## <a name="perform-runtime-debugging"></a>Ejecución de la depuración en tiempo de ejecución

Para ayudar con la depuración, puede agregar pasos de diagnóstico a un flujo de trabajo de aplicación lógica, además de revisar el historial de desencadenadores y ejecuciones. Por ejemplo, puede agregar pasos que utilicen el servicio [Webhook Tester](https://webhook.site/) para poder inspeccionar las solicitudes HTTP y determinar su tamaño, forma y formato exactos.

1. Vaya al sitio de [comprobación de webhooks](https://webhook.site/) y copie la dirección URL única generada.

1. En la aplicación lógica, agregue una acción HTTP POST junto con el contenido del cuerpo que quiere probar, como una expresión o la salida de otro paso.

1. Pegue la dirección URL que se ha generado en este sitio en la acción HTTP POST.

1. Para examinar cómo se forma una solicitud cuando se genera desde el motor de Logic Apps, ejecute la aplicación lógica y visite de nuevo el sitio de comprobación de webhooks para más información.

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión de la aplicación lógica](../logic-apps/monitor-logic-apps.md)
