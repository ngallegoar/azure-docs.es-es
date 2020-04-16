---
title: Envío o recepción de eventos desde Azure Event Hubs mediante Java (más reciente)
description: Este artículo es un tutorial para crear una aplicación de Java que envíe eventos a Azure Event Hubs, o los reciba de él, mediante el paquete azure-messaging-eventhubs más reciente.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: 824244c0c3247e5a218c1551dd95de6e1d6e1007
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419228"
---
# <a name="use-java-to-send-events-to-or-receive-events-from-azure-event-hubs-azure-messaging-eventhubs"></a>Use Java para enviar eventos a Azure Event Hubs o recibir eventos de él (azure-messaging-eventhubs)
En este inicio rápido se muestra cómo enviar y recibir eventos desde un centro de eventos mediante el paquete de Java **azure-messaging-eventhubs**.

> [!IMPORTANT]
> Este inicio rápido usa el nuevo paquete **azure-messaging-eventhubs**. Para ver un inicio rápido que use los paquetes **azure-eventhubs** y **azure-eventhubs-eph** anteriores, consulte [Envío y recepción de eventos mediante azure-eventhubs y azure-eventhubs-eph](event-hubs-java-get-started-send.md). 


## <a name="prerequisites"></a>Prerrequisitos
Si es la primera vez que usa Azure Event Hubs, consulte la [información general de Event Hubs](event-hubs-about.md) antes de continuar con este inicio rápido. 

Para completar este tutorial de inicio rápido, debe cumplir los siguientes requisitos previos:

- Una **suscripción a Microsoft Azure**. Para usar los servicios de Azure, entre los que se incluye Azure Event Hubs, se necesita una suscripción.  Si no se dispone de una cuenta de Azure, es posible registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/), o bien usar las ventajas que disfrutan los suscriptores MSDN al [crear una cuenta](https://azure.microsoft.com).
- Un entorno de desarrollo de Java. Este inicio rápido utiliza [Eclipse](https://www.eclipse.org/). Se requiere el kit de desarrollo de Java (JDK), versión 8 o posteriores. 
- **Creación de un espacio de nombres de Event Hubs y un centro de eventos**. El primer paso consiste en usar [Azure Portal](https://portal.azure.com) para crear un espacio de nombres de tipo Event Hubs y obtener las credenciales de administración que la aplicación necesita para comunicarse con el centro de eventos. Para crear un espacio de nombres y un centro de eventos, siga el procedimiento que se indica en [este artículo](event-hubs-create.md). Después, obtenga la **cadena de conexión para el espacio de nombres de Event Hubs**. Para ello, siga las instrucciones del artículo: [Obtenga la cadena de conexión](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). La utilizará más adelante en este inicio rápido.

## <a name="send-events"></a>Envío de eventos 
En esta sección se muestra cómo crear una aplicación de Java para enviar eventos a un centro de eventos. 

### <a name="add-reference-to-azure-event-hubs-library"></a>Incorporación de una referencia a la biblioteca de Azure Event Hubs

La biblioteca de cliente de Java para Event Hubs está disponible en [repositorio central de Maven](https://search.maven.org/search?q=a:azure-messaging-eventhubs). Puede hacer referencia a esta biblioteca con la siguiente declaración de dependencia en el archivo de proyecto de Maven:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-eventhubs</artifactId>
    <version>5.0.1</version>
</dependency>
```

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Escritura de código para enviar mensajes al centro de eventos

Para el ejemplo siguiente, primero cree un nuevo proyecto de Maven para una aplicación de consola o shell en su entorno de desarrollo de Java favorito. Agregue una clase denominada `Sender` y agréguele el código siguiente:

```java
import com.azure.messaging.eventhubs.*;
import static java.nio.charset.StandardCharsets.UTF_8;

public class Sender {
       public static void main(String[] args) {
    }
}
```

### <a name="connection-string-and-event-hub"></a>Cadena de conexión y centro de eventos
Este código usa la cadena de conexión al espacio de nombres de Event Hubs y el nombre del centro de eventos para compilar un cliente de Event Hubs. 

```java
String connectionString = "<CONNECTION STRING to EVENT HUBS NAMESPACE>";
String eventHubName = "<EVENT HUB NAME>";
```

### <a name="create-an-event-hubs-producer-client"></a>Creación de un cliente productor de Event Hubs 
Este código crea un objeto cliente productor que se usa para generar o enviar eventos al centro de eventos. 

```java
EventHubProducerClient producer = new EventHubClientBuilder()
    .connectionString(connectionString, eventHubName)
    .buildProducerClient();
```

### <a name="prepare-a-batch-of-events"></a>Preparación de un lote de eventos
Este código prepara un lote de eventos. 

```java
EventDataBatch batch = producer.createBatch();
batch.tryAdd(new EventData("First event"));
batch.tryAdd(new EventData("Second event"));
batch.tryAdd(new EventData("Third event"));
batch.tryAdd(new EventData("Fourth event"));
batch.tryAdd(new EventData("Fifth event"));
```

### <a name="send-the-batch-of-events-to-the-event-hub"></a>Envío del lote de eventos al centro de eventos
Este código envía al centro de eventos el lote de eventos que preparó en el paso anterior. El siguiente código se bloquea en la operación de envío. 

```java
producer.send(batch);
```

### <a name="close-and-cleanup"></a>Cierre y limpieza
Este código cierra el productor. 

```java
producer.close();
```
### <a name="complete-code-to-send-events"></a>Código completo para enviar eventos
Este es el código completo para enviar eventos al centro de eventos. 

```java
import com.azure.messaging.eventhubs.*;

public class Sender {
    public static void main(String[] args) {
        final String connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
        final String eventHubName = "EVENT HUB NAME";

        // create a producer using the namespace connection string and event hub name
        EventHubProducerClient producer = new EventHubClientBuilder()
            .connectionString(connectionString, eventHubName)
            .buildProducerClient();

        // prepare a batch of events to send to the event hub    
        EventDataBatch batch = producer.createBatch();
        batch.tryAdd(new EventData("First event"));
        batch.tryAdd(new EventData("Second event"));
        batch.tryAdd(new EventData("Third event"));
        batch.tryAdd(new EventData("Fourth event"));
        batch.tryAdd(new EventData("Fifth event"));

        // send the batch of events to the event hub
        producer.send(batch);

        // close the producer
        producer.close();
    }
}
```

Compile el programa y asegúrese de que no hay errores. Este programa se ejecutará después de ejecutar el programa del destinatario. 

## <a name="receive-events"></a>Recepción de eventos
El código de este tutorial se basa en el [ejemplo EventProcessorClient de GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/EventProcessorClientSample.java), que se puede examinar para ver toda la aplicación en funcionamiento.

> [!NOTE]
> Si trabaja en Azure Stack Hub, esa plataforma puede admitir una versión diferente del SDK de Blob Storage que las que suelen estar disponibles en Azure. Por ejemplo, si trabaja [en la versión 2002 de Azure Stack Hub](https://docs.microsoft.com/azure-stack/user/event-hubs-overview), la versión más alta disponible para el servicio Storage es 2017-11-09. En este caso, además de seguir los pasos de esta sección, también tendrá que agregar código para usar como destino la versión 2017-11-09 de la API del servicio de almacenamiento. Consulte [este ejemplo en GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithCustomStorageVersion.java) para ver cómo usar como destino una versión específica de la API de Storage. Para más información sobre las versiones del servicio Azure Storage que se admiten en Azure Stack Hub, consulte [Almacenamiento de Azure Stack Hub: diferencias y consideraciones](https://docs.microsoft.com/azure-stack/user/azure-stack-acs-differences).

### <a name="create-a-java-project"></a>Creación de un proyecto en Java

La biblioteca de cliente de Java para Event Hubs está disponible para su uso en proyectos de Maven en el [repositorio central de Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22), y se puede hacer referencia a ella mediante la siguiente declaración de dependencia en el archivo de proyecto de Maven: 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs</artifactId>
        <version>5.0.1</version>
    </dependency>
</dependencies>
```

1. Cree una clase nueva denominada `Receiver`con el código siguiente. Reemplace los siguientes marcadores de posición por los valores que usó al crear el centro de eventos y la cuenta de almacenamiento:
   
   ```java
     import com.azure.messaging.eventhubs.*;
     import com.azure.messaging.eventhubs.models.ErrorContext;
     import com.azure.messaging.eventhubs.models.EventContext;
     import java.util.concurrent.TimeUnit;
     import java.util.function.Consumer;
    
     public class Receiver {
    
         final static String connectionString = "<EVENT HUBS NAMESPACE - CONNECTION STRING>";
         final static String eventHubName = "<EVENT HUB NAME>";
         
         public static void main(String[] args) throws Exception {
    
             // function to process events
             Consumer<EventContext> processEvent = eventContext  -> {
                 System.out.print("Received event: ");
                 // print the body of the event
                 System.out.println(eventContext.getEventData().getBodyAsString());
                 eventContext.updateCheckpoint();
             };
    
             // function to process errors
             Consumer<ErrorContext> processError = errorContext -> {
                 // print the error message
                 System.out.println(errorContext.getThrowable().getMessage());
             };
    
            
             EventProcessorClient eventProcessorClient = new EventProcessorClientBuilder()
                     .connectionString(connectionString, eventHubName)
                     .processEvent(processEvent)
                     .processError(processError)
                     .consumerGroup(EventHubClientBuilder.DEFAULT_CONSUMER_GROUP_NAME)
                     .checkpointStore(new InMemoryCheckpointStore())
                     .buildEventProcessorClient();
    
             System.out.println("Starting event processor");
             eventProcessorClient.start();
    
             System.out.println("Press enter to stop.");
             System.in.read();
    
             System.out.println("Stopping event processor");
             eventProcessorClient.stop();
             System.out.println("Event processor stopped.");
    
             System.out.println("Exiting process");
         }
     }
    ```
    
2. Descargue el archivo **InMemoryCheckpointStore.jav** de [GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/InMemoryCheckpointStore.java) y agréguelo a su proyecto. 
3. Compile el programa y asegúrese de que no hay errores. 

## <a name="run-the-applications"></a>Ejecución de las aplicaciones
1. Ejecute primero la aplicación del **destinatario**.
1. Luego, ejecute la aplicación del **remitente**. 
1. En la ventana de la aplicación del **destinatario**, confirme que ve los eventos publicados por la aplicación del remitente.
1. Presione **ENTRAR** en la ventana de la aplicación del destinatario para detener la aplicación. 

## <a name="next-steps"></a>Pasos siguientes
Consulte [Ejemplos del SDK de Java en GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)

