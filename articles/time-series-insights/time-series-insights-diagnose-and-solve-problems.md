---
title: 'Diagnóstico y resolución de problemas: Azure Time Series Insights | Microsoft Docs'
description: En este artículo se describe cómo diagnosticar y solucionar problemas comunes en el entorno de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 02/04/2020
ms.custom: seodec18
ms.openlocfilehash: 209df97169c71d910677ffdb2e2b12593882445b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80152605"
---
# <a name="diagnose-and-solve-issues-in-your-time-series-insights-environment"></a>Diagnóstico y solución de problemas de su entorno Time Series Insights

En este artículo se describen algunos problemas que podría encontrar en el entorno de Azure Time Series Insights. Se ofrecen posibles causas y soluciones para la resolución.

## <a name="video"></a>Vídeo

### <a name="learn-about-common-time-series-insights-customer-challenges-and-mitigationsbr"></a>Conozca las mitigaciones y desafíos comunes del cliente de Time Series Insights.</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-no-data-is-shown"></a>Problema: no se muestra ningún dato

Hay varias razones por las que podría no ver ningún dato en el [explorador de Azure Time Series Insights](https://insights.timeseries.azure.com):

### <a name="cause-a-event-source-data-isnt-in-json-format"></a>Causa A: los datos de origen del evento no tienen formato JSON

Azure Time Series Insights solo admite datos JSON. Para ver ejemplos de JSON, lea [Formas de JSON admitidas](./how-to-shape-query-json.md).

### <a name="cause-b-the-event-source-key-is-missing-a-required-permission"></a>Causa B: a la clave de origen del evento le falta un permiso necesario

* Para IoT Hub en Azure IoT Hub, debe proporcionar la clave con los permisos de **conexión de servicio**. Seleccione las directivas **iothubowner** o **service**, dado que ambas tienen permisos de **conexión de servicio**.

   [![Permisos de conexión de servicio de IoT Hub](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png#lightbox)

* Para un centro de eventos de Azure Event Hubs, debe proporcionar la clave con permisos de **escucha**. Servirán las directivas **read** o **manage**, dado que ambas tienen permisos de **escucha**.

   [![Permisos de escucha de centro de eventos](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)](media/diagnose-and-solve-problems/eventhub-listen-permissions.png#lightbox)

### <a name="cause-c-the-consumer-group-provided-isnt-exclusive-to-time-series-insights"></a>Causa C: el grupo de consumidores proporcionado no es exclusivo de Time Series Insights

Al registrar un centro de IoT o un centro de eventos, es importante establecer el grupo de consumidores que quiere usar para leer los datos. Este grupo de consumidores *no puede ser compartido*. Si el grupo de consumidores es compartido, el centro de IoT o el centro de eventos subyacente desconectará automáticamente uno de los lectores de forma aleatoria. Proporcione un grupo de consumidor exclusivo del que Time Series Insights pueda leer.

### <a name="cause-d-the-environment-has-just-been-provisioned"></a>Causa D: el entorno se ha aprovisionado recientemente

Los datos aparecerán en el explorador de Time Series Insights en unos minutos después de que se creen por primera vez el entorno y sus datos.

## <a name="problem-some-data-is-shown-but-data-is-missing"></a>Problema: se muestran algunos datos, pero faltan otros

Cuando los datos aparecen solo parcialmente y parece que están atrasados, debe considerar varias posibilidades.

### <a name="cause-a-your-environment-is-being-throttled"></a>Causa A: puede que se esté limitando el entorno

La [limitación](time-series-insights-environment-mitigate-latency.md) es un problema común cuando se aprovisionan entornos después de crear un origen de eventos que tiene datos. Azure IoT Hub y Azure Events Hubs almacenan datos durante siete días como máximo. Time Series Insights siempre empieza con el evento más antiguo en el origen de eventos (primero en entrar, primero en salir o *FIFO*).

Por ejemplo, si tiene 5 millones de eventos en un origen de eventos al conectarse a un entorno S1 de Time Series Insights de una sola unidad, Time Series Insights lee aproximadamente 1 millón de eventos al día. Puede parecer que Time Series Insights está experimentando cinco días de latencia. Sin embargo, lo que sucede es que el entorno está limitado.

Si tiene los eventos antiguos en el origen de eventos, puede enfocarlo de dos maneras:

- Cambie los límites de retención del origen de eventos para ayudar a eliminar los eventos antiguos que no quiere que aparezcan en Time Series Insights.
- Aprovisione un tamaño mayor de entorno (número de unidades) para aumentar el rendimiento de los eventos antiguos. Siguiendo con el ejemplo anterior, si aumenta ese mismo entorno de S1 a cinco unidades durante un día, debería actualizarse en el plazo de un día. Si la producción de eventos de estado estable es un millón o menos eventos al día, puede reducir la capacidad del evento a una unidad después de la actualización.

La limitación se aplica según el tipo y la capacidad de la SKU del entorno. Todos los orígenes de eventos del entorno comparten esta capacidad. Si el origen de eventos de la instancia de IoT Hub o del centro de evento inserta datos por encima de los límites exigidos, experimentará limitaciones y retrasos.

En la siguiente figura se muestra un entorno de Time Series Insights con una SKU S1 y una capacidad 3. Este entorno registra 3 millones de eventos al día.

[![Capacidad actual de la SKU del entorno](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)](media/diagnose-and-solve-problems/environment-sku-current-capacity.png#lightbox)

Por ejemplo, supongamos un entorno que ingiere mensajes de un centro de eventos. La velocidad de entrada diaria es de 67 000 mensajes aproximadamente. Esta velocidad se traduce aproximadamente en 46 mensajes por minuto. 

* Si cada mensaje de centro de eventos se acopla a un evento de Time Series Insights, no se produce ninguna limitación. 
* Si cada mensaje de centro de eventos se acopla a 100 eventos de Time Series Insights, se deben ingerir 4600 mensajes por minuto. 

Un entorno de SKU S1 que tiene una capacidad de 3 solo puede admitir la entrada de 2100 eventos por minuto (1 millón de eventos por día = 700 eventos por minuto por 3 unidades = 2100 eventos por minuto). 

Para comprender de una manera más profunda cómo funciona la lógica de aplanamiento, consulte [Formas JSON admitidas](./how-to-shape-query-json.md).

#### <a name="recommended-resolutions-for-excessive-throttling"></a>Resoluciones recomendadas para la limitación excesiva

Para corregir el retraso, aumente la capacidad de la SKU de su entorno. Para más información, consulte [Escalado de su entorno de Time Series Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="cause-b-initial-ingestion-of-historical-data-slows-ingress"></a>Causa B: la ingesta inicial de los datos históricos ralentiza la entrada

Si va a conectar un origen de eventos existente, es probable que su centro de eventos o instancia de IoT Hub ya contengan datos. El entorno comienza a extraer datos del principio del período de retención del mensaje de origen del evento. Este proceso predeterminado no se puede invalidar. Puede exponerse a limitaciones. La limitación puede tardar un rato en alcanzarse ya que ingiere datos históricos.

#### <a name="recommended-resolutions-for-large-initial-ingestion"></a>Resoluciones recomendadas para una ingesta inicial grande

Para corregir el retraso:

1. Aumente la capacidad de la SKU al valor máximo permitido (10 en este caso). Después de aumentar la capacidad, se inicia el proceso de entrada para ponerse al día mucho más rápidamente. El aumento de capacidad se le cobra. Para visualizar lo rápido que se está recuperando, puede ver el gráfico de disponibilidad del [explorador de Time Series Insights](https://insights.timeseries.azure.com).

2. Cuando se actualice el retraso, reduzca la capacidad de la SKU a la velocidad de entrada normal.

## <a name="problem-data-was-showing-previously-but-is-no-longer-showing"></a>Problema: los datos se mostraban anteriormente, pero ya no lo hacen

TSI ya no ingiere datos, pero se siguen transmitiendo eventos a IoT Hub o al centro de eventos.

### <a name="cause-a-your-hub-access-key-was-regenerated-and-your-environment-needs-updating"></a>Causa A: se ha regenerado la clave de acceso del centro y su entorno necesita actualizarse

Este problema se produce cuando la clave proporcionada al crear el origen del evento ya no es válida. Vería la telemetría en el centro, pero ningún mensaje de entrada recibido en Time Series Insights. Si no está seguro de si se ha regenerado la clave, puede buscar "crear o actualizar reglas de autorización de espacio de nombres" o "crear o actualizar recursos de IotHub" para IoT Hub en el registro de actividad de Event Hubs.

Para actualizar el entorno de Time Series Insights con la nueva clave, abra el recurso del centro en Azure Portal y copie la nueva clave. Vaya al recurso de TSI y haga clic en Orígenes de eventos. 

   [![Actualización de la clave.](media/diagnose-and-solve-problems/update-hub-key-step-1.png)](media/diagnose-and-solve-problems/update-hub-key-step-1.png#lightbox)

Seleccione los orígenes de eventos en los que se ha detenido la ingesta, pegue la nueva clave y haga clic en Guardar.

   [![Actualización de la clave.](media/diagnose-and-solve-problems/update-hub-key-step-2.png)](media/diagnose-and-solve-problems/update-hub-key-step-2.png#lightbox)

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Problema: mi valor de nombre de propiedad de marca de tiempo del origen de eventos no funciona

Asegúrese de que el nombre y el valor de la propiedad timestamp se ajustan a las reglas siguientes:

* El nombre de la propiedad timestamp distingue mayúsculas de minúsculas.
* El valor de la propiedad timestamp que procede del origen del evento, como una cadena JSON, debe tener el formato _dd-MM-aaaaTHH:mm:ss.FFFFFFFK_. Un ejemplo es **2008-04-12T12:53Z**.

La manera más fácil de asegurarse de que el nombre de la propiedad timestamp se captura y funciona correctamente consiste en utilizar el explorador de Time Series Insights. En el explorador de Time Series Insights mediante el gráfico, seleccione un período de tiempo después de proporcionar el nombre de la propiedad timestamp. Haga clic con el botón derecho en la selección y, a continuación, seleccione la opción de **exploración de eventos**.

El primer encabezado de columna debe ser el nombre de propiedad timestamp. Junto a la palabra **Timestamp**, se mostrará **($ts)** .

No se mostrarán estos valores:

- *(abc)* : indica que Time Series Insights lee los valores de datos como cadenas.
- *Icono de calendario*: indica que Time Series Insights lee los valores de datos como *datetime*.
- *#* : indica que Time Series Insights lee los valores de datos como un entero.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [mitigación de la latencia en Azure Time Series Insights](time-series-insights-environment-mitigate-latency.md).

- Aprenda a [escalar el entorno de Time Series Insights](time-series-insights-how-to-scale-your-environment.md).