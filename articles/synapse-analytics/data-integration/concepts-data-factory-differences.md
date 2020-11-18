---
title: Diferencias con respecto a Azure Data Factory
description: Aprenda cómo las funcionalidades de integración de datos de Azure Synapse Analytics difieren de las de Azure Data Factory
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 11/06/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: 10f5336dd4c8a02acf623b1b14226ca676006953
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2020
ms.locfileid: "94357656"
---
# <a name="data-integration-in-azure-synapse-analytics-versus-azure-data-factory"></a>Integración de datos en Azure Synapse Analytics frente a Azure Data Factory

En Azure Synapse Analytics, las funcionalidades de integración de datos, como los flujos de datos y las canalizaciones de Synapse, se basan en las de Azure Data Factory. Para más información, vea [¿Qué es Azure Data Factory?](../../data-factory/introduction.md). Casi todas las funcionalidades son idénticas o similares, y la documentación se comparte entre los dos servicios. En este artículo se resaltan e identifican las diferencias actuales entre Azure Data Factory y Azure Synapse.

Para ver si una característica o un artículo de Azure Data Factory se aplica a Azure Synapse, consulte el moniker en la parte superior del artículo.

![Se aplica al moniker](../media/concepts-data-factory-differences/applies-to-moniker.png "Se aplica al moniker")

## <a name="features-in-azure-data-factory-not-planned-for-azure-synapse"></a>Características de Azure Data Factory no planeadas para Azure Synapse

Las siguientes características están disponibles en Azure Data Factory, pero no están previstas para Azure Synapse.

* **Migración de paquetes SSIS mediante lift-and-shift:** en Azure Data Factory, tiene la capacidad migrar paquetes SSIS mediante lift-and-shif en el entorno de ejecución de integración de SSIS. Tanto el entorno de ejecución de integración de SSIS como la ejecución de una actividad del paquete de SSIS no están disponibles en las áreas de trabajo de Synapse. 
* **Período de vida:** período de vida es un parámetro en el entorno de ejecución de integración de SSIS que permite que el clúster de Spark en los flujos de datos de asignación permanezca *activo* durante un período de tiempo después de la finalización de un flujo de datos. Esta característica no está disponible en las áreas de trabajo de Synapse.

## <a name="azure-synapse-features-not-supported-in-azure-data-factory"></a>Características de Azure Synapse no admitidas en Azure Data Factory

Las siguientes características están disponibles en Azure Synapse, pero no están previstas para Azure Data Factory.

* **Supervisión de trabajos de Spark de flujos de datos de asignación:** En Synapse, el motor de Spark está incluido en la suscripción del usuario para que los usuarios puedan ver los registros de Spark detallados. En Azure Data Factory, la ejecución del trabajo se realiza en un clúster de Spark administrado por Azure Data Factory y esta información no está disponible. 

## <a name="azure-data-factory-features-that-behave-differently-in-synapse"></a>Características de Azure Data Factory que se comportan de manera diferente en Synapse

Las siguientes características se comportan de manera diferente o no existen actualmente en Azure Synapse. 

* **Flujos de datos de limpieza y transformación:** la actividad de flujo de datos de limpieza y transformación solo está disponible en Azure Data Factory en este momento.
* **La galería de plantillas de solución:** en Azure Data Factory, los usuarios pueden encontrar plantillas de canalización en la galería de plantillas de solución. En las áreas de trabajo de Synapse, el centro de información contiene un conjunto de plantillas diferente junto con conjuntos de datos y scripts SQL adicionales. 
* **Integración de Git y una solución nativa de CI/CD:** actualmente, un área de trabajo de Synapse no se puede conectar a un repositorio de Git ni sigue el mismo proceso de integración y entrega continuos que Azure Data Factory.
* **Integración con Azure Monitor:** las áreas de trabajo de Synapse no se integran con Azure Monitor igual que Azure Data Factory.
* **Configuración de entorno de ejecución de integración híbrido:** dentro de un área de trabajo de Synapse, un usuario no puede tener un entorno de ejecución de integración de red virtual administrada y un entorno de Azure IR. Esta funcionalidad se admite en Azure Data Factory.
* **Uso compartido del entorno de ejecución de integración:** los entornos de ejecución de integración autohospedados no se pueden compartir entre áreas de trabajo de Synapse. Esta funcionalidad se admite en Azure Data Factory.
* **Entornos de ejecución de integración entre regiones para flujos de datos:** los flujos de datos no se pueden ejecutar en entornos de ejecución de integración en distintas regiones que un área de trabajo de Synapse. Esta funcionalidad se admite en Azure Data Factory.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con la integración de datos en el área de trabajo de Synapse, aprenda cómo [ingerir datos en una cuenta de Azure Data Lake Storage Gen2](data-integration-data-lake.md).
