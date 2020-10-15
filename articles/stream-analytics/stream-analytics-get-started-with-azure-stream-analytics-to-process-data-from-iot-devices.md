---
title: Procesamiento de flujos de datos de IoT en tiempo real con Azure Stream Analytics
description: Flujos de datos y SensorTags de IoT con análisis de transmisiones y procesamiento de datos en tiempo real
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 11/26/2019
ms.openlocfilehash: a40f92e88d2d8e5ca253446b9c67ad30df538a5b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86043434"
---
# <a name="process-real-time-iot-data-streams-with-azure-stream-analytics"></a>Procesamiento de flujos de datos de IoT en tiempo real con Azure Stream Analytics

En este artículo, aprenderá a crear una lógica de procesamiento de flujos para recopilar datos desde dispositivos de Internet de las cosas (IoT). Usará un caso de uso real de Internet de las cosas para mostrar cómo puede crear una solución de forma rápida y económica.

## <a name="prerequisites"></a>Prerrequisitos

* Cree una [suscripción de Azure](https://azure.microsoft.com/pricing/free-trial/) gratuita.
* Descargue los archivos de datos y consultas de ejemplo desde [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot).

## <a name="scenario"></a>Escenario

Contoso es una empresa del sector de la automatización industrial que ha automatizado completamente su proceso de fabricación. La maquinaria de esta planta cuenta con sensores capaces de emitir flujos de datos en tiempo real. En este escenario, un administrador del piso de producción desea tener información en tiempo real de los datos provenientes de los sensores para buscar patrones y llevar a cabo las acciones que sean necesarias. Puede usar el lenguaje de consulta de Stream Analytics (SAQL) sobre los datos de los sensores para encontrar patrones interesantes en los flujos de datos entrantes.

En este ejemplo, los datos se generan desde un dispositivo SensorTag de Texas Instruments. La carga de los datos está en formato JSON y tiene un aspecto similar al siguiente:

```json
{
    "time": "2016-01-26T20:47:53.0000000",  
    "dspl": "sensorE",  
    "temp": 123,  
    "hmdt": 34  
}  
```

En un escenario real, podría haber cientos de estos sensores generando eventos en forma de secuencia. Lo ideal sería que hubiera un dispositivo de puerta de enlace que ejecutara código para insertar estos eventos en [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) o en [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/). Un trabajo de Stream Analytics ingeriría estos eventos desde Event Hubs y ejecutar consultas de análisis en tiempo real en las secuencias. Después, los resultados se podrían enviar a una de las [salidas admitidas](stream-analytics-define-outputs.md).

Para facilitar su uso, esta guía de introducción proporciona un archivo con datos de ejemplo que se capturan desde dispositivos de SensorTag reales. Puede ejecutar consultas en los datos de ejemplo y ver los resultados. En tutoriales subsiguientes, aprenderá a conectar el trabajo a las entradas y salidas y a implementarlas en el servicio de Azure.

## <a name="create-a-stream-analytics-job"></a>Creación de un trabajo de Stream Analytics

1. En [Azure Portal](https://portal.azure.com), seleccione **+ Crear un recurso** en el menú de navegación izquierdo. A continuación, seleccione **Trabajo de Stream Analytics** en **Analytics**.
   
    ![Crear un nuevo trabajo de Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

1. Escriba un nombre de trabajo único y compruebe que la suscripción sea la correcta para su trabajo. Cree un nuevo grupo de recursos o seleccione uno existente de la suscripción.

1. Seleccione una ubicación para el trabajo. Use la misma ubicación para el grupo de recursos y para todos los recursos con el fin de aumentar la velocidad de procesamiento y reducir los costos. Una vez realizadas las configuraciones, seleccione **Crear**.
   
    ![Detalles de la creación de un trabajo de Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

## <a name="create-an-azure-stream-analytics-query"></a>Creación de una consulta de Azure Stream Analytics
El siguiente paso después de crear el trabajo es escribir una consulta. Puede probar las consultas con datos de ejemplo sin necesidad de conectar una entrada o salida a su trabajo.

Descargue [HelloWorldASA-InputStream.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json
) desde GitHub. Después, vaya al trabajo de Azure Stream Analytics en Azure Portal.

Seleccione **Consulta** en **Topología de trabajo** en el menú de la izquierda. A continuación, seleccione **Cargar entrada de muestra**. Cargue el archivo `HelloWorldASA-InputStream.json` y seleccione **Aceptar**.

![Icono de consulta en el panel de Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

Observe que se rellena automáticamente una vista previa de los datos en la tabla **Vista previa de entrada**.

![Vista previa de los datos de entrada de muestra](./media/stream-analytics-get-started-with-iot-devices/input-preview.png)

### <a name="query-archive-your-raw-data"></a>Consulta: Archivado de los datos sin procesar

La forma más sencilla de una consulta es una consulta de paso a través que archiva todos los datos de entrada en la salida designada. Esta consulta es la consulta predeterminada que se rellena en un nuevo trabajo de Azure Stream Analytics.

```sql
SELECT 
    *
INTO
    Output
FROM
    InputStream
```

Seleccione **Consulta de prueba** y vea los resultados en la tabla **Resultados de pruebas**.

![Probar los resultados de la consulta de Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Consulta: Filtro de los datos en función de una condición

Intentemos filtrar los resultados según una condición. Nos gustaría mostrar los resultados solo para los eventos que provienen del "SensorA".

```sql
SELECT 
    time,
    dspl AS SensorName,
    temp AS Temperature,
    hmdt AS Humidity
INTO
   Output
FROM
    InputStream
WHERE dspl='sensorA'
```

Pegue la consulta en el editor y seleccione **Consulta de prueba** para revisar los resultados.

![Filtrado de un flujo de datos](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Consulta: Alertas para desencadenar un flujo de trabajo de negocio

Aumentemos el grado de detalle de la consulta. En cada tipo de sensor, deseamos supervisar la temperatura media por ventana de 30 segundos y mostrar los resultados solo si la temperatura media supera los 100 grados.

```sql
SELECT 
    System.Timestamp AS OutputTime,
    dspl AS SensorName,
    Avg(temp) AS AvgTemperature
INTO
   Output
FROM
    InputStream TIMESTAMP BY time
GROUP BY TumblingWindow(second,30),dspl
HAVING Avg(temp)>100
```

![Consulta de filtro de 30 segundos](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Debería ver los resultados que contienen solo las 245 filas y nombres de sensores en los que la temperatura promedio supera los 100 grados. Esta consulta agrupa el flujo de eventos por **dspl**, que es el nombre del sensor y con respecto a una **ventana de saltos de tamaño constante** de 30 segundos. Las consultas temporales deben indicar cómo desea que transcurra el tiempo. Mediante la cláusula **TIMESTAMP BY**, ha especificado la columna **OUTPUTTIME** para asociar los tiempos con todos los cálculos temporales. Para obtener información detallada, lea los artículos sobre [Administración del tiempo](https://docs.microsoft.com/stream-analytics-query/time-management-azure-stream-analytics) y [Funciones de ventana](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics).

### <a name="query-detect-absence-of-events"></a>Consulta: Detección de la ausencia de eventos

¿Cómo podemos escribir una consulta que busque una falta de eventos de entrada? Busquemos la última vez que un sensor envió datos y luego no envió ningún evento en los 5 segundos posteriores.

```sql
SELECT 
    t1.time,
    t1.dspl AS SensorName
INTO
   Output
FROM
    InputStream t1 TIMESTAMP BY time
LEFT OUTER JOIN InputStream t2 TIMESTAMP BY time
ON
    t1.dspl=t2.dspl AND
    DATEDIFF(second,t1,t2) BETWEEN 1 and 5
WHERE t2.dspl IS NULL
```

![Detección de la ausencia de eventos](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Aquí vamos a usar una combinación **LEFT OUTER** en el mismo flujo de datos (autocombinación). Para una combinación **INNER**, solo se devuelve un resultado cuando se encuentra una coincidencia.  En el caso de una combinación **LEFT OUTER**, si un evento procedente del lado izquierdo de la combinación no tiene coincidencia, se devolverá una fila con el valor NULL en todas las columnas de la derecha. Esta técnica resulta muy útil para buscar la ausencia de eventos. Para más información, consulte [JOIN](https://docs.microsoft.com/stream-analytics-query/join-azure-stream-analytics).

## <a name="conclusion"></a>Conclusión

El objetivo de este artículo es mostrar cómo escribir diferentes consultas en el lenguaje de consulta de Stream Analytics y ver los resultados en el explorador. Sin embargo, se trata solo de una introducción. Stream Analytics admite una gran variedad de entradas y salidas, e incluso puede usar las funciones de Azure Machine Learning, lo que hace que sea una herramienta sólida para el análisis de flujos de datos. Para más información acerca de cómo escribir consultas, lea el artículo sobre [patrones comunes de consulta](stream-analytics-stream-analytics-query-patterns.md).

