---
title: Supervisión de aplicaciones de Python con Azure Monitor (versión preliminar) | Microsoft Docs
description: Se proporcionan instrucciones para conectar Python de OpenCensus con Azure Monitor.
ms.topic: conceptual
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6ef0675e3ae3f7a5da38138177f3033051723411
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537115"
---
# <a name="set-up-azure-monitor-for-your-python-application"></a>Configuración de Azure Monitor para la aplicación de Python

Azure Monitor admite seguimiento distribuido, recopilación de métricas y registro de aplicaciones de Python gracias a la integración con [OpenCensus](https://opencensus.io). En este artículo, encontrará instrucciones para configurar OpenCensus para Python y enviar los datos de supervisión a Azure Monitor.

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Instalación de Python. En este artículo se usa [Python 3.7.0](https://www.python.org/downloads/), aunque es probable que las versiones anteriores funcionen con cambios menores.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="create-an-application-insights-resource-in-azure-monitor"></a>Creación de un recurso de Application Insights en Azure Monitor

En primer lugar, tiene que crear un recurso de Application Insights en Azure Monitor, que generará una clave de instrumentación (ikey). La clave ikey se usará entonces para configurar el SDK de OpenCensus y enviar datos de telemetría a Azure Monitor.

1. Seleccione **Crear un recurso** > **Herramientas de desarrollo** > **Application Insights**.

   ![Adición de recursos en Application Insights](./media/opencensus-python/0001-create-resource.png)

1. Aparece un cuadro de configuración. Use la tabla siguiente para rellenar los campos de entrada.

   | Configuración        | Value           | Descripción  |
   | ------------- |:-------------|:-----|
   | **Nombre**      | Valor único globalmente | Nombre que identifica la aplicación que está supervisando. |
   | **Grupo de recursos**     | myResourceGroup      | Nombre para el nuevo grupo de recursos que hospedará los datos de Application Insights. |
   | **Ubicación** | Este de EE. UU. | Ubicación cerca de usted o de donde se hospeda la aplicación. |

1. Seleccione **Crear**.

## <a name="instrument-with-opencensus-python-sdk-for-azure-monitor"></a>Instrumentación con el SDK de Python para OpenCensus en Azure Monitor

Instale los exportadores de Azure Monitor de OpenCensus:

```console
python -m pip install opencensus-ext-azure
```

Para una lista completa de los paquetes y las integraciones, consulte [Paquetes de OpenCensus](https://docs.microsoft.com/azure/azure-monitor/app/nuget#common-packages-for-python-using-opencensus).

> [!NOTE]
> El comando `python -m pip install opencensus-ext-azure` supone que tiene una variable de entorno `PATH` establecida para la instalación de Python. Si no ha configurado esta variable, deberá proporcionar la ruta de acceso completa del directorio a la ubicación del archivo ejecutable de Python. El resultado es un comando como el siguiente: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

El SDK emplea tres exportadores de Azure Monitor para enviar distintos tipos de telemetría a Azure Monitor: seguimiento, métricas y registros. Para obtener más información sobre estos tipos de telemetría, consulte [la información general sobre la plataforma de datos](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform). Utilice las instrucciones siguientes para enviar estos tipos de telemetría a través de los tres exportadores.

## <a name="telemetry-type-mappings"></a>Asignaciones de tipos de telemetría

Estos son los exportadores que proporciona OpenCensus asignados a los tipos de telemetría que verá en Azure Monitor.

![Captura de pantalla de la asignación de tipos de telemetría de OpenCensus a Azure Monitor](./media/opencensus-python/0012-telemetry-types.png)

### <a name="trace"></a>Seguimiento

> [!NOTE]
> `Trace` en OpenCensus hace referencia al [seguimiento distribuido](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing). `AzureExporter` envía datos de telemetría de `requests` y `dependency` a Azure Monitor.

1. En primer lugar, vamos a generar algunos datos de seguimiento localmente. En el IDLE de Python, o en el editor que prefiera, escriba el código siguiente:

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. Al ejecutar el código, se le solicitará varias veces que escriba un valor. Con cada entrada, el valor se imprimirá en el shell y el módulo para Python de OpenCensus generará un fragmento correspondiente de `SpanData`. El proyecto OpenCensus define un [seguimiento como un árbol de intervalos](https://opencensus.io/core-concepts/tracing/).
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. Aunque introducir valores es útil con fines de demostración, la finalidad básica es emitir el valor de `SpanData` a Azure Monitor. Pase la cadena de conexión directamente al exportador. Como alternativa, especifíquela en una variable de entorno `APPLICATIONINSIGHTS_CONNECTION_STRING`. Modifique el código del paso anterior en función del ejemplo de código siguiente:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. Ahora, al ejecutar el script de Python, todavía se le seguirá pidiendo que especifique los valores, pero en el shell solo se imprime el valor. El valor de `SpanData` creado se enviará a Azure Monitor. Puede encontrar los datos del intervalo emitidos en `dependencies`. Para obtener más detalles sobre las solicitudes de salida, vea las [dependencias](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-dependency) de Python para OpenCensus.
Para obtener más detalles sobre las solicitudes de entrada, vea las [solicitudes](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-request) de Python para OpenCensus.

#### <a name="sampling"></a>muestreo

Para información sobre el muestreo en OpenCensus, eche un vistazo al [muestreo en OpenCensus](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications).

#### <a name="trace-correlation"></a>Correlación de seguimiento

Para obtener detalles sobre la correlación de telemetría de los datos de seguimiento, eche un vistazo a la [correlación de telemetría](https://docs.microsoft.com/azure/azure-monitor/app/correlation#telemetry-correlation-in-opencensus-python) de Python para OpenCensus.

#### <a name="modify-telemetry"></a>Modificación de la telemetría

Para obtener detalles sobre cómo modificar la telemetría sometida a seguimiento antes de enviarla a Azure Monitor, vea los [procesadores de telemetría](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors) de Python para OpenCensus.

### <a name="metrics"></a>Métricas

1. Primero, se van a generar algunos datos de métrica locales. Se creará una métrica sencilla para realizar el seguimiento del número de veces que el usuario presiona la tecla Entrar.

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. Al ejecutar el código, se le solicitará varias veces que presione Entrar. Se crea una métrica para realizar el seguimiento del número de veces que se presiona esta tecla. Con cada entrada, el valor se incrementará y la información de la métrica se mostrará en la consola. La información incluye el valor actual y la marca de tiempo actual en el momento en que se actualizó la métrica.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Aunque introducir valores es útil con fines de demostración, la finalidad básica es emitir los datos de la métrica a Azure Monitor. Pase la cadena de conexión directamente al exportador. Como alternativa, especifíquela en una variable de entorno `APPLICATIONINSIGHTS_CONNECTION_STRING`. Modifique el código del paso anterior en función del ejemplo de código siguiente:

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')

    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. El exportador enviará los datos de métricas a Azure Monitor según un intervalo fijo. El valor predeterminado es cada 15 segundos. Se está realizando un seguimiento de una sola métrica. Por tanto, los datos de esta métrica se enviarán en cada intervalo con el valor y la marca de tiempo que contengan. Puede encontrar los datos en `customMetrics`.

#### <a name="standard-metrics"></a>Métricas estándar

De forma predeterminada, el exportador de métricas envía un conjunto de métricas estándar a Azure Monitor. Puede deshabilitar este comportamiento si establece la marca `enable_standard_metrics` en `False` en el constructor del exportador de métricas.

    ```python
    ...
    exporter = metrics_exporter.new_metrics_exporter(
      enable_standard_metrics=False,
      connection_string='InstrumentationKey=<your-instrumentation-key-here>')
    ...
    ```
A continuación se muestra una lista de métricas estándar que se envían actualmente:

- Memoria disponible (bytes)
- Tiempo de procesador de CPU (porcentaje)
- Tasa de solicitudes entrantes (por segundo)
- Tiempo de ejecución promedio de solicitudes entrantes (milisegundos)
- Tasa de solicitudes salientes (por segundo)
- Uso de CPU de proceso (porcentaje)
- Bytes privados del proceso (bytes)

Debería poder ver estas métricas en `performanceCounters`. La tasa de solicitudes entrantes debe encontrarse en `customMetrics`. Para más información, consulte [Contadores de rendimiento](https://docs.microsoft.com/azure/azure-monitor/app/performance-counters).

#### <a name="modify-telemetry"></a>Modificación de la telemetría

Para obtener detalles sobre cómo modificar la telemetría sometida a seguimiento antes de enviarla a Azure Monitor, vea los [procesadores de telemetría](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors) de Python para OpenCensus.

### <a name="logs"></a>Registros

1. Primero, se van a generar algunos datos de registro locales.

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  El código pedirá continuamente que se escriba un valor. Se emite una entrada de registro para cada valor especificado.

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. Aunque introducir valores es útil con fines de demostración, la finalidad básica es emitir los datos del registro a Azure Monitor. Pase la cadena de conexión directamente al exportador. Como alternativa, especifíquela en una variable de entorno `APPLICATIONINSIGHTS_CONNECTION_STRING`. Modifique el código del paso anterior en función del ejemplo de código siguiente:

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. El exportador enviará los datos de registro a Azure Monitor. Puede encontrar los datos en `traces`. 

> [!NOTE]
> En este contexto, `traces` no es igual que `Tracing`. `traces` hace referencia al tipo de telemetría que verá en Azure Monitor al utilizar `AzureLogHandler`. `Tracing` hace referencia a un concepto de OpenCensus y se relaciona con el [seguimiento distribuido](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing).

5. Para dar formato a los mensajes de registro, puede usar `formatters` en la [API de registro](https://docs.python.org/3/library/logging.html#formatter-objects) de Python integrada.

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    format_str = '%(asctime)s - %(levelname)-8s - %(message)s'
    date_format = '%Y-%m-%d %H:%M:%S'
    formatter = logging.Formatter(format_str, date_format)
    # TODO: replace the all-zero GUID with your instrumentation key.
    handler = AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

6. También puede agregar propiedades personalizadas a los mensajes de registro del argumento de palabra clave *extra* mediante el campo custom_dimensions. Estas aparecerán como pares de clave y valor en `customDimensions` en Azure Monitor.
> [!NOTE]
> Para que esta característica funcione, debe pasar un diccionario al campo custom_dimensions. Si se pasan argumentos de cualquier otro tipo, el registrador los omitirá.

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )

    properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

    # Use properties in logging statements
    logger.warning('action', extra=properties)
    ```

#### <a name="sending-exceptions"></a>Envío de excepciones

OpenCensus Python no realiza de forma automática un seguimiento de la telemetría de `exception`, ni tampoco la envía. Se envían mediante `AzureLogHandler` por medio de excepciones a través de la biblioteca de registro de Python. Puede agregar propiedades personalizadas de la misma manera que con el registro normal.

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )

    properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

    # Use properties in exception logs
    try:
        result = 1 / 0  # generate a ZeroDivisionError
    except Exception:
        logger.exception('Captured an exception.', extra=properties)
    ```
Dado que debe registrar las excepciones de manera explícita, es decisión del usuario cómo quiere registrar las excepciones no controladas. OpenCensus no impone restricciones en el modo en que un usuario quiera hacerlo, siempre y cuando registre explícitamente una telemetría de excepciones.

#### <a name="sampling"></a>muestreo

Para información sobre el muestreo en OpenCensus, eche un vistazo al [muestreo en OpenCensus](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications).

#### <a name="log-correlation"></a>Correlación de registros

Para más información sobre cómo enriquecer los registros con los datos de contexto de seguimiento, vea la [integración de registros](https://docs.microsoft.com/azure/azure-monitor/app/correlation#log-correlation) de Python para OpenCensus.

#### <a name="modify-telemetry"></a>Modificación de la telemetría

Para obtener detalles sobre cómo modificar la telemetría sometida a seguimiento antes de enviarla a Azure Monitor, vea los [procesadores de telemetría](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors) de Python para OpenCensus.

## <a name="view-your-data-with-queries"></a>Visualización de los datos con consultas

Puede ver los datos de telemetría que se enviaron desde la aplicación mediante la pestaña **Registros (Analytics)** .

![Captura de pantalla del panel de información general con la pestaña "Registros (Analytics)" seleccionada en el cuadro rojo.](./media/opencensus-python/0010-logs-query.png)

En la lista de **Activo**:

- En el caso de la telemetría enviada con el exportador de seguimiento de Azure Monitor, las solicitudes entrantes se muestran en `requests`. Las solicitudes salientes o en proceso se muestran en `dependencies`.
- En el caso de la telemetría enviada con el exportador de métricas de Azure Monitor, las métricas enviadas se muestran en `customMetrics`.
- En el caso de la telemetría enviada con el exportador de registros de Azure Monitor, los registros se muestran en `traces`. Las excepciones aparecen en `exceptions`.

Para obtener información más detallada sobre cómo usar las consultas y los registros, consulte [Registros en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs).

## <a name="learn-more-about-opencensus-for-python"></a>Más información sobre OpenCensus para Python

* [Python de OpenCensus en GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Personalización](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Exportadores de Azure Monitor en GitHub](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)
* [Integraciones de OpenCensus](https://github.com/census-instrumentation/opencensus-python#extensions)
* [Aplicaciones de ejemplo de Azure Monitor](https://github.com/Azure-Samples/azure-monitor-opencensus-python)

## <a name="next-steps"></a>Pasos siguientes

* [Seguimiento de las solicitudes entrantes](./../../azure-monitor/app/opencensus-python-dependency.md)
* [Seguimiento de las solicitudes salientes](./../../azure-monitor/app/opencensus-python-request.md)
* [Mapa de aplicación](./../../azure-monitor/app/app-map.md)
* [Supervisión del rendimiento de un extremo a otro](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Alertas

* [Pruebas de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md): cree pruebas para asegurarse de que el sitio sea visible en la Web.
* [Diagnósticos inteligentes](../../azure-monitor/app/proactive-diagnostics.md): estas pruebas se realizan automáticamente, por lo que no es preciso hacer nada para configurarlas. Le indican si la aplicación tiene una tasa de solicitudes con error inusual.
* [Alertas de métricas](../../azure-monitor/app/alerts.md): Establezca alertas que le adviertan si una métrica supera un umbral. Puede establecerlas en las métricas personalizadas que codifique en la aplicación.
