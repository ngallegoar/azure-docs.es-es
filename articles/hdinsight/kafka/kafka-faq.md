---
title: Preguntas frecuentes sobre Apache Kafka en Azure HDInsight
description: Obtenga respuestas a preguntas habituales sobre Apache Kafka en Azure HDInsight, un servicio en la nube de Hadoop administrado.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: ff4079263fd7afb02e132a798997687fad7e9961
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206986"
---
# <a name="frequently-asked-questions-about-apache-kafka-in-azure-hdinsight"></a>Preguntas frecuentes sobre Apache Kafka en Azure HDInsight

En este artículo se tratan algunas preguntas comunes sobre Apache Kafka en Azure HDInsight.

## <a name="what-kafka-versions-are-supported-by-hdinsight"></a>¿Qué versiones de Kafka admite HDInsight?

Obtenga más información sobre las versiones de componentes oficialmente compatibles con HDInsight en [¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?](../hdinsight-component-versioning.md#supported-hdinsight-versions). Se recomienda usar siempre la versión más reciente para garantizar el mejor rendimiento posible y la mejor experiencia del usuario.

## <a name="what-resources-are-provided-in-an-hdinsight-kafka-cluster-and-what-resources-am-i-charged-for"></a>¿Qué recursos se proporcionan en un clúster de Kafka en HDInsight y qué recursos son de pago?

Un clúster de Kafka en HDInsight incluye los siguientes recursos:

* Nodos principales
* Nodos Zookeeper
* Nodos (de trabajo) de agente 
* Azure Managed Disks conectado a los nodos de agente
* Nodos de puerta de enlace

Todos estos recursos conllevan un costo en función del [modelo de precios de HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/), excepto los nodos de puerta de enlace. No se aplicarán cargos por los nodos de puerta de enlace.

Para obtener una descripción más detallada de los distintos tipos de nodo, consulte [Arquitectura de red virtual de Azure HDInsight](../hdinsight-virtual-network-architecture.md). Los precios se basan en el uso de nodos por minuto. Los precios varían en función del tamaño del nodo, el número de nodos, el tipo de disco administrado usado y la región.

## <a name="do-apache-kafka-apis-work-with-hdinsight"></a>¿Funcionan las API de Apache Kafka con HDInsight?

Sí, HDInsight usa las API nativas de Kafka. No es necesario cambiar el código de la aplicación cliente. Consulte [Tutorial: Uso de Producer API y Consumer API de Apache Kafka](./apache-kafka-producer-consumer-api.md) para ver cómo puede usar las API de productor y consumidor basadas en Java con el clúster.

## <a name="can-i-change-cluster-configurations"></a>¿Se pueden cambiar las configuraciones del clúster?

Sí, en el portal de Ambari. Cada componente del portal tiene una sección **configs** que se puede usar para cambiar las configuraciones del componente. Algunos cambios pueden requerir que se reinicie el agente.

## <a name="what-type-of-authentication-does-hdinsight-support-for-apache-kafka"></a>¿Qué tipo de autenticación admite HDInsight para Apache Kafka?

Con [Enterprise Security Package (ESP)](../domain-joined/apache-domain-joined-architecture.md), puede obtener seguridad de nivel de tema para los clústeres de Kafka. Consulte [Tutorial: Configuración de directivas de Apache Kafka en HDInsight con Enterprise Security Package (versión preliminar)](../domain-joined/apache-domain-joined-run-kafka.md) para más información.

## <a name="is-my-data-encrypted-can-i-use-my-own-keys"></a>¿Se cifran los datos? ¿Puedo usar mis propias claves?

Todos los mensajes de Kafka en los discos administrados se cifran con [Azure Storage Service Encryption (SSE)](../../storage/common/storage-service-encryption.md). Los datos en tránsito (por ejemplo, los datos que se transmiten desde los clientes a los agentes y viceversa) no se cifran de forma predeterminada. Es posible cifrar este tráfico si [configura SSL personalmente](./apache-kafka-ssl-encryption-authentication.md). Además, HDInsight le permite administrar sus propias claves para cifrar los datos en reposo. Para obtener más información, consulte [Cifrado de disco mediante claves administradas por el cliente](../disk-encryption.md).

## <a name="how-do-i-connect-clients-to-my-cluster"></a>¿Cómo se conectan los clientes al clúster?

Para que los clientes de Kafka se comuniquen con agentes de Kafka, deben ser capaces de acceder a los agentes a través de la red. En el caso de los clústeres de HDInsight, la red virtual es el límite de seguridad. Por lo tanto, la forma más sencilla de conectar los clientes al clúster de HDInsight es crear clientes en la misma red virtual que el clúster. Los siguientes son otros posibles escenarios:

* Conexión de clientes de una red virtual de Azure diferente: empareje la red virtual del clúster y la red virtual del cliente y configure el clúster para [anunciar direcciones IP](apache-kafka-connect-vpn-gateway.md#configure-kafka-for-ip-advertising). Al usar el anuncio de direcciones IP, los clientes de Kafka deben usar direcciones IP de agente para conectarse con los agentes, en lugar de nombres de dominio completos (FQDN).

* Conexión de clientes locales: uso de una red VPN y configuración de servidores DNS personalizados, tal como se describe en [Planeamiento de una red virtual para Azure HDInsight](../hdinsight-plan-virtual-network-deployment.md).

* Creación de un punto de conexión público para el servicio de Kafka: si los requisitos de seguridad empresariales lo permiten, puede implementar un punto de conexión público para los agentes de Kafka o un punto de conexión de REST de código abierto autoadministrado con un punto de conexión público.

## <a name="can-i-add-more-disk-space-on-an-existing-cluster"></a>¿Puedo agregar más espacio en disco en un clúster existente?

Para incrementar la cantidad de espacio disponible para mensajes Kafka, puede aumentar el número de nodos. Actualmente no se admite la adición de más discos a un clúster existente.

## <a name="can-a-kafka-cluster-work-with-databricks"></a>¿Puede un clúster de Kafka trabajar con Databricks? 

Sí, los clústeres de Kafka pueden trabajar con Databricks, siempre y cuando estén en la misma red virtual. Para usar un clúster de Kafka con Databricks, cree una red virtual con un clúster de Kafka de HDInsight en ella y, después, especifique esa red virtual al crear el área de trabajo de Databricks y usar la inyección de red virtual. Para más información, consulte [Implementación de Azure Databricks en la red virtual de Azure (inyección de red virtual)](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject). Tendrá que proporcionar los nombres de agente de arranque del clúster de Kafka al crear el área de trabajo de Databricks. Para obtener información sobre cómo recuperar los nombres de los agentes de Kafka, consulte [Obtención de la información del host de Apache Zookeeper y del agente](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started#getkafkainfo).

## <a name="how-can-i-have-maximum-data-durability"></a>¿Cómo puedo conseguir la máxima durabilidad de los datos?

La durabilidad de los datos reduce al mínimo el riesgo de pérdida de mensajes. Para lograr la máxima durabilidad de los datos, se recomienda la siguiente configuración:

* Usar un factor de replicación mínimo de 3 en la mayoría de las regiones.
* Usar un factor de replicación mínimo de 4 en regiones con solo dos dominios de error.
* Deshabilitar elecciones de líderes no limpios.
* Establecer **min.insync.replicas** en 2 o más; esto cambia el número de réplicas que deben estar completamente sincronizadas con el líder antes de que pueda continuar un proceso de escritura.
* Establecer la propiedad **acks** en **all**; esta propiedad requiere que todas las réplicas confirmen todos los mensajes.

Si se configura Kafka para una mayor coherencia de datos, la disponibilidad de los agentes para producir solicitudes se verá afectada.

## <a name="can-i-replicate-my-data-to-multiple-clusters"></a>¿Puedo replicar mis datos en varios clústeres?

Sí, los datos se pueden replicar en varios clústeres mediante Kafka MirrorMaker. Vea los detalles sobre la configuración de MirrorMaker en [Replicación de temas de Apache Kafka](apache-kafka-mirroring.md). Además, existen otros proveedores y tecnologías de código abierto autoadministradas que pueden ayudar a lograr la replicación en varios clústeres, como [Brooklin](https://github.com/linkedin/Brooklin/).

## <a name="can-i-upgrade-my-cluster-how-should-i-upgrade-my-cluster"></a>¿Puedo actualizar mi clúster? ¿Cómo debo actualizar mi clúster?

Actualmente no se admiten actualizaciones locales de versiones de clúster. Para actualizar el clúster a una versión superior de Kafka, cree un nuevo clúster con la versión que desee y migre los clientes de Kafka para que usen el nuevo clúster.

## <a name="how-do-i-monitor-my-kafka-cluster"></a>¿Cómo puedo supervisar el clúster de Kafka?

Use Azure Monitor para analizar los [registros de Kafka](./apache-kafka-log-analytics-operations-management.md).

## <a name="next-steps"></a>Pasos siguientes

* [Configuración del cifrado y la autenticación de Capa de sockets seguros (SSL) para Apache Kafka en Azure HDInsight](./apache-kafka-ssl-encryption-authentication.md)
* [Uso de MirrorMaker para replicar temas de Apache Kafka con Kafka en HDInsight](./apache-kafka-mirroring.md)
