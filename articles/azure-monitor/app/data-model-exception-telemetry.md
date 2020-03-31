---
title: Modelo de datos de telemetría de excepciones de Azure Application Insights
description: Modelo de datos de Application Insights para la telemetría de excepciones
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: fa9bc4efd9549cbcb1d50439989c5dea79d9d60c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671943"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Telemetría de excepciones: Modelo de datos de Application Insights

En [Application Insights](../../azure-monitor/app/app-insights-overview.md), una instancia de Exception representa una excepción controlada o no controlada que se produjo durante la ejecución de la aplicación supervisada.

## <a name="problem-id"></a>Identificador del problema

Identificador de dónde se produjo la excepción en el código. Se usa para el agrupamiento de las excepciones. Normalmente es una combinación del tipo de excepción y una función de la pila de llamadas.

Longitud máxima: 1024 caracteres

## <a name="severity-level"></a>Nivel de gravedad

El nivel de gravedad del seguimiento. El valor puede ser `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Detalles de la excepción

(Se ampliará)

## <a name="custom-properties"></a>Propiedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Medidas personalizadas

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Pasos siguientes

- Consulte [modelo de datos](data-model.md) para los tipos y el modelo de datos de Application Insights.
- Obtenga información para el [diagnóstico de excepciones en aplicaciones web con Application Insights](../../azure-monitor/app/asp-net-exceptions.md).
- Consulte las [plataformas](../../azure-monitor/app/platforms.md) compatibles con Application Insights.
