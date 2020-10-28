---
title: Niveles de coherencia de Apache Cassandra y Azure Cosmos DB
description: Niveles de coherencia de Apache Cassandra y Azure Cosmos DB.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/12/2020
ms.reviewer: sngun
ms.openlocfilehash: 155e1ed5b089c16887c845c94f36c9c6d9f0af6e
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283293"
---
# <a name="apache-cassandra-and-azure-cosmos-db-consistency-levels"></a>Niveles de coherencia de Apache Cassandra y Azure Cosmos DB

A diferencia de Azure Cosmos DB, Apache Cassandra no proporciona de forma nativa garantías de coherencia definidas con precisión. En su lugar, Cassandra Apache proporciona un nivel de coherencia de escritura y de lectura para permitir los inconvenientes que representan la alta disponibilidad, coherencia, disponibilidad y latencia. Al usar Cassandra API de Azure Cosmos DB:

* El nivel de coherencia de escritura de Apache Cassandra se asigna al nivel de coherencia predeterminado configurado en la cuenta de Azure Cosmos. No se puede cambiar la coherencia de una operación de escritura (CL) en cada solicitud.

* Azure Cosmos DB asignará dinámicamente el nivel de coherencia de lectura que el controlador de cliente de Cassandra especifique en uno de los niveles de coherencia de Azure Cosmos DB configurados dinámicamente en una solicitud de lectura.

## <a name="mapping-consistency-levels"></a>Asignación de niveles de coherencia

En la tabla siguiente se muestra cómo se asignan los niveles de coherencia nativos de Cassandra para los niveles de coherencia de Azure Cosmos DB cuando se usa Cassandra API:  

:::image type="content" source="./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png" alt-text="Asignación de modelos de coherencia de Cassandra" lightbox="./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png" :::

Si la cuenta de Azure Cosmos está configurada con un nivel de coherencia que no es el de coherencia alta, puede averiguar la probabilidad de que los clientes obtengan lecturas de coherencia alta para sus cargas de trabajo consultando la métrica de *obsolescencia limitada de probabilidad* (PBS). Esta métrica se expone en Azure Portal; para obtener más información, consulte [Supervisión de la métrica de obsolescencia limitada de manera probabilística (PBS)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

La obsolescencia limitada de probabilidad muestra cómo de posible es la coherencia final. Esta métrica proporciona una visión general de la frecuencia con la que puede obtener una coherencia mayor que el nivel de coherencia que tiene configurado actualmente en su cuenta de Azure Cosmos. En otras palabras, puede ver la probabilidad (en milisegundos) de obtener lecturas con coherencia alta para una combinación de regiones de escritura y lectura.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la distribución global y los niveles de coherencia en Azure Cosmos DB:

* [Introducción a la distribución global](distribute-data-globally.md)
* [Información general sobre el nivel de coherencia](consistency-levels.md)
* [Alta disponibilidad](high-availability.md)
