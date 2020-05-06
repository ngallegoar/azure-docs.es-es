---
title: Creación de particiones y escalado horizontal en Azure Cosmos DB
description: Obtenga información sobre cómo funciona la creación de particiones en Azure Cosmos DB, cómo configurar la creación de particiones y las claves de partición y cómo seleccionar la clave de partición correcta para su aplicación.
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 19e4c61ba930bb9b127e2401174bcea3fd240dce
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82234233"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Creación de particiones y escalado horizontal en Azure Cosmos DB

En este artículo, se explica la relación entre las particiones lógicas y las físicas. También se describen los procedimientos recomendados para crear particiones y se proporciona una perspectiva detallada sobre el funcionamiento del escalado horizontal en Azure Cosmos DB. No es necesario comprender estos detalles internos para [seleccionar la clave de partición](partitioning-overview.md#choose-partitionkey), pero hemos preferido explicarlos para que sepa cómo funciona Azure Cosmos DB.

## <a name="logical-partitions"></a>Particiones lógicas

Una partición lógica consta de un conjunto de elementos con la misma clave de partición. Por ejemplo, en un contenedor con datos sobre nutrición, todos los elementos contienen la propiedad `foodGroup`. Puede utilizar `foodGroup` como clave de partición del contenedor. Los grupos de elementos que tienen valores de `foodGroup` específicos, como `Beef Products`, `Baked Products` y `Sausages and Luncheon Meats`, conforman distintas particiones lógicas. No tiene que preocuparse de quitar una partición lógica una vez eliminados los datos subyacentes.

Una partición lógica también define el ámbito de las transacciones de base de datos. Puede actualizar los elementos dentro de una partición lógica mediante el uso de una [transacción con aislamiento de instantánea](database-transactions-optimistic-concurrency.md). Cuando se agregan nuevos elementos al contenedor, se crean nuevas particiones lógicas de forma transparente por el sistema.

No existen límites en el número de particiones lógicas que puede tener el contenedor. Cada partición lógica puede almacenar un máximo de 20 GB de datos. Las claves de partición correctas son aquellas que tienen una amplia gama de valores posibles. Por ejemplo, en un contenedor donde todos los elementos contienen la propiedad `foodGroup`, los datos de la partición lógica `Beef Products` podrían crecer hasta los 20 GB. [Seleccionar una clave de partición](partitioning-overview.md#choose-partitionkey) con una amplia gama de valores posibles garantiza que el contenedor se puede escalar.

## <a name="physical-partitions"></a>Particiones físicas

Cuando se escala un contenedor de Azure Cosmos, los datos y el rendimiento se distribuyen entre las distintas particiones lógicas. De forma interna, a cada partición física se asignan una o varias particiones lógicas. La mayoría de los contenedores de Cosmos pequeños tienen muchas particiones lógicas, pero solo necesitan una partición física. A diferencia de las particiones lógicas, las particiones físicas son una implementación interna del sistema y es Azure Cosmos DB quien se encarga en exclusiva de su administración.

El número de particiones físicas del contenedor de Cosmos dependerá de lo siguiente:

- Cantidad de rendimiento aprovisionado (cada partición física puede proporcionar un rendimiento de hasta 10 000 unidades de solicitud por segundo)
- Almacenamiento total de datos (cada partición física puede almacenar un máximo de 50 GB)

No existen límites en el número de particiones físicas que puede tener el contenedor. A medida que aumente el tamaño de los datos o el rendimiento aprovisionado, Azure Cosmos DB creará nuevas particiones físicas automáticamente dividiendo las particiones existentes. Las divisiones de las particiones físicas no afectan a la disponibilidad de la aplicación. Cuando una partición física se divide, todos los datos que estén en una partición lógica específica se guardarán en la misma partición física. Las divisiones de las particiones físicas simplemente crean una nueva asignación entre las particiones lógicas y las particiones físicas.

El rendimiento aprovisionado para un contenedor se divide uniformemente entre las particiones físicas. Un diseño de clave de partición que no distribuye uniformemente las solicitudes de rendimiento puede crear particiones "activas". Las particiones activas pueden conllevar un uso ineficaz y que limite la velocidad del rendimiento aprovisionado y costos más elevados.

Puede ver las particiones físicas de un contenedor en la sección **Almacenamiento** de la **hoja de métricas** de Azure Portal:

[![Consulta del número de particiones físicas](./media/partition-data/view-partitions-zoomed-out.png) ](./media/partition-data/view-partitions-zoomed-in.png#lightbox)

En este contenedor de ejemplo en el que hemos elegido `/foodGroup` como clave de partición, cada uno de los tres rectángulos representa una partición física. En la imagen, el **intervalo de claves de partición** es el mismo que el de una partición física. La partición física seleccionada contiene tres particiones lógicas: `Beef Products`, `Vegetable and Vegetable Products` y `Soups, Sauces, and Gravies`.

Si se aprovisiona un rendimiento de 18 000 unidades de solicitud por segundo (RU/s), cada una de las tres particiones físicas podrá usar un tercio del rendimiento aprovisionado total. Dentro de la partición física seleccionada, las claves de partición lógicas `Beef Products`, `Vegetable and Vegetable Products` y `Soups, Sauces, and Gravies` podrán, en conjunto, utilizar 6 000 RU/s aprovisionadas de la partición física. Como el rendimiento aprovisionado se reparte uniformemente entre las particiones físicas del contenedor, es importante distribuir uniformemente el consumo del rendimiento [eligiendo la clave de partición lógica adecuada](partitioning-overview.md#choose-partitionkey). Si elige una clave de partición que distribuya uniformemente el consumo del rendimiento por las particiones lógicas, tendrá la seguridad de que el consumo del rendimiento está equilibrado entre las particiones físicas.

## <a name="replica-sets"></a>Conjuntos de réplicas

Cada partición física se compone de un [*conjunto de réplicas*](global-dist-under-the-hood.md). Cada conjunto de réplicas hospeda una instancia del motor de base de datos de Azure Cosmos. Un conjunto de réplicas hace que los datos almacenados en la partición física sean duraderos, coherentes y tengan una alta disponibilidad. Cada réplica que compone la partición física hereda la cuota de almacenamiento. Y todas las réplicas de una partición física admiten colectivamente el rendimiento asignado a la partición física. Azure Cosmos DB administra automáticamente los conjuntos de réplicas.

La mayoría de los contenedores pequeños de Cosmos solo necesitarán una única partición física, pero seguirán teniendo al menos cuatro réplicas.

La siguiente imagen muestra cómo se asignan particiones lógicas a particiones físicas distribuidas globalmente:

![Una imagen que muestra las particiones de Azure Cosmos DB](./media/partition-data/logical-partitions.png)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre la [elección de una clave de partición](partitioning-overview.md#choose-partitionkey).
* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md).
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).
