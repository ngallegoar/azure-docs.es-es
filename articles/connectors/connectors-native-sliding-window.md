---
title: Programación de tareas para administrar datos contiguos
description: Creación y ejecución de tareas periódicas que administren datos contiguos mediante ventanas deslizantes en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: deli, klam, logicappspm
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: ab4bf802772c95d8c48a8cdba48def05e8a2761b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74786918"
---
# <a name="schedule-and-run-tasks-for-contiguous-data-by-using-the-sliding-window-trigger-in-azure-logic-apps"></a>Programación y ejecución de tareas para datos contiguos mediante el desencadenador de ventana deslizante en Azure Logic Apps

Para ejecutar con regularidad tareas, procesos o trabajos que deben administrar los datos en fragmentos continuos, puede iniciar el flujo de trabajo de aplicación lógica con el desencadenador de **ventana deslizante**. Puede establecer una fecha y hora, así como una zona horaria para iniciar el flujo de trabajo y una periodicidad para la repetición de ese flujo de trabajo. Si se pierde la periodicidad por cualquier motivo, este desencadenador procesa las periodicidades que faltan. Por ejemplo, cuando sincronice datos entre la base de datos y el almacenamiento de copia de seguridad, use el desencadenador de ventana deslizante para que se sincronicen los datos sin existan intervalos. Para obtener más información sobre los desencadenadores y las acciones de programación integrados, consulte [Programar y ejecutar tareas y flujos de trabajo automatizados y periódicos con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Aquí se presentan algunos patrones que admite este desencadenador:

* Ejecutar inmediatamente y repetir cada *n* segundos, minutos y horas.

* Empezar a una fecha y hora específicas y después ejecutar y repetir cada número ( *n*) de segundos, minutos u horas. Con este desencadenador, puede especificar una hora de inicio pasada, que ejecutará todas las periodicidades anteriores.

* Retrasar cada periodicidad de una duración específica antes de la ejecución.

Para ver las diferencias entre este desencadenador y el desencadenador de periodicidad, o para obtener más información sobre la programación de flujos de trabajo periódicos, consulte [Programación y ejecución de tareas, procesos y flujos de trabajo automatizados periódicos con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Si desea desencadenar la aplicación lógica y ejecutarla solo una vez en el futuro, consulte [Ejecución de trabajos una sola vez](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción, puede [registrarse para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* Conocimientos básicos sobre [aplicaciones lógicas](../logic-apps/logic-apps-overview.md). Si es la primera vez que interactúa con las aplicaciones lógicas, consulte el artículo sobre [cómo crear la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-sliding-window-trigger"></a>Agregar desencadenador de ventana deslizante

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Crear una aplicación lógica en blanco.

1. Cuando se abra el Diseñador de aplicaciones lógicas, en el cuadro de búsqueda, escriba el filtro "sliding window" (ventana deslizante). En la lista de desencadenadores, seleccione este desencadenador como primer paso en el flujo de trabajo de la aplicación lógica: **Ventana deslizante**

   ![Seleccionar el desencadenador "Ventana deslizante"](./media/connectors-native-sliding-window/add-sliding-window-trigger.png)

1. Defina el intervalo y la frecuencia de periodicidad. En este ejemplo, defina estas propiedades para ejecutar el flujo de trabajo todas las semanas.

   ![Establecer el intervalo y la frecuencia](./media/connectors-native-sliding-window/sliding-window-trigger-details.png)

   | Propiedad | Obligatorio | Nombre JSON | Tipo | Descripción |
   |----------|----------|-----------|------|-------------|
   | **Intervalo** | Sí | interval | Entero | Entero positivo que describe la frecuencia con la que se ejecuta el flujo de trabajo. Estos son los intervalos mínimo y máximo: <p>- Hora: 1-12 000 horas </br>- Minuto: 1-72 000 minutos </br>- Segundo: 1-9 999 999 segundos<p>Por ejemplo, si el intervalo es 6 y la frecuencia es "hour", la periodicidad es cada 6 horas. |
   | **Frecuencia** | Sí | frequency | String | Unidad de tiempo que se usa para la periodicidad: **Second**, **Minute** o **Hour** |
   ||||||

   ![Opciones avanzadas de periodicidad](./media/connectors-native-sliding-window/sliding-window-trigger-more-options-details.png)

   Para ver más propiedades de periodicidad, abra la lista **Agregar nuevo parámetro**. 
   Las opciones que seleccione aparecerán en el desencadenador después de la selección.

   | Propiedad | Obligatorio | Nombre JSON | Tipo | Descripción |
   |----------|----------|-----------|------|-------------|
   | **Delay** | No | delay | String | La duración del retraso para cada periodicidad con la [especificación de fecha y hora ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations). |
   | **Zona horaria** | No | timeZone | String | Solo se aplica cuando se especifica una hora de inicio porque este desencadenador no acepta [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Seleccione la zona horaria que desea aplicar. |
   | **Hora de inicio** | No | startTime | String | Proporcione una fecha y hora de inicio en este formato: <p>AAAA-MM-DDThh:mm:ss si selecciona una zona horaria <p>O bien <p>AAAA-MM-DDThh:mm:ssZ si no selecciona una zona horaria <p>Por ejemplo, si desea la fecha del 18 de septiembre de 2017 a las 14:00, especifique entonces "2017-09-18T14:00:00" y especifique una zona horaria como "Hora estándar del Pacífico". O bien, especifique "2017-09-18T14:00:00Z" sin una zona horaria. <p>**Nota:** Esta hora de inicio debe seguir la [especificación de fecha y hora ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) en [formato de hora y fecha UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), pero sin una [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Si no se selecciona una zona horaria, debe agregar la letra "Z" al final sin espacios. Esta "Z" se refiere al equivalente de [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). <p>Para las programaciones simples, la hora de inicio es la primera aparición, mientras que para periodicidades avanzadas, el desencadenador no se activa antes de la hora de inicio. [ *¿De qué formas puedo usar la fecha y hora de inicio?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   |||||

1. Ahora cree el flujo de trabajo restante con otras acciones. Para ver más acciones que se puedan agregar, consulte [Conectores para Azure Logic Apps](../connectors/apis-list.md).

## <a name="workflow-definition---sliding-window"></a>Definición de flujo de trabajo: ventana deslizante

En la definición de flujo de trabajo subyacente de la aplicación lógica, que usa JSON, podrá ver la definición del desencadenador de ventana deslizante con las opciones que eligió. Para ver esta definición, en la barra de herramientas del diseñador, elija **Vista Código**. Para volver al diseñador, elija la barra de herramientas del diseñador, **Diseñador**.

En este ejemplo se muestra el aspecto que podría tener una definición de desencadenador de ventana deslizante en una definición de flujo de trabajo subyacente donde el retraso para cada periodicidad es de cinco segundos para una periodicidad por hora:

``` json
"triggers": {
   "Recurrence": {
      "type": "SlidingWindow",
      "Sliding_Window": {
         "inputs": {
            "delay": "PT5S"
         },
         "recurrence": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2019-05-13T14:00:00Z",
            "timeZone": "Pacific Standard Time"
         }
      }
   }
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Retraso de la acción siguiente en flujos de trabajo](../connectors/connectors-native-delay.md)
* [Conectores de Logic Apps](../connectors/apis-list.md)