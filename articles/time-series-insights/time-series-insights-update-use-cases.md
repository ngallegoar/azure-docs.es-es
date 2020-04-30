---
title: 'Casos de uso de la versión preliminar: Azure Time Series Insights | Microsoft Docs'
description: Más información sobre casos de uso de la versión preliminar de Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/29/2020
ms.custom: seodec18
ms.openlocfilehash: 93813dfaecf40fcb5cab0856d013fb890118886c
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82580663"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Casos de uso de la versión preliminar de Azure Time Series Insights

En este artículo se resumen varios casos de uso comunes de la versión preliminar de Azure Time Series Insights. Las recomendaciones de este artículo le servirán como punto de partida para desarrollar aplicaciones y soluciones con Time Series Insights.

Concretamente, en este artículo se responde a las siguientes preguntas:

* ¿Cuáles son los casos de uso comunes de Time Series Insights?
* ¿Cuáles son las ventajas de usar Time Series Insights para la [detección de anomalías visuales y la exploración de datos](#data-exploration-and-visual-anomaly-detection)?
* ¿Cuáles son las ventajas de usar Time Series Insights para realizar [análisis operativos y garantizar la eficacia del proceso](#operational-analysis-and-driving-process-efficiency)?
* ¿Cuáles son las ventajas de usar Time Series Insights para realizar [análisis avanzados](#advanced-analytics)?

En las secciones siguientes se incluye información general de estos escenarios de uso.

## <a name="introduction"></a>Introducción

Azure Time Series Insights es una oferta de plataforma como servicio de un extremo a otro. Se usa para recopilar, procesar, almacenar, analizar y consultar datos a escala de IoT optimizados para series temporales y muy contextualizados. Time Series Insights es ideal para la exploración de datos ad hoc y para el análisis operativo. Igualmente, Time Series Insights es una oferta de servicio de extensibilidad única y personalizada que satisface las amplias necesidades de las implementaciones de IoT industriales.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Exploración de datos y detección anomalías visuales

Explore y analice al instante miles de millones de eventos para detectar anomalías y tendencias ocultas en sus datos. Time Series Insights ofrece un rendimiento casi en tiempo real para las cargas de trabajo de análisis de IoT y DevOps.

[![Explorador de datos](media/v2-update-use-cases/data-explorer.png)](media/v2-update-use-cases/data-explorer.png#lightbox)

La mayoría de los clientes aceptan que la cantidad de tiempo mínima necesaria para obtener información es una de las principales características de Time Series Insights:

* Además, Azure Time Series Insights no requiere la preparación inicial de los datos.
* Funciona rápido para conectarse en minutos a miles de millones de eventos en instancias de Azure IoT Hub o de Azure Event Hubs.
* Una vez esté conectado, puede visualizar y analizar al instante miles de millones de eventos para detectar anomalías y tendencias ocultas en sus datos.

Time Series Insights es intuitivo y fácil de usar. Además, puede interactuar con sus datos sin tener que escribir una sola línea de código. Tampoco se le exige que aprenda ningún lenguaje nuevo, aunque Time Series Insights proporciona un lenguaje de consulta basado en texto granular para usuarios avanzados familiarizados con SQL. En cuanto a los principiantes, también proporciona la opción de explorar seleccionando y haciendo clic en el contenido.

Los clientes pueden aprovechar esta velocidad para poder diagnosticar rápidamente problemas relacionados con los recursos. Asimismo, pueden realizar análisis de operaciones de DevOps para llegar a la raíz de un error en una solución de IoT. También pueden identificar las áreas que se van a marcar para una investigación más exhaustiva como parte de sus iniciativas de ciencia de datos.

Existen tres formas principales de interactuar con los datos almacenados en Time Series Insights:

* La primera y la forma más fácil de comenzar es con el Explorador de la versión preliminar de Time Series Insights. Puede usarlo para visualizar rápidamente todos sus datos de IoT en un solo lugar. Además, le proporciona herramientas como el mapa térmico, que le ayudarán a detectar anomalías en sus datos. También proporciona una vista en perspectiva. Puede usar esta vista para comparar hasta cuatro vistas de uno o más entornos de Time Series Insights en un único panel. Este panel le ofrece una vista de sus datos de serie temporal en todas sus ubicaciones. Obtenga más información sobre el [explorador de la versión preliminar de Time Series Insights](./time-series-insights-update-explorer.md). Para planificar su entorno de Time Series Insights, lea la [planificación de Time Series Insights](./time-series-insights-update-plan.md).

* La segunda forma de comenzar es usar el SDK de JavaScript para insertar rápidamente eficaces tablas y gráficos en su aplicación web. Con solo unas pocas líneas de código, puede crear consultas realmente eficaces. Úselas para rellenar gráficos de líneas, gráficos circulares, gráficos de barras, mapas térmicos, cuadrículas de datos y muchos más. Si usa el SDK, tiene todos estos elementos listos para usar. El SDK también sintetiza las API de consulta de Time Series Insights. Puede usarlas para crear predicados similares a SQL para consultar los datos que quiera mostrar en un panel. En cuanto a las soluciones de la capa de presentación híbridas, Time Series Insights le ofrece direcciones URL con parámetros. Estas le proporcionan puntos de conexión sencillos con el explorador de versión preliminar de Time Series Insights para que pueda profundizar en los datos.

  * Para más información sobre el SDK de JavaScript, lea la [biblioteca de cliente de JS de Time Series Insights](https://github.com/microsoft/tsiclient/blob/master/docs/API.md) y la documentación del [cliente de Time Series Insights](https://github.com/Microsoft/tsiclient).

  * Para obtener más información acerca del uso compartido de URL y la nueva IU, consulte [Visualize data in the Azure Time Series Insights Preview explorer](time-series-insights-update-explorer.md) (Visualizar datos en el explorador de la versión preliminar de Azure Time Series Insights).

* La tercera forma de comenzar es usar las eficaces API para consultar los datos almacenados en Time Series Insights. Time Series Insights tiene operadores temporales, como `from`, `to`, `first` y `last`, y tiene agregaciones y transformaciones, como `average`, `min`, `max`, `split by`, `order by` y `DateHistogram`. También tiene operadores de filtrado, como `has`, `in`, `and`, `or`, `greater than` y `REGEX`. Todos estos operadores permiten que las aplicaciones posteriores encuentren rápidamente tendencias y patrones interesantes en sus datos. Puede usarlos para rellenar las visualizaciones locales y así poder detectar anomalías.

## <a name="operational-analysis-and-driving-process-efficiency"></a>Análisis operativo y procesos eficientes

Use Time Series Insights para supervisar el mantenimiento, el uso y el rendimiento del equipo a escala. Time Series Insights le proporciona una manera fácil de medir la eficacia operativa. Igualmente, Time Series Insights facilita la administración de cargas de trabajo de IoT diversas e impredecibles, sin sacrificar el rendimiento de la ingesta o de las consultas.

[![Información general](media/v2-update-use-cases/overview.png)](media/v2-update-use-cases/overview.png#lightbox)

La transmisión y el procesamiento continuo de datos provenientes de procesos operativos pueden transformar con éxito cualquier negocio si se combinan con la tecnología o la solución adecuada. A menudo, estas soluciones son una combinación de varios sistemas. Permiten la exploración y el análisis de datos que cambian constantemente, especialmente en el ámbito de IoT, y comparten un patrón común.

Estos patrones a menudo comienzan con plataformas habilitadas para IoT que ingieren miles de millones de eventos de dispositivos y sensores que abarcan varias configuraciones regionales. Asimismo, estos sistemas procesan y analizan los datos de streaming para obtener detalles y acciones en tiempo real. Los datos normalmente se archivan mediante un almacenamiento en reposo e intermedio para realizar un análisis casi en tiempo real y por lotes.

Los datos que se recopilan pasan por una serie de procesos de limpieza y contextualización para agregarlos a escenarios de análisis y consultas posteriores. Azure ofrece sofisticados servicios que se pueden aplicar a los escenarios de IoT, como el mantenimiento y la fabricación de recursos. Estos servicios incluyen: Time Series Insights, IoT Hub, Event Hubs, Azure Stream Analytics, Azure Functions, Azure Logic Apps, Azure Databricks, Azure Machine Learning y Power BI.

La arquitectura de la solución se puede obtener de la siguiente manera:

* Ingerir datos a través de IoT Hub o Event Hubs para obtener seguridad, rendimiento y latencia de vanguardia.
* Realizar el procesamiento de datos y los cálculos. Canalice los datos ingeridos a través de servicios como Stream Analytics, Logic Apps y Azure Functions. El servicio que use dependerá de las necesidades específicas de procesamiento de datos.
* Las señales calculadas de la canalización de procesamiento se insertan en Time Series Insights para su almacenaje y análisis.

Time Series Insights le ofrece la oportunidad de explorar los datos casi en tiempo real, así como detalles basados en recursos sobre datos históricos. Dependiendo de las necesidades de su negocio, los trabajos de MapReduce y Hive pueden ejecutarse en datos almacenados en Time Series Insights si se conecta Time Series Insights a Azure HDInsight. Los datos almacenados en Time Series Insights están disponibles para Power BI y otras aplicaciones de cliente a través de las API de consulta de superficie públicas de Time Series Insights. Estos datos se pueden usar en escenarios de inteligencia profunda operativa y empresarial.

## <a name="advanced-analytics"></a>Análisis avanzado

Integre sus soluciones con servicios de análisis avanzado, como Azure Machine Learning y Azure Databricks. Time Series Insights incorpora datos sin procesar de millones de dispositivos. Además, agrega datos contextuales que un conjunto de servicios de análisis de Azure pueden consumir de forma sencilla.

[![Análisis](media/v2-update-use-cases/advanced-analytics.png)](media/v2-update-use-cases/advanced-analytics.png#lightbox)

El análisis avanzado y el aprendizaje automático consumen y procesan grandes volúmenes de datos. Estos datos se usan para tomar decisiones basadas en datos y realizar análisis predictivos. En los casos de uso de IoT, los algoritmos de análisis avanzados aprenden de los datos recopilados de millones de dispositivos. Estos dispositivos transmiten datos varias veces cada segundo. Los datos recopilados de los dispositivos de IoT están sin procesar. Carecen de información contextual, como la ubicación del dispositivo y la unidad de lectura del sensor. Como resultado, los datos sin procesar son difíciles de consumir directamente para realizar análisis avanzados.

Time Series Insights cierra esta brecha entre los datos de IoT y el análisis avanzado de dos maneras simples y rentables:

* Primero, Time Series Insights recopila los datos de telemetría sin procesar de millones de dispositivos mediante IoT Hub. A continuación, enriquece los datos con información contextual y los transforma en un formato parquet. Este formato se puede integrar fácilmente con otros servicios analíticos avanzados, como Machine Learning, Azure Databricks y aplicaciones de terceros.

    Time Series Insights puede servir como fuente de datos para todos los datos de toda la organización. Crea un repositorio central para que las cargas de trabajo analíticas descendentes se consuman. Como Time Series Insights es un servicio de almacenamiento casi en tiempo real, los modelos analíticos avanzados pueden aprender continuamente de los datos de telemetría entrantes de IoT. Gracias a ello, los modelos pueden hacer predicciones más precisas.

* En segundo lugar, Time Series Insights puede beneficiarse de la salida del aprendizaje automático y los modelos de predicción para visualizar y almacenar sus resultados. Este procedimiento permite que las organizaciones tengan la oportunidad de optimizar y ajustar sus modelos. Time Series Insights simplifica la visualización de los datos de telemetría de streaming en el mismo plano que las salidas del modelo entrenado. De esta manera, ayuda a los equipos de ciencia de datos a detectar anomalías e identificar patrones.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre el [explorador de la versión preliminar de Time Series Insights](./time-series-insights-update-explorer.md).
* Para planificar su entorno, lea la [planificación de la versión preliminar de Time Series Insights](./time-series-insights-update-plan.md).
* Lea la documentación sobre el [cliente de Time Series Insights](https://github.com/Microsoft/tsiclient).
