---
title: Migración de cargas de trabajo de Apache Kafka a Azure HDInsight 4.0
description: Obtenga información sobre cómo migrar las cargas de trabajo de Apache Kafka en HDInsight 3.6 a HDInsight 4.0.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/18/2019
ms.openlocfilehash: 66ea8a93723c117a364a188ec05b050e1ef60564
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76548091"
---
# <a name="migrate-apache-kafka-workloads-to-azure-hdinsight-40"></a>Migración de cargas de trabajo de Apache Kafka a Azure HDInsight 4.0

Azure HDInsight 4.0 ofrece los componentes de código abierto más recientes con mejoras significativas en el rendimiento, la conectividad y la seguridad. En este documento se explica cómo migrar las cargas de trabajo de Apache Kafka en HDInsight 3.6 a HDInsight 4.0. Después de migrar las cargas de trabajo a HDInsight 4.0, podrá usar muchas de las nuevas características que no están disponibles en HDInsight 3.6.

## <a name="hdinsight-36-kafka-migration-paths"></a>Rutas de migración de HDInsight 3.6 Kafka

HDInsight 3.6 admite dos versiones de Kafka: 1.0.0 y 1.1.0. HDInsight 4.0 es compatible con las versiones 1.1.0 y 2.1.0. En función de la versión de Kafka y de la versión de HDInsight que desee ejecutar, hay varias rutas de migración admitidas. Estas rutas se explican a continuación y se ilustran en el diagrama siguiente.

* **Ejecutar las versiones más recientes de Kafka y HDInsight (recomendado)** : migre una aplicación de HDInsight 3.6 y Kafka 1.0.0 o 1.1.0 a HDInsight 4.0 con Kafka 2.1.0 (rutas D y E del diagrama).
* **Ejecutar la versión más reciente de HDInsight, pero solo una versión más reciente de Kafka**: migre una aplicación de HDInsight 3.6 y Kafka 1.0.0 a HDInsight 4.0 con Kafka 1.1.0 (ruta B del diagrama).
* **Ejecutar HDInsight en la versión más reciente, conservar la versión de Kafka**: migre una aplicación de HDInsight 3.6 y Kafka 1.1.0 a HDInsight 4.0 con Kafka 1.1.0 (ruta C del diagrama).
* **Ejecutar Kafka en una versión más reciente, conservar la versión de HDInsight**: migre una aplicación de Kafka 1.0.0 a 1.1.0 y permanecer en HDInsight 3.6 (ruta A del diagrama). Tenga en cuenta que con esta opción se sigue requiriendo implementar un nuevo clúster. No se admite la actualización de la versión de Kafka en un clúster existente. Después de crear un clúster con la versión que desee, migre los clientes de Kafka para que usen el nuevo clúster.

![Rutas de actualización para Apache Kafka en 3.6](./media/upgrade-threesix-to-four/apache-kafka-upgrade-path.png)

## <a name="apache-kafka-versions"></a>Versiones de Apache Kafka

### <a name="kafka-110"></a>Kafka 1.1.0
  
Si migra de Kafka 1.0.0 a 1.1.0, puede aprovechar las siguientes características nuevas:

* Las mejoras en el controlador de Kafka agilizan el cierre controlado, por lo que puede reiniciar los agentes y recuperarse de los problemas más rápidamente. 
* Las mejoras en la [lógica de FetchRequests](https://issues.apache.org/jira/browse/KAFKA-6254) que permiten tener más particiones (y, por tanto, más temas) en el clúster. 
* Kafka Connect admite [encabezados de registro](https://issues.apache.org/jira/browse/KAFKA-5142) y [expresiones regulares](https://issues.apache.org/jira/browse/KAFKA-3073) para temas. 

Para obtener una lista completa de las actualizaciones, vea las [notas de la versión de Apache Kafka 1.1](https://archive.apache.org/dist/kafka/1.1.0/RELEASE_NOTES.html).

### <a name="apache-kafka-210"></a>Apache Kafka 2.1.0

Si migra a Kafka 2.1, puede aprovechar las siguientes características nuevas:

* Mejora de la resistencia del agente debido a un protocolo de replicación mejorado.
* Nueva funcionalidad de la API KafkaAdminClient.
* Administración de cuotas configurable.
* Compatibilidad con la compresión Zstandard.

Para obtener una lista completa de las actualizaciones, vea las [notas de la versión de Apache Kafka 2.0](https://archive.apache.org/dist/kafka/2.0.0/RELEASE_NOTES.html) y las [notas de la versión de Apache Kafka 2.1](https://archive.apache.org/dist/kafka/2.1.0/RELEASE_NOTES.html).

## <a name="kafka-client-compatibility"></a>Compatibilidad con el cliente de Kafka

Los nuevos agentes de Kafka admiten clientes más antiguos. [KIP-35: recuperación de la versión de protocolo](https://cwiki.apache.org/confluence/display/KAFKA/KIP-35+-+Retrieving+protocol+version) presentó un mecanismo para determinar de forma dinámica la funcionalidad de un agente de Kafka y [KIP-97: mejora de la directiva de compatibilidad de RPC de cliente de Kafka](https://cwiki.apache.org/confluence/display/KAFKA/KIP-97%3A+Improved+Kafka+Client+RPC+Compatibility+Policy) presentó una nueva directiva de compatibilidad y garantías para el cliente de Java. Anteriormente, un cliente de Kafka tenía que interactuar con un agente de la misma versión o una versión más reciente. Ahora, las versiones más recientes de los clientes de Java y otros clientes que admiten KIP-35, como `librdkafka`, pueden revertir a tipos de solicitud anteriores o producir errores adecuados si la funcionalidad no está disponible.

![Actualización de la compatibilidad con el cliente de Kafka](./media/upgrade-threesix-to-four/apache-kafka-client-compatibility.png)

Tenga en cuenta que no significa que el cliente admita agentes más antiguos.  Para obtener más información, vea [Matriz de compatibilidad](https://cwiki.apache.org/confluence/display/KAFKA/Compatibility+Matrix).

## <a name="general-migration-process"></a>Proceso de migración general

En la siguiente guía de migración se asume un clúster de Apache Kafka 1.0.0 o 1.1.0 implementado en HDInsight 3.6 en una única red virtual. El agente existente tiene algunos temas y los productores y consumidores los utilizan activamente.

![Entorno Kafka actual supuesto](./media/upgrade-threesix-to-four/apache-kafka-presumed-environment.png)

Realice los pasos siguientes para completar la migración:

1. **Implemente un nuevo clúster y clientes de HDInsight 4.0 para la prueba.** Implemente un nuevo clúster de HDInsight 4.0 Kafka. Si se pueden seleccionar varias versiones de clúster de Kafka, se recomienda seleccionar la versión más reciente. Después de la implementación, establezca algunos parámetros necesarios y cree un tema con el mismo nombre que el del entorno existente. Además, establezca la SSL y Bring Your Own Key (BYOK) según sea necesario. A continuación, compruebe si funciona correctamente con el nuevo clúster.

    ![Implementación de nuevos clústeres de HDInsight 4.0](./media/upgrade-threesix-to-four/deploy-new-hdinsight-clusters.png)

1. **Cambie el clúster para la aplicación de productor y espere hasta que los consumidores actuales consuman todos los datos de la cola.** Cuando el nuevo clúster de HDInsight 4.0 Kafka esté listo, cambie el destino del productor existente al nuevo clúster. Déjelo como está hasta que la aplicación de consumidor existente haya consumido todos los datos del clúster existente.

    ![Cambio del clúster de la aplicación de productor](./media/upgrade-threesix-to-four/switch-cluster-producer-app.png)

1. **Cambie el clúster en la aplicación de consumidor.** Después de confirmar que la aplicación de consumidor existente ha terminado de consumir todos los datos del clúster existente, cambie la conexión al nuevo clúster.

    ![Cambio del clúster en la aplicación de consumidor](./media/upgrade-threesix-to-four/switch-cluster-consumer-app.png)

1. **Quite el clúster anterior y pruebe las aplicaciones según sea necesario.** Una vez completado el cambio y que funcione correctamente, quite el antiguo clúster de HDInsight 3.6 Kafka y los productores y consumidores que se usan en la prueba, según sea necesario.

## <a name="next-steps"></a>Pasos siguientes

* [Optimización del rendimiento para los clústeres de Apache Kafka HDInsight](apache-kafka-performance-tuning.md)
* [Inicio rápido: Creación de un clúster de Apache Kafka en Azure HDInsight mediante Azure Portal](apache-kafka-get-started.md)
