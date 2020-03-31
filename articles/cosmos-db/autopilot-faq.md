---
title: Preguntas más frecuentes sobre el rendimiento del modo Autopilot de Azure Cosmos DB
description: Preguntas más frecuentes sobre el modo Autopilot para bases de datos y contenedores de Azure Cosmos DB
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: d2ad585cabefb9ea78b88e748e422c745c11d03f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75476010"
---
# <a name="frequently-asked-questions-about-provisioned-throughput-in-azure-cosmos-db-autopilot-mode-preview"></a>Preguntas más frecuentes sobre el rendimiento aprovisionado en el modo Autopilot de Azure Cosmos DB (versión preliminar)
Con el modo Autopilot, Azure Cosmos DB administrará y escalará automáticamente las RU por segundo del contenedor o base de datos en función del uso. En este artículo se responden las preguntas más frecuentes sobre el modo Autopilot. 

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="is-autopilot-mode-supported-for-all-apis"></a>¿El modo Autopilot se admite para todas las API?
Sí, el modo Autopilot se admite para todas las API: Core (SQL), Gremlin, Table, Cassandra y API para MongoDB.

### <a name="is-autopilot-mode-supported-for-multi-master-accounts"></a>¿Se admite el modo Autopilot para cuentas de arquitectura multimaestro?
Sí, se admite el modo Autopilot para cuentas de arquitectura multimaestro. Las RU por segundo máximas están disponible en cada región que se agrega a la cuenta de Cosmos. 

### <a name="what-is-the-pricing-for-autopilot"></a>¿Cuál es el precio del modo Autopilot?
Para más información, consulte la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/) de Azure Cosmos DB. 

### <a name="how-do-i-enable-autopilot-for-my-containers-or-databases"></a>¿Cómo habilito Autopilot para mis contenedores o bases de datos?
El modo Autopilot se puede habilitar en los nuevos contenedores y bases de datos creados mediante Azure Portal. 

### <a name="is-there-cli-or-sdk-support-to-create-containers-or-databases-with-autopilot-mode"></a>¿La CLI o el SDK admiten la creación contenedores o bases de datos con el modo Autopilot?
Actualmente, en la versión preliminar, solo se pueden crear recursos con el modo Autopilot desde Azure Portal. La compatibilidad con la CLI y el SDK todavía no está disponible.

### <a name="can-i-enable-autopilot-on-an-existing-container-or-a-database"></a>¿Se puede habilitar el modo Autopilot en una base de datos o un contenedor existente?
Actualmente, puede habilitar Autopilot en nuevos contenedores y bases de datos al crearlos. Todavía no está disponible la compatibilidad para habilitar el modo Autopilot en contenedores y bases de datos existentes. Puede migrar los contenedores existentes a un nuevo contenedor mediante [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) o una [fuente de cambios](change-feed.md). 

### <a name="can-i-turn-off-autopilot-mode-on-a-container-or-database"></a>¿Se puede desactivar el modo Autopilot en un contenedor o base de datos?
Sí, puede desactivar Autopilot cambiando a la opción "Manual" en el rendimiento aprovisionado. En la versión preliminar, después de cambiar del modo Autopilot al modo manual, ya no podrá habilitar Autopilot de nuevo para el mismo recurso. 

### <a name="is-autopilot-mode-supported-for-shared-throughput-databases"></a>¿Se admite el modo Autopilot en las bases de datos de rendimiento compartido?
Sí, el modo Autopilot se admite en las bases de datos de rendimiento compartido. Para habilitar esta característica, seleccione el modo Autopilot y la opción **Aprovisionar rendimiento** al crear la base de datos. 

### <a name="what-is-the-number-of-allowed-collections-per-shared-throughput-database-when-autopilot-is-enabled"></a>¿Cuál es el número de colecciones permitidas por base de datos de rendimiento compartido cuando está habilitado el modo Autopilot?
En el caso de las bases de datos de rendimiento compartido cuando está habilitado el modo Autopilot, el número de colecciones permitidas es: MIN(25, RU por segundo máximas de base de datos/1000). Por ejemplo, si el rendimiento máximo de la base de datos es 20 000 RU por segundo, la base de datos puede tener MIN(25, 20 000 RU por segundo/1000) = 20 colecciones. 


### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>¿Cuál es el límite de almacenamiento asociado a cada opción de RU por segundo máximas?  
El límite de almacenamiento en GB por cada número máximo de RU/s es: Número máximo de RU/s para una base de datos o contenedor/100. Por ejemplo, si las RU por segundo máximas son 20 000 RU por segundo, el recurso puede admitir 200 GB de almacenamiento. Consulte el artículo sobre los [límites de Autopilot](provision-throughput-autopilot.md#autopilot-limits) para ver las opciones de almacenamiento y RU por segundo máximas disponibles. 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>¿Qué ocurre si se supera el límite de almacenamiento asociado al rendimiento máximo?
Si se supera el límite de almacenamiento asociado al rendimiento máximo de la base de datos o el contenedor, Azure Cosmos DB aumentará automáticamente el rendimiento máximo al siguiente nivel más alto que pueda admitir ese nivel de almacenamiento. Por ejemplo, supongamos que se ha aprovisionado una base de datos o un contenedor con la opción de rendimiento de 4000 RU por segundo máximas, que tiene un límite de almacenamiento de 50 GB. Si el almacenamiento del recurso aumenta hasta 100 GB, las RU por segundo máximas de la base de datos o del contenedor se incrementará automáticamente a 20 000 RU por segundo, lo que puede admitir hasta 200 GB. 

### <a name="how-quickly-will-autopilot-scale-up-and-down-based-on-spikes-in-traffic"></a>¿Con qué rapidez se escalará y reducirá verticalmente el modo Autopilot en función de los picos de tráfico?
Autopilot realizará el escalado o reducción vertical de las RU por segundo de forma instantánea en el intervalo de RU por segundo mínimas y máximas, en función del tráfico entrante. La facturación se realiza en una granularidad de una hora, donde se le cobra por las RU por segundo más altas en una hora determinada. 

### <a name="can-i-specify-a-custom-max-throughput-rus-value-for-autopilot-mode"></a>¿Se puede especificar un valor de rendimiento máximo personalizado (RU por segundo) para el modo Autopilot?
Actualmente, durante la versión preliminar, puede seleccionar entre [cuatro opciones](provision-throughput-autopilot.md#autopilot-limits) para el rendimiento máximo (RU por segundo).

### <a name="can-i-increase-the-max-rus-move-to-a-higher-tier-on-the-database-or-container"></a>¿Puedo aumentar las RU por segundo máximas (trasladar a un nivel superior) en la base de datos o el contenedor? 
Sí. En la opción **Scale & Settings** (Escala y configuración) para el contenedor o la opción **Scale** (Escala) para la base de datos, puede seleccionar un valor de RU por segundo máximas superior para Autopilot. Se trata de una operación de escalado vertical asincrónica que puede tardar unos minutos en completarse (normalmente entre 4 y 6 horas, según las RU por segundo seleccionadas), ya que el servicio aprovisiona más recursos para admitir la escala más alta. 

### <a name="can-i-reduce-the-max-rus-move-to-a-lower-tier-on-the-database-or-container"></a>¿Puedo reducir las RU por segundo máximas (trasladar a un nivel inferior) en la base de datos o el contenedor?
Sí. Siempre que el almacenamiento actual de la base de datos o del contenedor esté por debajo del [límite de almacenamiento](#what-is-the-storage-limit-associated-with-each-max-rus-option) asociado a las RU por segundo máximas que desea reducir verticalmente, puede reducir las RU por segundo máximas a ese nivel. Por ejemplo, si ha seleccionado 20 000 RU por segundo como las RU por segundo máximas, puede reducir verticalmente el máximo de RU por segundo a 4000 RU por segundo si tiene menos de 50 GB de almacenamiento (el límite de almacenamiento asociado con 4000 RU por segundo).

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>¿Cuál es la asignación entre el número máximo de RU por segundo y las particiones físicas?
La primera vez que se seleccionan las RU por segundo máximas, Azure Cosmos DB aprovisionará: RU/s máximas/10 000 RU/s = número de particiones físicas. Cada [partición física](partition-data.md#physical-partitions) puede admitir hasta 10 000 RU por segundo y un almacenamiento de 50 GB. A medida que aumenta el tamaño del almacenamiento, Azure Cosmos DB dividirá automáticamente las particiones para agregar más particiones físicas con el fin de controlar el aumento del almacenamiento, o bien para aumentar el nivel de las RU por segundo máximas si el almacenamiento [supera el límite asociado](#what-is-the-storage-limit-associated-with-each-max-rus-option). 

Las RU por segundo máximas de la base de datos o el contenedor se dividen uniformemente entre todas las particiones físicas. Por lo tanto, el rendimiento total de una sola partición física que se puede escalar es: RU por segundo máximas para una base de datos o contenedor divididas por el número de particiones físicas. 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>¿Qué ocurre si las solicitudes entrantes superan las RU por segundo máximas de la base de datos o el contenedor?
Si el total de RU por segundo consumidas supera las RU por segundo máximas del contenedor o la base de datos, las solicitudes que superen las RU por segundo máximas se limitarán y devolverán un código de estado 429. Las solicitudes que dan lugar a más de un 100 % de la utilización normalizada también se limitarán. La utilización normalizada se define como el máximo del uso de las RU por segundo en todas las particiones físicas. Por ejemplo, supongamos que el rendimiento máximo es 20 000 RU por segundo y tiene dos particiones físicas, P_1 y P_2, cada una con capacidad de escalado a 10 000 RU por segundo. En un segundo determinado, si P_1 ha usado 6000 RU y P_2 ha usado 8000 RU, la utilización normalizada es MAX(6000 RU/10 000 RU, 8000 RU/10 000 RU) = 0,8.

> [!NOTE]
> Los SDK de cliente de Azure Cosmos DB y las herramientas de importación de datos (Azure Data Factory, la biblioteca de ejecutores en masa) reintentan automáticamente en el código de estado 429, por lo que estos códigos de estado ocasionales son correctos. Un gran número sostenido de códigos de estado 429 puede indicar que necesita aumentar las RU por segundo máximas o revisar la estrategia de partición para una [partición frecuente](#is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autopilot-is-enabled).

### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autopilot-is-enabled"></a>¿Sigue siendo posible ver códigos de estado 429 (limitación/limitación de velocidad) cuando Autopilot está habilitado? 
Sí. Es posible ver códigos de estado 429 en dos escenarios. En primer lugar, cuando el total de las RU por segundo consumidas supera el número máximo de las RU por segundo del contenedor o la base de datos, el servicio limitará las solicitudes en consecuencia. 

En segundo lugar, si hay una partición frecuente, es decir, un valor de clave de partición lógica que tiene una cantidad de solicitudes desproporcionada en comparación con otros valores de la clave de partición, es posible que la partición física subyacente supere el presupuesto de las RU por segundo. Como procedimiento recomendado, para evitar las particiones frecuentes, [elija una buena clave de partición](partitioning-overview.md#choose-partitionkey) que dé lugar a una distribución uniforme tanto del almacenamiento como del rendimiento. 

Por ejemplo, si selecciona la opción de rendimiento de 20 000 RU por segundo máximas y tiene 200 GB de almacenamiento, con cuatro particiones físicas, cada partición física se puede escalar automáticamente hasta 5000 RU por segundo. Si había una partición frecuente en una clave de partición lógica determinada, verá varios códigos de estado 429 cuando la partición física subyacente en la que reside supera las 5000 RU por segundo, es decir, supera el 100 % de la utilización normalizada.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [habilitar Autopilot en un contenedor o base de datos de Azure Cosmos](provision-throughput-autopilot.md#create-a-database-or-a-container-with-autopilot-mode).
* Más información sobre las [ventajas del modo Autopilot](provision-throughput-autopilot.md#benefits-of-autopilot-mode).
* Más información sobre las [particiones lógicas y físicas](partition-data.md).
