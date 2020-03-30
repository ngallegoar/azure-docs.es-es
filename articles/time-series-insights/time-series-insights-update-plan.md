---
title: 'Planeamiento de un entorno de versión preliminar: Azure Time Series Insights | Microsoft Docs'
description: Procedimientos recomendados para configurar, administrar, planear e implementar el entorno en versión preliminar de Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: seodec18
ms.openlocfilehash: b16d78c9670d05fcec8126c5544d1dd97f6a03bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045712"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Planeamiento del entorno de versión preliminar de Azure Time Series Insights

En este artículo se describen los procedimientos recomendados para planear y comenzar a usar rápidamente la versión preliminar de Azure Time Series Insights.

> [!NOTE]
> Para conocer los procedimientos recomendados para planear una instancia de Time Series Insights de disponibilidad general, lea [Planeamiento del entorno de Azure Time Series Insights de disponibilidad general](time-series-insights-environment-planning.md).

## <a name="best-practices-for-planning-and-preparation"></a>Procedimientos recomendados para el planeamiento y la preparación

En los artículos siguientes se describen con más detalles los procedimientos recomendados para planear y preparar el entorno:

* Lo que se obtiene cuando se [aprovisiona un entorno de versión preliminar de Time Series Insights](#the-preview-environment).
* Cuáles son las [propiedades IDs y Timestamp de Times Series](#configure-time-series-ids-and-timestamp-properties).
* Cuál es el nuevo [modelo de Times Series](#understand-the-time-series-model) y cómo crear el suyo.
* Cómo [enviar eventos de forma eficaz en JSON](#shape-your-events).
* [Opciones comerciales de recuperación ante desastres](#business-disaster-recovery) de Time Series Insights.

Azure Time Series Insights emplea un modelo comercial de pago por uso. Para más información sobre la capacidad y los cargos, lea [Precios de Azure Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-preview-environment"></a>El entorno de versión preliminar

Al aprovisionar un entorno de versión preliminar de Time Series Insights, creará dos recursos de Azure:

* Un entorno de la versión preliminar de Azure Time Series Insights.
* Una cuenta de uso general V1 de Azure Storage.

Como parte del proceso de aprovisionamiento, especifique si desea habilitar un almacenamiento intermedio. El almacenamiento intermedio proporciona una experiencia de consulta en niveles. Cuando esté habilitado, debe especificar un período de retención entre 7 y 30 días. Las consultas que se ejecutan dentro del período de retención del almacenamiento intermedio generalmente proporcionan tiempos de respuesta más rápidos. Cuando una consulta abarca el período de retención de almacenamiento intermedio, se sirve desde el almacenamiento en reposo.

Las consultas que están en el almacenamiento intermedio son gratuitas, mientras que las del almacenamiento en reposo generan costos. Es importante comprender los modelos de consulta y planear la configuración del almacenamiento intermedio correspondiente. Se recomienda que los análisis interactivos de los datos más recientes residan en el almacenamiento intermedio y el análisis de patrones y las tendencias a largo plazo residan en reposo.

> [!NOTE]
> Para obtener más información sobre cómo consultar los datos semiactivos, lea la [referencia de API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters).

Para empezar, necesita tres elementos adicionales:

* Un [modelo de Time Series](./time-series-insights-update-tsm.md)
* Un [origen de eventos conectado a Time Series Insights](./time-series-insights-how-to-add-an-event-source-iothub.md)
* [Eventos que fluyen hacia el origen del evento](./time-series-insights-send-events.md) que se asignan al modelo y están en formato JSON válido

## <a name="review-preview-limits"></a>Revisión de los límites de la versión preliminar

[!INCLUDE [Review Time Series Insights Preview limits](../../includes/time-series-insights-preview-limits.md)]

## <a name="configure-time-series-ids-and-timestamp-properties"></a>Configurar las propiedades IDs y Timestamp de Time Series

Para crear un entorno de Time Series Insights, seleccione un identificador de Time Series. Al hacer esto se crea una partición lógica para los datos. Como se indicó, asegúrese de tener listos los identificadores de serie temporal.

> [!IMPORTANT]
> Los identificadores de serie temporal *no se pueden cambiar más adelante*. Compruebe cada uno de ellos antes de pasar a la selección final y el primer uso.

Puede seleccionar hasta tres claves para diferenciar los recursos de forma única. Para más información, lea [Best practices for choosing a Time Series ID](./time-series-insights-update-how-to-id.md) (Procedimientos recomendados para elegir un identificador de Time Series) y [Storage and ingress](./time-series-insights-update-storage-ingress.md) (Almacenamiento y entrada).

La propiedad **Timestamp** también es importante. Esta propiedad se puede designar al agregar orígenes de eventos. Cada origen de eventos tiene una propiedad Timestamp opcional que se usa para realizar el seguimiento de los orígenes de eventos con el tiempo. Los valores de Timestamp distinguen mayúsculas de minúsculas y su formato debe seguir la especificación de cada origen de evento.

> [!TIP]
> Compruebe los requisitos de formato y análisis de los orígenes de eventos.

Si se deja en blanco, la hora de puesta en cola del evento se usa como marca de tiempo del evento. Si envía datos históricos o eventos por lotes, personalizar la propiedad Timestamp es más útil que la hora predeterminada de puesta en cola del evento. Para más información, lea acerca de cómo [agregar orígenes de eventos en Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="understand-the-time-series-model"></a>El modelo de serie temporal

Ahora puede configurar el modelo de serie temporal del entorno de Time Series Insights. El nuevo modelo facilita la búsqueda y el análisis de los datos de IoT. Además, permite la protección, el mantenimiento y el enriquecimiento de datos de serie temporal y ayuda a preparar los conjuntos de datos listos para el consumidor. El modelo usa identificadores de Time Series, que se asignan a una instancia que asocia el recurso único con variables, conocidas como tipos, y jerarquías. Lea sobre el nuevo [modelo de serie temporal](./time-series-insights-update-tsm.md).

El modelo es dinámico, por lo que se puede compilar en cualquier momento. Para empezar a trabajar rápidamente, compílelo y cárguelo antes de insertar datos en Time Series Insights. Para compilar el modelo, lea [Modelado de datos en la versión preliminar de Azure Time Series Insights](./time-series-insights-update-how-to-tsm.md).

Para muchos clientes, el modelo de serie temporal se asigna a un modelo de recursos existente o a un sistema ERP ya implantado. Si no tiene un modelo existente, se [proporciona](https://github.com/Microsoft/tsiclient) una experiencia de usuario ya creada para ponerse en marcha rápidamente. Para saber cómo puede ayudarle un modelo, vea el [entorno de demostración de ejemplo](https://insights.timeseries.azure.com/preview/demo).

## <a name="shape-your-events"></a>Moldear los eventos

Puede comprobar el modo en que los eventos se envían a Time Series Insights. Lo ideal es que los eventos se desnormalicen bien y de manera eficaz.

Una buena regla general es la siguiente:

* Almacenar los metadatos en el modelo de Time Series.
* Asegúrese de que el modo de serie temporal, los campos de instancia y los eventos incluyen únicamente la información necesaria, como los identificadores de serie temporal o la propiedad Timestamp.

Para más información, lea [Moldeo de eventos](./time-series-insights-send-events.md#supported-json-shapes).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Pasos siguientes

- Revise [Azure Advisor](../advisor/advisor-overview.md) para planear las opciones de configuración de recuperación de negocio.
- Lea más sobre [el almacenamiento y la entrada](./time-series-insights-update-storage-ingress.md) en la versión preliminar de Time Series Insights.
- Lea más sobre [el modelo de datos](./time-series-insights-update-tsm.md) en la versión preliminar de Time Series Insights.
