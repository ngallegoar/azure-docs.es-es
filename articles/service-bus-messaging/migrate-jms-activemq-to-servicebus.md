---
title: Migrar aplicaciones de Java Message Service (JMS) de ActiveMQ a Azure Service Bus | Microsoft Docs
description: En este artículo se explica cómo migrar aplicaciones JMS existentes que interactúan con Active MQ para interactuar con Azure Service Bus.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2020
ms.author: aschhab
ms.openlocfilehash: 3da4f693f4cfec47c5456a0c5998f58f5fe02949
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122224"
---
# <a name="migrate-existing-java-message-service-jms-20-applications-from-active-mq-to-azure-service-bus"></a>Migrar aplicaciones de Java Message Service (JMS) 2.0 de Active MQ a Azure Service Bus

Azure Service Bus admite cargas de trabajo de Java/J2EE y Spring que usan la API de Java Message Service (JMS) 2.0 en el protocolo Advanced Message Queuing Protocol (AMQP).

En esta guía se describe lo que debe tener en cuenta cuando desee modificar una aplicación existente de Java Message Service (JMS) 2.0 que interactúe con un agente JMS (específicamente Apache ActiveMQ o Amazon MQ) para interactuar con Azure Service Bus.

## <a name="before-you-start"></a>Antes de comenzar

### <a name="differences-between-azure-service-bus-and-apache-activemq"></a>Diferencias entre Azure Service Bus y Apache ActiveMQ

Azure Service Bus y Apache ActiveMQ son agentes de mensajes que funcionan como proveedores JMS para que las aplicaciones del cliente envíen y reciban mensajes. Ambos permiten la semántica punto a punto con **colas** y la semántica de publicación-suscripción con **temas** y **suscripciones**. 

Aún así, hay algunas diferencias en las dos.

| Category | Active MQ | Azure Service Bus |
| --- | --- | --- |
| Aplicación por niveles | Monolito en clúster | Dos niveles <br> (Puerta de enlace + back-end) |
| Compatibilidad con protocolos | <ul> <li>AMQP</li> <li> STOMP </li> <li> OpenWire </li> </ul> | AMQP |
| Modo de aprovisionamiento | <ul> <li> laaS local </li> <li> Amazon MQ (PaaS administrado) </li> | PaaS administrado |
| Tamaño del mensaje | Configurable por el usuario | 1 MB (nivel premium) |
| Alta disponibilidad | Administrado por el cliente | Administrado por plataforma |
| Recuperación ante desastres | Administrado por el cliente | Administrado por plataforma | 

### <a name="current-supported-and-unsupported-features"></a>Características admitidas y no admitidas actualmente

[!INCLUDE [service-bus-jms-features-list](../../includes/service-bus-jms-feature-list.md)]

### <a name="caveats"></a>Advertencias

La naturaleza de dos niveles de Azure Service Bus ofrece diversas funcionalidades de continuidad del negocio (alta disponibilidad y recuperación ante desastres). Sin embargo, hay algunas consideraciones que se deben tener en cuenta al usar las características de JMS.

#### <a name="service-upgrades"></a>Actualizaciones del servicio

En el caso de las actualizaciones y los reinicios de Service Bus, se eliminarán las colas o los temas temporales.

Si la aplicación es sensible a la pérdida de datos en colas o temas temporales, se recomienda **no** usar colas o temas temporales y usar en su lugar colas, temas y suscripciones duraderos.

#### <a name="data-migration"></a>Migración de datos

Como parte de la migración o modificación de las aplicaciones cliente para interactuar con Azure Service Bus, los datos que se conservan en ActiveMQ no se migrarán a Service Bus.

Es posible que se necesite una aplicación personalizada para purgar las colas, los temas y las suscripciones de ActiveMQ y reproducir los mensajes para las colas, los temas y las suscripciones de Service Bus'.

#### <a name="authentication-and-authorization"></a>Autenticación y autorización

El Access Control basado en roles (RBAC) respaldado por Azure ActiveDirectory es el mecanismo de autenticación preferido para Azure Service Bus.

Sin embargo, dado que RBAC no se admite actualmente debido a la falta de compatibilidad con la autenticación basada en notificaciones mediante Apache QPID JMS.

Por ahora, solo se admite la autenticación con claves SAS.

## <a name="pre-migration"></a>Antes de la migración

### <a name="version-check"></a>Comprobación de versión

A continuación se muestran los componentes que se usan al escribir las aplicaciones JMS y las versiones específicas que se admiten. 

| Componentes | Versión |
|---|---|
| API de Java Message Service (JMS) | 1.1 o superior |
| Protocolo AMQP | 1.0 |

### <a name="ensure-that-amqp-ports-are-open"></a>Asegúrese de que los puertos AMQP estén abiertos.

Azure Service Bus admite la comunicación a través del protocolo AMQP. Para ello, es necesario habilitar la comunicación a través de los puertos 5671 (AMQP) y 443 (TCP). Dependiendo de dónde se hospeden las aplicaciones cliente, es posible que necesite una incidencia de soporte técnico para permitir la comunicación a través de estos puertos.

> [!IMPORTANT]
> Azure Service Bus **solo** admite el protocolo AMQP 1.0.

### <a name="set-up-enterprise-configurations-vnet-firewall-private-endpoint-etc"></a>Establecer configuraciones empresariales (VNET, firewall, punto de conexión privado, etc.).

Azure Service Bus habilita diversas características de seguridad y alta disponibilidad de la empresa. Para obtener más información, siga los vínculos de la documentación siguiente.

  * [Puntos de conexión de servicio de red virtual](service-bus-service-endpoints.md)
  * [Firewall](service-bus-ip-filtering.md)
  * [Cifrado del lado del servicio con clave administrada por el cliente (BYOK)](configure-customer-managed-key.md)
  * [Puntos de conexión privados](private-link-service.md)
  * [Autenticación y autorización](service-bus-authentication-and-authorization.md)

### <a name="monitoring-alerts-and-tracing"></a>Supervisión, alertas y seguimiento

Las métricas se publican para cada espacio de nombres de Service Bus en Azure Monitor y se pueden aprovechar para alertas y escalado dinámico de recursos asignados al espacio de nombres.

Obtenga más información sobre las diferentes métricas y cómo configurar alertas en [métricas de Service Bus en Azure Monitor](service-bus-metrics-azure-monitor.md).

Además, obtenga más información sobre [el seguimiento del lado cliente para las operaciones de datos](service-bus-end-to-end-tracing.md) y [el registro operativo o de diagnóstico para las operaciones de administración](service-bus-diagnostic-logs.md).

### <a name="metrics---newrelic"></a>Métricas: NewRelic

A continuación se muestra una práctica guía sobre qué métricas de ActiveMQ se asignan a las métricas de Azure Service Bus. A continuación se hace referencia desde NewRelic.

  * [Métricas de ActiveMQ/Amazon MQ NewRelic](https://docs.newrelic.com/docs/integrations/amazon-integrations/aws-integrations-list/aws-mq-integration)
  * [Métricas de Azure Service Bus NewRelic](https://docs.newrelic.com/docs/integrations/microsoft-azure-integrations/azure-integrations-list/azure-service-bus-monitoring-integration)

> [!NOTE]
> Actualmente, NewRelic no tiene una integración perfecta con ActiveMQ directamente, pero tienen métricas disponibles para Amazon MQ.
> Puesto que Amazon MQ se deriva de ActiveMQ, la guía siguiente asigna las métricas NewRelic de AmazonMQ a Azure Service Bus.
>

|Agrupación de métricas| Métricas de AmazonMQ/Active MQ | Métricas de Azure Service Bus |
|------------|---------------------------|--------------------------|
|Agente|`CpuUtilization`|`CPUXNS`|
|Agente|`MemoryUsage`|`WSXNS`|
|Agente|`CurrentConnectionsCount`|`activeConnections`|
|Agente|`EstablishedConnectionsCount`|`activeConnections` + `connectionsClosed`|
|Agente|`InactiveDurableTopicSubscribersCount`|Aprovechamiento de las métricas de suscripción|
|Agente|`TotalMessageCount`|Aprovechamiento del nivel de cola/tema/suscripción `activeMessages`|
|Cola o tema|`EnqueueCount`|`incomingMessages`|
|Cola o tema|`DequeueCount`|`outgoingMessages`|
|Cola|`QueueSize`|`sizeBytes`|



## <a name="migration"></a>Migración

A fin de migrar la aplicación JMS 2.0 existente para interactuar con Azure Service Bus, es necesario realizar los pasos siguientes.

### <a name="export-topology-from-activemq-and-create-the-entities-in-azure-service-bus-optional"></a>Exportar la topología desde ActiveMQ y crear las entidades en Azure Service Bus (opcional).

Para asegurarse de que las aplicaciones cliente puedan conectarse sin problemas con Azure Service Bus, la topología, que incluye colas, temas y suscripciones, debe migrarse desde **Apache ActiveMQ** a **Azure Service Bus**.

> [!NOTE]
> En el caso de las aplicaciones de Java Message Service (JMS), la creación de colas, temas y suscripciones es una operación de runtime. La mayoría de los proveedores de servicios de mensajes de Java (JMS) ofrecen la funcionalidad para crear *colas*, *temas* y *suscripciones* en runtime.
>
> Por lo tanto, el paso anterior es opcional.
>
> Para asegurarse de que la aplicación tiene los permisos necesarios para crear la topología en runtime, asegúrese de que se usa la cadena de conexión con los permisos de ***'Administración' de SAS***.

Para hacer esto 
  * Aproveche las herramientas de la línea de comandos de [ActiveMQ](https://activemq.apache.org/activemq-command-line-tools-reference) para exportar la topología.
  * Recreación de una topología mediante el uso de una [plantilla de Azure Resource Manager](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md).
  * Ejecute la plantilla de Azure Resource Manager.


### <a name="import-the-maven-dependency-for-service-bus-jms-implementation"></a>Importe la dependencia de Maven para la implementación de Service Bus JMS.

Para garantizar una conectividad sin problemas con Azure Service Bus, es necesario agregar el paquete ***Azure-ServiceBus-JMS*** como una dependencia al archivo `pom.xml` de Maven como se indica a continuación.

```xml
<dependencies>
...
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus-jms</artifactId>
    </dependency>
...
</dependencies>
```

### <a name="application-server-configuration-changes"></a>Cambios en la configuración del servidor de aplicaciones

Esta parte está personalizada para el servidor de aplicaciones que hospeda las aplicaciones cliente que se conectan a Active MQ.

#### <a name="tomcat"></a>Tomcat

Aquí comenzamos con la configuración específica de Active MQ, tal como se muestra en el archivo de `/META-INF/context.xml`.

```XML
<Context antiJARLocking="true">
    <Resource
        name="jms/ConnectionFactory"
        auth="Container"
        type="org.apache.activemq.ActiveMQConnectionFactory"
        description="JMS Connection Factory"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        brokerURL="tcp://localhost:61616"
        brokerName="LocalActiveMQBroker"
        useEmbeddedBroker="false"/>

    <Resource name="jms/topic/MyTopic"
        auth="Container"
        type="org.apache.activemq.command.ActiveMQTopic"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO"/>
    <Resource name="jms/queue/MyQueue"
        auth="Container"
        type="org.apache.activemq.command.ActiveMQQueue"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO.QUEUE"/>
</Context>
```

que se puede adaptar como se indica a continuación para apuntar a Azure Service Bus.

```xml
<Context antiJARLocking="true">
    <Resource
        name="jms/ConnectionFactory"
        auth="Container"
        type="com.microsoft.azure.servicebus.jms.ServiceBusJmsConnectionFactory"
        description="JMS Connection Factory"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        connectionString="<INSERT YOUR SERVICE BUS CONNECTION STRING HERE>"/>

    <Resource name="jms/topic/MyTopic"
        auth="Container"
        type="org.apache.qpid.jms.JmsTopic"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO"/>
    <Resource name="jms/queue/MyQueue"
        auth="Container"
        type="org.apache.qpid.jms.JmsQueue"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO.QUEUE"/>
</Context>
```

#### <a name="spring-applications"></a>Aplicaciones de spring

##### <a name="update-applicationproperties-file"></a>Actualizar el archivo de `application.properties`

Si usa una aplicación de Spring boot para conectarse a ActiveMQ.

Aquí, el objetivo es ***quitar*** las propiedades específicas de ActiveMQ del archivo de `application.properties`.

```properties
spring.activemq.broker-url=<ACTIVEMQ BROKER URL>
spring.activemq.user=<ACTIVEMQ USERNAME>
spring.activemq.password=<ACTIVEMQ PASSWORD>
```

A continuación, ***agregar*** las propiedades específicas de Service Bus al archivo de `application.properties`.

```properties
azure.servicebus.connection-string=Endpoint=myEndpoint;SharedAccessKeyName=mySharedAccessKeyName;SharedAccessKey=mySharedAccessKey
```

##### <a name="replace-the-activemqconnectionfactory-with-servicebusjmsconnectionfactory"></a>Reemplace ActiveMQConnectionFactory por ServiceBusJmsConnectionFactory.

El siguiente paso es reemplazar la instancia de ActiveMQConnectionFactory con ServiceBusJmsConnectionFactory.

> [!NOTE] 
> Los cambios de código reales son específicos de la aplicación y cómo se administran las dependencias, pero en el ejemplo siguiente se proporcionan instrucciones sobre ***qué*** debe cambiar.
>

Anteriormente, es posible que haya creado la instancia de un objeto del ActiveMQConnectionFactory como se indica a continuación.

```java

String BROKER_URL = "<URL of the hosted ActiveMQ broker>";
ConnectionFactory factory = new ActiveMQConnectionFactory(BROKER_URL);

Connection connection = factory.createConnection();
connection.start();

```

Esto se cambiaría a crear la instancia de un objeto de ServiceBusJmsConnectionFactory.

```java

ServiceBusJmsConnectionFactorySettings settings = new ServiceBusJmsConnectionFactorySettings();
String SERVICE_BUS_CONNECTION_STRING = "<Service Bus Connection string>";

ConnectionFactory factory = new ServiceBusJmsConnectionFactory(SERVICE_BUS_CONNECTION_STRING, settings);

Connection connection = factory.createConnection();
connection.start();

```

## <a name="post-migration"></a>Después de la migración

Ahora que ha modificado la aplicación para iniciar el envío y la recepción de mensajes desde Azure Service Bus, debe comprobar que funciona como se espera. Una vez hecho esto, puede continuar para refinar y modernizar la pila de aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

Aproveche [Spring Boot Starter para JMS de Azure Service Bus](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-azure-service-bus) para lograr una integración sin problemas con Azure Service Bus.

Para obtener más información sobre la mensajería de Service Bus y Java Message Service (JMS), consulte los siguientes temas:

* [JMS de Service Bus](service-bus-java-how-to-use-jms-api-amqp.md)
* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)
