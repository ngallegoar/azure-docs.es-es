---
title: 'Mensajería de Azure Service Bus: colas, temas y suscripciones'
description: En este artículo se proporciona información general sobre las entidades de mensajería (colas, temas y suscripciones) de Azure Service Bus.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: deeebf56d6e2f4ccfac37c70170a0d1cb4d272a9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119180"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Colas, temas y suscripciones de Service Bus

Microsoft Azure Service Bus admite un conjunto de tecnologías de middleware basadas en la nube y orientadas a mensajes, incluidas una cola de mensajes de confianza y una mensajería de publicación/suscripción duradera. Estas funcionalidades de mensajería "asíncrona" pueden considerarse como características de mensajería desacopladas que admiten la publicación-suscripción, el desacoplamiento temporal y los escenarios de equilibrio de carga mediante la carga de trabajo de mensajería de Service Bus. La comunicación desacoplada ofrece muchas ventajas; por ejemplo, los clientes y servidores pueden conectarse según sea necesario y realizar sus operaciones de forma asincrónica.

Las entidades de mensajería que forman el núcleo de las funcionalidades de mensajería de Service Bus son las colas, los temas y suscripciones y las reglas o acciones.

## <a name="queues"></a>Colas

Las colas ofrecen una entrega de mensajes según el modelo *primero en entrar, primero en salir (FIFO [PEPS])* a uno o más destinatarios de la competencia. Es decir, los receptores normalmente reciben y procesan los mensajes en el orden en el que se agregaron a la cola y solo un destinatario de mensaje recibe y procesa cada uno de los mensajes. La principal ventaja del uso de colas es conseguir un "desacoplamiento temporal" de los componentes de la aplicación. En otras palabras, los productores (remitentes) y los consumidores (receptores) no tienen que enviar y recibir mensajes al mismo tiempo, ya que los mensajes se almacenan de forma duradera en la cola. El productor no tiene que esperar una respuesta del destinatario para continuar el proceso y el envío de más mensajes.

Una ventaja relacionada es la "nivelación de la carga", lo que permite a los productores y consumidores enviar y recibir mensajes con distintas velocidades. En muchas aplicaciones, la carga del sistema varía con el tiempo, mientras que el tiempo de procesamiento requerido por cada unidad de trabajo suele ser constante. La intermediación de productores y consumidores de mensajes con una cola implica que la aplicación consumidora solo necesita ser aprovisionada para administrar una carga promedio en lugar de una carga pico. La profundidad de la cola aumenta y se contrae a medida que varíe la carga entrante, lo que permite ahorrar dinero directamente en función de la cantidad de infraestructura requerida para dar servicio a la carga de la aplicación. A medida que aumenta la carga, se pueden agregar más procesos de trabajo para que puedan leerse desde la cola. Cada mensaje se procesa únicamente por uno de los procesos de trabajo. Es más, este equilibrio de carga basado en la extracción permite el uso óptimo de los equipos de trabajo aunque estos equipos difieran en términos de capacidad de procesamiento, ya que extraerán mensajes a su frecuencia máxima propia. Este patrón con frecuencia se denomina "patrón de consumo de competidor".

El uso de colas para intermediar entre los consumidores y productores de mensajes proporciona un acoplamiento no estricto inherente entre los componentes. Dado que los productores y consumidores no están relacionados entre sí, un consumidor puede actualizarse sin tener ningún efecto en el productor.

### <a name="create-queues"></a>Creación de colas

Puede crear colas mediante [Azure Portal](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), la [CLI](service-bus-quickstart-cli.md) o [plantillas de Resource Manager](service-bus-resource-manager-namespace-queue.md). Luego envía y recibe mensajes mediante un objeto [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient).

Para información rápida sobre cómo crear una cola y luego enviar y recibir mensajes desde y hacia la cola, consulte los [inicios rápidos](service-bus-quickstart-portal.md) de cada método. Para un tutorial más detallado sobre cómo usar las colas, consulte [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md).

Para un ejemplo funcional, consulte el [ejemplo BasicSendReceiveUsingQueueClient](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) en GitHub.

### <a name="receive-modes"></a>Modos de recepción

Puede especificar dos nodos distintos en los que Service Bus recibe los mensajes: *ReceiveAndDelete* o *PeekLock*. Al usar el modo [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode), la operación de recepción consta de una sola fase; es decir, cuando Service Bus recibe la solicitud del consumidor, marca el mensaje como consumido y lo devuelve a la aplicación del consumidor. El modo **ReceiveAndDelete** es el modelo más sencillo y funciona mejor para los escenarios en los que la aplicación puede tolerar no procesar un mensaje en caso de error. Para entender este escenario, pongamos una situación en la que un consumidor emite la solicitud de recepción que se bloquea antes de procesarla. Como Service Bus marca el mensaje como consumido, cuando la aplicación se reinicie y empiece a consumir mensajes de nuevo, habrá perdido el mensaje que se consumió antes del bloqueo.

En el modo [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode), la operación de recepción se convierte en una operación de dos fases que hace posible admitir aplicaciones que no toleran la pérdida de mensajes. Cuando Service Bus recibe una solicitud, busca el siguiente mensaje que se va a consumir, lo bloquea para impedir que otros consumidores lo reciban y, a continuación, lo devuelve a la aplicación. Una vez que la aplicación termina de procesar el mensaje (o lo almacena de forma fiable para su futuro procesamiento), completa la segunda fase del proceso de recepción creando la llamada [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) en el mensaje recibido. Cuando Service Bus ve la llamada **CompleteAsync**, marca el mensaje como consumido.

Si por cualquier motivo la aplicación no puede procesar el mensaje, realice la llamada al método [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) del mensaje recibido (en lugar de [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync)). Este método permite que Service Bus desbloquee el mensaje y esté disponible para que el mismo consumidor u otro vuelvan a recibirlo. En segundo lugar, hay un tiempo de espera asociado con el bloqueo y, si la aplicación no puede procesar el mensaje antes de que el tiempo de espera del bloqueo expire (por ejemplo, si la aplicación se bloquea), Service Bus desbloquea el mensaje y hace que esté disponible para recibirse de nuevo (básicamente realizando una operación [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) de forma predeterminada).

En caso de que la aplicación se bloquee después de procesar el mensaje, pero antes de que se emita la solicitud **CompleteAsync**, se volverá a enviar el mensaje a la aplicación cuando esta se reinicie. Este proceso se conoce como procesamiento *Al menos una vez*; es decir, cada mensaje se procesa como mínimo una vez. Sin embargo, en determinadas situaciones, es posible que se vuelva a entregar el mismo mensaje. Si el escenario no puede tolerar el procesamiento duplicado, se requiere una lógica adicional en la aplicación para detectar duplicados que se puedan conseguir de acuerdo con la propiedad [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) del mensaje, que permanece constante en los intentos de entrega. Esta característica se conoce como procesamiento *Exactamente una vez*.

## <a name="topics-and-subscriptions"></a>Temas y suscripciones

A diferencia de las colas, en las que un solo consumidor procesa cada mensaje, los *temas* y las *suscripciones* proporcionan una forma de comunicación de uno a varios mediante un patrón de *publicación o suscripción*. Cada mensaje publicado se pone a disposición de todas las suscripciones registradas en el tema, lo que es útil para escalar a un gran número de destinatarios. Los mensajes se envían a un tema y se entregan a una o varias suscripciones asociadas, según las reglas de filtro que se pueden establecer por suscripción. Las suscripciones pueden usar filtros adicionales para restringir los mensajes que desean recibir. Los mensajes se envían a un tema de la misma manera que se envían a una cola, pero no se reciben del tema directamente. En su lugar, se reciben de las suscripciones. Una suscripción al tema se parece a una cola virtual en que recibe copias de los mensajes que se envían al tema. Los mensajes se reciben de una suscripción exactamente de la misma forma en que se reciben de una cola.

Por medio de la comparación, la funcionalidad de envío de mensajes de una cola los asigna directamente a un tema y su funcionalidad de recepción de mensajes a una suscripción. Entre otras cosas, esta característica significa que las suscripciones admiten los mismos patrones que se han descrito en esta misma sección con respecto a las colas: consumidor en competencia, desacoplamiento temporal, nivelación de carga y equilibrio de la carga.

### <a name="create-topics-and-subscriptions"></a>Creación de temas y suscripciones

La creación de un tema es similar a la creación de una cola, como se describe en la sección anterior. Después puede enviar mensajes mediante la clase [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient). Para recibir mensajes, cree una o varias suscripciones al tema. Al igual que las colas, los mensajes se reciben de una suscripción mediante un objeto [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient), en lugar de un objeto [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient). Cree el cliente de la suscripción y pase el nombre del tema, el nombre de la suscripción y (opcionalmente) el modo de recepción como parámetros.

Si desea ver un ejemplo funcional completo, consulte el [ejemplo BasicSendReceiveUsingTopicSubscriptionClient](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) en GitHub.

### <a name="rules-and-actions"></a>Reglas y acciones

En muchos escenarios, los mensajes que tienen características específicas deben procesarse de maneras diferentes. Para permitir este procesamiento, puede configurar suscripciones para buscar los mensajes que tengan las propiedades deseadas y, después, realizar determinadas modificaciones en dichas propiedades. Mientras que las suscripciones del Service Bus ven todos los mensajes enviados al tema, solo se puede copiar un subconjunto de dichos mensajes en la cola de suscripción virtual. Este filtrado se consigue mediante los filtros de suscripción. Dichas modificaciones se denominan *acciones de filtrado*. Al crear una suscripción, se puede incluir una expresión de filtro que opere en las propiedades del mensaje, tanto en las propiedades del sistema (por ejemplo, **Label**) como en las propiedades de la aplicación personalizada (por ejemplo, **StoreName**). La expresión de filtro SQL es opcional en este caso; sin una expresión de filtro SQL, todas las acciones de filtro definidas en una suscripción se realizarán en todos los mensajes de dicha suscripción.

Si desea ver un ejemplo funcional completo, consulte el [ejemplo TopicSubscriptionWithRuleOperationsSample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) en GitHub.

Para más información sobre los valores de filtro posibles, vea la documentación de las clases [SqlFilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) y [SqlRuleAction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction).

## <a name="java-message-service-jms-20-entities-preview"></a>Entidades de Java Message Service (JMS) 2.0 (versión preliminar)

Las aplicaciones cliente que se conectan a Azure Service Bus Premium y usan la [biblioteca JMS de Azure Service Bus](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms) pueden aprovechar las siguientes entidades.

### <a name="queues"></a>Colas

Las colas de JMS se pueden comparar semánticamente con las colas de Service Bus tradicionales explicadas anteriormente.

Para crear una cola, use los métodos siguientes de la clase `JMSContext`:

```java
Queue createQueue(String queueName)
```

### <a name="topics"></a>Temas

Los temas de JMS se pueden comparar semánticamente con los temas de Service Bus tradicionales explicados anteriormente.

Para crear un tema, use los métodos siguientes de la clase `JMSContext`:

```java
Topic createTopic(String topicName)
```

### <a name="temporary-queues"></a>Colas temporales

Cuando una aplicación cliente requiere una entidad temporal que existe durante la vigencia de la aplicación, puede usar colas temporales. Estas se utilizan en el patrón de [solicitud-respuesta](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html).

Para crear una cola temporal, use los métodos siguientes de la clase `JMSContext`:

```java
TemporaryQueue createTemporaryQueue()
```

### <a name="temporary-topics"></a>Temas temporales

Al igual que las colas temporales, los temas temporales existen para habilitar la publicación o suscripción a través de una entidad temporal que existe durante la vigencia de la aplicación.

Para crear un tema temporal, use los métodos siguientes de la clase `JMSContext`:

```java
TemporaryTopic createTemporaryTopic()
```

### <a name="java-message-service-jms-subscriptions"></a>Suscripciones de Java Message Service (JMS)

Aunque estas son semánticamente similares a las suscripciones descritas anteriormente (es decir, existen en un tema y habilitan la semántica de publicación o suscripción), la especificación de Java Message Service presenta los conceptos de los atributos **Compartido**, **No compartido**, **Duradero** y **No duradero** para una suscripción determinada.

> [!NOTE]
> Las suscripciones siguientes están disponibles en el nivel Premium de Azure Service Bus para la versión preliminar de las aplicaciones cliente que se conectan a Azure Service Bus mediante la [biblioteca JMS de Azure Service Bus](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms).
>
> En el caso de la versión preliminar pública, estas suscripciones no se pueden crear mediante Azure Portal.
>

#### <a name="shared-durable-subscriptions"></a>Suscripciones duraderas compartidas

Una suscripción duradera compartida se usa cuando una aplicación va a recibir y procesar todos los mensajes publicados en un tema, independientemente de si la aplicación se utiliza de forma activa en la suscripción en todo momento.

Puesto que se trata de una suscripción compartida, cualquier aplicación autenticada para recibir de Service Bus puede recibir de la suscripción.

Para crear una suscripción duradera compartida, use los métodos siguientes de la clase `JMSContext`:

```java
JMSConsumer createSharedDurableConsumer(Topic topic, String name)

JMSConsumer createSharedDurableConsumer(Topic topic, String name, String messageSelector)
```

La suscripción duradera compartida seguirá existiendo a menos que se elimine mediante el método `unsubscribe` de la clase `JMSContext`.

```java
void unsubscribe(String name)
```

#### <a name="unshared-durable-subscriptions"></a>Suscripciones duraderas no compartidas

Al igual que una suscripción duradera compartida, una suscripción duradera no compartida se usa cuando una aplicación va a recibir y procesar todos los mensajes publicados en un tema, independientemente de si la aplicación se utiliza de forma activa en la suscripción en todo momento.

Sin embargo, puesto que se trata de una suscripción no compartida, solo la aplicación que creó la suscripción puede recibir de ella.

Para crear una suscripción duradera no compartida, use los métodos siguientes de la clase `JMSContext`: 

```java
JMSConsumer createDurableConsumer(Topic topic, String name)

JMSConsumer createDurableConsumer(Topic topic, String name, String messageSelector, boolean noLocal)
```

> [!NOTE]
> La característica `noLocal` no se admite actualmente y se omite.
>

La suscripción duradera no compartida seguirá existiendo a menos que se elimine mediante el método `unsubscribe` de la clase `JMSContext`.

```java
void unsubscribe(String name)
```

#### <a name="shared-non-durable-subscriptions"></a>Suscripciones no duraderas compartidas

Una suscripción no duradera compartida se usa cuando es necesario que varias aplicaciones cliente reciban y procesen mensajes de una suscripción única, solo hasta que se utilicen de forma activa en ella o reciban de ella.

Dado que la suscripción no es duradera, no se conserva. Esta suscripción no recibe los mensajes cuando no hay consumidores activos en la misma.

Para crear una suscripción no duradera compartida, cree `JmsConsumer` como se muestra en los siguientes métodos de la clase `JMSContext`:

```java
JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName)

JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName, String messageSelector)
```

La suscripción no duradera compartida seguirá existiendo hasta que haya consumidores activos que reciban de la misma.

#### <a name="unshared-non-durable-subscriptions"></a>Suscripciones no duraderas no compartidas

Una suscripción no duradera no compartida se usa cuando es necesario que la aplicación cliente reciba y procese un mensaje de una suscripción, solo hasta que se utilice de forma activa en ella. Solo puede existir un consumidor en esta suscripción, es decir, el cliente que creó la suscripción.

Dado que la suscripción no es duradera, no se conserva. Esta suscripción no recibe los mensajes cuando no hay ningún consumidor activo en la misma.

Para crear una suscripción no duradera no compartida, cree `JMSConsumer` como se muestra en los siguientes métodos de la clase `JMSContext: 

```java
JMSConsumer createConsumer(Destination destination)

JMSConsumer createConsumer(Destination destination, String messageSelector)

JMSConsumer createConsumer(Destination destination, String messageSelector, boolean noLocal)
```

> [!NOTE]
> La característica `noLocal` no se admite actualmente y se omite.
>

La suscripción no duradera no compartida seguirá existiendo hasta que haya un consumidor activo que reciba de la misma.

#### <a name="message-selectors"></a>Selectores de mensajes

Al igual que existen **filtros y acciones** para las suscripciones normales a Service Bus, existen **selectores de mensajes** para las suscripciones de JMS.

Los selectores de mensajes se pueden configurar en cada una de las suscripciones de JMS y existen como una condición de filtro en las propiedades del encabezado del mensaje. Solo se entregarán aquellos mensajes cuyas propiedades de encabezado coincidan con la expresión de selector de mensajes. Un valor de null o una cadena vacía indica que no hay ningún selector de mensajes para el consumidor o la suscripción de JMS.

## <a name="next-steps"></a>Pasos siguientes

Para más información y ejemplos de cómo usar la mensajería de Service Bus, consulte estos temas avanzados:

* [Introducción a la mensajería de Service Bus](service-bus-messaging-overview.md)
* [Inicio rápido: Envío y recepción de mensajes mediante Azure Portal y .NET](service-bus-quickstart-portal.md)
* [Tutorial: Actualización del inventario mediante Azure Portal y temas y suscripciones](service-bus-tutorial-topics-subscriptions-portal.md)


