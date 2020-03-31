---
title: Creación de particiones y escalado horizontal en Azure Cosmos DB
description: Obtenga información sobre cómo funciona la creación de particiones en Azure Cosmos DB, cómo configurar la creación de particiones y las claves de partición y cómo seleccionar la clave de partición correcta para su aplicación.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: cbd171e10cc1a8b27de98d9d4d779f345ac5a3ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225668"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Creación de particiones y escalado horizontal en Azure Cosmos DB

En este artículo se explican las particiones físicas y lógicas en Azure Cosmos DB. También se describen los procedimientos recomendados de escalado y creación de particiones. 

## <a name="logical-partitions"></a>Particiones lógicas

Una partición lógica consta de un conjunto de elementos con la misma clave de partición. Por ejemplo, en un contenedor donde todos los elementos contienen una propiedad `City`, puede usar `City` como clave de partición para el contenedor. Los grupos de elementos que tienen valores específicos para `City`, tales como `London`, `Paris` y `NYC`, forman distintas particiones lógicas. No tiene que preocuparse acerca de cómo eliminar una partición cuando se eliminan los datos subyacentes.

En Azure Cosmos DB, un contenedor es la unidad fundamental de escalabilidad. Se crean automáticamente particiones (horizontales) de los datos agregados al contenedor y el rendimiento que se aprovisiona en el contenedor, a través de un conjunto de particiones lógicas. Los datos y el rendimiento tienen particiones que se crean en función de la clave de partición que se especifica en el contenedor de Azure Cosmos. Para obtener más información, consulte [Creación de un contenedor de Azure Cosmos](how-to-create-container.md).

Una partición lógica también define el ámbito de las transacciones de base de datos. Puede actualizar los elementos dentro de una partición lógica mediante el uso de una [transacción con aislamiento de instantánea](database-transactions-optimistic-concurrency.md). Cuando se agregan nuevos elementos al contenedor, se crean nuevas particiones lógicas de forma transparente por el sistema.

## <a name="physical-partitions"></a>Particiones físicas

Un contenedor de Azure Cosmos se escala mediante la distribución de los datos y el rendimiento entre un gran número de particiones lógicas. Internamente, una o varias particiones lógicas se asignan a una partición física que consta de un conjunto de réplicas que se conoce como [*conjunto de réplicas*](global-dist-under-the-hood.md). Cada conjunto de réplicas hospeda una instancia del motor de base de datos de Azure Cosmos. Un conjunto de réplicas hace que los datos almacenados en la partición física sean duraderos, coherentes y tengan una alta disponibilidad. Una partición física admite una cantidad máxima de almacenamiento y unidades de solicitud (RU). Cada réplica que compone la partición física hereda la cuota de almacenamiento. Y todas las réplicas de una partición física admiten colectivamente el rendimiento asignado a la partición física. 

La siguiente imagen muestra cómo se asignan particiones lógicas a particiones físicas distribuidas globalmente:

![Una imagen que muestra las particiones de Azure Cosmos DB](./media/partition-data/logical-partitions.png)

El rendimiento aprovisionado para un contenedor se divide uniformemente entre las particiones físicas. Un diseño de clave de partición que no distribuye uniformemente las solicitudes de rendimiento puede crear particiones "activas". Las particiones activas pueden conllevar un uso ineficaz y que limite la velocidad del rendimiento aprovisionado y costos más elevados.

A diferencia de las particiones lógicas, las particiones físicas son una implementación interna del sistema. No se puede controlar el tamaño, la ubicación o el número de particiones físicas, ni tampoco la asignación entre las particiones lógicas y las particiones físicas. Sin embargo, puede controlar el número de particiones lógicas, así como la distribución de datos, la carga de trabajo y el rendimiento mediante la elección de la [clave de partición lógica correcta](partitioning-overview.md#choose-partitionkey).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre la [elección de una clave de partición](partitioning-overview.md#choose-partitionkey).
* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md).
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).
