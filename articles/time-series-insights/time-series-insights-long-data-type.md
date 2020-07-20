---
title: Adición de compatibilidad para el tipo de datos Long | Microsoft Docs
description: Adición de compatibilidad para el tipo de datos Long
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/16/2020
ms.custom: dpalled
ms.openlocfilehash: a0b53749a32e79d690cf4412fdac82b18dfe2f2e
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86183637"
---
# <a name="adding-support-for-long-data-type"></a>Adición de compatibilidad para el tipo de datos Long

Estos cambios se aplicarán solo a entornos de versión preliminar (pago por uso). Si tiene un entorno de TSI de SKU estándar (S), puede omitir estos cambios.

Estamos realizando cambios en la forma de almacenar e indexar los datos numéricos en Azure Time Series Insights versión preliminar que podrían afectar al usuario. Si se ve afectado por cualquiera de los casos siguientes, realice los cambios necesarios lo más pronto posible. Los datos empezarán a indexarse con los tipos Long y Double entre el 29 de junio y el 30 de junio de 2020, en función de su región. Si tiene alguna pregunta o duda sobre este cambio, envíe una incidencia de soporte técnico a través de Azure Portal y mencione esta publicación.

Este cambio afecta su entorno en los siguientes casos:

1. Si actualmente usa variables del modelo de Time Series y envía solo tipos de datos enteros en los datos de telemetría.
1. Si actualmente usa variables del modelo de Time Series y envía tipos de datos enteros y no enteros en los datos de telemetría.
1. Si usa variables de categorías para asignar valores enteros a las categorías.
1. Si usa el SDK de JavaScript para crear una aplicación de front-end personalizado.
1. Si está cerca de llegar al límite de 1000 nombres de propiedades en el almacenamiento intermedio (WS) y envía datos enteros y no enteros, el recuento de propiedades se puede ver como una métrica en [Azure Portal](https://portal.azure.com/).

Si alguno de los casos anteriores se aplica a su entorno, deberá realizar cambios en el modelo para dar adaptarse a este cambio. Actualice la expresión de Time Series en la definición de la variable tanto en el explorador de TSI como en cualquier cliente personalizado mediante nuestras API con los cambios recomendados. Consulte a continuación para más información.

En función de las restricciones y la solución de IoT, es posible que no pueda ver los datos que se envían a su entorno de pago por uso de TSI. Si no está seguro de si los datos son solo enteros, o enteros y no enteros, tiene algunas opciones. Puede esperar a que se publique la característica y, a continuación, explorar los eventos sin procesar en la interfaz de usuario del explorador para comprender qué propiedades se guardaron en dos columnas independientes. Puede realizar los siguientes cambios de forma preventiva para todas las etiquetas numéricas, o puede enrutar temporalmente un subconjunto de eventos al almacenamiento para explorar y comprender mejor el esquema. Para almacenar los eventos, active la opción de [captura de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) para Event Hubs o [enrútelos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c#azure-storage) de la instancia de IoT Hub a Azure Blob Storage. Los datos también se pueden observar a través del [explorador del centro de eventos](https://marketplace.visualstudio.com/items?itemName=Summer.azure-event-hub-explorer) o mediante el [host del procesador de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send#receive-events). Si usa IoT Hub, consulte la documentación [de este vínculo](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin) sobre cómo obtener acceso al punto de conexión integrado.

Tenga en cuenta que si se ve afectados por estos cambios y no puede hacerlo en las fechas anteriores, podría producirse una interrupción en la que las variables de la serie temporal afectadas a las que se tiene acceso a través de las API de consulta o el explorador de Time Series Insights devolverán *null* (es decir, no se muestra ningún dato en el explorador).

## <a name="recommended-changes"></a>Cambios recomendados

Caso 1 y 2: **Uso de variables del modelo de Time Series y envío de tipos de datos enteros o tanto de enteros como no enteros en los datos de telemetría.**

Si actualmente está enviando datos de telemetría de tipo entero, los datos se dividirán en dos columnas: "propertyValue_double" y "propertyValue_long".

Los datos enteros se escribirán en la columna "propertyValue_long" cuando los cambios se apliquen y los datos numéricos que se ingirieron previamente (y en el futuro) en "propertyValue_double" no se copiarán.

Si quiere consultar los datos de estas dos columnas para la propiedad "propertyValue", tendrá que usar la función escalar *coalesce()* en TSX. La función acepta argumentos del mismo DataType y devuelve el primer valor distinto de NULL en la lista de argumentos (obtenga más información sobre el uso [en este vínculo](https://docs.microsoft.com/rest/api/time-series-insights/preview#other-functions)).

### <a name="variable-definition-in-time-series-explorer---numeric"></a>Definición de variables en el explorador de Time Series: numéricas

*Definición de variable anterior:*

[![Definición de variable anterior](media/time-series-insights-long-data-type/var-def-previous.png)](media/time-series-insights-long-data-type/var-def-previous.png#lightbox)

*Nueva definición de variable:*

[![Nueva definición de variable](media/time-series-insights-long-data-type/var-def.png)](media/time-series-insights-long-data-type/var-def.png#lightbox)

También puede usar *"coalesce($event.propertyValue.Double, toDouble($event.propertyValue.Long))"* como [expresión de Time Series](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) personalizada.

### <a name="inline-variable-definition-using-time-series-query-apis---numeric"></a>Definición de variables insertadas mediante la API de consulta de Time Series: numéricas

*Definición de variable anterior:*

```tsx
"PropertyValueVariable": {

    "kind": "numeric",

    "value": {

        "tsx": "$event.propertyValue.Double"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
```

*Nueva definición de variable:*

```tsx
"PropertyValueVariable ": {

    "kind": "numeric",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, toLong($event.propertyValue.Double))"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
```

También puede usar *"coalesce($event.propertyValue.Double, toDouble($event.propertyValue.Long))"* como [expresión de Time Series](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) personalizada.

> [!NOTE]
> Se recomienda actualizar estas variables en todos los lugares en los que se puedan usar (modelo de Time Series, consultas guardadas, consultas de conector de Power BI).

Caso 3: **Uso de variables de categorías para asignar valores enteros a las categorías**

Si actualmente usa variables de categorías que asignan valores enteros a las categorías, probablemente está usando la función toLong para convertir los datos de tipol Double al tipo Long. Al igual que en los casos anteriores, deberá fusionar las columnas con DataType Double y Long.

### <a name="variable-definition-in-time-series-explorer---categorical"></a>Definición de variables en el explorador de Time Series: categoría

*Definición de variable anterior:*

[![Definición de variable anterior](media/time-series-insights-long-data-type/var-def-cat-previous.png)](media/time-series-insights-long-data-type/var-def-cat-previous.png#lightbox)

*Nueva definición de variable:*

[![Nueva definición de variable](media/time-series-insights-long-data-type/var-def-cat.png)](media/time-series-insights-long-data-type/var-def-cat.png#lightbox)

También puede usar *"coalesce($event.propertyValue.Double, toDouble($event.propertyValue.Long))"* como [expresión de Time Series](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) personalizada.

Las variables de categoría aún requieren que el valor sea de tipo entero. El valor DataType de todos los argumentos en la función coalesce() debe ser de tipo Long en la [expresión de Time Series](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) personalizada.

### <a name="inline-variable-definition-using-time-series-query-apis---categorical"></a>Definición de variables insertadas mediante la API de consulta de Time Series: categorías

*Definición de variable anterior:*

```tsx
"PropertyValueVariable_Long": {

    "kind": "categorical",

    "value": {

        "tsx": "tolong($event.propertyValue.Double)"

    },

    "categories": [

    {
        "label": "Good",

        "values": [0, 1, 2 ]

    },

    {

        "label": "Bad",

        "values": [ 3, 4 ]

    } ],

    "defaultCategory": {

        "label": "Unknown"

    }

}
```

*Nueva definición de variable:*

```tsx
"PropertyValueVariable_Long": {

    "kind": "categorical",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, tolong($event.propertyValue.Double))"

    },

    "categories": [

    {
        "label": "Good",

        "values": [0, 1, 2 ]

    },

    {

        "label": "Bad",

        "values": [ 3, 4 ]

    } ],

    "defaultCategory": {

        "label": "Unknown"

    }

}
```

Las variables de categoría aún requieren que el valor sea de tipo entero. El valor DataType de todos los argumentos en la función coalesce() debe ser de tipo Long en la [expresión de Time Series](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) personalizada.

> [!NOTE]
> Se recomienda actualizar estas variables en todos los lugares en los que se puedan usar (modelo de Time Series, consultas guardadas, consultas de conector de Power BI).

Caso 4: **Uso del SDK de JavaScript para crear una aplicación de front-end personalizado**

Si se ve afectado por los casos 1 a 3 anteriores y crea aplicaciones personalizadas, debe actualizar las consultas para usar la función *coalesce()* , como se muestra en los ejemplos anteriores.

Caso 5: **Cercanía al límite de 1000 propiedades en el almacenamiento intermedio**

Si es un usuario del almacenamiento intermedio, tiene un gran número de propiedades y cree que este cambio hará que su entorno supere el límite de 1000 nombres de propiedad del almacenamiento intermedio, envíe una incidencia de soporte técnico a través de Azure Portal y mencione esta publicación.

## <a name="next-steps"></a>Pasos siguientes

* Consulte [este](concepts-supported-data-types.md) artículo para ver la lista completa de tipos de datos admitidos.
