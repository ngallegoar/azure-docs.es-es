---
title: Depuración de las consultas de Azure Stream Analytics localmente mediante un diagrama de trabajos de Visual Studio
description: En este artículo se describe cómo depurar las consultas localmente mediante un diagrama de trabajos en las herramientas de Azure Stream Analytics para Visual Studio.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 01/23/2020
ms.openlocfilehash: e9d6bbdf325c0da96b81c1f2146da284ea744def
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92218262"
---
# <a name="debug-azure-stream-analytics-queries-locally-using-job-diagram-in-visual-studio"></a>Depuración de las consultas de Azure Stream Analytics localmente mediante un diagrama de trabajos de Visual Studio

Los trabajos que no generan resultados o que generan resultados inesperados son escenarios comunes de solución de problemas para la realización de consultas. Puede usar el diagrama de trabajos mientras prueba la consulta localmente en Visual Studio para examinar el conjunto de resultados intermedio y las métricas de cada paso. Los diagramas de trabajos pueden ayudarle a aislar rápidamente el origen de un problema al solucionar problemas.

## <a name="debug-a-query-using-job-diagram"></a>Depuración de una consulta mediante un diagrama de trabajos

Se utiliza un script de Azure Stream Analytics para transformar los datos de entrada en datos de salida. El diagrama de trabajos muestra cómo fluyen los datos de los orígenes de entrada (centro de eventos, IoT Hub, etc.) por varios pasos de consulta y, por último, a los receptores de salida. Cada paso de la consulta se asigna a un conjunto de resultados temporal definido en el script mediante una instrucción `WITH`. Puede ver los datos, así como las métricas de cada paso de consulta, en cada uno de los conjuntos de resultados intermedios para encontrar el origen de un problema.

> [!NOTE]
> En este diagrama de trabajos solo se muestran los datos y las métricas para las pruebas locales en un único nodo. No debe usarse para el ajuste del rendimiento y la solución de problemas.

### <a name="start-local-testing"></a>Inicio de las pruebas locales

Use este [inicio rápido](stream-analytics-quick-create-vs.md) para aprender a crear un trabajo de Stream Analytics mediante Visual Studio o [exportar un trabajo existente a un proyecto local](stream-analytics-vs-tools.md#export-jobs-to-a-project). Si desea probar la consulta con datos de entrada locales, siga estas [instrucciones](stream-analytics-live-data-local-testing.md). Si desea probar con una entrada activa, vaya al paso siguiente.

> [!NOTE]
> Si exporta un trabajo al proyecto local y desea realizar una prueba en un flujo de entrada activo, debe especificar de nuevo las credenciales para todas las entradas.  

Elija el origen de entrada y salida en el editor de scripts y seleccione **Ejecutar de forma local** . El diagrama de trabajos aparece a la derecha.

### <a name="view-the-intermediate-result-set"></a>Visualización del conjunto de resultados intermedio  

1. Seleccione el paso de consulta para desplazarse al script. Se le dirigirá automáticamente al script correspondiente en el editor situado a la izquierda.

   ![Script de navegación del diagrama de trabajos](./media/debug-locally-using-job-diagram/navigate-script.png)

2. Seleccione el paso de consulta y, después, **Vista previa** en el cuadro de diálogo emergente. El conjunto de resultados se muestra en una pestaña en la ventana de resultados inferior.

   ![Resultado de la vista previa del diagrama de trabajos](./media/debug-locally-using-job-diagram/preview-result.png)

### <a name="view-step-metrics"></a>Visualización de las métricas de los pasos

En esta sección, explorará las métricas disponibles para cada parte del diagrama.

#### <a name="input-sources-live-stream"></a>Orígenes de entrada (flujo activo)

![Orígenes de entrada activos del diagrama de trabajos](./media/debug-locally-using-job-diagram/live-input.png)

|Métrica|Descripción|
|-|-|
|**TaxiRide**| Nombre de la entrada.|
|**Event Hubs** | Tipo de origen de entrada.|
|**Eventos**|El número de eventos leídos.|
|**Orígenes de eventos pendientes**|Cuántos mensajes más deben leerse para las entradas de Event Hubs y IoT Hub.|
|**Eventos en bytes**|El número de bytes leídos.|
| **Eventos degradados**|El número de eventos que tuvieron un problema distinto al de la deserialización.|
|**Eventos tempranos**| El número de eventos con una marca de tiempo de la aplicación antes del límite máximo.|
|**Eventos retrasados**| El número de eventos con una marca de tiempo de la aplicación después del límite máximo.|
|**Orígenes de eventos**| El número de unidades de datos leídas. Por ejemplo, el número de blobs.|

#### <a name="input-sources-local-input"></a>Orígenes de entrada (entrada local)

![Orígenes de entrada locales del diagrama de trabajos](./media/debug-locally-using-job-diagram/local-input.png)

|Métrica|Descripción|
|-|-|
|**TaxiRide**| Nombre de la entrada.|
|**Recuento de filas**| El número de filas generadas a partir del paso.|
|**Tamaño de los datos**| El tamaño de los datos generados en este paso.|
|**Entrada local**| Use datos locales como entrada.|

#### <a name="query-steps"></a>Pasos de la consulta

![Paso de consulta del diagrama de trabajos](./media/debug-locally-using-job-diagram/query-step.png)

|Métrica|Descripción|
|-|-|
|**TripData**|El nombre del conjunto de resultados temporal.|
|**Recuento de filas**| El número de filas generadas a partir del paso.|
|**Tamaño de los datos**| El tamaño de los datos generados en este paso.|
  
#### <a name="output-sinks-live-output"></a>Receptores de salida (salida activa)

![Diagrama de trabajos que muestra los receptores locales de la salida.](./media/debug-locally-using-job-diagram/live-output.png)

|Métrica|Descripción|
|-|-|
|**regionaggEH**|Nombre de la salida.|
|**Eventos**|El número de eventos que se envían a los receptores.|

#### <a name="output-sinks-local-output"></a>Receptores de salida (salida local)

![Receptores de salida locales del diagrama de trabajos](./media/debug-locally-using-job-diagram/local-output.png)

|Métrica|Descripción|
|-|-|
|**regionaggEH**|Nombre de la salida.|
|**Resultado local**| Salida del resultado a un archivo local.|
|**Recuento de filas**| El número de filas que se envían al archivo local.|
|**Tamaño de los datos**| El tamaño de los datos que se envían al archivo local.|

### <a name="close-job-diagram"></a>Cierre del diagrama de trabajos

Si ya no necesita el diagrama de trabajos, seleccione **Cerrar** en la esquina superior derecha. Después de cerrar la ventana del diagrama, debe volver a iniciar las pruebas locales para verla.

### <a name="view-job-level-metrics-and-stop-running"></a>Visualización de métricas de nivel de trabajo y detención de la ejecución

Otras métricas de nivel de trabajo aparecen en la consola emergente. Presione **Ctrl + C** en la consola de si quiere detener el trabajo.

![Trabajo de detención del diagrama de trabajos](./media/debug-locally-using-job-diagram/stop-job.png)

## <a name="limitations"></a>Limitaciones

* No se admiten los receptores de salida de Power BI y Azure Data Lake Storage Gen1 debido a las limitaciones del modelo de autenticación.

* Solo las opciones de entrada de nube tienen compatibilidad con [directivas de tiempo](stream-analytics-out-of-order-and-late-events.md), lo que no ocurre con las opciones de entrada locales.

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: creación de un trabajo de Stream Analytics con Visual Studio](stream-analytics-quick-create-vs.md)
* [Uso de Visual Studio para ver trabajos de Azure Stream Analytics](stream-analytics-vs-tools.md)
* [Prueba local de datos activos mediante herramientas de Azure Stream Analytics para Visual Studio (versión preliminar)](stream-analytics-live-data-local-testing.md)
