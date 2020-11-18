---
title: Métricas en Azure Monitor | Microsoft Docs
description: Describe las métricas en Azure Monitor que son datos de supervisión ligeros capaces de admitir escenarios casi en tiempo real.
documentationcenter: ''
author: bwren
manager: carmonm
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: f64a91e3b285c265296c361366a10443eda18201
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94489420"
---
# <a name="azure-monitor-metrics-overview"></a>Información general sobre las métricas en Microsoft Azure
Las métricas en Azure Monitor son una característica de Azure Monitor que recopila datos numéricos de [recursos supervisados](../monitor-reference.md) en una base de datos de serie temporal. Las métricas son valores numéricos que se recopilan a intervalos regulares y describen algún aspecto de un sistema en un momento determinado. Las métricas en Azure Monitor son ligeras y capaces de admitir escenarios casi en tiempo real, lo que hace que sean especialmente útiles para las alertas y una detección rápida de problemas. Puede analizarlas de forma interactiva con el explorador de métricas, recibir una notificación de forma proactiva con una alerta cuando un valor cruza un umbral o visualizarlas en un libro o panel.


> [!NOTE]
> Las métricas de Azure Monitor son la mitad de la plataforma de datos de Azure Monitor. La otra son los [registros de Azure Monitor](data-platform-logs.md), que recopilan y organizan los datos de registro y de rendimiento, y permiten que se analicen con un lenguaje de consulta enriquecido. Las métricas son más ligeras que los datos de los registros de Azure Monitor y capaces de admitir escenarios casi en tiempo real, lo que hace que sean especialmente útiles para las alertas y la detección rápida de problemas. Sin embargo, las métricas solo pueden almacenar datos numéricos en una estructura determinada, mientras que los registros pueden almacenar una variedad de tipos de datos distintos, cada uno con su propia estructura. También puede hacer análisis complejos en los datos de los registros mediante el uso de consultas de registros que no se pueden usar para el análisis de los datos de las métricas.


## <a name="what-can-you-do-with-azure-monitor-metrics"></a>¿Qué puede hacer con las métricas de Azure Monitor?
En la tabla siguiente se enumeran las distintas maneras en que se pueden usar las métricas en Azure Monitor.

|  |  |
|:---|:---|
| **Análisis** | Use el [Explorador de métricas](metrics-charts.md) para analizar las métricas recopiladas en un gráfico y compare las métricas de diferentes recursos. |
| **Alerta** | Configure una [regla de alertas de métricas](alerts-metric.md) que envíe una notificación o realice una [acción automatizada](action-groups.md) cuando el valor de la métrica cruce un umbral. |
| **Visualizar** | Ancle un gráfico del Explorador de métricas en un [panel de Azure](../learn/tutorial-app-dashboards.md).<br>Cree un [libro](./workbooks-overview.md) para combinar con varios conjuntos de datos en un informe interactivo. Exporte los resultados de una consulta a [Grafana](grafana-plugin.md) para aprovechar sus paneles y combinarlos con otros orígenes de datos. |
| **Automatización** |  Use la [escalabilidad automática](autoscale-overview.md) para aumentar o disminuir los recursos según un valor de métrica que cruce un umbral. |
| **Recuperar** | Obtenga acceso a los valores de métricas desde una línea de comandos mediante [cmdlets de PowerShell](/powershell/module/az.applicationinsights).<br>Obtenga acceso a los valores de métricas de aplicaciones personalizadas con [API REST](rest-api-walkthrough.md).<br>Obtenga acceso a los valores de métricas desde una línea de comandos mediante la [CLI](/cli/azure/monitor/metrics). |
| **Exportarar** | [Redirija las métricas a los registros](./resource-logs.md#send-to-azure-storage) para analizar los datos en las métricas en Microsoft Azure junto con los datos en los registros de Azure Monitor y para almacenar los valores de las métricas durante un período superior a 93 días.<br>Haga streaming de métricas a un [centro de eventos](stream-monitoring-data-event-hubs.md) para redirigirlas a sistemas externos. |
| **Archivar** | [Archivar](./platform-logs-overview.md) el historial de rendimiento o estado del recurso para fines de cumplimiento, auditoría o creación de informes sin conexión. |

![Información general de las métricas](media/data-platform-metrics/metrics-overview.png)


## <a name="data-collection"></a>datos, recopilación
Las métricas que Azure Monitor recopila proceden de tres orígenes fundamentales. Una vez que estas métricas se recopilan en la base de datos de métricas de Azure Monitor, se pueden evaluar juntas independientemente de su origen.

**Recursos de Azure**. Las métricas de plataforma se crean mediante los recursos de Azure y brindan visibilidad sobre su estado y rendimiento. Cada tipo de recurso crea un [conjunto distinto de métricas](metrics-supported.md) sin ninguna configuración necesaria. Las métricas de plataforma se recopilan de los recursos de Azure con una frecuencia de un minuto, a menos que se especifique lo contrario en la definición de la métrica. 

**Aplicaciones**. Las métricas se crean mediante Application Insights para sus aplicaciones supervisadas, y le ayudan a detectar problemas de rendimiento y a realizar un seguimiento de las tendencias de uso de la aplicación. Incluye valores como _Tiempo de respuesta del servidor_ y _Excepciones del explorador_.

**Agentes de máquina virtual**. Las métricas se recopilan desde el sistema operativo invitado de una máquina virtual. Habilite las métricas de SO invitado para máquinas virtuales Windows con la [extensión de diagnóstico de Windows (WAD)](./diagnostics-extension-overview.md) y para las máquinas virtuales Linux con el [agente de InfluxData Telegraf](https://www.influxdata.com/time-series-platform/telegraf/).

**Métricas personalizadas**. Puede definir métricas además de las métricas estándar que están disponibles automáticamente. También puede [definir métricas personalizadas en la aplicación](../app/api-custom-events-metrics.md) que se supervisa mediante Application Insights o crear métricas personalizadas para un servicio de Azure con la [API de métricas personalizadas](metrics-store-custom-rest-api.md).

- Consulte [¿Qué supervisa Azure Monitor?](../monitor-reference.md) para conocer una lista completa de los orígenes de datos que pueden enviar datos a métricas en Azure Monitor.

## <a name="metrics-explorer"></a>Explorador de métricas
Use el [Explorador de métricas](metrics-charts.md) para analizar de forma interactiva los datos en la base de datos de métricas y representar los valores de varias métricas con el tiempo. Puede anclar los gráficos a un panel para verlos con otras visualizaciones. También puede recuperar las métricas con la [API REST de supervisión de Azure](rest-api-walkthrough.md).

![Explorador de métricas](media/data-platform/metrics-explorer.png)

- Consulte [Introducción al Explorador de métricas de Azure Monitor](metrics-getting-started.md) para comenzar a usar el Explorador de métricas.

## <a name="data-structure"></a>Estructura de los datos
Los datos recopilados por las métricas en Azure Monitor se almacenan en una base de datos de serie temporal que está optimizada para el análisis de los datos con marca de tiempo. Cada conjunto de valores de métricas es una serie temporal con las siguientes propiedades:

* Hora en que se recopiló el valor.
* Recurso al que el valor está asociado.
* Espacio de nombres que actúa como una categoría para la métrica.
* Nombre de la métrica.
* El propio valor.
* Algunas métricas pueden tener varias dimensiones, como se describe en la sección [Métricas multidimensionales](#multi-dimensional-metrics). Las métricas personalizadas pueden tener hasta 10 dimensiones.

## <a name="multi-dimensional-metrics"></a>Métricas multidimensionales
Uno de los desafíos de los datos de métricas es que a menudo tienen información limitada para proporcionar contexto para los valores recopilados. Azure Monitor aborda este desafío con métricas multidimensionales. Las dimensiones de una métrica son pares nombre-valor que transportan datos adicionales para describir el valor de la métrica. Por ejemplo, una métrica _Espacio disponible en disco_ puede tener una dimensión denominada _Unidad_ con los valores _C:_ o _D:_ , que permitiría ver cualquier espacio disponible en disco de todas las unidades o de cada unidad individual.

En el ejemplo siguiente se muestran dos conjuntos de datos para una métrica hipotética denominada _Rendimiento de la red_. El primer conjunto de datos no tiene ninguna dimensión. El segundo conjunto de datos muestra los valores con dos dimensiones, _Dirección IP_ y _Dirección_:

### <a name="network-throughput"></a>Rendimiento de la red

| Timestamp     | Valor de métrica |
| ------------- |:-------------|
| 9/8/2017 8:14 | 1331,8 Kbps |
| 9/8/2017 8:15 | 1141,4 Kbps |
| 9/8/2017 8:16 | 1110,2 Kbps |

Esta métrica no dimensional solo puede responder una pregunta básica, como “¿cuál era el rendimiento de la red en un momento dado?”.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Rendimiento de la red + dos dimensiones (“IP” y “Dirección”)

| Timestamp     | Dimensión “IP”   | Dimensión “Dirección” | Valor de métrica|
| ------------- |:-----------------|:------------------- |:-----------|
| 9/8/2017 8:14 | IP="192.168.5.2" | Dirección="Envío"    | 646,5 Kbps |
| 9/8/2017 8:14 | IP="192.168.5.2" | Dirección="Recepción" | 420,1 Kbps |
| 9/8/2017 8:14 | IP="10.24.2.15"  | Dirección="Envío"    | 150,0 Kbps |
| 9/8/2017 8:14 | IP="10.24.2.15"  | Dirección="Recepción" | 115,2 Kbps |
| 9/8/2017 8:15 | IP="192.168.5.2" | Dirección="Envío"    | 515,2 Kbps |
| 9/8/2017 8:15 | IP="192.168.5.2" | Dirección="Recepción" | 371,1 Kbps |
| 9/8/2017 8:15 | IP="10.24.2.15"  | Dirección="Envío"    | 155,0 Kbps |
| 9/8/2017 8:15 | IP="10.24.2.15"  | Dirección="Recepción" | 100,1 Kbps |

Esta métrica puede responder a preguntas como “¿cuál era el rendimiento de la red para cada dirección IP?” y “¿cuántos datos se enviaron en comparación los datos que se recibieron?”. Las métricas multidimensionales llevan valores analíticos y de diagnóstico adicionales en comparación con las métricas no dimensionales.

## <a name="retention-of-metrics"></a>Retención de métricas
Para la mayoría de los recursos en Azure, las métricas se almacenan durante 93 días. Hay algunas excepciones:

**Métricas de SO invitado**
-   **Métricas clásicas de SO invitado**. Estos son los contadores de rendimiento que recopila la [extensión de diagnóstico de Windows (WAD)](./diagnostics-extension-overview.md) o la [extensión de diagnóstico de Linux (LAD)](../../virtual-machines/extensions/diagnostics-linux.md) y que se enrutan a una cuenta de almacenamiento de Azure. La retención de estas métricas es de 14 días.
-   **Métricas de SO invitado enviadas a las métricas de Azure Monitor**. Estos son los contadores de rendimiento recopilados por [Windows Diagnostic Extension (WAD)](diagnostics-extension-overview.md) que se envían al [receptor de datos de Azure Monitor](diagnostics-extension-overview.md#data-destinations) o que se envían mediante el [Agente de InfluxData Telegraf](https://www.influxdata.com/time-series-platform/telegraf/) en las máquinas Linux. La retención de estas métricas es de 93 días.
-   **Métricas de SO invitado que recopila el agente de Log Analytics**. Estos son contadores de rendimiento que recopila el agente de Log Analytics y que se envían a un área de trabajo de Log Analytics. La retención de estas métricas es de 31 días y se puede ampliar a un máximo de 2 años.

**Métricas basadas en registros de Application Insights**. 
- Entre bastidores, las [métricas basadas en registros](../app/pre-aggregated-metrics-log-metrics.md) se traducen en consultas de registros. Su retención coincide con la retención de eventos en registros subyacentes. Para los recursos de Application Insights, los registros se almacenan durante 90 días.


> [!NOTE]
> También puede [enviar métricas de plataforma para recursos de Azure Monitor a un área de trabajo de Log Analytics](./resource-logs.md#send-to-azure-storage) para tendencias a largo plazo.





## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [plataforma de datos de Azure Monitor](data-platform.md).
- Más información sobre los [datos de registro en Azure Monitor](data-platform-logs.md).
- Obtenga información sobre la [supervisión de datos disponible](data-sources.md) para diferentes recursos en Azure.

