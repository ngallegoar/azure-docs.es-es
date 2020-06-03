---
title: Preguntas más frecuentes sobre el rendimiento aprovisionado de escalabilidad automática en Azure Cosmos DB
description: Preguntas más frecuentes sobre el rendimiento aprovisionado por Escalabilidad automática para bases de datos y contenedores de Azure Cosmos DB
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/10/2020
ms.openlocfilehash: b398f739189232f39a2fee06fc6e6ff0d53348f0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83656618"
---
# <a name="frequently-asked-questions-about-autoscale-provisioned-throughput-in-azure-cosmos-db"></a>Preguntas más frecuentes sobre el rendimiento aprovisionado por Escalabilidad automática en Azure Cosmos DB

Con el rendimiento aprovisionado de escalabilidad automática, Azure Cosmos DB administrará y escalará automáticamente las RU/s de la base de datos o el contenedor en función del uso. En este artículo se responden las preguntas más frecuentes sobre la escalabilidad automática.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="what-is-the-difference-between-autopilot-and-autoscale-in-azure-cosmos-db"></a>¿Cuál es la diferencia entre "Autopilot" y "escalabilidad automática" en Azure Cosmos DB?
"Escalabilidad automática" o "rendimiento aprovisionado de escalabilidad automática" es el nombre actualizado de la característica, conocida anteriormente como "Autopilot". Con la versión actual de la escalabilidad automática, hemos agregado nuevas características, incluida la capacidad de establecer un número máximo de RU/s personalizado y la compatibilidad con acciones realizadas mediante programación. 

### <a name="what-happens-to-databases-or-containers-created-in-the-previous-autopilot-tier-model"></a>¿Qué ocurre con las bases de datos o los contenedores creados en el modelo del nivel de Autopilot anterior?
Los recursos que se crearon con el modelo del nivel anterior se admiten automáticamente con el nuevo modelo de RU/s personalizadas máximas de escalabilidad automática. El límite superior del nivel se convierte en las nuevas RU/s máximas, lo que da como resultado el mismo intervalo de escala. 

Por ejemplo, si anteriormente seleccionó el nivel que escalaba entre 400 y 4000 RU/s, la base de datos o el contenedor ahora mostrarán que tienen un máximo de 4000 RU/s, que se escalan entre 400 y 4000 RU/s. A partir de aquí, puede cambiar el número máximo de RU/s a un valor personalizado apropiado para su carga de trabajo. 

### <a name="how-quickly-will-autoscale-scale-up-and-down-based-on-spikes-in-traffic"></a>¿Con qué rapidez se escalará y reducirá verticalmente la escalabilidad automática en función de los picos de tráfico?
Con la escalabilidad automática, el sistema escala el rendimiento (RU/s) `T` en el intervalo entre `0.1 * Tmax` y `Tmax`, en función del tráfico entrante. Dado que el escalado es automático e instantáneo, puede consumir hasta el valor de `Tmax` aprovisionado en cualquier momento, sin ningún retraso. 

### <a name="how-do-i-determine-what-rus-the-system-is-currently-scaled-to"></a>¿Cómo puedo determinar a qué número de RU/s está escalado actualmente el sistema?
Use las [métricas de Azure Monitor](how-to-choose-offer.md#measure-and-monitor-your-usage) para supervisar el tamaño máximo de RU/s de escalabilidad automática aprovisionado y el rendimiento actual (RU/s) al que se escala el sistema. 

### <a name="what-is-the-pricing-for-autoscale"></a>¿Cuál es el precio de la escalabilidad automática?
Cada hora, se le facturará el mayor rendimiento `T` al que se escaló el sistema en dicha hora. Si el recurso no tiene ninguna solicitud durante dicha hora o no se escaló más allá de `0.1 * Tmax`, se le facturará por el mínimo de `0.1 * Tmax`. Para más información, consulte la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/) de Azure Cosmos DB. 

### <a name="how-does-autoscale-show-up-on-my-bill"></a>¿Cómo se muestra la escalabilidad automática en la factura?
En las cuentas con un solo maestro, la tasa de escalabilidad automática por 100 RU/s es 1,5 veces la tasa del rendimiento aprovisionado estándar (manual). En la factura, verá el medidor de rendimiento aprovisionado estándar existente. La cantidad de este medidor se multiplicará por 1,5. Por ejemplo, si el número mayor de RU/s al que se escaló el sistema en una hora fue de 6000 RU/s, se le facturarán 60 * 1,5 = 90 unidades del medidor durante esa hora.

En cuentas con varios maestros, la tasa de escalabilidad automática por 100 RU/s es la misma que la tasa del rendimiento aprovisionado estándar (manual) de varios maestros. En la factura, verá el medidor de varios maestros existente. Dado que las tarifas son las mismas, si usa la escalabilidad automática, verá la misma cantidad que con el rendimiento estándar.

### <a name="does-autoscale-work-with-reserved-capacity"></a>¿Funciona la escalabilidad automática con capacidad reservada?
Sí. Al adquirir capacidad reservada con un solo maestro, el descuento por reserva de los recursos de escalabilidad automática se aplica al uso del medidor con una proporción de 1,5 * la [proporción de la región específica](../cost-management-billing/reservations/understand-cosmosdb-reservation-charges.md#reservation-discount-per-region). 

La capacidad reservada con varios maestros funciona del mismo modo para la escalabilidad automática y el rendimiento aprovisionado estándar (manual). Consulte [Capacidad reservada de Azure Cosmos DB](cosmos-db-reserved-capacity.md).

### <a name="does-autoscale-work-with-free-tier"></a>¿Funciona la escalabilidad automática con el nivel Gratis?
Sí. En el nivel Gratis, puede usar el rendimiento de escalabilidad automática en un contenedor. La compatibilidad con las bases de datos de rendimiento compartido de escalabilidad automática con el número máximo de RU/s personalizado todavía no está disponible. Consulte [cómo funciona la facturación del nivel Gratis con la escalabilidad automática](understand-your-bill.md#billing-examples-with-free-tier-accounts).

### <a name="is-autoscale-supported-for-all-apis"></a>¿La escalabilidad automática se admite para todas las API?
Sí, la escalabilidad automática es compatible con todas las API: Core (SQL), Gremlin, Table, Cassandra y API para MongoDB.

### <a name="is-autoscale-supported-for-multi-master-accounts"></a>¿Se admite la escalabilidad automática para cuentas de arquitectura multimaestro?
Sí. Las RU/s máximas están disponibles en cada región que se agregue a la cuenta de Azure Cosmos DB. 

### <a name="how-do-i-enable-autoscale-on-new-databases-or-containers"></a>¿Cómo habilito la escalabilidad automática para nuevos contenedores o bases de datos?
Consulte este artículo acerca de [cómo habilitar la escalabilidad automática](how-to-provision-autoscale-throughput.md).

### <a name="can-i-enable-autoscale-on-an-existing-database-or-a-container"></a>¿Se puede habilitar la escalabilidad automática en una base de datos o un contenedor existentes?
Sí. También puede cambiar entre la escalabilidad automática y el rendimiento aprovisionado estándar (manual) según sea necesario. Actualmente, para todas las API, solo puede usar [Azure Portal](how-to-provision-autoscale-throughput.md#enable-autoscale-on-existing-database-or-container) para realizar estas operaciones.

### <a name="how-does-the-migration-between-autoscale-and-standard-manual-provisioned-throughput-work"></a>¿Cómo funciona la migración entre la escalabilidad automática y el rendimiento aprovisionado estándar (manual)?
Conceptualmente, el cambio del tipo de rendimiento es un proceso de dos fases. En primer lugar, se envía una solicitud para cambiar la configuración de rendimiento para usar la escalabilidad automática o el rendimiento aprovisionado manual. En ambos casos, el sistema determinará y establecerá automáticamente un valor de RU/s inicial, en función de la configuración de rendimiento actual y el almacenamiento. Durante este paso, no se aceptará ningún valor de RU/s proporcionado por el usuario. Después, una vez completada la actualización, puede [cambiar las RU/s](#can-i-change-the-max-rus-on-the-database-or-container) para que se adapten a su carga de trabajo. 

**Migración del rendimiento aprovisionado estándar (manual) a la escalabilidad automática**

En el caso de un contenedor, use la siguiente fórmula para calcular las RU/s máximas iniciales de la escalabilidad automática: ``MAX(4000, current manual provisioned RU/s, maximum RU/s ever provisioned / 10, storage in GB * 100)``, redondeado en miles de RU/s. Las RU/s máximas iniciales de la escalabilidad automática pueden variar en función de la configuración de la cuenta.

Ejemplo 1: Supongamos que tiene un contenedor con un rendimiento aprovisionado manual de 10 000 RU/s y 25 GB de almacenamiento. Al habilitar la escalabilidad automática, el número máximo de RU/s iniciales para la escalabilidad automática será: 10 000 RU/s, que se escalarán entre 1000 y 10 000 RU/s. 

Ejemplo 2: Supongamos que tiene un contenedor con un rendimiento aprovisionado manual de 50 000 RU/s y 2500 GB de almacenamiento. Al habilitar la escalabilidad automática, el número máximo de RU/s iniciales para la escalabilidad automática será: 250 000 RU/s, que se escalarán entre 25 000 y 250 000 RU/s. 

**Migración de la escalabilidad automática al rendimiento aprovisionado estándar (manual)**

El rendimiento inicial aprovisionado manualmente será igual a las RU/s máximas actuales de la escalabilidad automática. 

Ejemplo: Supongamos que tiene una base de datos o un contenedor con escalabilidad automática con un máximo de 20 000 RU/s (que se escalan entre 2000 y 20 000 RU/s). Al actualizar para usar el rendimiento aprovisionado manual, el rendimiento inicial será de 20 000 RU/s. 

### <a name="is-there-azure-cli-or-powershell-support-to-manage-databases-or-containers-with-autoscale"></a>¿Hay compatibilidad con la CLI de Azure o con PowerShell para administrar bases de datos o contenedores con escalabilidad automática?
Actualmente, solo puede crear y administrar recursos con escalabilidad automática desde Azure Portal, el SDK para .NET V3, el SDK para Java V4 y Azure Resource Manager. Todavía no está disponible la compatibilidad con la CLI de Azure, PowerShell y otros SDK.

### <a name="is-autoscale-supported-for-shared-throughput-databases"></a>¿Se admite la escalabilidad automática en las bases de datos de rendimiento compartido?
Sí, la escalabilidad automática es compatible con las bases de datos de rendimiento compartido. Para habilitar esta característica, seleccione la escalabilidad automática y la opción **Aprovisionar rendimiento** al crear la base de datos. 

### <a name="what-is-the-number-of-allowed-containers-per-shared-throughput-database-when-autoscale-is-enabled"></a>¿Cuál es el número de contenedores permitidos por base de datos de rendimiento compartido cuando está habilitada la escalabilidad automática?
Azure Cosmos DB aplica un máximo de 25 contenedores en una base de datos de rendimiento compartido, que se aplica a las bases de datos con escalabilidad automática o rendimiento estándar (manual). 

### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>¿Cuál es el límite de almacenamiento asociado a cada opción de RU por segundo máximas?  
El límite de almacenamiento en GB por cada número máximo de RU/s es: Número máximo de RU/s para una base de datos o contenedor/100. Por ejemplo, si las RU por segundo máximas son 20 000 RU por segundo, el recurso puede admitir 200 GB de almacenamiento. Consulte el artículo sobre los [límites de Escalabilidad automática](provision-throughput-autoscale.md#autoscale-limits) para ver las opciones de almacenamiento y RU por segundo máximas disponibles. 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>¿Qué ocurre si se supera el límite de almacenamiento asociado al rendimiento máximo?
Si se supera el límite de almacenamiento asociado al rendimiento máximo de la base de datos o el contenedor, Azure Cosmos DB aumentará automáticamente el rendimiento máximo hasta el siguiente número de RU/s que pueda admitir ese nivel de almacenamiento.

Por ejemplo, si comienza con un máximo de 50 000 RU/s (que se escalan entre 5000 y 50 000 RU/s), puede almacenar hasta 500 GB de datos. Si supera los 500 GB: por ejemplo, el almacenamiento es ahora de 600 GB, el nuevo número máximo será de 60 000 RU/s (que se escalan entre 6000 y 60 000 RU/s).

### <a name="can-i-change-the-max-rus-on-the-database-or-container"></a>¿Puedo cambiar el número máximo de RU/s en la base de datos o el contenedor? 
Sí. Consulte este [artículo](how-to-provision-autoscale-throughput.md) sobre cómo cambiar el número máximo de RU/s. Al cambiar el número máximo de RU/s, en función del valor solicitado, puede tratarse de una operación asincrónica que puede tardar algún tiempo en completarse (puede ser de hasta 4-6 horas, según las RU/s seleccionadas).

#### <a name="increasing-the-max-rus"></a>Aumento del número máximo de RU/s
Cuando se envía una solicitud para aumentar el número máximo de RU/s, `Tmax`, en función del número máximo de RU/s seleccionado, el servicio aprovisiona más recursos para admitir el número máximo de RU/s más alto. Mientras esto se lleva a cabo, la carga de trabajo y las operaciones existentes no se verán afectadas. El sistema seguirá escalando la base de datos o el contenedor entre el valor anterior desde `0.1*Tmax` hasta `Tmax` hasta que esté listo el nuevo intervalo de escalado desde `0.1*Tmax_new` hasta `Tmax_new`.

#### <a name="lowering-the-max-rus"></a>Reducción del número máximo de RU/s
Al reducir el número máximo de RU/s, el valor mínimo que se puede establecer es: `MAX(4000, highest max RU/s ever provisioned / 10, current storage in GB * 100)`, redondeado en miles de RU/s. 

Ejemplo 1: Supongamos que tiene un contenedor con escalabilidad automática con un máximo de 20 000 RU/s (que se escalan entre 2000 y 20 000 RU/s) y 50 GB de almacenamiento. El valor mínimo más bajo que puede establecer como número máximo de RU/s es: MAX(4000, 20 000 / 10, **50 * 100**) = 5000 RU/s (que se escalan entre 500 y 5000 RU/s).

Ejemplo 2: Supongamos que tiene un contenedor con escalabilidad automática con un máximo de 100 000 RU/s y 100 GB de almacenamiento. Ahora, escale el número máximo de RU/s hasta 150 000 RU/s (que se escalan entre 15 000 y 150 000 RU/s). El valor mínimo más bajo que puede establecer ahora como número máximo RU/s es: MAX(4 000, **150 000 / 10**, 100 * 100) = 15 000 RU/s (que se escalan entre 1500 y 15 000 RU/s). 

En el caso de una base de datos de rendimiento compartido, al reducir el número máximo de RU/s, el valor mínimo que se puede establecer es: `MAX(4000, highest max RU/s ever provisioned / 10, current storage in GB * 100,  4000 + (MAX(Container count - 25, 0) * 1000))`, redondeado en miles de RU/s.  

Las fórmulas y los ejemplos anteriores se refieren al valor mínimo del número máximo de RU/s de escalabilidad automática que se puede establecer y es distinta del intervalo desde `0.1 * Tmax` hasta `Tmax` con el que se escala automáticamente el sistema. Independientemente de cuál sea el número máximo de RU/s, el sistema siempre se escalará entre `0.1 * Tmax` y `Tmax`. 

### <a name="how-does-ttl-work-with-autoscale"></a>¿Cómo funciona TTL con la escalabilidad automática?
Con la escalabilidad automática, las operaciones de TTL no afectan al escalado de RU/s. Las RU consumidas debido a TTL no forman parte de las RU/s facturadas del contenedor con escalabilidad automática. 

Por ejemplo, supongamos que tiene un contenedor con escalabilidad automática con 400 a 4000 RU/s:
- Hora 1: T=0: el contenedor no tiene uso (sin solicitudes de TTL ni de la carga de trabajo). Las RU/s facturables son 400 RU/s.
- Hora 1: T=1: TTL está habilitado.
- Hora 1: T=2: el contenedor comienza a tener solicitudes, que consumen 1000 RU en 1 segundo. También han sido necesarias 200 RUs de TTL. Las RU/s facturables siguen siendo 1000 RU/s. Independientemente del momento en que se produzcan las operaciones de TTL, no afectarán a la lógica de la escalabilidad automática.

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>¿Cuál es la asignación entre el número máximo de RU por segundo y las particiones físicas?
La primera vez que se seleccionan las RU por segundo máximas, Azure Cosmos DB aprovisionará: RU/s máximas/10 000 RU/s = número de particiones físicas. Cada [partición física](partition-data.md#physical-partitions) puede admitir hasta 10 000 RU/s y un almacenamiento de 50 GB. A medida que aumenta el tamaño del almacenamiento, Azure Cosmos DB dividirá automáticamente las particiones para agregar más particiones físicas con el fin de controlar el aumento del almacenamiento o bien para aumentar el número máximo de RU/s si el almacenamiento [supera el límite asociado](#what-is-the-storage-limit-associated-with-each-max-rus-option). 

El número máximo de RU/s de la base de datos o el contenedor se dividen uniformemente entre todas las particiones físicas. Por lo tanto, el rendimiento total de una sola partición física que se puede escalar es: RU por segundo máximas para una base de datos o contenedor divididas por el número de particiones físicas. 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>¿Qué ocurre si las solicitudes entrantes superan las RU por segundo máximas de la base de datos o el contenedor?
Si el total de RU/s consumidas supera el número máximo de RU/s del contenedor o la base de datos, las solicitudes que superen el número máximo de RU/s se limitarán y devolverán un código de estado 429. Las solicitudes que dan lugar a más de un 100 % de la utilización normalizada también se limitarán. La utilización normalizada se define como el máximo del uso de las RU por segundo en todas las particiones físicas. Por ejemplo, supongamos que el rendimiento máximo es 20 000 RU por segundo y tiene dos particiones físicas, P_1 y P_2, cada una con capacidad de escalado a 10 000 RU por segundo. En un segundo determinado, si P_1 ha usado 6000 RU y P_2 ha usado 8000 RU, la utilización normalizada es MAX(6000 RU/10 000 RU, 8000 RU/10 000 RU) = 0,8.

> [!NOTE]
> Los SDK de cliente de Azure Cosmos DB y las herramientas de importación de datos (Azure Data Factory, la biblioteca de ejecutores en masa) reintentan automáticamente en el código de estado 429, por lo que estos códigos de estado ocasionales son correctos. Un gran número sostenido de códigos de estado 429 puede indicar que necesita aumentar las RU por segundo máximas o revisar la estrategia de partición para una [partición frecuente](#autoscale-rate-limiting).

### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autoscale-is-enabled"></a><a id="autoscale-rate-limiting"></a> ¿Sigue siendo posible ver códigos de estado 429 (limitación/limitación de velocidad) cuando la Escalabilidad automática está habilitada? 
Sí. Es posible ver códigos de estado 429 en dos escenarios. En primer lugar, cuando el total de las RU/s consumidas supera el número máximo de RU/s del contenedor o la base de datos, el servicio limitará las solicitudes en consecuencia. 

En segundo lugar, si hay una partición frecuente, es decir, un valor de clave de partición lógica que tiene una cantidad de solicitudes desproporcionada en comparación con otros valores de la clave de partición, es posible que la partición física subyacente supere el presupuesto de las RU por segundo. Como procedimiento recomendado, para evitar las particiones frecuentes, [elija una buena clave de partición](partitioning-overview.md#choose-partitionkey) que dé lugar a una distribución uniforme tanto del almacenamiento como del rendimiento. 

Por ejemplo, si selecciona la opción de rendimiento de 20 000 RU por segundo máximas y tiene 200 GB de almacenamiento, con cuatro particiones físicas, cada partición física se puede escalar automáticamente hasta 5000 RU por segundo. Si había una partición frecuente en una clave de partición lógica determinada, verá varios códigos de estado 429 cuando la partición física subyacente en la que reside supera las 5000 RU por segundo, es decir, supera el 100 % de la utilización normalizada.


## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [habilitar la escalabilidad automática en una base de datos o un contenedor de Azure Cosmos DB](how-to-provision-autoscale-throughput.md).
* Obtenga información sobre las [ventajas del rendimiento aprovisionado con escalabilidad automática](provision-throughput-autoscale.md#benefits-of-autoscale).
* Más información sobre las [particiones lógicas y físicas](partition-data.md).
                        