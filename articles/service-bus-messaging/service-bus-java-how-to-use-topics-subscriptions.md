---
title: Uso de temas y suscripciones de Azure Service Bus con Java (azure-messaging-servicebus)
description: En este inicio rápido, puede escribir código de Java con el paquete azure-messaging-servicebus para enviar mensajes a un tema de Azure Service Bus y recibir mensajes provenientes de las suscripciones a ese tema.
ms.devlang: Java
ms.topic: quickstart
ms.date: 11/09/2020
ms.openlocfilehash: 011dbfef1155b82daca216e9519db07188260130
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96489554"
---
# <a name="send-messages-to-an-azure-service-bus-topic-and-receive-messages-from-subscriptions-to-the-topic-java"></a>Envío de mensajes a un tema de Azure Service Bus y recepción de mensajes de las suscripciones a ese tema (Java)
En este inicio rápido, puede escribir código de Java con el paquete azure-messaging-servicebus para enviar mensajes a un tema de Azure Service Bus y recibir mensajes provenientes de las suscripciones a ese tema.

> [!IMPORTANT]
> En este inicio rápido se usa el nuevo paquete azure-messaging-servicebus. Para usar el paquete azure-servicebus anterior, consulte el inicio rápido [Envío y recepción de mensajes mediante azure-servicebus](service-bus-java-how-to-use-topics-subscriptions-legacy.md).

## <a name="prerequisites"></a>Requisitos previos

- Suscripción a Azure. Para completar este tutorial, deberá tener una cuenta de Azure. Puede activar sus [beneficios de suscriptor de Visual Studio o MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) o registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Siga los pasos de [Inicio rápido: Uso de Azure Portal para crear un tema de Service Bus y suscripciones a ese tema](service-bus-quickstart-topics-subscriptions-portal.md). Anote la cadena de conexión, el nombre del tema y un nombre de suscripción. Solo usará una suscripción para este inicio rápido. 
- Instale el [SDK de Azure para Java][Azure SDK for Java]. Si usa Eclipse, puede instalar [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse], que incluye el SDK de Azure para Java. Después puede agregar las **Bibliotecas de Microsoft Azure para Java** al proyecto. Si utiliza IntelliJ, consulte [Instalación de Azure Toolkit for IntelliJ](/azure/developer/java/toolkit-for-intellij/installation). 


## <a name="send-messages-to-a-topic"></a>Envío de mensajes a un tema
En esta sección creará un proyecto de consola de Java y agregará código para enviar mensajes al tema que ha creado. 

### <a name="create-a-java-console-project"></a>Creación de un proyecto de consola de Java
Cree un proyecto de Java mediante Eclipse o la herramienta que prefiera. 

### <a name="configure-your-application-to-use-service-bus"></a>Configuración de la aplicación para usar Service Bus
Agregue una referencia a la biblioteca de Azure Service Bus. La biblioteca cliente de Java para Service Bus está disponible en el [repositorio central de Maven](https://search.maven.org/search?q=a:azure-messaging-servicebus). Puede hacer referencia a esta biblioteca con la siguiente declaración de dependencia en el archivo de proyecto de Maven:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-servicebus</artifactId>
    <version>7.0.0</version>
</dependency>
```

### <a name="add-code-to-send-messages-to-the-topic"></a>Incorporación de código para enviar mensajes al tema
1. Agregue las siguientes instrucciones `import` al principio del archivo Java. 

    ```java
    import com.azure.messaging.servicebus.*;
    import com.azure.messaging.servicebus.models.*;
    import java.util.concurrent.TimeUnit;
    import java.util.function.Consumer;
    import java.util.Arrays;
    import java.util.List;
    ```    
5. En la clase, defina las variables para almacenar la cadena de conexión y el nombre del tema, como se muestra a continuación: 

    ```java
    static String connectionString = "<NAMESPACE CONNECTION STRING>";
    static String topicName = "<TOPIC NAME>";    
    static String subName = "<SUBSCRIPTION NAME>";
    ```

    Reemplace `<NAMESPACE CONNECTION STRING>` por la cadena de conexión del espacio de nombres de Service Bus. Reemplace `<TOPIC NAME>` por el nombre del tema.
3. Agregue un método denominado `sendMessage` a la clase para enviar un mensaje al tema. 

    ```java
        static void sendMessage()
    {
        // create a Service Bus Sender client for the queue 
        ServiceBusSenderClient senderClient = new ServiceBusClientBuilder()
                .connectionString(connectionString)
                .sender()
                .topicName(topicName)
                .buildClient();
        
        // send one message to the topic
        senderClient.sendMessage(new ServiceBusMessage("Hello, World!"));
        System.out.println("Sent a single message to the topic: " + topicName);        
    }
    ```
1. Agregue un método denominado `createMessages` a la clase para crear una lista de mensajes. Normalmente, estos mensajes se obtienen de distintas partes de la aplicación. En este caso crearemos una lista de mensajes de ejemplo.

    ```java
    static List<ServiceBusMessage> createMessages()
    {
        // create a list of messages and return it to the caller
        ServiceBusMessage[] messages = {
                new ServiceBusMessage("First message"),
                new ServiceBusMessage("Second message"),
                new ServiceBusMessage("Third message")
        };
        return Arrays.asList(messages);
    }
    ```
1. Agregue un método denominado `sendMessageBatch` para enviar mensajes al tema que ha creado. Este método crea un `ServiceBusSenderClient` para el tema, invoca el método `createMessages` para obtener la lista de mensajes, prepara uno o varios lotes y envía los lotes al tema. 

```java
    static void sendMessageBatch()
    {
        // create a Service Bus Sender client for the topic 
        ServiceBusSenderClient senderClient = new ServiceBusClientBuilder()
                .connectionString(connectionString)
                .sender()
                .topicName(topicName)
                .buildClient();

        // Creates an ServiceBusMessageBatch where the ServiceBus.
        ServiceBusMessageBatch messageBatch = senderClient.createMessageBatch();        
        
        // create a list of messages
        List<ServiceBusMessage> listOfMessages = createMessages();
        
        // We try to add as many messages as a batch can fit based on the maximum size and send to Service Bus when
        // the batch can hold no more messages. Create a new batch for next set of messages and repeat until all
        // messages are sent.        
        for (ServiceBusMessage message : listOfMessages) {
            if (messageBatch.tryAddMessage(message)) {
                continue;
            }

            // The batch is full, so we create a new batch and send the batch.
            senderClient.sendMessages(messageBatch);
            System.out.println("Sent a batch of messages to the topic: " + topicName);
            
            // create a new batch
            messageBatch = senderClient.createMessageBatch();

            // Add that message that we couldn't before.
            if (!messageBatch.tryAddMessage(message)) {
                System.err.printf("Message is too large for an empty batch. Skipping. Max size: %s.", messageBatch.getMaxSizeInBytes());
            }
        }

        if (messageBatch.getCount() > 0) {
            senderClient.sendMessages(messageBatch);
            System.out.println("Sent a batch of messages to the topic: " + topicName);
        }

        //close the client
        senderClient.close();
    }
```

## <a name="receive-messages-from-a-subscription"></a>Recepción de mensajes de una suscripción
En esta sección agregará código para recuperar mensajes de una suscripción al tema. 

1. Agregue un método denominado `receiveMessages` para recibir mensajes de la suscripción. Este método crea un `ServiceBusProcessorClient` para la suscripción al especificar un controlador para procesar los mensajes y otro, para los errores. A continuación, inicia el procesador, espera unos segundos, imprime los mensajes que se reciben, y detiene y cierra el procesador.

    ```java
    // handles received messages
    static void receiveMessages() throws InterruptedException
    {
        // Consumer that processes a single message received from Service Bus
        Consumer<ServiceBusReceivedMessageContext> messageProcessor = context -> {
            ServiceBusReceivedMessage message = context.getMessage();
            System.out.println("Received message: " + message.getBody().toString() + " from the subscription: " + subName);
        };

        // Consumer that handles any errors that occur when receiving messages
        Consumer<Throwable> errorHandler = throwable -> {
            System.out.println("Error when receiving messages: " + throwable.getMessage());
            if (throwable instanceof ServiceBusReceiverException) {
                ServiceBusReceiverException serviceBusReceiverException = (ServiceBusReceiverException) throwable;
                System.out.println("Error source: " + serviceBusReceiverException.getErrorSource());
            }
        };

        // Create an instance of the processor through the ServiceBusClientBuilder
        ServiceBusProcessorClient processorClient = new ServiceBusClientBuilder()
            .connectionString(connectionString)
            .processor()
            .topicName(topicName)
            .subscriptionName(subName)
            .processMessage(messageProcessor)
            .processError(errorHandler)
            .buildProcessorClient();

        System.out.println("Starting the processor");
        processorClient.start();

        TimeUnit.SECONDS.sleep(10);
        System.out.println("Stopping and closing the processor");
        processorClient.close();        
    }
    ```
2. Actualice el método `main` para invocar los métodos `sendMessage`, `sendMessageBatch` y `receiveMessages`, y para iniciar `InterruptedException`.     

    ```java
    public static void main(String[] args) throws InterruptedException {        
        sendMessage();
        sendMessageBatch();
        receiveMessages();
    }   
    ```

## <a name="run-the-app"></a>Ejecutar la aplicación
Ejecute el programa para ver una salida similar en la siguiente salida:

```console
Sent a batch of messages to the topic: mytopic
Starting the processor
Received message: First message from the subscription: mysub
Received message: Second message from the subscription: mysub
Received message: Third message from the subscription: mysub
Stopping and closing the processor
```

En la página **Información general** del espacio de nombres de Service Bus en Azure Portal, verá el recuento de mensajes **entrantes** y **salientes**. Es posible que tenga que esperar alrededor de un minuto y luego actualizar la página para ver los valores más recientes. 

:::image type="content" source="./media/service-bus-java-how-to-use-queues/overview-incoming-outgoing-messages.png" alt-text="Recuento de mensajes entrantes y salientes" lightbox="./media/service-bus-java-how-to-use-queues/overview-incoming-outgoing-messages.png":::

Cambie a la pestaña **Temas** en el panel de la parte inferior y seleccione el tema para ver la página **Tema de Service Bus** de su tema. En esta página debería ver cuatro mensajes entrantes y cuatro salientes en el gráfico **Mensajes**. 

:::image type="content" source="./media/service-bus-java-how-to-use-topics-subscriptions/topic-page-portal.png" alt-text="Mensajes entrantes y salientes" lightbox="./media/service-bus-java-how-to-use-topics-subscriptions/topic-page-portal.png":::

Si marca la llamada `receiveMessages` en el método `main` y vuelve a ejecutar la aplicación, en la página **Tema de Service Bus** verá 8 mensajes entrantes (4 nuevos), pero cuatro mensajes salientes. 

:::image type="content" source="./media/service-bus-java-how-to-use-topics-subscriptions/updated-topic-page.png" alt-text="Página del tema actualizado" lightbox="./media/service-bus-java-how-to-use-topics-subscriptions/updated-topic-page.png":::

En esta página, si selecciona una suscripción, irá a la página **Service Bus Subscription** (Suscripción de Service Bus). En esta página puede ver el recuento de mensajes activos, el de mensajes con problemas de entrega y mucho más. En este ejemplo hay cuatro mensajes activos que todavía no ha recibido ningún receptor. 

:::image type="content" source="./media/service-bus-java-how-to-use-topics-subscriptions/active-message-count.png" alt-text="Recuento de mensajes activos" lightbox="./media/service-bus-java-how-to-use-topics-subscriptions/active-message-count.png":::

## <a name="next-steps"></a>Pasos siguientes
Consulte la documentación y los ejemplos siguientes:

- [Biblioteca cliente de Azure Service Bus para Java: léame](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/servicebus/azure-messaging-servicebus/README.md).
- [Ejemplos en GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/servicebus/azure-messaging-servicebus/src/samples)
- [Referencia de API de Java](/java/api/overview/azure/servicebus?preserve-view=true&view=azure-java-preview)


[Azure SDK for Java]: /java/api/overview/azure/
[Azure Toolkit for Eclipse]: /azure/developer/java/toolkit-for-eclipse/installation
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.azure.servicebus.sqlfilter
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.azure.servicebus.sqlfilter.sqlexpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage