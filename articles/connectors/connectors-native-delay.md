---
title: Retraso de la acción siguiente en flujos de trabajo
description: Espera para la ejecución de la siguiente acción en flujos de trabajo de aplicaciones lógicas mediante acciones "Retraso" y "Retraso hasta" en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: deli, klam, logicappspm
ms.topic: conceptual
ms.date: 05/25/2019
tags: connectors
ms.openlocfilehash: 5348ade1ba6eec6cbd360849411b4520cb3c2b19
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74787343"
---
# <a name="delay-running-the-next-action-in-azure-logic-apps"></a>Retraso de la ejecución de la siguiente acción en Azure Logic Apps

Para que su aplicación lógica espere un período determinado antes de ejecutar la siguiente acción, puede agregar la acción **Programación de retraso** antes de una acción en el flujo de trabajo de su aplicación lógica. También puede agregar la acción **Programación de Retraso hasta** para esperar hasta una fecha y hora específicas antes de ejecutar la siguiente acción. Para obtener más información sobre los desencadenadores y las acciones de programación integradas, consulte [Programación y ejecución de tareas y flujos de trabajo automatizados periódicos con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

* **Retraso**: permite esperar durante un número especificado de unidades de tiempo, como segundos, minutos, horas, días o meses, antes de ejecutar la siguiente acción.

* **Retraso hasta**: permite esperar hasta la fecha y hora especificadas antes de que se ejecute la siguiente acción.

Aquí se muestran algunos ejemplos de uso de estas acciones:

* Esperar hasta un día de la semana para enviar una actualización de estado por correo electrónico.

* Retrasar el flujo de trabajo hasta que finalice una llamada HTTP antes de reanudarse y recuperar datos.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción, puede [registrarse para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* Conocimientos básicos sobre [aplicaciones lógicas](../logic-apps/logic-apps-overview.md). Antes de realizar una acción, debe iniciar su aplicación lógica con un desencadenador. Puede usar el desencadenador que desee y agregar otras acciones antes de añadir una acción de retraso. En este tema se usa un desencadenador de Office 365 Outlook. Si es la primera vez que interactúa con las aplicaciones lógicas, consulte el artículo sobre [cómo crear la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-delay"></a>

## <a name="add-the-delay-action"></a>Adición de la acción de retraso

1. En el diseñador de aplicaciones lógicas, en el paso donde desea agregar una acción de retraso, elija **Nuevo paso**.

   Para agregar la acción de retraso entre pasos, mueva el puntero sobre la flecha que conecta los pasos. Elija el signo más (+) que aparece y, a continuación, seleccione **Agregar una acción**.

1. En el cuadro de búsqueda, escriba "retraso" como filtro. En la lista de acciones, seleccione esta acción: **Delay**

   ![Adición de la acción "Retraso"](./media/connectors-native-delay/add-delay-action.png)

1. Especifique el tiempo de espera antes de que se ejecute la siguiente acción.

   ![Establecer la cantidad de tiempo de retraso](./media/connectors-native-delay/delay-time-intervals.png)

   | Propiedad | Nombre JSON | Obligatorio | Tipo | Descripción |
   |----------|-----------|----------|------|-------------|
   | Count | count | Sí | Entero | El número de unidades de tiempo de retraso |
   | Unidad | unit | Sí | String | La unidad de tiempo: `Second`, `Minute`, `Hour`, `Day`, `Week` o `Month`. |
   ||||||

1. Agregue las demás acciones que desee ejecutar en el flujo de trabajo.

1. Cuando haya terminado, guarde la aplicación lógica.

<a name="add-delay-until"></a>

## <a name="add-the-delay-until-action"></a>Adición de la acción Retraso hasta

1. En el diseñador de aplicaciones lógicas, en el paso donde desea agregar una acción de retraso, elija **Nuevo paso**.

   Para agregar la acción de retraso entre pasos, mueva el puntero sobre la flecha que conecta los pasos. Elija el signo más (+) que aparece y, a continuación, seleccione **Agregar una acción**.

1. En el cuadro de búsqueda, escriba "retraso" como filtro. En la lista de acciones, seleccione esta acción: **Retraso hasta**

   ![Agregar acción "Retraso hasta"](./media/connectors-native-delay/add-delay-until-action.png)

1. Proporcione la fecha y hora de finalización cuando desee reanudar el flujo de trabajo.

   ![Especificar la marca de tiempo de cuándo se debe finalizar el retraso](./media/connectors-native-delay/delay-until-timestamp.png)

   | Propiedad | Nombre JSON | Obligatorio | Tipo | Descripción |
   |----------|-----------|----------|------|-------------|
   | Timestamp | timestamp | Sí | String | La fecha y hora de finalización para reanudar el flujo de trabajo con este formato: <p>YYYY-MM-DDThh:mm:ssZ <p>Por ejemplo, si desea el 18 de septiembre de 2017 a las 14:00, especifique "2017-09-18T14:00:00Z". <p>**Nota:** Este formato de hora debe seguir la [especificación de fecha y hora ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) en [formato de hora y fecha UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), pero sin una [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Si no se selecciona una zona horaria, debe agregar la letra "Z" al final sin espacios. Esta "Z" se refiere al equivalente de [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). |
   ||||||

1. Agregue las demás acciones que desee ejecutar en el flujo de trabajo.

1. Cuando haya terminado, guarde la aplicación lógica.

## <a name="next-steps"></a>Pasos siguientes

* [Creación, programación y ejecución de tareas y flujos de trabajo periódicos con el desencadenador de periodicidad](../connectors/connectors-native-recurrence.md)
* [Conectores de Logic Apps](../connectors/apis-list.md)