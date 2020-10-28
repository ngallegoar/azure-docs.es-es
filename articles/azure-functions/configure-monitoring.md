---
title: Configuración de la supervisión para Azure Functions
description: Aprenda a conectar la aplicación de funciones a Application Insights para su supervisión y a configurar la recopilación de datos.
ms.date: 8/31/2020
ms.topic: how-to
ms.custom: contperfq2
ms.openlocfilehash: 50705eeedf9c985a053600a8c0b27c823231e9a3
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92217191"
---
# <a name="how-to-configure-monitoring-for-azure-functions"></a>Configuración de la supervisión para Azure Functions

Azure Functions se integra con Application Insights para permitirle supervisar mejor las aplicaciones de funciones. Application Insights, una característica de Azure Monitor, es un servicio de Application Performance Management (APM) extensible que recopila los datos generados por la aplicación de funciones, incluida la información que la aplicación escribe en los registros. La integración de Application Insights se suele habilitar al crear la aplicación de funciones. Si la aplicación no tiene establecida la clave de instrumentación, antes debe [habilitar la integración de Application Insights](#enable-application-insights-integration). 

Puede usar Application Insights sin ninguna configuración personalizada. La configuración predeterminada puede dar lugar a grandes volúmenes de datos. Si usa una suscripción de Visual Studio Azure, es posible que alcance su límite de datos para Application Insights. Para obtener más información sobre los costos de Application Insights, consulte [Administración del uso y los costos de Application Insights](../azure-monitor/app/pricing.md).

Más adelante en este artículo verá cómo configurar y personalizar los datos que envían las funciones a Application Insights. Para una aplicación de función, el registro se configura en el archivo [host.json]. 

> [!NOTE]
> Puede usar una configuración de la aplicación especialmente creada para representar valores específicos en un archivo host.json para un entorno específico. Esto le permitirá, de hecho, cambiar la configuración de host.json sin tener que volver a publicar este archivo en el proyecto. Para más información, consulte [Invalidación de valores de host.json](functions-host-json.md#override-hostjson-values).

## <a name="configure-categories"></a>Configuración de categorías

El registrador de Azure Functions incluye una *categoría* para cada registro. La categoría indica qué parte del código de tiempo de ejecución o del código de la función escribió el registro. Las categorías difieren entre la versión 1. x y las versiones posteriores. En el siguiente gráfico se describen las categorías principales de registros que crea el runtime. 

# <a name="v2x"></a>[v2.x y posteriores](#tab/v2)

| Category | Tabla | Descripción |
| ----- | ----- | ----- |
| **`Function.<YOUR_FUNCTION_NAME>`** | **dependencies**| Los datos de dependencia se recopilan automáticamente para algunos servicios. En el caso de ejecuciones correctas, estos registros son de nivel `Information`. Para obtener más información, consulte [Dependencias](functions-monitoring.md#dependencies). Las excepciones se registran en el nivel `Error`. El entorno de ejecución también crea registros de nivel `Warning`; por ejemplo, cuando los mensajes en cola se envían a la [cola de dudosos](functions-bindings-storage-queue-trigger.md#poison-messages). | 
| **`Function.<YOUR_FUNCTION_NAME>`** | **customMetrics**<br/>**customEvents** | Los SDK de C# y JavaScript permiten recopilar métricas personalizadas y registrar eventos personalizados. Para más información, consulte [Datos de telemetría personalizados](functions-monitoring.md#custom-telemetry-data).|
| **`Function.<YOUR_FUNCTION_NAME>`** | **traces**| Incluye registros de funciones iniciadas y completadas para ejecuciones de función específicas. En el caso de ejecuciones correctas, estos registros son de nivel `Information`. Las excepciones se registran en el nivel `Error`. El entorno de ejecución también crea registros de nivel `Warning`; por ejemplo, cuando los mensajes en cola se envían a la [cola de dudosos](functions-bindings-storage-queue-trigger.md#poison-messages). | 
| **`Function.<YOUR_FUNCTION_NAME>.User`** | **traces**| Registros generados por el usuario, que pueden integrarse en cualquier nivel de registro. Para obtener más información sobre cómo escribir en los registros desde las funciones, consulte [Escritura en los registros](functions-monitoring.md#writing-to-logs). | 
| **`Host.Aggregator`** | **customMetrics** | Estos registros generados en tiempo de ejecución proporcionan los recuentos y promedios de las invocaciones de función en un período de tiempo [configurable](#configure-the-aggregator). El período predeterminado es 30 segundos o 1000 resultados, lo que ocurra primero. Algunos ejemplos son el número de ejecuciones, la tasa de éxito y la duración. Todos estos registros se escriben en el nivel `Information`. Si filtra por `Warning` o superior, no verá ninguno de estos datos. |
| **`Host.Results`** | **requests** | Estos registros generados en tiempo de ejecución indican si una función se ha realizado correctamente o no. Todos estos registros se escriben en el nivel `Information`. Si filtra por `Warning` o superior, no verá ninguno de estos datos. |
| **`Microsoft`** | **traces** | Categoría de registro completa que refleja un componente de .NET en tiempo de ejecución invocado por el host.  |
| **`Worker`** | **traces** | Registros generados por el proceso de trabajo de los lenguajes que no son .NET. Estos registros también se pueden crear en una categoría `Microsoft.*`, como `Microsoft.Azure.WebJobs.Script.Workers.Rpc.RpcFunctionInvocationDispatcher`. Se escriben en el nivel `Information`.|

# <a name="v1x"></a>[v1.x](#tab/v1)

| Category | Tabla | Descripción |
| ----- | ----- | ----- |
| **`Function`** | **traces**| Registros generados por el usuario, que pueden integrarse en cualquier nivel de registro. Para obtener más información sobre cómo escribir en los registros desde las funciones, consulte [Escritura en los registros](functions-monitoring.md#writing-to-logs). | 
| **`Host.Aggregator`** | **customMetrics** | Estos registros generados en tiempo de ejecución proporcionan los recuentos y promedios de las invocaciones de función en un período de tiempo [configurable](#configure-the-aggregator). El período predeterminado es 30 segundos o 1000 resultados, lo que ocurra primero. Algunos ejemplos son el número de ejecuciones, la tasa de éxito y la duración. Todos estos registros se escriben en el nivel `Information`. Si filtra por `Warning` o superior, no verá ninguno de estos datos. |
| **`Host.Executor`** | **traces** | Incluye registros de **función iniciada** y **función completada** para ejecuciones de función específicas. Para las ejecuciones correctas, estos registros son de nivel `Information`. Las excepciones se registran en el nivel `Error`. El entorno de ejecución también crea registros de nivel `Warning`; por ejemplo, cuando los mensajes en cola se envían a la [cola de dudosos](functions-bindings-storage-queue-trigger.md#poison-messages).  |
| **`Host.Results`** | **requests** | Estos registros generados en tiempo de ejecución indican si una función se ha realizado correctamente o no. Todos estos registros se escriben en el nivel `Information`. Si filtra por `Warning` o superior, no verá ninguno de estos datos. |

---

En la columna **Tabla** se indica en qué tabla de Application Insights se escribe el registro. 

## <a name="configure-log-levels"></a>Configuración de los niveles de registro

[!INCLUDE [functions-log-levels](../../includes/functions-log-levels.md)]

Para cada categoría, debe indicar el nivel de registro mínimo para enviar. La configuración de host.json varía en función de la [versión del entorno de ejecución de Functions](functions-versions.md). 

En el ejemplo siguiente se define el registro en función de las siguientes reglas:

+ En el caso de los registros de las categorías `Host.Results` o `Function`, solo se deben registrar los eventos de nivel `Error` o de niveles superiores. 
+ En el caso de los registros de la categoría `Host.Aggregator`, se deben registrar todas las métricas generadas (`Trace`).
+ En el resto de los registros, incluidos los registros de usuario, solo se deben registrar los eventos de nivel `Information` y de niveles superiores.

# <a name="v2x"></a>[v2.x y posteriores](#tab/v2)

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host.Results": "Error",
      "Function": "Error",
      "Host.Aggregator": "Trace"
    }
  }
}
```

# <a name="v1x"></a>[v1.x](#tab/v1) 

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Trace"
      }
    }
  }
}
```

---

Si [host.json] incluye varios registros que comienzan con la misma cadena, los más definidos se asignan primero. Considere el ejemplo siguiente que registra todo en el entorno de ejecución, excepto la categoría `Host.Aggregator`, en el nivel `Error`:

# <a name="v2x"></a>[v2.x y posteriores](#tab/v2)

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host": "Error",
      "Function": "Error",
      "Host.Aggregator": "Information"
    }
  }
}
```

# <a name="v1x"></a>[v1.x](#tab/v1) 

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

---

Puede usar un valor de nivel de registro de `None` para evitar que se escriban registros en una categoría. 

## <a name="configure-the-aggregator"></a>Configurar el agregador

Como se indicó en la sección anterior, el tiempo de ejecución agrega datos acerca de las ejecuciones de la función durante un período de tiempo. El período predeterminado es 30 segundos o 1000 ejecuciones, lo que ocurra primero. Puede configurar este valor en el archivo [host.json].  Este es un ejemplo:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Configurar el muestreo

Application Insights tiene una característica de [muestreo](../azure-monitor/app/sampling.md) que le puede ayudar a impedir que se recopilen demasiados datos de telemetría sobre las ejecuciones completadas en los momentos de picos de carga. Cuando tasa de ejecuciones entrantes supera un umbral especificado, Application Insights empieza a omitir aleatoriamente algunas de las ejecuciones entrantes. La configuración predeterminada para el número máximo de ejecuciones por segundo es de 20 (cinco en la versión 1.x). Puede configurar el muestreo en [host.json](./functions-host-json.md#applicationinsights).  Este es un ejemplo:

# <a name="v2x"></a>[v2.x y posteriores](#tab/v2)

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 20,
        "excludedTypes": "Request;Exception"
      }
    }
  }
}
```


Puede excluir determinados tipos de datos de telemetría del muestreo. En este ejemplo, se excluyen del muestreo los datos de tipo `Request` y `Exception`. Esto garantiza que se registran *todas* las ejecuciones (solicitudes) y excepciones de las funciones, en tanto que otros tipos de datos de telemetría siguen sujetos al muestreo. 

# <a name="v1x"></a>[v1.x](#tab/v1)  

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```
---

Para obtener más información, consulte [Muestreo en Application Insights](../azure-monitor/app/sampling.md).

## <a name="configure-scale-controller-logs"></a>Configuración de los registros del controlador de escala

_Esta característica se encuentra en su versión preliminar._ 

Puede hacer que el [controlador de escala de Azure Functions](./functions-scale.md#runtime-scaling) emita registros en Application Insights o en Blob Storage para comprender mejor las decisiones que este controlador está tomando para la aplicación de funciones.

Para habilitar esta característica, agregue una configuración de aplicación denominada `SCALE_CONTROLLER_LOGGING_ENABLED` a los valores de la aplicación de funciones. El valor de esta configuración debe tener el formato `<DESTINATION>:<VERBOSITY>`, en función de lo siguiente:

[!INCLUDE [functions-scale-controller-logging](../../includes/functions-scale-controller-logging.md)]

Por ejemplo, el siguiente comando de la CLI de Azure activa el registro detallado del controlador de escala para Application Insights:

```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings SCALE_CONTROLLER_LOGGING_ENABLED=AppInsights:Verbose
```

En este ejemplo, reemplace `<FUNCTION_APP_NAME>` y `<RESOURCE_GROUP_NAME>` por el nombre de la aplicación de funciones y el nombre del grupo de recursos, respectivamente. 

El siguiente comando de la CLI de Azure deshabilita el registro estableciendo el nivel de detalle en `None`:

```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings SCALE_CONTROLLER_LOGGING_ENABLED=AppInsights:None
```

También puede deshabilitar el registro si quita la configuración de `SCALE_CONTROLLER_LOGGING_ENABLED` con el siguiente comando de la CLI de Azure:

```azurecli-interactive
az functionapp config appsettings delete --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--setting-names SCALE_CONTROLLER_LOGGING_ENABLED
```

## <a name="enable-application-insights-integration"></a>Habilitación de la integración de Application Insights

Para que una aplicación de función envíe datos a Application Insights tiene que conocer la clave de instrumentación de un recurso de Application Insights. La clave tiene que estar en un valor de configuración de la aplicación llamado **APPINSIGHTS_INSTRUMENTATIONKEY** .

Al crear la función de aplicaciones [en Azure Portal](functions-create-first-azure-function.md), desde la línea de comandos mediante [Azure Functions Core Tools](functions-create-first-azure-function-azure-cli.md) o [Visual Studio Code](functions-create-first-function-vs-code.md), la integración de Application Insights está habilitada de forma predeterminada. El recurso de Application Insights tiene el mismo nombre que la aplicación de función y se crea en la misma región o en la región más cercana.

### <a name="new-function-app-in-the-portal"></a>Nueva aplicación de función en el portal

Para revisar el recurso de Application Insights que se está creando, selecciónelo para expandir la ventana **Application Insights** . Puede cambiar el valor de **Nuevo nombre de recurso**  o elegir otro valor en **Ubicación** en la [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) donde quiera almacenar los datos.

![Habilitar Application Insights al crear una aplicación de función](media/functions-monitoring/enable-ai-new-function-app.png)

Cuando elige **Crear** , se crea un recurso de Application Insights con la aplicación de función, que tiene `APPINSIGHTS_INSTRUMENTATIONKEY` establecido en la configuración de la aplicación. Todo está listo para funcionar.

<a id="manually-connect-an-app-insights-resource"></a>
### <a name="add-to-an-existing-function-app"></a>Adición a una aplicación de función existente 

Si no se creó ningún recurso de Application Insights con la aplicación de funciones, siga los pasos que se describen a continuación para crearlo. A continuación, puede agregar la clave de instrumentación de ese recurso como una [configuración de la aplicación](functions-how-to-use-azure-function-app-settings.md#settings) en la aplicación de funciones.

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **aplicación de funciones** y, a continuación, elija la aplicación de funciones. 

1. Seleccione el banner **Application Insights no está configurado.** en la parte superior de la ventana. Si no ve el banner, es posible que la aplicación ya tenga Application Insights habilitado.

    :::image type="content" source="media/configure-monitoring/enable-application-insights.png" alt-text="Habilitación de Application Insights desde el portal":::

1. Expanda **Cambie su recurso** y cree un recurso de Application Insights, para lo que deberá usar los valores que se especifican en la siguiente tabla.  

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nuevo nombre de recurso** | Nombre de aplicación único | Lo más fácil es usar el mismo nombre que usa para la aplicación de función, que debe ser único en su suscripción. | 
    | **Ubicación** | Oeste de Europa | Si es posible, use la misma [región](https://azure.microsoft.com/regions/) que la aplicación de función o una que esté cerca de esa región. |

    :::image type="content" source="media/configure-monitoring/ai-general.png" alt-text="Habilitación de Application Insights desde el portal":::

1. Seleccione **Aplicar** . 

   El recurso de Application Insights se crea en el mismo grupo de recursos y suscripción que su aplicación de función. Después de crear el recurso, cierre la ventana Application Insights.

1. En la aplicación de funciones, seleccione **Configuración** en **Configuración** y, después, seleccione **Configuración de la aplicación** . Si ve una configuración denominada `APPINSIGHTS_INSTRUMENTATIONKEY`, significa que la integración de Application Insights está habilitada para la aplicación de función que se ejecuta en Azure. Si, por alguna razón, esta configuración no existe, agréguela utilizando la clave de instrumentación de Application Insights como valor.

> [!NOTE]
> Las primeras versiones de Functions usaban la supervisión integrada, que ya no se recomienda. Al habilitar la integración de Application Insights para una aplicación de función tal, también debe [deshabilitar el registro integrado](#disable-built-in-logging).  

## <a name="disable-built-in-logging"></a>Deshabilitar el registro integrado

Cuando habilite Application Insights, deshabilite el registro integrado que usa Azure Storage. El registro integrado es útil para realizar pruebas con cargas de trabajo ligeras, pero no está diseñado para su uso en producción de alta carga. Para la supervisión de producción, se recomienda Application Insights. Si el registro integrado se usa en producción, el registro resultante podría estar incompleto debido a la limitación de Azure Storage.

Para deshabilitar el registro integrado, elimine la configuración de la aplicación `AzureWebJobsDashboard`. Para obtener información acerca de cómo eliminar la configuración de la aplicación en Azure Portal, consulte la sección **Application settings** (Configuración de la aplicación) en [How to manage a function app](functions-how-to-use-azure-function-app-settings.md#settings) (Cómo administrar una aplicación de función). Antes de eliminar el valor de configuración de la aplicación, asegúrese de que no haya ninguna función existente en la misma aplicación de función que utilice dicho valor para los desencadenadores o enlaces de Azure Storage.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de la supervisión, consulte:

+ [Monitor Azure Functions](functions-monitoring.md)
+ [Análisis de datos de telemetría de Azure Functions en Application Insights](analyze-telemetry-data.md)
+ [Application Insights](/azure/application-insights/)


[host.json]: functions-host-json.md
