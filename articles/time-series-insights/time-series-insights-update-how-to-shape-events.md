---
title: 'Dar forma a eventos: Azure Time Series Insights | Microsoft Docs'
description: Obtenga información sobre los procedimientos recomendados y cómo dar forma a los eventos para realizar consultas en la versión preliminar de Azure Time Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/24/2020
ms.custom: seodec18
ms.openlocfilehash: 99a2f32c3f76d7fec475c9b299f7208b4db29cfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650930"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Dar forma a los eventos con la versión preliminar de Azure Time Series Insights

En este artículo se definen los procedimientos recomendados para dar forma a las cargas JSON para la ingesta en Azure Time Series Insights y para maximizar la eficacia de las consultas de la versión preliminar.

## <a name="best-practices"></a>Procedimientos recomendados

Es mejor considerar detenidamente cómo envía los eventos al entorno de versión preliminar de Time Series Insights. 

Entre los procedimientos recomendados generales se incluyen:

* Enviar los datos a través de la red de la forma más eficaz posible.
* Almacenar los datos de una manera que le ayude a agregarlos de manera más adecuada para su escenario.

Para obtener el mejor rendimiento de las consultas, cumpla las siguientes reglas de control:

* No envíe propiedades innecesarias. La versión preliminar de Time Series Insights factura en función del uso. Es mejor almacenar y procesar solo los datos que vaya a consultar.
* Use campos de instancia para los datos estáticos. Este procedimiento ayuda a evitar el envío de datos estáticos a través de la red. Los campos de instancia, un componente del modelo de Time Series Insights, funcionan como datos de referencia en el servicio Time Series Insights, que tiene disponibilidad general. Para más información sobre los campos de instancia, lea [Modelo de Time Series](./time-series-insights-update-tsm.md).
* Comparta propiedades de dimensión entre dos o más eventos. Este procedimiento le ayudará a enviar datos a través de la red de manera más eficaz.
* No utilice el anidamiento profundo de matriz. La versión preliminar de Time Series Insights admite hasta dos niveles de matrices anidadas que contienen objetos. Igualmente, la versión preliminar de Time Series Insights acopla las matrices en mensajes de varios eventos con pares de valores de propiedad.
* Si solo existen algunas de las medidas para todos o la mayoría de los eventos, es mejor que envíe estas medidas como propiedades independientes dentro del mismo objeto. Al enviarlas por separado se reduce el número de eventos y podría mejorar el rendimiento de consultas, ya que se deben procesar menos eventos.

## <a name="column-flattening"></a>Aplanamiento de columnas

Durante la ingesta, las cargas útiles que contienen objetos anidados se aplanarán, de modo que el nombre de la columna sea un valor único con un delineador.

* Por ejemplo, el siguiente JSON anidado:

   ```JSON
   "data": {
        "flow": 1.0172575712203979,
   },
   ```

   Se convierte en `data_flow` cuando se aplana.

> [!IMPORTANT]
> * Time Series Insights (versión preliminar) usa caracteres de subrayado (`_`) para la delineación de columnas.
> * Tenga en cuenta la diferencia con respecto a la disponibilidad general, que usa puntos (`.`).

A continuación, se muestran escenarios más complejos.

#### <a name="example-1"></a>Ejemplo 1:

En el siguiente escenario hay dos (o más) dispositivos que envían las medidas (señales): *Caudal*, *Presión de aceite del motor*, *Temperatura* y *Humedad*.

Hay un único mensaje de Azure IoT Hub enviado donde la matriz externa contiene una sección compartida de valores de dimensión comunes (tenga en cuenta las dos entradas de dispositivo en el mensaje).

```JSON
[
  {
    "deviceId":"FXXX",
    "timestamp":"2018-01-17T01:17:00Z",
    "series":[
      {
        "Flow Rate ft3/s":1.0172575712203979,
        "Engine Oil Pressure psi ":34.7
      },
      {
        "Flow Rate ft3/s":2.445906400680542,
        "Engine Oil Pressure psi ":49.2
      }
    ]
  },
  {
    "deviceId":"FYYY",
    "timestamp":"2018-01-17T01:18:00Z",
    "series":[
      {
        "Flow Rate ft3/s":0.58015072345733643,
        "Engine Oil Pressure psi ":22.2
      }
    ]
  }
]
```

**Puntos clave:**

* El JSON de ejemplo tiene una matriz externa que usa datos de [instancia de Time Series](./time-series-insights-update-tsm.md#time-series-model-instances) para aumentar la eficacia del mensaje. Aunque no es probable que cambien los metadatos de dispositivos de instancias de Time Series, suele proporcionar propiedades útiles para el análisis de datos.

* El archivo JSON combina dos o más mensajes (uno de cada dispositivo) en una sola carga, lo que permite ahorrar ancho de banda a lo largo del tiempo.

* Los puntos de datos de series individuales de cada dispositivo se combinan en un solo atributo de **serie**, lo que reduce la necesidad de transmitir continuamente las actualizaciones para cada dispositivo.

> [!TIP]
> Para reducir el número de mensajes necesarios para enviar datos y aumentar la eficacia de la telemetría, considere la posibilidad de procesar por lotes valores de dimensiones comunes y metadatos de instancias de Time Series en una sola carga JSON.

#### <a name="time-series-instance"></a>Instancia de serie temporal 

Echemos un vistazo más detallado sobre cómo usar la [instancia de Time Series](./time-series-insights-update-tsm.md#time-series-model-instances) para dar forma al JSON de manera óptima. 

> [!NOTE]
> Los siguientes [identificadores de Time Series](./time-series-insights-update-how-to-id.md) son *deviceIds*.

```JSON
[
  {
    "timeSeriesId":[
      "FXXX"
    ],
    "typeId":"17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds":[
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description":null,
    "instanceFields":{
      "L1":"REVOLT SIMULATOR",
      "L2":"Battery System"
    }
  },
  {
    "timeSeriesId":[
      "FYYY"
    ],
    "typeId":"17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds":[
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description":null,
    "instanceFields":{
      "L1":"COMMON SIMULATOR",
      "L2":"Battery System"
    }
  }
]
```

La versión preliminar de Time Series Insights une una tabla (después del acoplamiento) durante el tiempo de consulta. La tabla incluye columnas adicionales, como **Type**.

| deviceId  | Tipo | L1 | L2 | timestamp | series_Flow Rate ft3/s | series_Engine Oil Pressure psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMULADOR | Sistema de batería | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34.7 |
| `FXXX` | Default_Type | SIMULADOR |   Sistema de batería |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | LINE_DATA    COMMON | SIMULADOR |    Sistema de batería |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

> [!NOTE]
>  En la tabla anterior se representa la vista de consulta en el [explorador de versión preliminar de Time Series](./time-series-insights-update-explorer.md).

**Puntos clave:**

* En el ejemplo anterior, las propiedades estáticas se almacenan en la versión preliminar de Time Series Insights para optimizar los datos enviados a través de la red.
* Los datos de la versión preliminar de Time Series Insights se combinan cuando se realiza una consulta a través del identificador de Time Series definido en la instancia.
* Se utilizan dos niveles de anidamiento. Este número es el más compatible con la versión preliminar de Time Series Insights. Es fundamental para evitar matrices profundamente anidadas.
* Como hay pocas medidas, estas se envían como propiedades independientes en el mismo objeto. En este ejemplo, **series_Flow Rate psi**, **series_Engine Oil Pressure psi** y **series_Flow Rate ft3/s** son columnas únicas.

>[!IMPORTANT]
> Los campos de instancia no se almacenan con telemetría. Se almacenan con metadatos en el modelo de Time Series.

#### <a name="example-2"></a>Ejemplo 2:

Considere el siguiente JSON:

```JSON
{
  "deviceId": "FXXX",
  "timestamp": "2019-01-18T01:17:00Z",
  "data": {
        "flow": 1.0172575712203979,
    },
  "data_flow" : 1.76435072345733643
}
```

En el ejemplo anterior, la propiedad de `data["flow"]` aplanada presentaría una colisión de nombres con la propiedad `data_flow`.

En este caso, el valor de la propiedad *latest* sobrescribiría a la anterior. 

> [!TIP]
> Póngase en contacto con el equipo de Time Series Insights para obtener más ayuda.

> [!WARNING] 
> * En los casos en los que las propiedades duplicadas están presentes en la misma carga del evento (singular) debido al acoplamiento o a otro mecanismo, se almacena el valor de propiedad más reciente, sobrescribiendo todos los valores anteriores.
> * La serie de eventos combinados no se invalidará entre sí.

## <a name="next-steps"></a>Pasos siguientes

* Para poner en práctica estas pautas, consulte [Sintaxis de consultas de la versión preliminar de Azure Time Series Insights](./time-series-insights-query-data-csharp.md). Obtendrá más información sobre la sintaxis de consultas para la [API REST de la versión preliminar](https://docs.microsoft.com/rest/api/time-series-insights/preview) de acceso a datos de Time Series Insights.

* Combine los procedimientos recomendados de JSON con el [Modelado de datos de Time Series](./time-series-insights-update-how-to-tsm.md).
