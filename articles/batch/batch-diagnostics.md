---
title: Métricas, alertas y registros de diagnóstico
description: Registre y analice los eventos de registro de diagnóstico de los recursos de la cuenta de Azure Batch como tareas y grupos.
ms.topic: how-to
ms.date: 10/08/2020
ms.custom: seodec18
ms.openlocfilehash: 265149e8d3cd775974ec690ebffbce92a1b82b2e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91848694"
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Métricas, alertas y registros de Batch para evaluación de diagnóstico y supervisión

En este artículo se explica cómo supervisar una cuenta de Batch con las características de [Azure Monitor](../azure-monitor/overview.md). Azure Monitor recopila [métricas](../azure-monitor/platform/data-platform-metrics.md) y [registros de diagnóstico](../azure-monitor/platform/platform-logs-overview.md) de recursos en la cuenta de Batch. Recopile y consuma estos datos de diversas maneras para supervisar la cuenta de Batch y diagnosticar problemas. También puede configurar [alertas de métricas](../azure-monitor/platform/alerts-overview.md) para recibir notificaciones cuando una métrica alcance un valor especificado.

## <a name="batch-metrics"></a>Métricas de Batch

Las métricas son datos de telemetría de Azure (también denominados contadores de rendimiento) que emiten los recursos de Azure y que consume el servicio Azure Monitor. Ejemplos de métricas en una cuenta de Batch son: eventos de creación de grupos, recuento de nodos de prioridad baja y eventos de finalización de tareas.

Consulte la [lista de métricas admitidas de Batch](../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts).

Las métricas se caracterizan por:

- Se habilitan de forma predeterminada en cada cuenta de Batch sin ninguna configuración adicional.
- Se generan cada 1 minuto.
- No se guardan automáticamente, sino que tienen un historial acumulado de 30 días. Se pueden conservar las métricas de actividad como parte del registro de diagnóstico.

## <a name="view-batch-metrics"></a>Visualización de métricas de Batch

En Azure Portal, la página **Información general** de la cuenta mostrará de forma predeterminada las métricas clave de nodo, núcleo y tarea.

Para ver todas las métricas de la cuenta de Batch en Azure Portal:

1. En Azure Portal, seleccione **Todos los servicios** > **Cuentas de Batch** y, después, elija el nombre de la cuenta de Batch.
2. En **Supervisión**, seleccione **Métricas**.
3. Seleccione **Agregar métrica** y elija una métrica de la lista desplegable.
4. Seleccione una opción de **Agregación** para la métrica. En el caso de métricas basadas en recuentos (como "Recuento de núcleos dedicados" o "Recuento de nodos de baja prioridad"), use la agregación **Promedio**. En el caso de métricas basadas en eventos (como "Eventos de finalización de cambio de tamaño de grupo"), use la agregación **Recuento**.

   > [!WARNING]
   > No use la agregación "Suma", que suma los valores de todos los puntos de datos recibidos durante el período del gráfico.

5. Para agregar métricas adicionales, repita los pasos 3 y 4.

También puede recuperar las métricas mediante programación con las API de Azure Monitor. Para ver ejemplos, consulte [Recuperación de métricas de Azure Monitor con .NET](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/).

### <a name="batch-metric-reliability"></a>Confiabilidad de métricas de Batch

Las métricas pueden ayudar a identificar tendencias y se pueden usar para el análisis de datos. Es importante observar que la entrega de métricas no está garantizada, y puede estar sujeta a entregas desordenadas, pérdidas de datos o duplicaciones. Debido a esto, no se recomienda el uso de eventos únicos para funciones de alerta o de desencadenador. Para más información sobre cómo establecer los umbrales de alerta, consulte la sección siguiente.

Es posible que las métricas emitidas en los 3 últimos minutos sigan siendo agregadas, por lo que los valores de métricas pueden no corresponderse con la realidad durante este período de tiempo.

## <a name="batch-metric-alerts"></a>Alertas de métricas de Batch

Puede configurar *alertas de métricas* casi en tiempo real que se desencadenen cuando el valor de una métrica especificada supere un umbral asignado. La alerta genera una notificación cuando la alerta está "Activada" (si se sobrepasa el umbral y se cumple la condición de alerta), así como cuando queda en estado "Resuelto" (cuando se vuelve a sobrepasa el umbral y ya no se cumple la condición).

No se recomienda generar alertas que se desencadenen en un único punto de datos, ya que las métricas están sujetas a entregas desordenadas, pérdidas de datos o duplicaciones. Al crear alertas, puede usar umbrales para tener en cuenta estas incoherencias.

Por ejemplo, podría configurar una alerta de métrica cuando el recuento de núcleos de baja prioridad caiga hasta cierto punto, y así poder ajustar la composición de los grupos. Para obtener mejores resultados, establezca un período de 10 minutos o más en el que se desencadenen alertas si el recuento promedio de núcleos de prioridad baja cae por debajo del valor de umbral en todo el período. Esto deja más tiempo para que se agreguen métricas y así obtener resultados más precisos.

Para configurar una alerta de métrica en Azure Portal:

1. Seleccione **Todos los servicios** > **Cuentas de Batch** y, después, seleccione el nombre de la cuenta de Batch.
2. En **Supervisión**, seleccione **Alertas** y haga clic en **Nueva regla de alertas**.
3. Haga clic en **Select condition** (Seleccionar condición) y elija una métrica. Confirme los valores para **Período del gráfico**, **Threshold type** (Tipo de umbral), **Operator** (Operador) y **Tipo de agregación** y escriba un **valor de umbral**. A continuación, seleccione **Done** (Listo).
4. Agregue un grupo de acciones a la alerta, ya sea seleccionando un grupo de acciones existente o creando uno nuevo.
5. En la sección **Alert rule details** (Detalles de la regla de alerta), escriba un **nombre de la regla de alertas** y una **descripción** y seleccione **Severity** (Gravedad).
6. Seleccione **Crear regla de alertas**.

Para más información sobre la creación de alertas de métricas, consulte [Comprender cómo funcionan las alertas de métricas en Azure Monitor](../azure-monitor/platform/alerts-metric-overview.md) y [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../azure-monitor/platform/alerts-metric.md).

También puede configurar una alerta casi en tiempo real mediante la [API REST](/rest/api/monitor/) de Azure Monitor. Para más información sobre las alertas, consulte [Información general sobre las alertas en Microsoft Azure](../azure-monitor/platform/alerts-overview.md). Para incluir información específica de trabajos, tareas o grupos en las alertas, consulte la información sobre las consultas de búsqueda en [Respuesta a eventos con las alertas de Azure Monitor](../azure-monitor/learn/tutorial-response.md).

## <a name="batch-diagnostics"></a>Diagnóstico de Batch

Los registros de diagnóstico contienen información que emiten los recursos de Azure que describe el funcionamiento de cada recurso. En Batch, puede recopilar los registros siguientes:

- Eventos de **registros de servicio** que emite el servicio Azure Batch durante la vigencia de un recurso de Batch individual, como un grupo o una tarea.
- Registros de **métricas** en el nivel de cuenta.

La configuración para habilitar la recopilación de registros de diagnóstico no está habilitada de forma predeterminada. Habilite explícitamente la configuración de diagnóstico de cada cuenta de Batch que quiera supervisar.

### <a name="log-destinations"></a>Destinos de registro

Un escenario común consiste en seleccionar una cuenta de Azure Storage como destino del registro. Para almacenar los registros en Azure Storage, cree la cuenta antes de habilitar la recopilación de registros. Si asoció una cuenta de almacenamiento con su cuenta de Batch, puede elegir esa cuenta como destino de registro.

Como alternativa, puede:

- Transmita los eventos de registro de diagnóstico de Batch a un [centro de eventos de Azure](../event-hubs/event-hubs-about.md). Event Hubs puede ingerir millones de eventos por segundo, que posteriormente se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real.
- Envíe los registros de diagnóstico a los [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md), donde puede analizarlos o exportarlos para su análisis en Power BI o Excel.

> [!NOTE]
> Es posible que el almacenamiento o el procesamiento de datos de registro de diagnóstico con servicios de Azure conlleve costos adicionales.

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Habilitación de la recopilación de registros de diagnóstico de Batch

Para crear una configuración de diagnóstico en Azure Portal, siga los pasos que se indican a continuación.

1. En Azure Portal, seleccione **Todos los servicios** > **Cuentas de Batch** y, después, elija el nombre de la cuenta de Batch.
2. En **Supervisión**, seleccione **Configuración de diagnóstico**.
3. En **Configuración de diagnóstico**, seleccione **Agregar configuración de diagnóstico**.
4. Escriba un nombre para la configuración.
5. Seleccione un destino: **Enviar a Log Analytics**, **Archivar en una cuenta de almacenamiento** y **Transmitir a un centro de eventos**. Si selecciona una cuenta de almacenamiento, puede definir opcionalmente una directiva de retención. Si no especifica un número de días de retención, los datos se conservan durante la vigencia de la cuenta de almacenamiento.
6. Seleccione **ServiceLog**, **AllMetrics**, o ambas.
7. Seleccione **Guardar** para crear la configuración de diagnóstico.

También puede [habilitar la recopilación a través de Azure Monitor en Azure Portal](../azure-monitor/platform/diagnostic-settings.md) para configurar las opciones de diagnóstico, mediante una [plantilla de Resource Manager](../azure-monitor/platform/diagnostic-settings-template.md) o con Azure PowerShell o la CLI de Azure. Para más información, consulte [Introducción a los registros de plataforma Azure](../azure-monitor/platform/platform-logs-overview.md).

### <a name="access-diagnostics-logs-in-storage"></a>Acceso a los registros de diagnóstico en el almacenamiento

Si archiva registros de diagnóstico de Batch en una cuenta de almacenamiento, se crea un contenedor de almacenamiento en la cuenta de almacenamiento en cuanto se produce un evento relacionado. Se crean blobs según el siguiente patrón de nomenclatura:

```json
insights-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/
RESOURCEGROUPS/{resource group name}/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/{Batch account name}/y={four-digit numeric year}/
m={two-digit numeric month}/d={two-digit numeric day}/
h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Por ejemplo:

```json
insights-metrics-pt1m/resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/
RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/MYBATCHACCOUNT/y=2018/m=03/d=05/h=22/m=00/PT1H.json
```

Cada archivo de blob `PT1H.json` contiene eventos con formato JSON que se producen dentro de la hora especificada en la dirección URL del blob (por ejemplo, `h=12`). Durante la hora en cuestión, los eventos se anexan al archivo `PT1H.json` a medida que se producen. El valor de los minutos (`m=00`) siempre es `00`, ya que los eventos de los registros de diagnóstico se dividen en blobs individuales por hora. (Todas las horas se muestran en UTC).

A continuación se muestra un ejemplo de una entrada `PoolResizeCompleteEvent` en un archivo de registro `PT1H.json`. Incluye información sobre el número actual y el número que se desea conseguir de nodos dedicados y de baja prioridad, así como la hora de inicio y de finalización de la operación:

```json
{ "Tenant": "65298bc2729a4c93b11c00ad7e660501", "time": "2019-08-22T20:59:13.5698778Z", "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/BATCHACCOUNTS/MYBATCHACCOUNT/", "category": "ServiceLog", "operationName": "PoolResizeCompleteEvent", "operationVersion": "2017-06-01", "properties": {"id":"MYPOOLID","nodeDeallocationOption":"Requeue","currentDedicatedNodes":10,"targetDedicatedNodes":100,"currentLowPriorityNodes":0,"targetLowPriorityNodes":0,"enableAutoScale":false,"isAutoPool":false,"startTime":"2019-08-22 20:50:59.522","endTime":"2019-08-22 20:59:12.489","resultCode":"Success","resultMessage":"The operation succeeded"}}
```

Para más información sobre el esquema de registros de diagnóstico en la cuenta de almacenamiento, consulte [Archivado de registros de recurso de Azure en la cuenta de almacenamiento](../azure-monitor/platform/resource-logs.md#send-to-azure-storage). Para acceder a los registros de la cuenta de almacenamiento mediante programación, use las API de Storage.

### <a name="service-log-events"></a>Eventos de registro del servicio

Los registros de servicio de Azure Batch, cuando se recopilan, contienen los eventos que emite el servicio Azure Batch durante la vigencia de un recurso de Batch, como un grupo o una tarea. Cada evento que emite Batch se registra en formato JSON. Por ejemplo, este es el cuerpo de un **evento de creación de grupos** de ejemplo:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "5",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "taskSlotsPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Los eventos de registro de servicio emitidos por el servicio Batch incluyen los siguientes:

- [Creación del grupo](batch-pool-create-event.md)
- [Inicio de eliminación del grupo](batch-pool-delete-start-event.md)
- [Finalización de eliminación del grupo](batch-pool-delete-complete-event.md)
- [Inicio de cambio de tamaño del grupo](batch-pool-resize-start-event.md)
- [Finalización de cambio de tamaño del grupo](batch-pool-resize-complete-event.md)
- [Escalabilidad automática de grupo](batch-pool-autoscale-event.md)
- [Inicio de tarea](batch-task-start-event.md)
- [Finalización de tarea](batch-task-complete-event.md)
- [Error en la tarea](batch-task-fail-event.md)
- [Error en programación de tareas](batch-task-schedule-fail-event.md)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información acerca de las [API y herramientas de Batch](batch-apis-tools.md) disponibles para la creación de soluciones de Batch.
- Más información sobre la [supervisión de soluciones de Batch](monitoring-overview.md).
