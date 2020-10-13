---
title: 'Preguntas más frecuentes: Azure Event Hubs para Apache Kafka'
description: En este artículo se responden preguntas frecuentes sobre la compatibilidad de Azure Event Hubs con los clientes de Apache Kafka que no se trataron en ningún otro lugar.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: dc6a12b2098a1fdf33adda92b4347f91ab4e5489
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91828115"
---
# <a name="frequently-asked-questions---event-hubs-for-apache-kafka"></a>Preguntas más frecuentes: Event Hubs para Apache Kafka 
En este artículo se responden algunas de las preguntas más frecuentes sobre la migración a Event Hubs de Apache Kafka.

## <a name="does-azure-event-hubs-run-on-apache-kafka"></a>¿Azure Event Hubs se puede ejecutar en Apache Kafka?

No. Azure Event Hubs es un agente de varios niveles nativo de nube compatible con varios protocolos; Microsoft desarrolla y mantiene al agente, y este no usa ningún código de Apache Kafka. Uno de los protocolos admitidos es el protocolo RPC de Kafka para las API de productor y consumidor del cliente de Kafka. Event Hubs funciona con muchas de sus aplicaciones de Kafka actuales. Para obtener más información, consulte [Event Hubs para Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md). Dado que los conceptos de Apache Kafka y Event Hubs de Azure son muy similares (pero no idénticos), podemos ofrecer la inigualable confiabilidad de Azure Event Hubs a los clientes con inversiones de Apache Kafka existentes. 

## <a name="event-hubs-consumer-group-vs-kafka-consumer-group"></a>Grupo de consumidores de Event Hubs frente a grupo de consumidores de Kafka
¿Cuál es la diferencia entre un grupo de consumidores de Event Hubs y un grupo de consumidores de Kafka en Event Hubs? Los grupos de consumidores de Kafka en Event Hubs son completamente distintos de los grupos de consumidores de Event Hubs estándar.

**Grupo de consumidores de Event Hubs**

- Se administran con operaciones de creación, recuperación, actualización y eliminación (CRUD) mediante el portal, el SDK o las plantillas de Azure Resource Manager. Los grupos de consumidores de Event Hubs no pueden crearse de forma automática.
- Son entidades secundarias de un centro de eventos. Esto significa que el mismo nombre de grupo de consumidores puede usarse varias veces entre centros de eventos en el mismo espacio de nombres porque son entidades independientes.
- No se usan para almacenar desplazamientos. El consumo de AMQP organizado se realiza mediante almacenamiento externo del desplazamiento, por ejemplo en Azure Storage.

**Grupos de consumidores de Kafka**

- Se crean de forma automática.  Los grupos de Kafka se pueden administrar mediante las API de grupo de consumidores de Kafka.
- Pueden almacenar desplazamientos en el servicio de Event Hubs.
- Se usan como claves en lo que realmente es un almacén de pares de clave-valor de desplazamiento. Para un par único de `group.id` y `topic-partition`, se almacena un desplazamiento en Azure Storage (3x replicación). Los usuarios de Event Hubs no incurren en costos de almacenamiento adicionales por almacenar desplazamientos de Kafka. Los desplazamientos se pueden manipular mediante las API de grupo de consumidores de Kafka, pero las *cuentas* de almacenamiento de desplazamiento no son visibles directamente ni manipulables para los usuarios de Event Hubs.  
- Abarcan un espacio de nombres. El uso del mismo nombre de grupo de Kafka para varias aplicaciones en varios temas significa que todas las aplicaciones y sus clientes de Kafka se volverán a equilibrar siempre que solo sea necesario reequilibrar una sola aplicación.  Elija los nombres de grupo con cuidado.
- Son completamente diferentes de los grupos de consumidores de Event Hubs. **No** tiene que usar ' $Default ', ni tiene que preocuparse de que los clientes de Kafka interfieran con las cargas de trabajo de AMQP.
- No se pueden ver en Azure Portal. La información del grupo de consumidores es accesible mediante las API de Kafka.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información acerca de Event Hubs y Event Hubs para Kafka, consulte los artículos siguientes:  

- [Guía del desarrollador de Apache Kafka para Event Hubs](apache-kafka-developer-guide.md)
- [Guía de migración de Kafka para Event Hubs](apache-kafka-migration-guide.md)
- [Guía de solución de problemas de Apache Kafka para Event Hubs](apache-kafka-troubleshooting-guide.md)
- [Configuraciones recomendadas](apache-kafka-configurations.md)

