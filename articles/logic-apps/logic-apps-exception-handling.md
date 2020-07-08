---
title: Control de errores y excepciones en flujos de trabajo
description: Aprenda a administrar errores y excepciones que se producen en tareas y flujos de trabajo automatizados creados con Azure Logic Apps.
services: logic-apps
ms.suite: integration
author: dereklee
ms.author: deli
ms.reviewer: klam, estfan, logicappspm
ms.date: 01/11/2020
ms.topic: article
ms.openlocfilehash: 73b116117530e5a2103b604efbf757d691006508
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84704529"
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Control de errores y excepciones en Azure Logic Apps

La manera en la que una arquitectura de integración controla correctamente el tiempo de inactividad o los problemas causados por sistemas dependientes puede plantear un desafío. Para ayudarle a crear integraciones sólidas y resistentes que controlen correctamente los problemas y los errores, Logic Apps proporciona una experiencia de primera clase para el control de errores y excepciones.

<a name="retry-policies"></a>

## <a name="retry-policies"></a>Directivas de reintentos

Para el control de errores y excepciones más básico, puede usar una *directiva de reintentos* en cualquier acción o desencadenador compatible; por ejemplo, vea [Acción HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger). Una directiva de reintentos especifica cuándo y cómo la acción o desencadenador vuelve a intentar una solicitud cuando se agota el tiempo de espera de la solicitud original o se produce un error, como cualquier solicitud que devuelva una respuesta 408, 429 o 5xx. Si no se utiliza ninguna otra directiva de reintentos, se usará la directiva predeterminada.

Estos son los tipos de directivas de reintentos:

| Tipo | Descripción |
|------|-------------|
| **Valor predeterminado** | Esta directiva envía hasta cuatro reintentos en intervalos que *aumentan exponencialmente* a una escala de 7,5 segundos pero con unos límites de entre 5 y 45 segundos. |
| **Intervalo exponencial**  | Esta directiva espera un intervalo aleatorio seleccionado de un intervalo exponencialmente creciente antes de enviar la solicitud siguiente. |
| **Intervalo fijo**  | Esta directiva espera el intervalo especificado antes de enviar la solicitud siguiente. |
| **None**  | No volver a enviar la solicitud. |
|||

Para más información acerca de los límites de las directivas de reintentos, consulte [Límites y configuración de Logic Apps](../logic-apps/logic-apps-limits-and-config.md#request-limits).

### <a name="change-retry-policy"></a>Cambio de la directiva de reintentos

Para seleccionar una directiva de reintentos diferente, siga estos pasos:

1. Abra la aplicación lógica en el Diseñador de aplicaciones lógicas.

1. Abra el elemento **Configuración** de una acción o un desencadenador.

1. Si la acción o el desencadenador admiten directivas de reintentos, en **Directiva de reintentos**, seleccione el tipo que desee.

O bien, puede especificar manualmente la directiva de reintentos en la sección `inputs` de una acción o un desencadenador que admitan directivas de reintentos. Si no se especifica una directiva de reintentos, la acción utiliza la directiva predeterminada.

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": {
      "<action-specific-inputs>",
      "retryPolicy": {
         "type": "<retry-policy-type>",
         "interval": "<retry-interval>",
         "count": <retry-attempts>,
         "minimumInterval": "<minimum-interval>",
         "maximumInterval": "<maximum-interval>"
      },
      "<other-action-specific-inputs>"
   },
   "runAfter": {}
}
```

*Obligatorio*

| Value | Tipo | Descripción |
|-------|------|-------------|
| <*retry-policy-type*> | String | El tipo de directiva de reintentos que quiere usar: `default`, `none`, `fixed` o `exponential`. |
| <*retry-interval*> | String | El intervalo de reintento, donde se debe usar el [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) para el valor. El intervalo mínimo predeterminado es `PT5S` y el intervalo máximo es `PT1D`. Cuando se usa la directiva de intervalo exponencial, puede especificar diferentes valores mínimos y máximos. |
| <*retry-attempts*> | Entero | El número de reintentos, que debe estar comprendido entre 1 y 90 |
||||

*Opcional*

| Value | Tipo | Descripción |
|-------|------|-------------|
| <*minimum-interval*> | String | Para la directiva de intervalo exponencial, el intervalo más pequeño para el intervalo seleccionado aleatoriamente en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) |
| <*maximum-interval*> | String | Para la directiva de intervalo exponencial, el intervalo más grande para el intervalo seleccionado aleatoriamente en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) |
||||

Aquí tiene más información acerca de los tipos de directivas diferentes.

<a name="default-retry"></a>

### <a name="default"></a>Valor predeterminado

Si no define una directiva de reintentos, la acción utiliza la directiva predeterminada, que es una [directiva de intervalo exponencial](#exponential-interval) que envía hasta cuatro reintentos en intervalos crecientes exponencialmente que aumentan 7,5 segundos cada vez. El intervalo se demora entre 5 y 45 segundos.

Aunque no definida explícitamente en la acción o el desencadenador, le mostramos cómo se comporta la directiva predeterminada en una acción HTTP de ejemplo:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "http://myAPIendpoint/api/action",
      "retryPolicy" : {
         "type": "exponential",
         "interval": "PT7S",
         "count": 4,
         "minimumInterval": "PT5S",
         "maximumInterval": "PT1H"
      }
   },
   "runAfter": {}
}
```

### <a name="none"></a>None

Para especificar que la acción o el desencadenador no vuelvan a intentar las solicitudes con error, establezca el elemento <*retry-policy-type*> en `none`.

### <a name="fixed-interval"></a>Intervalo fijo

Para especificar que la acción o el desencadenador esperen el intervalo especificado antes de enviar la solicitud siguiente, establezca el elemento <*retry-policy-type*> en `fixed`.

*Ejemplo*

Esta directiva de reintentos intenta obtener las últimas noticias dos veces más tras la primera solicitud con error con un retraso de 30 segundos entre cada intento:

```json
"Get_latest_news": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://mynews.example.com/latest",
      "retryPolicy": {
         "type": "fixed",
         "interval": "PT30S",
         "count": 2
      }
   }
}
```

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Intervalo exponencial

Para especificar que la acción o el desencadenador esperen un intervalo aleatorio antes de enviar la solicitud siguiente, establezca el elemento <*retry-policy-type*> en `exponential`. El intervalo aleatorio se selecciona de un intervalo exponencialmente creciente. Si lo desea, también puede invalidar los intervalos mínimo y máximo predeterminados especificando sus propios intervalos mínimo y máximo.

**Intervalos variables aleatorios**

Esta tabla muestra cómo Logic Apps genera una variable aleatoria uniforme dentro del intervalo especificado para cada reintento hasta el número de reintentos, este incluido:

| Número de reintentos | Intervalo mínimo | Intervalo máximo |
|--------------|------------------|------------------|
| 1 | max(0, <*minimum-interval*>) | min(interval, <*maximum-interval*>) |
| 2 | max(interval, <*minimum-interval*>) | min(2 * interval, <*maximum-interval*>) |
| 3 | max(2 * interval, <*minimum-interval*>) | min(4 * interval, <*maximum-interval*>) |
| 4 | max(4 * interval, <*minimum-interval*>) | min(8 * interval, <*maximum-interval*>) |
| .... | .... | .... |
||||

<a name="control-run-after-behavior"></a>

## <a name="catch-and-handle-failures-by-changing-run-after-behavior"></a>Detección y control de errores mediante el cambio del comportamiento de "ejecución posterior"

Cuando se agregan acciones en el Diseñador de aplicaciones lógicas, se declara de forma implícita el orden que se va a usar para ejecutarlas. Una vez finalizada la ejecución de una acción, se marca con un estado como `Succeeded`, `Failed`, `Skipped` o `TimedOut`. En cada definición de acción, la propiedad `runAfter` especifica la acción predecesora que debe finalizar primero y los estados permitidos para esa predecesora antes de que se pueda ejecutar la acción sucesora. De forma predeterminada, una acción que se agrega en el diseñador solo se ejecuta después de que se complete la predecesora con el estado `Succeeded`.

Cuando una acción inicia una excepción o un error no controlado, la acción se marca como `Failed` y las acciones sucesoras se marcan como `Skipped`. Si este comportamiento se produce para una acción que tiene ramas paralelas, el motor de Logic Apps sigue las otras ramas para determinar sus estados de finalización. Por ejemplo, si una rama finaliza con una acción `Skipped`, el estado de finalización de esa rama se basa en el estado de la predecesora de la acción omitida. Una vez completada la ejecución de la aplicación lógica, el motor determina el estado de la ejecución completa mediante la evaluación de todos los estados de la rama. Si alguna de las ramas finaliza en un error, la ejecución de toda la aplicación lógica se marca como `Failed`.

![Ejemplos en los que se muestra cómo se evalúan los estados de ejecución](./media/logic-apps-exception-handling/status-evaluation-for-parallel-branches.png)

Para asegurarse de que una acción se pueda seguir ejecutando a pesar del estado de su predecesora, [personalice el comportamiento de "ejecución posterior" de una acción](#customize-run-after) para controlar los estados incorrectos de la predecesora.

<a name="customize-run-after"></a>

### <a name="customize-run-after-behavior"></a>Personalización del comportamiento de "ejecución posterior"

Puede personalizar el comportamiento de "ejecución posterior" de una acción para que la acción se ejecute cuando el estado de la predecesora sea `Succeeded`, `Failed`, `Skipped`, `TimedOut` o cualquiera de estos estados. Por ejemplo, para enviar un correo electrónico después de que la acción predecesora `Add_a_row_into_a_table` de Excel Online se marque como `Failed`, en lugar de `Succeeded`, siga uno de estos pasos para cambiar el comportamiento de "ejecución posterior":

* En la vista de diseño, seleccione el botón de puntos suspensivos ( **...** ) y después **Configurar ejecución posterior**.

  ![Configuración del comportamiento de "ejecución posterior" para una acción](./media/logic-apps-exception-handling/configure-run-after-property-setting.png)

  La forma de la acción muestra el estado predeterminado necesario para la acción predecesora, que en este ejemplo es **Agregar una fila a una tabla**:

  ![Comportamiento de "ejecución posterior" predeterminado para una acción](./media/logic-apps-exception-handling/change-run-after-property-status.png)

  Cambie el comportamiento de "ejecución posterior" al estado que prefiera que, en este ejemplo, es **se ha producido un error**:

  ![Cambio del comportamiento de "ejecución posterior" a "se ha producido un error"](./media/logic-apps-exception-handling/run-after-property-status-set-to-failed.png)

  Para especificar que la acción se ejecute si la acción predecesora se marca como `Failed`, `Skipped` o `TimedOut`, seleccione los otros estados:

  ![Cambio del comportamiento de "ejecución posterior" para que tenga cualquier otro estado](./media/logic-apps-exception-handling/run-after-property-multiple-statuses.png)

* En la vista de código, en la definición de JSON de la acción, edite la propiedad `runAfter`, que sigue esta sintaxis:

  ```json
  "<action-name>": {
     "inputs": {
        "<action-specific-inputs>"
     },
     "runAfter": {
        "<preceding-action>": [
           "Succeeded"
        ]
     },
     "type": "<action-type>"
  }
  ```

  En este ejemplo, cambie la propiedad `runAfter` de `Succeeded` a `Failed`:

  ```json
  "Send_an_email_(V2)": {
     "inputs": {
        "body": {
           "Body": "<p>Failed to&nbsp;add row to &nbsp;@{body('Add_a_row_into_a_table')?['Terms']}</p>",,
           "Subject": "Add row to table failed: @{body('Add_a_row_into_a_table')?['Terms']}",
           "To": "Sophia.Owen@fabrikam.com"
        },
        "host": {
           "connection": {
              "name": "@parameters('$connections')['office365']['connectionId']"
           }
        },
        "method": "post",
        "path": "/v2/Mail"
     },
     "runAfter": {
        "Add_a_row_into_a_table": [
           "Failed"
        ]
     },
     "type": "ApiConnection"
  }
  ```

  Para especificar que la acción se ejecute si la acción predecesora se marca como `Failed`, `Skipped` o `TimedOut`, agregue los otros estados:

  ```json
  "runAfter": {
     "Add_a_row_into_a_table": [
        "Failed", "Skipped", "TimedOut"
     ]
  },
  ```

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Evaluación de las acciones con ámbitos y sus resultados

De forma similar a la ejecución de pasos tras acciones individuales con la propiedad `runAfter`, puede agrupar acciones dentro de un [ámbito](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md). Puede usar ámbitos si desea agrupar acciones lógicamente, evaluar el estado global del ámbito y realizar acciones basadas en ese estado. Una vez que todas las acciones de un ámbito acaben de ejecutarse, el propio ámbito obtiene su estado.

Para comprobar el estado de un ámbito, puede usar los mismos criterios que se usan para comprobar el estado de ejecución de la aplicación lógica, como `Succeeded`, `Failed`, etc.

De forma predeterminada, cuando todas las acciones del ámbito se ejecutan correctamente, el estado del ámbito se marca como `Succeeded`. Si la acción final de un ámbito da como resultado `Failed` o `Aborted`, el estado del ámbito se marca como `Failed`.

Para detectar las excepciones en un ámbito `Failed` y ejecutar acciones que controlen estos errores, puede usar la propiedad `runAfter` para ese ámbito `Failed`. De ese modo, si se produce un error en *cualquier* acción del ámbito, y usa la propiedad `runAfter` para ese ámbito, puede crear una única acción para detectar errores.

Para conocer los límites en los ámbitos, consulte [Límites y configuración](../logic-apps/logic-apps-limits-and-config.md).

<a name="get-results-from-failures"></a>

### <a name="get-context-and-results-for-failures"></a>Obtención del contexto y resultados de errores

Aunque la detección de errores desde un ámbito es útil, quizás también necesite el contexto como ayuda para comprender exactamente qué acciones han producido un error y los errores o códigos de estado que se hayan devuelto.

La función [`result()`](../logic-apps/workflow-definition-language-functions-reference.md#result) proporciona contexto sobre los resultados de todas las acciones de un ámbito. La función `result()` acepta un único parámetro, que es el nombre del ámbito, y devuelve una matriz que contiene todos los resultados de acción de dentro de ese ámbito. Estos objetos de acción incluyen los mismos atributos que el objeto `actions()`, como la hora de inicio, la hora de finalización, el estado, las entradas, los identificadores de correlación y las salidas de la acción. Para enviar el contexto de las acciones que produjeron un error dentro de un ámbito, puede emparejar fácilmente una expresión `@result()` con la propiedad `runAfter`.

Para ejecutar una acción para cada una de las acciones de un ámbito que tenga un resultado de `Failed`, y para filtrar la matriz de resultados por las acciones con error, puede emparejar una expresión `@result()` con una acción [**Filtrar matriz**](logic-apps-perform-data-operations.md#filter-array-action) y un bucle [**For each**](../logic-apps/logic-apps-control-flow-loops.md). Puede tomar la matriz de resultados filtrada y realizar una acción para cada error mediante el bucle `For_each`.

En este ejemplo, seguido de una explicación detallada, se envía una solicitud HTTP POST con el cuerpo de respuesta de todas las acciones que produjeron un error dentro del ámbito "My_Scope":

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": "@result('My_Scope')",
      "where": "@equals(item()['status'], 'Failed')"
   },
   "runAfter": {
      "My_Scope": [
         "Failed"
      ]
    }
},
"For_each": {
   "type": "foreach",
   "actions": {
      "Log_exception": {
         "type": "Http",
         "inputs": {
            "method": "POST",
            "body": "@item()['outputs']['body']",
            "headers": {
               "x-failed-action-name": "@item()['name']",
               "x-failed-tracking-id": "@item()['clientTrackingId']"
            },
            "uri": "http://requestb.in/"
         },
         "runAfter": {}
      }
   },
   "foreach": "@body('Filter_array')",
   "runAfter": {
      "Filter_array": [
         "Succeeded"
      ]
   }
}
```

Este es un tutorial detallado que describe lo que sucede en este ejemplo:

1. Para obtener el resultado de todas las acciones de "My_Scope", la acción **Filter Array** usa esta expresión de filtro: `@result('My_Scope')`.

1. La condición para **Filtrar matriz** es cualquier elemento `@result()` que tenga un estado `Failed`. Esta condición filtra la matriz que contiene todos los resultados de acción de "My_Scope" a una matriz con solo los resultados de acción que hayan producido un error.

1. Realiza una acción de bucle `For_each` en las salidas de la *matriz filtrada*. Este paso realiza una acción para cada resultado de acción con errores que se filtrara antes.

   Si solo se produce un error en una acción del ámbito, las acciones del bucle `For_each` solo se ejecutan una vez. Muchas acciones con error causan una acción por cada error.

1. Envía una solicitud HTTP POST en el cuerpo de respuesta del elemento `For_each`, que es la expresión `@item()['outputs']['body']`.

   La forma del elemento `@result()` es la misma que la forma de `@actions()` y se puede analizar del mismo modo.

1. Incluya dos encabezados personalizados con el nombre de la acción con errores (`@item()['name']`) y el id. de seguimiento de cliente de la ejecución con errores `@item()['clientTrackingId']`.

Como referencia, este es un ejemplo de un solo elemento `@result()`, que muestra las propiedades `name`, `body` y `clientTrackingId` que se analizan en el ejemplo anterior. Fuera de una acción `For_each`, `@result()` devuelve una matriz de estos objetos.

```json
{
   "name": "Example_Action_That_Failed",
   "inputs": {
      "uri": "https://myfailedaction.azurewebsites.net",
      "method": "POST"
   },
   "outputs": {
      "statusCode": 404,
      "headers": {
         "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
         "Server": "Microsoft-IIS/8.0",
         "X-Powered-By": "ASP.NET",
         "Content-Length": "68",
         "Content-Type": "application/json"
      },
      "body": {
         "code": "ResourceNotFound",
         "message": "/docs/folder-name/resource-name does not exist"
      }
   },
   "startTime": "2016-08-11T03:18:19.7755341Z",
   "endTime": "2016-08-11T03:18:20.2598835Z",
   "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
   "clientTrackingId": "08587307213861835591296330354",
   "code": "NotFound",
   "status": "Failed"
}
```

Para poner en práctica diferentes patrones de control de excepciones, puede usar las expresiones descritas anteriormente en este artículo. Podría optar por ejecutar una única acción de control de excepciones fuera del ámbito, que acepte toda la matriz filtrada de errores y quite la acción `For_each`. También puede incluir otras propiedades útiles de la respuesta `\@result()`, como se ha mostrado antes.

## <a name="set-up-azure-monitor-logs"></a>Configuración de los registros de Azure Monitor

Los patrones anteriores son una manera excelente de controlar errores y excepciones dentro de una ejecución, pero también puede identificar y responder a los errores con independencia de la ejecución en sí. [Azure Monitor](../azure-monitor/overview.md) ofrece un método sencillo de enviar todos los eventos del flujo de trabajo (incluidos todos los estados de ejecución y acción) a un [área de trabajo de Log Analytics](../azure-monitor/platform/data-platform-logs.md), a una [cuenta de almacenamiento de Azure](../storage/blobs/storage-blobs-overview.md) o a [Azure Event Hubs](../event-hubs/event-hubs-about.md).

Para evaluar los estados de ejecución, puede supervisar los registros y las métricas, o publicarlos en la herramienta de supervisión que prefiera. Una posible opción es transmitir todos los eventos mediante Event Hubs a [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). En Stream Analytics, puede escribir consultas en directo de anomalías, promedios o errores de los registros de diagnóstico. Asimismo, puede usar Stream Analytics para enviar información a otros orígenes de datos, como colas, temas, SQL, Azure Cosmos DB o Power BI.

## <a name="next-steps"></a>Pasos siguientes

* [Vea cómo un cliente crea el control de errores con Azure Logic Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Encuentre más escenarios y ejemplos de Logic Apps](../logic-apps/logic-apps-examples-and-scenarios.md)
