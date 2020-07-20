---
title: Uso de AMQP con Java Message Service API y Azure Service Bus
description: Uso de Java Message Service (JMS) con Azure Service Bus y Advanced Message Queuing Protocol (AMQP) 1.0.
ms.topic: article
ms.date: 06/23/2020
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 04d2595951640b7fe878decfeb862863f06c17a2
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119165"
---
# <a name="use-the-java-message-service-jms-with-azure-service-bus-and-amqp-10"></a>Uso de Java Message Service (JMS) con Service Bus y AMQP 1.0

La compatibilidad con **Advanced Message Queuing Protocol (AMQP) 1.0** en Azure Service Bus significa que puede utilizar las características de mensajería asincrónica para colas, publicación o suscripción de una amplia variedad de plataformas mediante un protocolo binario eficaz. Además, puede desarrollar aplicaciones formadas por componentes creados con una mezcla de lenguajes, marcos y sistemas operativos.

En este artículo se explica cómo utilizar las características de mensajería de Azure Service Bus (colas y temas de publicación y suscripción) desde aplicaciones de Java mediante la popular API **Java Message Service (JMS)** a través del protocolo AMQP.

## <a name="get-started-with-service-bus"></a>Introducción a Service Bus
En esta guía se da por hecho que ya dispone de un espacio de nombres de Service Bus con una cola denominada `basicqueue`. Si no es así, puede [crear el espacio de nombres y la cola](service-bus-create-namespace-portal.md) con [Azure Portal](https://portal.azure.com). Para obtener más información sobre cómo crear espacios de nombres y colas de Service Bus, vea [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Los temas y colas con particiones también admiten AMQP. Para más información, consulte [Temas y colas con particiones](service-bus-partitioning.md) y [Compatibilidad de AMQP 1.0 con los temas y las colas con particiones de Service Bus](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
>

## <a name="what-jms-features-are-supported"></a>¿Qué características de JMS se admiten?

Estas son las características de JMS que se admiten en Azure Service Bus.

| Características | Nivel Estándar de Azure Service Bus: JMS 1.1 | Nivel Premium de Azure Service Bus: JMS 2.0 (versión preliminar) |
|---|---|---|
| Creación automática de entidades a través de AMQP | No compatible | **Compatible** |
| Colas | **Compatible** | **Compatible** |
| Temas | **Compatible** | **Compatible** |
| Colas temporales | No compatible <br/> (Cree una cola normal con *AutoDeleteOnIdle* en su lugar) | **Compatible** |
| Temas temporales | No compatible | **Compatible** |
| Selectores de mensajes | No compatible | **Compatible** |
| Exploradores de colas | No compatible <br/> (Use la funcionalidad *Peek* de la API de Service Bus) | **Compatible** |
| Suscripciones duraderas compartidas | **Compatible** | **Compatible**|
| Suscripciones duraderas no compartidas | No compatible | **Compatible** |
| Suscripciones no duraderas compartidas | No compatible | **Compatible** |
| Suscripciones no duraderas no compartidas | No compatible | **Compatible** |
| Cancelar suscripción para suscripciones duraderas | No compatible | **Compatible** |
| ReceiveNoWait | No compatible | **Compatible** |
| Transacciones distribuidas | No compatible | No compatible |
| Término duradero | No compatible | No compatible |

### <a name="additional-caveats-for-service-bus-standard-tier"></a>Advertencias adicionales para el nivel Estándar de Service Bus
Solo se permite un elemento **MessageProducer** o **MessageConsumer** por cada **sesión**. Si tiene que crear varios elementos **MessageProducers** o **MessageConsumers** en una aplicación, cree una **sesión** dedicada para cada uno de ellos.

## <a name="downloading-the-java-message-service-jms-client-library"></a>Descarga de la biblioteca cliente de Java Message Service (JMS)

Para conectarse con Azure Service Bus y aprovechar la API de Java Message Service (JMS) a través de AMQP, se deben utilizar las bibliotecas siguientes. Se deben agregar a la ruta de acceso de compilación con la herramienta de administración de dependencias preferida para el proyecto.

La biblioteca cliente necesaria depende del plan de tarifa que se use.

### <a name="premium-tier---jms-20-over-amqp-preview"></a>Nivel Premium: JMS 2.0 a través de AMQP (versión preliminar)

Para aprovechar todas las características en versión preliminar disponibles en el nivel Premium de Azure Service Bus, use la biblioteca [Azure-servicebus-jms](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms).

### <a name="standard-tier---jms-11-over-amqp"></a>Nivel Estándar: JMS 1.1 a través de AMQP

Para usar las características de JMS admitidas por el nivel Estándar de Service Bus, use las bibliotecas siguientes (consulte [¿Qué características de JMS se admiten?](service-bus-java-how-to-use-jms-api-amqp.md#what-jms-features-are-supported)):

* [Especificación Geronimo JMS 1.1](https://search.maven.org/artifact/org.apache.geronimo.specs/geronimo-jms_1.1_spec)
* [Cliente JMS de Qpid](https://search.maven.org/artifact/org.apache.qpid/qpid-jms-client)

> [!NOTE]
> Los nombres y las versiones de los archivos JAR de JMS pueden haber cambiado. Para más información, consulte [Qpid JMS - AMQP 1.0](https://qpid.apache.org/maven.html#qpid-jms-amqp-10).
>

## <a name="coding-java-applications"></a>Codificación de las aplicaciones Java

Una vez que se han importado las dependencias, las aplicaciones de Java se pueden escribir en un proveedor de JMS de manera independiente.

Como Azure Service Bus Standard y Premium difieren en las dependencias y el número de características de JMS que admiten, el modelo de programación es ligeramente diferente para los dos.

> [!IMPORTANT]
> En la guía siguiente se muestra cómo conectarse a Azure Service Bus dada una aplicación sencilla.
>
> Dado que la mayoría de las arquitecturas de aplicaciones empresariales pueden tener una manera personalizada de administrar las dependencias y las configuraciones, use la siguiente guía para comprender lo que necesita y adaptarse a la aplicación de forma adecuada.
>

### <a name="connecting-to-azure-service-bus-using-jms"></a>Conexión a Azure Service Bus mediante JMS

Para conectarse a Azure Service Bus mediante clientes de JMS, necesita **ConnectionString**, que está disponible en [Azure Portal](https://portal.azure.com), en la opción **Cadena de conexión principal** de las directivas de acceso compartido.


#### <a name="connecting-to-azure-service-bus-premium-over-jms-20-preview"></a>Conexión a Azure Service Bus Premium mediante JMS 2.0 (versión preliminar)

1. Creación de una instancia de `ServiceBusJmsConnectionFactorySettings`
    ```java
    ServiceBusJmsConnectionFactorySettings connFactorySettings = new ServiceBusJmsConnectionFactorySettings();

    connFactorySettings.setConnectionIdleTimeoutMS(20000);
    ```
2. Cree una instancia de `ServiceBusJmsConnectionFactory` con valor de `ServiceBusConnectionString` adecuado.
    ```java
    String ServiceBusConnectionString = "<SERVICE_BUS_CONNECTION_STRING_WITH_MANAGE_PERMISSIONS>";
    ConnectionFactory factory = new ServiceBusJmsConnectionFactory(ServiceBusConnectionString, connFactorySettings);
    ```

3. Use `ConnectionFactory` para crear `Connection` y, a continuación, `Session` 
    ```java
    Connection connection = factory.createConnection();
    Session session = connection.createSession();
    ```
    o `JMSContext` (para clientes de JMS 2.0).

    ```java
    JMSContext jmsContext = factory.createContext();
    ```

#### <a name="connecting-to-azure-service-bus-standard-over-jms-11"></a>Conexión a Azure Service Bus estándar mediante JMS 1.1

1. Inserte la configuración de Azure Service Bus en el archivo de propiedades de JNDI llamado **servicebus.properties**.
    ```properties
    # servicebus.properties - sample JNDI configuration
    
    # Register a ConnectionFactory in JNDI using the form:
    # connectionfactory.[jndi_name] = [ConnectionURL]
    connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    ```

2. Configuración del contexto de JNDI y de ConnectionFactory
    ```java
    ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);

    // set up JNDI context
    Hashtable<String, String> hashtable = new Hashtable<>();
    hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
    hashtable.put("queue.QUEUE", "BasicQueue");
    hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
    Context context = new InitialContext(hashtable);
    
    ConnectionFactory factory = (ConnectionFactory) context.lookup("SBCF");
    ```
3. Use `ConnectionFactory` para crear `Connection` y, a continuación, `Session`.
    ```java
    Connection connection - factory.createConnection(csb.getSasKeyName(), csb.getSasKey());
    Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
    ```

### <a name="write-the-jms-application"></a>Escritura de la aplicación JMS

Una vez creadas las instancias de `Session` o `JMSContext`, la aplicación puede aprovechar las conocidas API de JMS para realizar operaciones de administración y de datos.

Consulte la lista de [características de JMS admitidas](service-bus-java-how-to-use-jms-api-amqp.md#what-jms-features-are-supported) en el nivel Estándar y Premium para ver qué API se admitirán en cada nivel.

## <a name="summary"></a>Resumen
En esta guía se muestra cómo las aplicaciones cliente de Java que usan Java Message Service (JMS) a través de AMQP 1.0 pueden interactuar con Azure Service Bus.

También puede utilizar AMQP 1.0 de Service Bus desde otros lenguajes, como .NET, C, Python y PHP. Los componentes creados utilizando estos lenguajes pueden intercambiar mensajes con seguridad y fidelidad gracias a la compatibilidad de AMQP 1.0 en Service Bus.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Service Bus y las entidades de Java Message Service (JMS), consulte los vínculos siguientes: 
* [Colas, temas y suscripciones de Service Bus](service-bus-queues-topics-subscriptions.md)
* [Service Bus: entidades de Java Message Service](service-bus-queues-topics-subscriptions.md#java-message-service-jms-20-entities-preview)
* [Compatibilidad de AMQP 1.0 en Azure Service Bus](service-bus-amqp-overview.md)
* [Guía para desarrolladores sobre AMQP 1.0 de Service Bus](service-bus-amqp-dotnet.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)

