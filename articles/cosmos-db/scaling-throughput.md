---
title: Escalado del rendimiento para contenedores de Azure Cosmos DB
description: En este artículo se describe cómo escala Azure Cosmos DB el rendimiento en las distintas regiones en las que se aprovisiona la cuenta de Azure Cosmos.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: 94b5b3d2ab1f576f87ead23b389252ec96a20f11
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91397358"
---
# <a name="how-does-azure-cosmos-db-globally-scale-the-provisioned-throughput"></a>¿Cómo escala globalmente Azure Cosmos DB el rendimiento aprovisionado?

En Azure Cosmos DB, el rendimiento aprovisionado se representa como unidades de solicitud por segundo (RU/s). RU mide el coste de las operaciones de lectura y escritura en el contenedor de Cosmos, tal como se muestra en la imagen siguientes:

:::image type="content" source="./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png" alt-text="Unidades de solicitud" border="false":::

Puede aprovisionar RU en un contenedor de Cosmos o en una base de datos de Cosmos. Las RU aprovisionadas en un contenedor están disponibles exclusivamente para las operaciones que se realicen en ese contenedor. Las RU aprovisionadas en una base de datos se comparten entre todos los contenedores de esa base de datos (excepto con los contenedores con RU asignadas exclusivamente).

Para escalar elásticamente el rendimiento aprovisionado, puede aumentar o disminuir el valor de las RU/s aprovisionadas en cualquier momento. Para más información, consulte [Aprovisionamiento del rendimiento](set-throughput.md) y escale elásticamente las bases de datos y los contenedores de Cosmos. Para escalar globalmente el rendimiento, puede agregar o quitar regiones de su cuenta de Cosmos en cualquier momento. Para obtener más información, consulte [Incorporación o eliminación de regiones de una cuenta de base de datos](how-to-manage-database-account.md#addremove-regions-from-your-database-account). En muchos escenarios, es importante asociar varias regiones con una cuenta de Cosmos para lograr una latencia baja y [alta disponibilidad](high-availability.md) en todo el mundo.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>Cómo se distribuye el rendimiento aprovisionado entre las regiones

Si aprovisiona RU de *"R"* en un contenedor de Cosmos (o base de datos), Cosmos DB garantiza que las RU de *"R"* estarán disponibles en *cada* región asociada con su cuenta de Cosmos. Cada vez que agrega una nueva región a la cuenta, Cosmos DB aprovisiona automáticamente RU de *"R"* en la región recién agregada. Se garantiza que las operaciones realizadas en el contenedor de Cosmos obtendrán RU de *"R"* en cada región. No se pueden asignar RU de forma selectiva a una región específica. Las RU aprovisionadas en un contenedor de Cosmos (o base de datos) se aprovisionan en todas las regiones asociadas con la cuenta de Cosmos.

Suponiendo que un contenedor de Cosmos está configurado con RU de *"R"* y hay *"N"* regiones asociadas con la cuenta de Cosmos, entonces:

- Si la cuenta de Cosmos está configurada con una región de escritura única, el número de RU disponibles globalmente en el contenedor = *R* x *N*.

- Si la cuenta de Cosmos está configurada con varias regiones de escritura, el número de RU disponibles globalmente en el contenedor = *R* x (*N*+1). Las RU de *R* adicionales se aprovisionan automáticamente para procesar conflictos de actualización y tráfico de antientropía entre las regiones.

La elección del [modelo de coherencia](consistency-levels.md) también afecta al rendimiento. Aproximadamente, puede obtener el doble de rendimiento de lectura para niveles de coherencia más relajados (por ejemplo, por *sesión*, *prefijo de coherencia* y coherencia *final*), en comparación con niveles de coherencia más altos (como *obsolescencia limitada* o coherencia *alta*).

## <a name="next-steps"></a>Pasos siguientes

A continuación puede aprender a configurar el rendimiento en un contenedor o base de datos:

* [Obtener y configurar rendimiento para contenedores y bases de datos](set-throughput.md) 

