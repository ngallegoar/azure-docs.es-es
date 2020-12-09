---
title: 'Introducción a la ingesta: Azure Time Series Insights Gen2 | Microsoft Docs'
description: Aprenda sobre la ingesta de datos en Azure Time Series Insights Gen2.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/02/2020
ms.custom: seodec18
ms.openlocfilehash: 2b76490ab23cb14755f6f90a27ec8b176bb30a11
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96780032"
---
# <a name="azure-time-series-insights-gen2-data-ingestion-overview"></a>Introducción a la ingesta de datos en Azure Time Series Insights Gen2

El entorno de Azure Time Series Insights Gen2 contiene un *motor de ingesta* para recopilar, procesar y almacenar datos de series temporales de streaming. A medida que los datos llegan a los orígenes de eventos, Azure Time Series Insights Gen2 consumirá y almacenará los datos casi en tiempo real.

[![Introducción a la ingesta](media/concepts-ingress-overview/ingress-overview.png)](media/concepts-ingress-overview/ingress-overview.png#lightbox)

## <a name="ingestion-topics"></a>Temas de ingesta de datos

Los siguientes artículos tratan a fondo del procesamiento de datos, incluidos los procedimientos recomendados que se deben seguir:

* Obtenga información sobre [orígenes de eventos](./concepts-streaming-ingestion-event-sources.md) y orientación sobre cómo seleccionar una marca de tiempo de origen de eventos.

* Revisar los [tipos de datos](./concepts-supported-data-types.md) admitidos

* Comprender cómo el motor de ingesta aplicará un conjunto de [reglas](./concepts-json-flattening-escaping-rules.md) a las propiedades JSON para crear las columnas de la cuenta de almacenamiento.

* Revise en el entorno las [limitaciones de rendimiento](./concepts-streaming-ingress-throughput-limits.md) para planificar sus necesidades de escala.

## <a name="next-steps"></a>Pasos siguientes

* Continúe para aprender sobre los [orígenes de eventos](./concepts-streaming-ingestion-event-sources.md) en el entorno de Azure Time Series Insights Gen2.
