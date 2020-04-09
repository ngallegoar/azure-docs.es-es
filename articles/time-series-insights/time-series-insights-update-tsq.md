---
title: Consulta de datos de la versión preliminar de Azure Time Series Insights | Microsoft Docs
description: Información general sobre los conceptos de consulta de datos y la API REST de HTTP en Azure Time Series Insights Preview.
author: shreyasharmamsft
ms.author: shresha
manager: dpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 03/25/2020
ms.custom: seodec18
ms.openlocfilehash: 23094ec71dac5780def10e16b90de0b818ef3c68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284898"
---
# <a name="data-querying-in-azure-time-series-insights-preview"></a>Consulta de datos de la versión preliminar de Azure Time Series Insights

Azure Time Series Insights permite consultar datos en los eventos y los metadatos almacenados en el entorno a través de las API de superficie públicas. Estas API también se usan en el [explorador de Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer).

Existen tres categorías principales de API en Time Series Insights:

* **API de entorno**: estas API permiten consultas en el propio entorno de Time Series Insights. Se pueden usar para recopilar una lista de los entornos a los que el autor de la llamada tiene acceso y los metadatos del entorno.
* **API de consulta de modelo de serie temporal (TSM-Q)** : permite crear, leer, actualizar y eliminar (CRUD) operaciones en metadatos almacenados en el modelo de serie temporal del entorno. Se pueden usar para acceder a las instancias, los tipos y las jerarquías, así como editarlos.
* **API de consulta de serie temporal (TSQ)** : permite la recuperación de datos de telemetría o eventos a medida que se registran desde el proveedor de origen, así como realizar cálculos y agregaciones de rendimiento de los datos usando funciones escalares y de agregado avanzadas.

Time Series Insights usa un lenguaje de expresiones enriquecido basado en cadenas, [Expresión de serie temporal (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax), para expresar los cálculos.

## <a name="azure-time-series-insights-core-apis"></a>API principales de Azure Time Series Insights

Se admiten los siguientes tipos de API principal.

[![Información general sobre la consulta de serie temporal](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>API de entorno

* [Get Environments API](https://docs.microsoft.com/rest/api/time-series-insights/management/environments/get): Devuelve la lista de entornos a los que el autor de la llamada puede obtener acceso.
* [Get Environments Availability API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability): Devuelve la distribución del recuento de eventos a través de la marca de tiempo `$ts` del evento. Esta API ayuda a determinar si hay eventos en el entorno, devolviendo para ello el recuento de eventos (si los hay) desglosado en intervalos.
* [Get Event Schema API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema): Devuelve los metadatos de esquema de eventos para un intervalo de búsqueda determinado. Esta API le ayuda a recuperar todos los metadatos y propiedades disponibles en el esquema para el intervalo de búsqueda determinado.

## <a name="time-series-model-query-tsm-q-apis"></a>API de consulta de modelo de serie temporal (TSM-Q)

La mayoría de estas API admite la operación de ejecución por lotes para permitir operaciones CRUD de lote en varias entidades de modelo de serie temporal:

* [Model Settings API](https://docs.microsoft.com/rest/api/time-series-insights/preview#model-settings-api): permite operaciones *GET* y *PATCH* en el tipo predeterminado y el nombre del modelo del entorno.
* [Types API](https://docs.microsoft.com/rest/api/time-series-insights/preview#types-api): Habilita CRUD en tipos de serie temporal y en sus variables asociadas.
* [Hierarchies API](https://docs.microsoft.com/rest/api/time-series-insights/preview#hierarchies-api): Habilita CRUD en jerarquías de serie temporal y en sus variables asociadas.
* [Instances API](https://docs.microsoft.com/rest/api/time-series-insights/preview#instances-api): Habilita CRUD en jerarquías de serie temporal y en sus campos de instancias asociados. Además, Instances API admite las siguientes operaciones:
  * [Buscar](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/search): recupera una lista parcial de aciertos en la búsqueda de instancias de serie temporal según los atributos de instancia.
  * [Sugerir](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/suggest): busca y sugiere una lista parcial de aciertos en la búsqueda de instancias de serie temporal según los atributos de instancia.

## <a name="time-series-query-tsq-apis"></a>API de consulta de serie temporal (TSQ)

Estas API están disponibles en ambos almacenes en nuestra solución de almacenamiento multicapa en Time Series Insights. Los parámetros de dirección URL de consulta se usan para especificar el [tipo de almacén](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters) en el que se debe ejecutar la consulta:

* [Get Events API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents): Permite la consulta y recuperación de los datos sin procesar y las marcas de tiempo de evento correspondientes que se registran en la instancia de Time Series Insights del proveedor de origen. Esta API permite la recuperación de eventos sin procesar según un intervalo de búsqueda y un identificador de serie temporal dados. Esta API admite la paginación para recuperar el conjunto de datos de respuesta completo de la entrada seleccionada. 

* [Get Series API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries): Permite la consulta y recuperación de los valores calculados y las marcas de tiempo de evento correspondientes, aplicando cálculos definidos por variables en los eventos sin procesar. Estas variables se pueden definir en el modelo de serie temporal o proporcionarse en línea en la consulta. Esta API admite la paginación para recuperar el conjunto de datos de respuesta completo de la entrada seleccionada. 

* [Aggregate Series API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries): Permite la consulta y recuperación de los valores agregados y las marcas de tiempo de intervalo correspondientes, aplicando cálculos definidos por variables en los eventos sin procesar. Estas variables se pueden definir en el modelo de serie temporal o proporcionarse en línea en la consulta. Esta API admite la paginación para recuperar el conjunto de datos de respuesta completo de la entrada seleccionada. 
  
  En un intervalo de búsqueda especificado, esta API devuelve una respuesta agregada por variable y por intervalo de un identificador de serie temporal. El número de intervalos en el conjunto de datos de respuesta se calcula contando los tics de época (número de milisegundos transcurrido desde la época Unix, esto es, desde el 1 de enero 1970) y dividiendo los tics entre el tamaño del intervalo especificado en la consulta.

  Las marcas de tiempo devueltas en el conjunto de respuestas corresponden a los límites de intervalo que quedan, no a los eventos muestreados del intervalo. 

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las distintas variables que se pueden definir en el [modelo de serie temporal](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-tsm).
- Obtenga más información sobre cómo consultar datos desde el [Explorador de Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-explorer).
