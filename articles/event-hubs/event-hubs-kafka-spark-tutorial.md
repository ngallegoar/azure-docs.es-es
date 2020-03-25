---
title: 'Conexión de la aplicación de Apache Spark: Azure Event Hubs | Microsoft Docs'
description: En este artículo se proporciona información sobre cómo usar Apache Spark con Azure Event Hubs para Kafka.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: tutorial
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 5bd25604844ce0521b6bac2d637a2d48a6ef089b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "72555740"
---
# <a name="connect-your-apache-spark-application-with-kafka-enabled-azure-event-hubs"></a>Conexión de la aplicación de Apache Spark con Azure Event Hubs habilitado para Kafka
Este tutorial le guiará a través de la conexión de la aplicación de Spark a Event Hubs habilitado para Kafka para el streaming en tiempo real. Esta integración permite el streaming sin tener que cambiar sus clientes de protocolo o ejecutar sus propios clústeres de Kafka o Zookeeper. Este tutorial necesita Apache Spark v2.4 o superior y Apache Kafka v2.0 o superior.

> [!NOTE]
> Este ejemplo está disponible en [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/spark/).

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Creación de un espacio de nombres de Event Hubs
> * Clonación del proyecto de ejemplo
> * Ejecución de Spark
> * Lectura de Event Hubs para Kafka
> * Escritura de Event Hubs para Kafka

## <a name="prerequisites"></a>Prerequisites

Antes de comenzar este tutorial, asegúrese de que dispone de lo siguiente:
-   Suscripción de Azure. En caso de no tener ninguna, [cree una cuenta gratuita](https://azure.microsoft.com/free/).
-   [Apache Spark v2.4](https://spark.apache.org/downloads.html)
-   [Apache Kafka v2.0]( https://kafka.apache.org/20/documentation.html)
-   [Git](https://www.git-scm.com/downloads)

> [!NOTE]
> El adaptador Spark-Kafka se actualizó para ser compatible con Kafka v2.0 a partir de Spark v2.4. En versiones anteriores de Spark, el adaptador admitía Kafka v0.10 y versiones posteriores, pero confiaba específicamente en las API de Kafka v0.10. Como Event Hubs para Kafka no admite Kafka v0.10, los adaptadores Spark-Kafka de versiones de Spark anteriores a la v2.4 no se admiten en Event Hubs para ecosistemas de Kafka.


## <a name="create-an-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs
Se requiere un espacio de nombres de Event Hubs para enviar y recibir de cualquier servicio de Event Hubs. Consulte [Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal](event-hubs-create.md) para obtener instrucciones sobre cómo obtener un punto de conexión de Kafka para Event Hubs. Obtenga la cadena de conexión de Event Hubs y el nombre de dominio completo (FQDN) para su uso posterior. Para obtener instrucciones, consulte [Get an Event Hubs connection string](event-hubs-get-connection-string.md) (Obtención de una cadena de conexión de Event Hubs). 

## <a name="clone-the-example-project"></a>Clonación del proyecto de ejemplo
Clone el repositorio de Azure Event Hubs y vaya a la subcarpeta `tutorials/spark`:

```bash
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/spark
```

## <a name="read-from-event-hubs-for-kafka"></a>Lectura de Event Hubs para Kafka
Con unos pocos cambios de configuración, puede empezar a leer desde Event Hubs para Kafka. Actualice **BOOTSTRAP_SERVERS** y **EH_SASL** con los detalles de su espacio de nombres y puede empezar a transmitir con Event Hubs como lo haría con Kafka. Para ver el código de ejemplo completo, consulte el archivo sparkConsumer.scala en GitHub. 

```scala
//Read from your Event Hub!
val df = spark.readStream
    .format("kafka")
    .option("subscribe", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("kafka.request.timeout.ms", "60000")
    .option("kafka.session.timeout.ms", "30000")
    .option("kafka.group.id", GROUP_ID)
    .option("failOnDataLoss", "false")
    .load()

//Use dataframe like normal (in this example, write to console)
val df_write = df.writeStream
    .outputMode("append")
    .format("console")
    .start()
```

## <a name="write-to-event-hubs-for-kafka"></a>Escritura de Event Hubs para Kafka
También puede escribir en Event Hubs de la misma manera que escribe en Kafka. No olvide actualizar su configuración para cambiar **BOOTSTRAP_SERVERS** y **EH_SASL** con información de su espacio de nombres de Event Hubs.  Para ver el código de ejemplo completo, consulte el archivo sparkProducer.scala en GitHub. 

```scala
df = /**Dataframe**/

//Write to your Event Hub!
df.writeStream
    .format("kafka")
    .option("topic", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("checkpointLocation", "./checkpoint")
    .start()
```



## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a transmitir mediante el conector Spark-Kafka y Event Hubs para Kafka. Ha realizado los pasos siguientes: 

> [!div class="checklist"]
> * Creación de un espacio de nombres de Event Hubs
> * Clonación del proyecto de ejemplo
> * Ejecución de Spark
> * Lectura de Event Hubs para Kafka
> * Escritura de Event Hubs para Kafka

Para obtener más información acerca de Event Hubs y Event Hubs para Kafka, consulte el tema siguiente:  

- [Más información sobre Events Hubs](event-hubs-what-is-event-hubs.md)
- [Event Hubs para Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md)
- [How to create Kafka enabled Event Hubs](event-hubs-create-kafka-enabled.md) (Cómo crear Event Hubs habilitados para Kafka)
- [Stream into Event Hubs from your Kafka applications](event-hubs-quickstart-kafka-enabled-event-hubs.md) (Transmitir en Event Hubs desde sus aplicaciones de Kafka)
- [Creación de un reflejo de un agente de Kafka en un centro de eventos habilitado para Kafka](event-hubs-kafka-mirror-maker-tutorial.md)
- [Conexión de Apache Flink a un centro de eventos habilitado para Kafka](event-hubs-kafka-flink-tutorial.md)
- [Integración de Kafka Connect con un centro de eventos habilitado para Kafka](event-hubs-kafka-connect-tutorial.md)
- [Conexión de Akka Streams a un centro de eventos habilitado para Kafka](event-hubs-kafka-akka-streams-tutorial.md)
- [Exploración de ejemplos en nuestro GitHub](https://github.com/Azure/azure-event-hubs-for-kafka)
