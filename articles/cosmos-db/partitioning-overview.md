---
title: Creación de particiones en Azure Cosmos DB
description: Obtenga información sobre la creación de particiones en Azure Cosmos DB, los procedimientos recomendados al elegir una clave de partición y cómo administrar particiones lógicas.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 551703b5dcca082904197010366ee059998dde4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79227284"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Creación de particiones en Azure Cosmos DB

Azure Cosmos DB usa la creación de particiones con el fin de escalar contenedores individuales en una base de datos para satisfacer las necesidades de rendimiento de la aplicación. En la creación de particiones, los elementos de un contenedor se dividen en distintos subconjuntos, que se llaman *particiones lógicas*. Las particiones lógicas se crean en función del valor de una *clave de partición* que está asociada con cada elemento de un contenedor. Todos los elementos de una partición lógica tienen el mismo valor de clave de partición.

Por ejemplo, un contenedor contiene elementos. Cada elemento tiene un valor único para la propiedad `UserID`. Si `UserID` actúa como la partición de clave para los elementos de un contenedor y hay 1000 valores `UserID` exclusivos, se crearán 1000 particiones lógicas del contenedor.

Además de una clave de partición que determina la partición lógica del elemento, cada elemento de un contenedor tiene también un *identificador de elemento* (que es único dentro de una partición lógica). Al combinar la clave de partición y el identificador del elemento crea el *índice* del elemento, que identifica de forma única el elemento.

[Elegir una clave de partición](partitioning-overview.md#choose-partitionkey) es una decisión importante que afecta al rendimiento de la aplicación.

## <a name="managing-logical-partitions"></a>Administración de particiones lógicas

Azure Cosmos DB administra de forma transparente y automática la ubicación de las particiones lógicas en particiones físicas para satisfacer de manera eficaz las necesidades de escalabilidad y rendimiento del contenedor. A medida que aumentan los requisitos de rendimiento y almacenamiento de la aplicación, Azure Cosmos DB mueve las particiones lógicas para distribuir automáticamente la carga entre un número más elevado de servidores. 

Azure Cosmos DB usa la creación de particiones por hash para distribuir las particiones lógicas entre las particiones físicas. Azure Cosmos DB aplica un algoritmo hash al valor de clave de partición de un elemento. El resultado con hash determina la partición física. A continuación, Azure Cosmos DB asigna el espacio de claves de los hash de claves de partición uniformemente entre las particiones físicas.

Las consultas que tienen acceso a datos dentro de una sola partición lógica son más rentables que las consultas que tienen acceso a varias particiones. Las transacciones (en procedimientos almacenados o desencadenadores) solo se permiten con elementos dentro de una única partición lógica.

Para obtener más información sobre cómo Azure Cosmos DB administra las particiones, vea el artículo de [Particiones lógicas](partition-data.md). No es necesario conocer los detalles internos para compilar o ejecutar las aplicaciones, pero se recopilan aquí para que pueda consultarlos.

## <a name="choosing-a-partition-key"></a><a id="choose-partitionkey"></a>Elegir una clave de partición

A continuación, encontrará una guía clara para elegir una clave de partición:

* En una única partición lógica se admite un límite máximo de 20 GB de almacenamiento.  

* Los contenedores de Azure Cosmos tienen un rendimiento mínimo de 400 unidades de solicitud por segundo (RU/s). Cuando el rendimiento se aprovisiona en una base de datos, el valor mínimo de RU por contenedor es de cien unidades de solicitud por segundo (RU/s). Las solicitudes a la misma clave de partición no pueden exceder el rendimiento asignado a una partición. Si las solicitudes superan el rendimiento asignado, tendrá la velocidad limitada. Por tanto, es importante elegir una clave de partición que no dé como resultado "zonas activas" dentro de la aplicación.

* Elija una clave de partición que tenga una amplia gama de valores y patrones de acceso que se distribuyan uniformemente entre las particiones lógicas. Esto ayuda a distribuir los datos y la actividad en el contenedor entre el conjunto de particiones lógicas, para que los recursos de rendimiento y almacenamiento de datos se puedan distribuir entre las particiones lógicas.

* Elija una clave de partición que distribuya la carga de trabajo de manera uniforme entre todas las particiones y también a lo largo del tiempo. La elección de la clave de partición debe equilibrar la necesidad de consultas o transacciones de partición eficaces con el objetivo de distribuir los elementos entre varias particiones para conseguir escalabilidad.

* Los candidatos para las claves de partición pueden incluir las propiedades que aparecen con frecuencia como un filtro en las consultas. Las consultas se pueden enrutar de manera eficaz si se incluye la clave de partición en el predicador de filtro.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear particiones y escalado horizontal en Azure Cosmos DB](partition-data.md).
* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md).
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md).
