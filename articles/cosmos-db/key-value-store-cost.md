---
title: Cargos de unidades de solicitud para Azure Cosmos DB como almacén de pares de clave y valor
description: Obtenga información sobre los cargos de unidades de solicitud de Azure Cosmos DB para operaciones simples de escritura y lectura cuando se utiliza como un almacén de pares de clave y valor.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 1cd6b4b52db224db5febcec1eff79b01379a5956
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85262827"
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure Cosmos DB como almacén de pares valor-clave: Información general de costos

Azure Cosmos DB es un servicio de base de datos multimodelo globalmente distribuido para crear aplicaciones de alta disponibilidad y a gran escala fácilmente. De forma predeterminada, Azure Cosmos DB indexa automática y eficazmente todos los datos que ingiere. Esto permite consultas [SQL](how-to-sql-query.md) (y [JavaScript](stored-procedures-triggers-udfs.md)) rápidas y coherentes en los datos. 

En este artículo se describe el costo de Azure Cosmos DB para operaciones simples de escritura y lectura cuando se utiliza como un almacén de pares valor-clave. Las operaciones de escritura incluyen inserciones, reemplazos, eliminaciones y upserts de elementos de datos. Además de garantizar un Acuerdo de Nivel de Servicio de disponibilidad del 99,999 % para todas las cuentas de varias regiones, Azure Cosmos DB ofrece una latencia garantizada de < 10 ms para lecturas y para escrituras (indexadas) en el percentil 99. 

## <a name="why-we-use-request-units-rus"></a>¿Por qué usamos unidades de solicitud (RU)?

El rendimiento de Azure Cosmos DB se basa en la cantidad de rendimiento aprovisionado expresado en las [unidades de solicitud](request-units.md) (RU/s). El aprovisionamiento tiene una granularidad de un segundo y se adquiere en RU/s ([no se debe confundir con la facturación horaria](https://azure.microsoft.com/pricing/details/cosmos-db/)). Las RU deben considerarse una abstracción lógica (una moneda) que simplifica el aprovisionamiento de rendimiento necesario para la aplicación. Los usuarios no tienen que pensar en diferenciar entre rendimiento de lectura y escritura. El modelo de moneda única de RU crea eficiencias para compartir la capacidad aprovisionada entre lecturas y escrituras. Este modelo de capacidad aprovisionada permite que el servicio proporcione un **rendimiento predecible y coherente, baja latencia garantizada y alta disponibilidad**. Por último, mientras que el modelo de RU se usa para representar el rendimiento, cada RU aprovisionada también tiene una cantidad definida de recursos (p. ej., memoria, núcleos/CPU e IOPS).

Como un sistema de base de datos distribuido globalmente, Cosmos DB es el único servicio de Azure que proporciona un Acuerdo de Nivel de Servicio completo que abarca la latencia, el rendimiento, la coherencia y la alta disponibilidad. El rendimiento que se aprovisiona se aplica a cada una de las regiones asociadas a su cuenta de Cosmos. Para lecturas, Cosmos DB ofrece varios [niveles de coherencia](consistency-levels.md) bien definidos entre los que elegir. 

En la tabla siguiente se muestra el número de RU necesarias para realizar operaciones de lectura y escritura basadas en un elemento de datos con un tamaño de 1 KB y 100 KB con la indexación automática predeterminada desactivada. 

|Tamaño del elemento|1 lectura|1 escritura|
|-------------|------|-------|
|1 KB|1 RU|5 RU|
|100 KB|10 RU|50 RU|

## <a name="cost-of-reads-and-writes"></a>Costo de lecturas y escrituras

Si aprovisiona 1000 RU/s, esto asciende a 3,6 millones de RU/hora y costará 0,08 USD la hora (en Estados Unidos y Europa). Para un elemento de datos de 1 KB de tamaño, esto significa que puede consumir 3,6 millones de lecturas o 0,72 millones de escrituras (3,6 millones de RU / 5) con el rendimiento aprovisionado. Normalizado a millones de lecturas y escrituras, el costo sería de 0,022 USD/millones de lecturas (0,08 USD/3,6) y 0,111/millones de escrituras (0,08 USD/0,72). El costo por millón pasa a ser mínimo tal como se muestra en la tabla siguiente.

|Tamaño del elemento|Costo de 1 millón de lecturas|Costo de 1 millón de escrituras|
|-------------|-------|--------|
|1 KB|0,022 USD|0,111 USD|
|100 KB|0,222 USD|1,111 USD|


La mayoría de los almacenes de blobs u objetos básicos cobran 0,40 USD por cada millón de transacciones de lectura y 5 USD por cada millón de transacciones de escritura. Si se usa de forma óptima, Cosmos DB puede ser hasta un 98 % más económico que estas otras soluciones (para transacciones de 1 KB).

## <a name="next-steps"></a>Pasos siguientes

* Use la [calculadora de RU](https://cosmos.azure.com/capacitycalculator/) para calcular el rendimiento de las cargas de trabajo.

