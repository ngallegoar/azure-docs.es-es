---
title: Diferencias con respecto a Azure Data Factory
description: Aprenda cómo las funcionalidades de integración de datos de Azure Synapse Analytics difieren de las de Azure Data Factory
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 09/23/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: be098977ba51e529aaecfb5dc3b7a03444f55a7e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91339723"
---
# <a name="data-integration-in-azure-synapse-analytics-versus-azure-data-factory"></a>Integración de datos en Azure Synapse Analytics frente a Azure Data Factory

En Azure Synapse Analytics, las funcionalidades de integración de datos, como los flujos de datos y las canalizaciones de Synapse, se basan en las de Azure Data Factory. Para más información, vea [¿Qué es Azure Data Factory?](../../data-factory/introduction.md). Casi todas las funcionalidades son idénticas o similares, y la documentación se comparte entre los dos servicios. En este artículo se resaltan e identifican las diferencias actuales entre Azure Data Factory y Azure Synapse.

Para ver si una característica o un artículo de Azure Data Factory se aplica a Azure Synapse, consulte el moniker en la parte superior del artículo.

![Se aplica al moniker](../media/concepts-data-factory-differences/applies-to-moniker.png "Se aplica al moniker")

## <a name="features-in-azure-data-factory-not-planned-for-azure-synapse"></a>Características de Azure Data Factory no planeadas para Azure Synapse

Las siguientes características están disponibles en Azure Data Factory, pero no están previstas para Azure Synapse.

* La capacidad para levantar y mover paquetes de SSIS.
* Snowflake como receptor en la actividad de copia y el flujo de datos de asignación.
* El valor del período de vida del flujo de datos de asignación del entorno de ejecución de integración.

## <a name="azure-synapse-features-not-supported-in-azure-data-factory"></a>Características de Azure Synapse no admitidas en Azure Data Factory

Las siguientes características están disponibles en Azure Synapse, pero no están previstas para Azure Data Factory.

* La supervisión de trabajos de Spark de flujos de datos de asignación solo está disponible en Synapse. En Synapse, el motor de Spark está incluido en la suscripción del usuario para que los usuarios puedan ver los registros de Spark detallados. En Azure Data Factory, la ejecución del trabajo se realiza en un clúster Spark administrado por Azure Data Factory. 

## <a name="azure-data-factory-features-that-behave-differently-in-synapse"></a>Características de Azure Data Factory que se comportan de manera diferente en Synapse

Las siguientes características se comportan de manera diferente o no existen actualmente en Azure Synapse. 

* Flujos de datos de limpieza y transformación
* La galería de plantillas de solución
* Integración de Git y una solución nativa de CI/CD
* Integración con Azure Monitor
* Cambio de nombre de los recursos después de la publicación
* Configuración del entorno de ejecución de integración híbrida dentro de un área de trabajo de Synapse. Un usuario no puede tener un entorno de ejecución de integración de VNET administrada y Azure IR.
* Uso compartido del entorno de ejecución de integración entre áreas de trabajo de Synapse

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con la integración de datos en el área de trabajo de Synapse, aprenda cómo [ingerir datos en una cuenta de Azure Data Lake Storage Gen2](data-integration-data-lake.md).
