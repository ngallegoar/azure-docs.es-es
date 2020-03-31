---
title: Soluciones que usan análisis distribuido globalmente en Azure Cosmos DB
description: Obtenga información sobre las soluciones que se pueden crear mediante un análisis distribuido globalmente en Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.topic: conceptual
ms.service: cosmos-db
ms.date: 09/26/2019
ms.reviewer: sngun
ms.openlocfilehash: d0b5042c1401a95cc75f4c01ace843659b6babfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73681718"
---
# <a name="solutions-using-globally-distributed-analytics-in-azure-cosmos-db"></a>Soluciones que usan análisis distribuido globalmente en Azure Cosmos DB

En este artículo se describen las soluciones que se pueden crear mediante un análisis distribuido globalmente en Azure Cosmos DB.

## <a name="retail-and-consumer-goods"></a>Venta al por menor y bienes de consumo

Puede usar la compatibilidad con Spark en Azure Cosmos DB para ofrecer recomendaciones y ofertas en tiempo real. Puede ayudar a los clientes a descubrir los artículos que necesitan con la personalización en tiempo real y las recomendaciones de productos.

* Puede usar la compatibilidad integrada de Machine Learning proporcionada por el motor en tiempo de ejecución de Apache Spark para generar recomendaciones en tiempo real en los catálogos de productos.

* Puede extraer con un clic datos de streaming, datos de compra y datos de cliente para proporcionar recomendaciones específicas que promuevan valor a largo plazo.

* Con la característica de distribución global de Azure Cosmos DB, los grandes volúmenes de datos de productos que se distribuyen entre regiones se pueden analizar en milisegundos.

* Puede obtener rápidamente conclusiones para datos y usuarios geográficamente distribuidos. Puede mejorar la tasa de conversión de promoción ofreciendo el anuncio adecuado al usuario adecuado en el momento adecuado.

* Puede aprovechar la funcionalidad de streaming integrada de Spark para enriquecer datos en directo combinándolos con datos de clientes estáticos. De este modo, podrá ofrecer anuncios más personalizados y dirigidos en tiempo real y en contexto con lo que hacen los clientes.

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB para optimizar los precios y las promociones:

![Compatibilidad de Spark con Azure Cosmos DB para optimizar los precios y las promociones](./media/analytics-solution-architectures/optimize-pricing-and-promotions.png)


La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB en el motor de recomendaciones en tiempo real:

![Compatibilidad de Spark con Azure Cosmos DB en el motor de recomendaciones en tiempo real](./media/analytics-solution-architectures/real-time-recommendation-engine.png)

## <a name="manufacturing-and-iot"></a>Fabricación y IoT

La plataforma de análisis incorporada de Azure Cosmos DB permite habilitar el análisis en tiempo real de los datos de IoT desde millones de dispositivos a escala global. Puede realizar innovaciones modernas como predecir patrones meteorológicos, análisis predictivos y optimizaciones de energía.

* Mediante el uso de Azure Cosmos DB, puede extraer datos, como métricas de recursos en tiempo real y factores meteorológicos, y luego aplicar el análisis de red de distribución inteligente de electricidad para optimizar el rendimiento de los dispositivos conectados en el campo. El análisis de red de distribución inteligente de electricidad es la clave para controlar los costos de funcionamiento, mejorar la confiabilidad de la red y ofrecer servicios energéticos personalizados a los consumidores.

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB para leer métricas de dispositivos IoT y aplicar el análisis de red de distribución inteligente de electricidad:

![Compatibilidad de Spark con Azure Cosmos DB para leer métricas de dispositivos IoT](./media/analytics-solution-architectures/read-metrics-from-iot-devices.png)

## <a name="predictive-maintenance"></a>Mantenimiento predictivo

* El mantenimiento de recursos como los compresores que se usan en equipos de perforación pequeños a plataformas de aguas profunda, es un esfuerzo complejo. Estos recursos se encuentran por todo el mundo y generan petabytes de datos. Mediante el uso de Azure Cosmos DB, puede crear una canalización de datos predictivos de un extremo a otro, que use streaming de Spark para procesar grandes cantidades de telemetría de sensor, piezas de recursos de almacén y datos de asignaciones de sensor.

* Puede crear e implementar modelos de aprendizaje automático para predecir errores en los recursos antes de que se produzcan, y emitir pedidos de trabajo de mantenimiento antes de que se produzca el error.

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB para crear un sistema de mantenimiento predictivo:

![Compatibilidad de Spark con Azure Cosmos DB para crear un sistema de mantenimiento predictivo](./media/analytics-solution-architectures/predictive-maintenance-system.png)

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB para crear un sistema de diagnóstico de vehículos en tiempo real:

![Compatibilidad de Spark con Azure Cosmos DB para crear un sistema de diagnóstico de vehículos en tiempo real](./media/analytics-solution-architectures/real-time-vehicle-diagnostic-system.png)

## <a name="gaming"></a>Juegos

* Gracias a la compatibilidad integrada con Spark, Azure Cosmos DB le permite compilar, escalar e implementar fácilmente modelos de aprendizaje automático y análisis avanzados en unos minutos para crear la mejor experiencia de juego posible.

* Puede analizar el reproductor, la compra y los datos de comportamiento para crear ofertas personalizadas apropiadas con el fin de lograr altos índices de conversión.

* Con el aprendizaje automático de Spark, puede analizar y obtener conclusiones sobre los datos de telemetría de juegos. Puede diagnosticar y evitar tiempos de carga lentos y problemas en el juego.

La siguiente imagen muestra cómo se usa la compatibilidad de Spark con Azure Cosmos DB en los análisis de juego:

![Compatibilidad de Spark de Azure Cosmos DB en el análisis de juegos](./media/analytics-solution-architectures/gaming-analytics.png)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información acerca de las ventajas de Azure Cosmos DB, consulte el artículo de [introducción](introduction.md).
* [Introducción a la API de Azure Cosmos DB para MongoDB](mongodb-introduction.md)
* [Introducción a Cassandra API de Azure Cosmos DB](cassandra-introduction.md)
* [Introducción a Gremlin API de Azure Cosmos DB](graph-introduction.md)
* [Introducción a Table API de Azure Cosmos DB](table-introduction.md)
