---
title: 'Supervisiones en Durable Functions: Azure'
description: Aprenda a implementar un monitor de estado con la extensión Durable Functions para Azure Functions.
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: ed92156df9d8e1e07b56cea4b1e64edee11d68d9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77562129"
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Escenario de supervisión en Durable Functions: ejemplo de supervisión meteorológica

El patrón de supervisión hace referencia a un proceso *periódico* flexible de un flujo de trabajo; por ejemplo, realizar un sondeo hasta que se cumplan determinadas condiciones. En este artículo se explica un ejemplo que usa [Durable Functions](durable-functions-overview.md) para implementar la supervisión.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Información general de escenario

En este ejemplo se supervisan las condiciones meteorológicas actuales de una ubicación y alerta al usuario por SMS cuando el cielo está despejado. Puede utilizar una función regular desencadenada por temporizador para comprobar el tiempo y enviar alertas. Sin embargo, un problema con este enfoque es la **administración del ciclo de vida**. Si solo se debe enviar una alerta, la supervisión necesita deshabilitarse después de que se detecte un clima despejado. El patrón de supervisión puede finalizar su propia ejecución, entre otras ventajas:

* Las supervisiones se ejecutan en intervalos, no en programaciones: un desencadenador en temporizador *se ejecuta* cada hora; una supervisión *espera* una hora entre acciones. Las acciones de una supervisión no se solaparán a menos que se especifique, lo que puede ser importante para tareas de ejecución prolongada.
* Las supervisiones pueden tener intervalos dinámicos: el tiempo de espera puede cambiar en función de alguna condición.
* Las supervisiones pueden finalizar cuando se cumple alguna condición o cuando otro proceso las finaliza.
* Las supervisiones pueden aceptar parámetros. El ejemplo muestra cómo se puede aplicar el mismo proceso de supervisión meteorológica a cualquier ubicación y número de teléfono solicitados.
* Las supervisiones son escalables. Debido a que cada supervisión es una instancia de orquestación, se pueden crear múltiples supervisiones sin tener que crear nuevas funciones ni definir más código.
* Las supervisiones se integran fácilmente en flujos de trabajo mayores. Una supervisión puede ser una sección de una función de orquestación más compleja, o una [suborquestación](durable-functions-sub-orchestrations.md).

## <a name="configuration"></a>Configuración

### <a name="configuring-twilio-integration"></a>Configuración de la integración de Twilio

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

### <a name="configuring-weather-underground-integration"></a>Configuración de la integración de Weather Underground

Este ejemplo implica el uso de la API de Weather Underground para comprobar las condiciones meteorológicas actuales de una ubicación.

Lo primero que necesita es una cuenta de Weather Underground. Puede crear una gratis en [https://www.wunderground.com/signup](https://www.wunderground.com/signup). Cuando tenga la cuenta, tendrá que adquirir una clave de API. Puede hacerlo si visita [https://www.wunderground.com/weather/api](https://www.wunderground.com/weather/api/?MR=1) y, después, si selecciona la configuración de la clave. El plan para desarrolladores Stratus es gratis y suficiente para ejecutar este ejemplo.

Cuando tenga una clave de API, agregue la siguiente **configuración de la aplicación** a la aplicación de función.

| Nombre del valor de configuración de la aplicación | Descripción del valor |
| - | - |
| **WeatherUndergroundApiKey**  | La clave de API de Weather Underground. |

## <a name="the-functions"></a>Funciones

En este artículo se explican las funciones siguientes en la aplicación de ejemplo:

* `E3_Monitor`: una [función de orquestador](durable-functions-bindings.md#orchestration-trigger) que llama periódicamente a `E3_GetIsClear`. Llama a `E3_SendGoodWeatherAlert` si `E3_GetIsClear` devuelve true.
* `E3_GetIsClear`: una [función de la actividad](durable-functions-bindings.md#activity-trigger) que comprueba las condiciones meteorológicas actuales de una ubicación.
* `E3_SendGoodWeatherAlert`: una función de la actividad que envía un mensaje SMS por Twilio.

### <a name="e3_monitor-orchestrator-function"></a>Función de orquestador E3_Monitor

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=41-78,97-115)]

El orquestador requiere una ubicación para supervisar y un número de teléfono al que enviar un mensaje cuando el tiempo aparece despejado en la ubicación. Estos datos se pasan al orquestador como un objeto `MonitorRequest` con un tipo inflexible.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

La función **E3_Monitor** utiliza la norma *function.json* para las funciones de orquestador.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/function.json)]

Este es el código que implementa la función:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/index.js)]

---

Esta función de orquestador realiza las acciones siguientes:

1. Obtiene la función **MonitorRequest** que consta de la *ubicación* para supervisar y el  *número de teléfono* al que se enviará una notificación por SMS.
2. Determina el tiempo de expiración de la supervisión. El ejemplo utiliza un valor modificable por brevedad.
3. Llama a **E3_GetIsClear** para determinar si el cielo está despejado en la ubicación solicitada.
4. Si está despejado, llama a **E3_SendGoodWeatherAlert** para enviar una notificación por SMS al número de teléfono solicitado.
5. Crea un temporizador duradero para reanudar la orquestación en el siguiente intervalo de sondeo. El ejemplo utiliza un valor modificable por brevedad.
6. Continúa ejecutándose hasta que la hora universal coordinada actual pasa la hora de expiración de la supervisión o se envía una alerta por SMS.

Se pueden ejecutar varias instancias de orquestador de forma simultanea llamando varias veces a la función de orquestador. Se puede especificar la ubicación que se va a supervisar, así como el número de teléfono para enviar una alerta por SMS.

### <a name="e3_getisclear-activity-function"></a>Función de actividad E3_GetIsClear

Al igual que otros ejemplos, las funciones auxiliares de actividad son básicamente funciones normales que usan el enlace del desencadenador `activityTrigger`. La función **E3_GetIsClear** obtiene las condiciones meteorológicas actuales utilizando la API de Weather Underground y determina si el cielo está despejado.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=80-85)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

*function.json* se define como sigue:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/function.json)]

Y esta es la implementación.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/index.js)]

---

### <a name="e3_sendgoodweatheralert-activity-function"></a>Función de actividad E3_SendGoodWeatherAlert

La función **E3_SendGoodWeatherAlert** usa el enlace de Twilio para enviar un mensaje SMS que notifica al usuario final que es un buen momento para dar un paseo.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=87-96,140-205)]

> [!NOTE]
> Tendrá que instalar el paquete NuGet `Microsoft.Azure.WebJobs.Extensions.Twilio` para ejecutar el código de ejemplo.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

La *function.json* es simple:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/function.json)]

Y este es el código que envía el mensaje SMS:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/index.js)]

---

## <a name="run-the-sample"></a>Ejecución del ejemplo

Con las funciones desencadenadas mediante HTTP del ejemplo, puede iniciar la orquestación con el envío de la siguiente solicitud HTTP POST:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "location": { "city": "Redmond", "state": "WA" }, "phone": "+1425XXXXXXX" }
```

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

Se inicia la instancia de **E3_Monitor** y consulta las condiciones meteorológicas actuales para la ubicación solicitada. Si el tiempo está despejado, llama a una función de actividad para enviar una alerta; de lo contrario, establece un temporizador. Cuando expire el temporizador, se reanudará la orquestación.

Puede ver los resultados de la actividad de orquestación al examinar los registros de la función en el portal de Azure Functions.

```
2018-03-01T01:14:41.649 Function started (Id=2d5fcadf-275b-4226-a174-f9f943c90cd1)
2018-03-01T01:14:42.741 Started orchestration with ID = '1608200bb2ce4b7face5fc3b8e674f2e'.
2018-03-01T01:14:42.780 Function completed (Success, Id=2d5fcadf-275b-4226-a174-f9f943c90cd1, Duration=1111ms)
2018-03-01T01:14:52.765 Function started (Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb)
2018-03-01T01:14:52.890 Received monitor request. Location: Redmond, WA. Phone: +1425XXXXXXX.
2018-03-01T01:14:52.895 Instantiating monitor for Redmond, WA. Expires: 3/1/2018 7:14:52 AM.
2018-03-01T01:14:52.909 Checking current weather conditions for Redmond, WA at 3/1/2018 1:14:52 AM.
2018-03-01T01:14:52.954 Function completed (Success, Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb, Duration=189ms)
2018-03-01T01:14:53.226 Function started (Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859)
2018-03-01T01:14:53.808 Function completed (Success, Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859, Duration=582ms)
2018-03-01T01:14:53.967 Function started (Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c)
2018-03-01T01:14:53.996 Next check for Redmond, WA at 3/1/2018 1:44:53 AM.
2018-03-01T01:14:54.030 Function completed (Success, Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c, Duration=62ms)
```

La orquestación [finalizará](durable-functions-instance-management.md) una vez que se alcance el tiempo de espera o se detecten cielos despejados. También puede usar `TerminateAsync` (.NET) o `terminate` (JavaScript) dentro de otra función, o bien invocar el webhook HTTP POST **terminatePostUri** al que se hace referencia en la respuesta 202 anterior y reemplazar `{text}` por el motivo de la finalización:

```
POST https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="next-steps"></a>Pasos siguientes

Este ejemplo ha demostrado cómo usar Durable Functions para supervisar el estado de un origen externo mediante [temporizadores durables](durable-functions-timers.md) y lógica condicional. En el ejemplo siguiente se muestra cómo usar eventos externos y [temporizadores duraderos](durable-functions-timers.md) para controlar la interacción humana.

> [!div class="nextstepaction"]
> [Ejecución de la interacción humana de ejemplo](durable-functions-phone-verification.md)
