---
title: Migración de las métricas de Azure Storage | Microsoft Docs
description: Más información sobre cómo migrar las métricas antiguas a las métricas nuevas que Azure Monitor administra.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 537369c9466b1083723642ec9e93fcdf25056c5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68855336"
---
# <a name="azure-storage-metrics-migration"></a>Migración de las métricas de Azure Storage

Alineado con la estrategia de unificar la experiencia de monitor en Azure, Azure Storage integra métricas en la plataforma de Azure Monitor. En el futuro, finalizará el servicio de las métricas antiguas con un aviso por adelantado en función de la directiva de Azure. Si usa las métricas de almacenamiento antiguas, debe migrarlas antes de la fecha de finalización del servicio para mantener su información de métricas.

En este artículo se muestra cómo migrar desde las métricas antiguas hasta a las métricas nuevas.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Descripción de las métricas antiguas que administra Azure Storage

Azure Storage recopila valores de métricas antiguas, y los agrega y almacena en tablas $Metric dentro de la misma cuenta de almacenamiento. Puede usar Azure Portal para configurar un gráfico de supervisión. También puede utilizar los SDK de Azure Storage para leer los datos de las tablas $Metric que se basan en el esquema. Para más información, consulte [Storage Analytics](./storage-analytics.md).

Las métricas antiguas proporcionan métricas de capacidad solo sobre Azure Blob Storage. Las métricas antiguas proporcionan métricas de transacciones sobre Blob Storage, Table Storage, Azure Files y Queue Storage.

Las métricas antiguas están diseñadas en un esquema sin formato. El diseño genera un valor de métrica de cero cuando los patrones de tráfico no desencadenan la métrica. Por ejemplo, el valor de **ServerTimeoutError** se establece en 0 en las tablas $Metric incluso cuando no se recibe ningún error de tiempo de expiración del servidor a partir del tráfico real a una cuenta de almacenamiento.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Descripción de las métricas nuevas que administra Azure Monitor

En el caso de las métricas de almacenamiento nuevas, Azure Storage emite los datos de métrica al back-end de Azure Monitor. Azure Monitor proporciona una experiencia de supervisión unificada, incluidos los datos del portal, además de la ingesta de datos. Para más detalles, puede consultar este [artículo](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Las métricas nuevas proporcionan métricas de capacidad y métricas de transacción en Blob, Table, File, Queue y Premium Storage.

La multidimensionalidad es una de las características que Azure Monitor proporciona. Azure Storage adopta el diseño en la definición del nuevo esquema de métricas. Para las dimensiones compatibles con las métricas, puede encontrar detalles en [Métricas de Azure Storage en Azure Monitor](./storage-metrics-in-azure-monitor.md). El diseño multidimensional brinda rentabilidad tanto en el ancho de banda de la ingesta y la capacidad de almacenamiento de métricas. Por lo tanto, si el tráfico no ha desencadenado métricas relacionadas, no se generarán los datos de métricas relacionados. Por ejemplo, si el tráfico no desencadenó ningún error de tiempo de expiración de servidor, Azure Monitor no devuelve ningún dato cuando se consulta el valor de la métrica **Transactions** con la dimensión **ResponseType** equivalente a **ServerTimeoutError**.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Asignación de métricas entre métricas antiguas y métricas nuevas

Si lee los datos de métricas mediante programación, debe adoptar el esquema de métricas nuevo en sus programas. Para entender mejor los cambios, puede consultar la asignación que aparece en la tabla siguiente:

**Métricas de capacidad**

| Métrica antigua | Métrica nueva |
| ------------------- | ----------------- |
| **Capacity**            | **BlobCapacity** con la dimensión **BlobType** equivalente a **BlockBlob** o **PageBlob** |
| **ObjectCount**        | **BlobCount** con la dimensión **BlobType** equivalente a **BlockBlob** o **PageBlob** |
| **ContainerCount**      | **ContainerCount** |

Las métricas siguientes son ofertas nuevas no compatibles con las métricas antiguas:
* **TableCapacity**
* **TableCount**
* **TableEntityCount**
* **QueueCapacity**
* **QueueCount**
* **QueueMessageCount**
* **FileCapacity**
* **FileCount**
* **FileShareCount**
* **UsedCapacity**

**Métricas de transacción**

| Métrica antigua | Métrica nueva |
| ------------------- | ----------------- |
| **AnonymousAuthorizationError** | Transacciones con la dimensión **ResponseType** equivalente a **AuthorizationError**, y la dimensión **Authentication** igual a **Anonymous** |
| **AnonymousClientOtherError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientOtherError**, y la dimensión **Authentication** igual a **Anonymous** |
| **AnonymousClientTimeoutError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientTimeoutError**, y la dimensión **Authentication** igual a **Anonymous** |
| **AnonymousNetworkError** | Transacciones con la dimensión **ResponseType** equivalente a **NetworkError**, y la dimensión **Authentication** igual a **Anonymous** |
| **AnonymousServerOtherError** | Transacciones con la dimensión **ResponseType** equivalente a **ServerOtherError**, y la dimensión **Authentication** igual a **Anonymous** |
| **AnonymousServerTimeoutError** | Transacciones con la dimensión **ResponseType** equivalente a **ServerTimeoutError**, y la dimensión **Authentication** igual a **Anonymous** |
| **AnonymousSuccess** | Transacciones con la dimensión **ResponseType** equivalente a **Success**, y la dimensión **Authentication** igual a **Anonymous** |
| **AnonymousThrottlingError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientThrottlingError** o **ServerBusyError**, y la dimensión **Authentication** igual a **Anonymous** |
| **AuthorizationError** | Transacciones con la dimensión **ResponseType** equivalente a **AuthorizationError** |
| **Disponibilidad** | **Disponibilidad** |
| **AverageE2ELatency** | **SuccessE2ELatency** |
| **AverageServerLatency** | **SuccessServerLatency** |
| **ClientOtherError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientOtherError** |
| **ClientTimeoutError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientTimeoutError** |
| **NetworkError** | Transacciones con la dimensión **ResponseType** equivalente a **NetworkError** |
| **PercentAuthorizationError** | Transacciones con la dimensión **ResponseType** equivalente a **AuthorizationError** |
| **PercentClientOtherError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientOtherError** |
| **PercentNetworkError** | Transacciones con la dimensión **ResponseType** equivalente a **NetworkError** |
| **PercentServerOtherError** | Transacciones con la dimensión **ResponseType** equivalente a **ServerOtherError** |
| **PercentSuccess** | Transacciones con la dimensión **ResponseType** equivalente a **Success** |
| **PercentThrottlingError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientThrottlingError** o **ServerBusyError** |
| **PercentTimeoutError** | Transacciones con la dimensión **ResponseType** equivalente a **ServerTimeoutError** o **ResponseType** equivalente a **ClientTimeoutError** |
| **SASAuthorizationError** | Transacciones con la dimensión **ResponseType** equivalente a **AuthorizationError**, y la dimensión **Authentication** igual a **SAS** |
| **SASClientOtherError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientOtherError**, y la dimensión **Authentication** igual a **SAS** |
| **SASClientTimeoutError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientTimeoutError**, y la dimensión **Authentication** igual a **SAS** |
| **SASNetworkError** | Transacciones con la dimensión **ResponseType** equivalente a **NetworkError**, y la dimensión **Authentication** igual a **SAS** |
| **SASServerOtherError** | Transacciones con la dimensión **ResponseType** equivalente a **ServerOtherError**, y la dimensión **Authentication** igual a **SAS** |
| **SASServerTimeoutError** | Transacciones con la dimensión **ResponseType** equivalente a **ServerTimeoutError**, y la dimensión **Authentication** igual a **SAS** |
| **SASSuccess** | Transacciones con la dimensión **ResponseType** equivalente a **Success**, y la dimensión **Authentication** igual a **SAS** |
| **SASThrottlingError** | Transacciones con la dimensión **ResponseType** equivalente a **ClientThrottlingError** o **ServerBusyError**, y la dimensión **Authentication** igual a **SAS** |
| **ServerOtherError** | Transacciones con la dimensión **ResponseType** equivalente a **ServerOtherError** |
| **ServerTimeoutError** | Transacciones con la dimensión **ResponseType** equivalente a **ServerTimeoutError** |
| **Success** | Transacciones con la dimensión **ResponseType** equivalente a **Success** |
| **ThrottlingError** | **Transacciones** con la dimensión **ResponseType** equivalente a **ClientThrottlingError** o **ServerBusyError**|
| **TotalBillableRequests** | **Transactions** |
| **TotalEgress** | **Salida** |
| **TotalIngress** | **Entrada** |
| **TotalRequests** | **Transactions** |

## <a name="faq"></a>Preguntas más frecuentes

### <a name="how-should-i-migrate-existing-alert-rules"></a>¿Cómo se deben migrar las reglas de alertas existentes?

Si creó reglas de alertas clásicas en función de métricas de almacenamiento antiguas, debe crear reglas de alertas nuevas según el esquema de métricas nuevo.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>¿Los datos de métricas nuevas se almacenan en la misma cuenta de almacenamiento de manera predeterminada?

No. Para archivar los datos de métricas en una cuenta de almacenamiento, use la [API de configuración de diagnóstico de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate).

## <a name="next-steps"></a>Pasos siguientes

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Métricas de Azure Storage en Azure Monitor](./storage-metrics-in-azure-monitor.md)
