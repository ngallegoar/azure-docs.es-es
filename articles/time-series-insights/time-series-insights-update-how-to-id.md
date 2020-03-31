---
title: 'Procedimientos recomendados para elegir un identificador de serie temporal: Azure Time Series Insights | Microsoft Docs'
description: Conozca los procedimientos recomendados para elegir un identificador de serie temporal en la versión preliminar de Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: a62c2460698408f6a2bfa51c6638bdeaf88bb31f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083529"
---
# <a name="best-practices-for-choosing-a-time-series-id"></a>Procedimientos recomendados al elegir un id. de serie temporal

En este artículo se resume la importancia del identificador de serie temporal en el entorno de la versión preliminar de Azure Time Series Insights y los procedimiento recomendados para elegir uno.

## <a name="choose-a-time-series-id"></a>Elección de un identificador de Time Series

Es fundamental seleccionar un identificador de serie temporal adecuado. Elegir un id. de serie temporal es como elegir una clave de partición para una base de datos. Se requiere al crear un entorno de versión preliminar de Time Series Insights. 

> [!IMPORTANT]
> Los identificadores de serie temporal son los siguientes:
> * Propiedad *case-sensitive*: se hace distinción de mayúsculas y minúsculas en búsquedas, comparaciones, y actualizaciones, así como al crear particiones.
> * Propiedad *immutable*: una vez creada, no se puede cambiar.

> [!TIP]
> Si el origen del evento es un centro de IoT, es probable que el identificador de serie temporal sea ***iothub-connection-device-id***.

Los principales procedimientos recomendados que han de seguirse incluyen lo siguiente:

* Elija una clave de partición con muchos valores distintos (por ejemplo, centenares o miles). En muchos casos, puede ser el identificador de dispositivo, identificador de sensor o identificador de etiqueta de JSON.
* El id. de serie temporal debe ser único en el nivel de nodo hoja de su [Modelo de serie temporal](./time-series-insights-update-tsm.md).
* El límite de caracteres de la cadena de nombre de la propiedad del identificador de serie temporal es 128. En el caso del valor de la propiedad del identificador de serie temporal, el límite de caracteres es 1.024.
* Si falta un valor de propiedad único para el identificador de serie temporal, se trata como un valor NULL y sigue la misma regla de la restricción de unicidad.
* Puede seleccionar hasta *tres* propiedades clave como identificador de serie temporal. Su combinación será una clave compuesta que representa el identificador de serie temporal.  
  > [!NOTE]
  > Las tres propiedades clave deben ser cadenas.
  > Tendría que realizar consultas en esta clave compuesta, en lugar de hacerlo propiedad a propiedad.

## <a name="select-more-than-one-key-property"></a>Selección de más de una propiedad de clave

En los siguientes escenarios se describe cómo seleccionar más de una propiedad clave como identificador de serie temporal.  

### <a name="example-1-time-series-id-with-a-unique-key"></a>Ejemplo 1: identificador de serie temporal con una clave única

* Tiene flotas de recursos heredadas. Cada una tiene una clave única.
* Cada flota se identifica de forma única mediante la propiedad **deviceId**. En el caso de otra flota, la propiedad única es **objectId**. Ninguna de las flotas contiene la propiedad única de la otra. En este ejemplo, debe seleccionar dos claves, **deviceId** y **objectId**, como claves únicas.
* Aceptamos valores NULL y la falta de la presencia de una propiedad en la carga del evento cuenta como un valor NULL. Esta es también la forma adecuada de controlar el envío de datos a dos orígenes de eventos diferentes, donde los datos de cada origen de eventos tienen un identificador de serie temporal único.

### <a name="example-2-time-series-id-with-a-composite-key"></a>Ejemplo 2: identificador de serie temporal con una clave compuesta

* Necesita que varias propiedades sean únicas dentro del mismo tipo de activos. 
* Es fabricante de edificios inteligentes e implementa sensores en todas las habitaciones. En cada una de ellas, lo habitual es que tenga los mismos valores para **sensorId**. Algunos ejemplos son **sensor1**, **sensor2**y **sensor3**.
* Los números de las plantas y de las estancias del edificio se solapan en la propiedad **flrRm**. Estos números tienen valores como **1a**, **2b** y **3a**.
* Tiene una propiedad, **location**, que contiene valores como **Redmond**, **Barcelona** y **Tokio**. Para que los valores sean únicos, designe las tres propiedades siguientes como sus claves del identificador de serie temporal: **sensorId**, **flrRm** y **location**.

Ejemplo de evento sin procesar:

```JSON
{
  "sensorId": "sensor1",
  "flrRm": "1a",
  "location": "Redmond",
  "temperature": 78
}
```

En Azure Portal, puede especificar después la clave compuesta de la manera siguiente: 

```JSON
[{"name":"sensorId","type":"String"},{"name":"flrRm","type":"String"},{"name":"location","type":"string"}]
```

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre el [modelado de datos](./time-series-insights-update-tsm.md).

* Planee el [entorno de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-plan.md).