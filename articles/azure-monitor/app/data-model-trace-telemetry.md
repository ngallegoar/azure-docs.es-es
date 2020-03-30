---
title: 'Modelo de datos de Azure Application Insights: telemetría de seguimiento'
description: Modelo de datos de Application Insights para la telemetría de seguimientos
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 31958b26cdb8a7897cf0051af6600014c07949fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671960"
---
# <a name="trace-telemetry-application-insights-data-model"></a>Telemetría de seguimientos: modelo de datos de Application Insights

La telemetría de seguimientos (en [Application Insights](../../azure-monitor/app/app-insights-overview.md)) representa instrucciones de seguimiento de estilo `printf` en las que se pueden realizar búsquedas de texto. `Log4Net`, `NLog` y las demás entradas de archivo de registro basadas en texto se convierten a instancias de este tipo. El seguimiento no tiene medidas como una extensibilidad.

## <a name="message"></a>Message

Mensaje de seguimiento.

Longitud máxima: 32 768 caracteres

## <a name="severity-level"></a>Nivel de gravedad

El nivel de gravedad del seguimiento. El valor puede ser `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Propiedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Exploración de los registros de seguimiento de .NET en Application Insights](../../azure-monitor/app/asp-net-trace-logs.md).
- [Exploración de los registros de seguimiento de Java en Application Insights](../../azure-monitor/app/java-trace-logs.md).
- Consulte [modelo de datos](data-model.md) para los tipos y el modelo de datos de Application Insights.
- [Escritura de telemetría de seguimiento personalizada](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)
- Consulte las [plataformas](../../azure-monitor/app/platforms.md) compatibles con Application Insights.
