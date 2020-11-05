---
title: Distribución global de los datos con Azure Cosmos DB
description: Aprenda sobre la replicación geográfica, las escrituras en varias regiones, la conmutación por error y la recuperación de datos a escala mundial con bases de datos globales de Azure Cosmos DB, un servicio de base de datos de varios modelos distribuido globalmente.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: 709c802b8b23a39a0f723db3ac7b4636a0c53930
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93096774"
---
# <a name="distribute-your-data-globally-with-azure-cosmos-db"></a>Distribución de los datos globalmente con Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Las aplicaciones actuales deben estar siempre en línea y tener una alta capacidad de respuesta. Para lograr baja latencia y alta disponibilidad, las instancias de estas aplicaciones deben implementarse en centros de datos que están cerca de sus usuarios. Estas aplicaciones se implementan normalmente en varios centros de datos y se denominan "distribuidas globalmente". Las aplicaciones distribuidas globalmente necesitan una base de datos distribuida globalmente que puede replicar de forma transparente los datos en cualquier lugar del mundo para permitir que las aplicaciones funcionen en una copia de los datos que están cerca de los usuarios. 

Azure Cosmos DB es un servicio de base de datos distribuida globalmente que está diseñado para proporcionar baja latencia, escalabilidad elástica del rendimiento, semántica bien definida para la coherencia de datos y alta disponibilidad. En resumen, si la aplicación necesita un tiempo de respuesta rápido en cualquier parte del mundo, si necesita estar siempre en línea y precisa escalabilidad elástica e ilimitada de rendimiento y almacenamiento, considere la posibilidad de crear aplicaciones con Azure Cosmos DB.

Puede configurar sus bases de datos para que se distribuyan de manera global y estén disponibles en cualquiera de las regiones de Azure. Para reducir la latencia, coloque los datos en la ubicación más cercana a la de los usuarios. Elegir las regiones requeridas depende del alcance global de la aplicación y de dónde se encuentran los usuarios. Cosmos DB replica los datos con transparencia en todas las regiones asociadas a su cuenta de Cosmos. Proporciona una sola imagen de sistema de los contenedores y la base de datos de Azure Cosmos globalmente distribuida, para que la aplicación pueda leer y escribir de manera local. 

Con Azure Cosmos DB, puede agregar o quitar las regiones asociadas con su cuenta en cualquier momento. La aplicación no necesita pausarse o volver a implementarse para agregar o quitar una región. Sigue ofreciendo una alta disponibilidad en todo momento debido a las funcionalidades de hospedaje múltiple que el servicio ofrece de forma nativa.

:::image type="content" source="./media/distribute-data-globally/deployment-topology.png" alt-text="Topología de implementación de alta disponibilidad" border="false":::

## <a name="key-benefits-of-global-distribution"></a>Ventajas clave de distribución global

**Compilación de aplicaciones globales activo/activo.** Con el novedoso protocolo de replicación de escrituras en varias regiones, cada región admite lecturas y escrituras. La capacidad de escrituras en varias regiones también permite:

- Escalabilidad de escritura y lectura elásticas ilimitada.
- 99,999 % de disponibilidad de lectura y escritura en todo el mundo.
- Garantía de lecturas y escrituras atendidas en menos de 10 milisegundos en el percentil 99.

Con las API de hospedaje múltiple de Azure Cosmos DB, la aplicación sabe cuál es la región más cercana y envía las solicitudes a esa región. La región más cercana se identifica sin ningún cambio de configuración. Como agrega y quita regiones desde la cuenta de Azure Cosmos DB, no es necesario volver a implementar la aplicación ni pausarla, y esta sigue teniendo alta disponibilidad.

**Creación de aplicaciones con alta capacidad de respuesta.** La aplicación puede realizar casi en tiempo real lecturas y escrituras en todas las regiones que eligió para la base de datos. De manera interna, Azure Cosmos DB controla la replicación de datos entre regiones de una manera que garantiza el nivel de coherencia elegido.

**Creación de aplicaciones de alta disponibilidad.** Ejecutar una base de datos en varias regiones aumenta la disponibilidad de la base de datos. Si alguna región no está disponible, otras regiones gestionan automáticamente las solicitudes de la aplicación. Azure Cosmos DB ofrece el 99,999 % de disponibilidad de lectura y escritura para las bases de datos de varias regiones.

**Mantenimiento de la continuidad empresarial durante interrupciones regionales.** Azure Cosmos DB admite [conmutación por error automática](how-to-manage-database-account.md#automatic-failover) durante una interrupción regional. Durante una interrupción regional, Azure Cosmos DB sigue manteniendo sus SLA de rendimiento, coherencia, disponibilidad y latencia. Para ayudar a garantizar que toda la aplicación tiene una alta disponibilidad, Cosmos DB ofrece la API de conmutación por error manual para simular una interrupción regional. Con esta API, puede realizar maniobras de continuidad empresarial regulares.

**Escalabilidad global del rendimiento de lectura y escritura.** Puede permitir que todas las regiones sean de escritura y que escale lecturas y escrituras en todo el mundo. El rendimiento que la aplicación configura en un contenedor o una base de datos de Azure Cosmos DB se aprovisiona en todas las regiones asociadas con su cuenta de Azure Cosmos DB. El rendimiento aprovisionado se garantiza por [Acuerdo de Nivel de Servicio con respaldo financiero](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/).

**Elección entre varios modelos de coherencia bien definidos.** El protocolo de replicación de Azure Cosmos DB ofrece cinco modelos de coherencia prácticos, intuitivos y bien definidos. Cada modelo tiene un equilibrio entre coherencia y rendimiento. Use estos modelos de coherencia para compilar con facilidad las aplicaciones distribuidas globalmente.

## <a name="next-steps"></a><a id="Next Steps"></a>Pasos siguientes

Lea más sobre la distribución global en estos artículos:

* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Configuración de varias regiones de escritura en las aplicaciones](how-to-multi-master.md)
* [Configuración de los clientes para el hospedaje múltiple](how-to-manage-database-account.md#configure-multiple-write-regions)
* [Incorporación o eliminación de regiones de una cuenta de Azure Cosmos DB](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Creación de una directiva de resolución de conflictos personalizada para cuentas de API de SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
* [Modelos de coherencia programables en Cosmos DB](consistency-levels.md)
* [Elección del nivel de coherencia adecuado para la aplicación](./consistency-levels.md)
* [Niveles de coherencia en Azure Cosmos DB](./consistency-levels.md)
* [Availability and performance tradeoffs for various consistency levels](./consistency-levels.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)