---
title: Creación de particiones en Azure Cosmos DB
description: Obtenga información sobre la creación de particiones en Azure Cosmos DB, los procedimientos recomendados al elegir una clave de partición y cómo administrar particiones lógicas.
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2020
ms.openlocfilehash: a9368e67abf3c45981cf1f85fe46a2a2799a6877
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82864341"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Creación de particiones en Azure Cosmos DB

Azure Cosmos DB usa la creación de particiones con el fin de escalar contenedores individuales en una base de datos para satisfacer las necesidades de rendimiento de la aplicación. En la creación de particiones, los elementos de un contenedor se dividen en distintos subconjuntos, que se llaman *particiones lógicas*. Las particiones lógicas se crean en función del valor de una *clave de partición* que está asociada con cada elemento de un contenedor. Todos los elementos de una partición lógica tienen el mismo valor de clave de partición.

Por ejemplo, un contenedor contiene elementos. Cada elemento tiene un valor único para la propiedad `UserID`. Si `UserID` actúa como la partición de clave para los elementos de un contenedor y hay 1000 valores `UserID` exclusivos, se crearán 1000 particiones lógicas del contenedor.

Además de una clave de partición que determina la partición lógica del elemento, cada elemento de un contenedor tiene también un *id. de elemento* (que es único dentro de una partición lógica). Al combinar la clave de partición y el *id. del elemento* se crea el *índice* del elemento, que los identifica de forma única.

[Elegir una clave de partición](partitioning-overview.md#choose-partitionkey) es una decisión importante que afecta al rendimiento de la aplicación.

## <a name="managing-logical-partitions"></a>Administración de particiones lógicas

Azure Cosmos DB administra de forma transparente y automática la ubicación de las particiones lógicas en particiones físicas para satisfacer de manera eficaz las necesidades de escalabilidad y rendimiento del contenedor. A medida que aumentan los requisitos de rendimiento y almacenamiento de la aplicación, Azure Cosmos DB mueve las particiones lógicas para distribuir automáticamente la carga entre un número más elevado de particiones físicas. Puede obtener más información sobre las [particiones físicas](partition-data.md#physical-partitions).

Azure Cosmos DB usa la creación de particiones por hash para distribuir las particiones lógicas entre las particiones físicas. Azure Cosmos DB aplica un algoritmo hash al valor de clave de partición de un elemento. El resultado con hash determina la partición física. A continuación, Azure Cosmos DB asigna el espacio de claves de los hash de claves de partición uniformemente entre las particiones físicas.

Las transacciones (en procedimientos almacenados o desencadenadores) solo se permiten con elementos dentro de una única partición lógica.

Puede obtener más información sobre [cómo Azure Cosmos DB administra las particiones](partition-data.md). No es necesario conocer los detalles internos para compilar o ejecutar las aplicaciones, pero se recopilan aquí para que pueda consultarlos.

## <a name="choosing-a-partition-key"></a><a id="choose-partitionkey"></a>Elegir una clave de partición

La selección de la clave de partición es una decisión de diseño sencilla pero importante en Azure Cosmos DB. Una vez que haya seleccionado la clave de partición, no es posible cambiarla en contexto. Si necesita cambiarla, debe trasladar los datos a un nuevo contenedor con la nueva clave de partición deseada.

Para **todos** los contenedores, la clave de partición debe:

* Ser una propiedad con un valor que no cambie. Si una propiedad es la clave de partición, no se puede actualizar su valor.
* Tener una cardinalidad alta. En otras palabras, la propiedad debe tener una amplia gama de valores posibles.
* Distribuir el consumo de unidades de solicitud (RU) y el almacenamiento de datos uniformemente en todas las particiones lógicas. Esto garantiza una distribución uniforme del consumo de RU y el almacenamiento en las particiones físicas.

Si necesita [transacciones ACID de varios elementos](database-transactions-optimistic-concurrency.md#multi-item-transactions) en Azure Cosmos DB, deberá usar [procedimientos almacenados o desencadenadores](how-to-write-stored-procedures-triggers-udfs.md#stored-procedures). Todos los procedimientos almacenados y desencadenadores basados en JavaScript están limitados a una única partición lógica.

## <a name="partition-keys-for-read-heavy-containers"></a>Claves de partición para contenedores con lecturas frecuentes

Para la mayoría de los contenedores, los criterios anteriores se deben tener en cuenta al elegir una clave de partición. No obstante, en el caso de los contenedores con lecturas frecuentes, es posible que quiera elegir una clave de partición que aparece con frecuencia como filtro en las consultas. Las consultas se pueden [enrutar de manera eficaz solo a las particiones físicas pertinentes](how-to-query-container.md#in-partition-query) si se incluye la clave de partición en el predicado de filtro.

Si la mayoría de las solicitudes de la carga de trabajo son consultas, y la mayoría de las consultas tiene un filtro de igualdad en la misma propiedad, esta puede ser una buena opción de clave de partición. Por ejemplo, si ejecuta frecuentemente una consulta que filtra por `UserID`, al seleccionar `UserID` como clave de partición se reduciría el número de [consultas entre particiones](how-to-query-container.md#avoiding-cross-partition-queries).

Sin embargo, si el contenedor es pequeño, probablemente no tenga suficientes particiones físicas como para que deba preocuparle el impacto de las consultas entre particiones en el rendimiento. La mayoría de los contenedores pequeños en Azure Cosmos DB requieren solo una o dos particiones físicas.

Si el contenedor puede aumentar a unas cuantas particiones físicas, debe asegurarse de elegir una clave de partición que minimice las consultas entre particiones. El contenedor necesitará más de unas cuantas particiones físicas cuando se cumpla alguna de las siguientes condiciones:

* El contenedor tiene aprovisionadas más de 30 000 RU.
* El contenedor almacenará más de 100 GB de datos.

## <a name="using-item-id-as-the-partition-key"></a>Uso del id. de elemento como clave de partición

Si el contenedor tiene una propiedad con una amplia gama de posibles valores, probablemente sea una buena elección de clave de partición. Un posible ejemplo de una propiedad de este tipo, es el *id. de elemento*. En el caso de contenedores pequeños de lectura o escritura frecuente de cualquier tamaño, el *id. de elemento* es naturalmente una excelente opción de clave de partición.

Se garantiza que la propiedad del sistema *id. de elemento* existe en todos los elementos del contenedor de Cosmos. Es posible que tenga otras propiedades que representen un identificador lógico para el elemento. En muchos casos, también son excelentes claves de partición por los mismos motivos que el *id. de elemento*.

El *id. de elemento* es una excelente opción de clave de partición por los siguientes motivos:

* Hay una amplia gama de valores posibles (un *id. de elemento* único por elemento).
* Dado que cada elemento tiene un *id.* único, este *id. de elemento* realiza un trabajo excelente para equilibrar de manera uniforme el consumo de RU y el almacenamiento de datos.
* Puede realizar fácilmente lecturas puntuales, ya que siempre conocerá la clave de partición de un elemento si conoce su *identificador*.

Algunos aspectos que se deben tener en cuenta al seleccionar el *id. de elemento* como clave de partición son:

* Si el *id. de elemento* es la clave de partición, se convertirá en un identificador único en todo el contenedor. No podrá tener elementos con *id. de elemento* duplicados.
* Si tiene un contenedor con lecturas frecuentes que tiene muchas [particiones físicas](partition-data.md#physical-partitions), las consultas serán más eficaces si tienen un filtro de igualdad con el *id. de elemento*.
* No se pueden ejecutar procedimientos almacenados ni desencadenadores en varias particiones lógicas.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear particiones y escalado horizontal en Azure Cosmos DB](partition-data.md).
* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md).
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md).
