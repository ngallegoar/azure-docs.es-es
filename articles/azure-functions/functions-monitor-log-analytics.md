---
title: Supervisión de Azure Functions con registros de Azure Monitor
description: Obtenga información sobre cómo usar los registros de Azure Monitor con Azure Functions para supervisar las ejecuciones de funciones.
author: craigshoemaker
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: cshoe
ms.openlocfilehash: 13c72a1cf8a0dd4a1124e51b9ceee04ae04bf261
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77649881"
---
# <a name="monitoring-azure-functions-with-azure-monitor-logs"></a>Supervisión de Azure Functions con registros de Azure Monitor

Azure Functions ofrece integración con los [registros de Azure Monitor](../azure-monitor/platform/data-platform-logs.md) para supervisar las funciones. En este artículo se muestra cómo configurar Azure Functions para enviar los registros generados por el sistema y generados por el usuario a los registros de Azure Monitor.

Los registros de Azure Monitor le ofrecen la posibilidad de consolidar registros de distintos recursos en la misma área de trabajo, donde se pueden analizar con [consultas](../azure-monitor/log-query/log-query-overview.md) para recuperar, consolidar y analizar rápidamente los datos recopilados.  Puede crear y probar consultas mediante [Log Analytics](../azure-monitor/log-query/portals.md) en Azure Portal y después analizar los datos directamente mediante estas herramientas o guardar las consultas para usarlas con las [visualizaciones](../azure-monitor/visualizations.md) o las [reglas de alertas](../azure-monitor/platform/alerts-overview.md).

Azure Monitor utiliza una versión del [lenguaje de consulta Kusto](/azure/kusto/query/) utilizado por Azure Data Explorer, que es adecuado para realizar búsquedas de registros simples, pero también dispone de funciones avanzadas, como agregaciones, combinaciones y análisis inteligentes. Puede aprender rápidamente el lenguaje de consulta con [diversas lecciones](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> La integración con los registros de Azure Monitor se encuentra actualmente en versión preliminar pública para las aplicaciones de funciones que se ejecutan en planes de hospedaje de consumo, premium y dedicado de Windows.

## <a name="setting-up"></a>Instalación

En la sección **Supervisión**, seleccione **Configuración de diagnóstico** y haga clic en **Agregar configuración de diagnóstico**.

![Adición de una configuración de diagnóstico](media/functions-monitor-log-analytics/diagnostic-settings-add.png)

En la página **Configuración de diagnóstico**, elija **Enviar a Log Analytics** y, a continuación, seleccione el área de trabajo de Log Analytics. En **registro** seleccione **FunctionAppLogs**, esta tabla contiene los registros deseados.

![Adición de una configuración de diagnóstico](media/functions-monitor-log-analytics/choose-table.png)

## <a name="user-generated-logs"></a>Registros generados por el usuario

Para generar registros personalizados, puede usar la instrucción de registro específica en función del lenguaje. Los siguientes son algunos fragmentos de código de ejemplo:


# <a name="c"></a>[C#](#tab/csharp)

```csharp
log.LogInformation("My app logs here.");
```

# <a name="java"></a>[Java](#tab/java)

```java
context.getLogger().info("My app logs here.");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
context.log('My app logs here.');
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Write-Host "My app logs here."
```

# <a name="python"></a>[Python](#tab/python)

```python
logging.info('My app logs here.')
```

---

## <a name="querying-the-logs"></a>Consultas de registros

Para consultar los registros generados, vaya al área de trabajo de Log Analytics que configuró para enviar los registros de la función y haga clic en **Registros**.

![Ventana de consulta en el área de trabajo de LA](media/functions-monitor-log-analytics/querying.png)

Azure Functions escribe todos los registros en la tabla **FunctionAppLogs**. A continuación, se muestran algunas consultas de ejemplo.

### <a name="all-logs"></a>Todos los registros

```

FunctionAppLogs
| order by TimeGenerated desc

```

### <a name="a-specific-function-logs"></a>Registros de una función específica

```

FunctionAppLogs
| where FunctionName == "<Function name>" 

```

### <a name="exceptions"></a>Excepciones

```

FunctionAppLogs
| where ExceptionDetails != ""  
| order by TimeGenerated asc

```

## <a name="next-steps"></a>Pasos siguientes

- Revise la [introducción a Azure Functions](functions-overview.md).
- Más información sobre los [registros en Azure Monitor](../azure-monitor/platform/data-platform-logs.md).
- Obtenga más información sobre el [lenguaje de consulta](../azure-monitor/log-query/get-started-queries.md).
