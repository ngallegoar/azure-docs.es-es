---
title: Uso de Azure Portal para crear una cola de Service Bus
description: En este inicio rápido, aprenderá a crear una cola de Service Bus con Azure Portal. Después, usará una aplicación cliente de ejemplo para enviar mensajes a la cola y recibirlos desde ella.
author: spelluru
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: spelluru
ms.openlocfilehash: 0753259f76c46c5df4246008f3f80ffa5bf35747
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85337328"
---
# <a name="quickstart-use-azure-portal-to-create-a-service-bus-queue"></a>Inicio rápido: Uso de Azure Portal para crear una cola de Service Bus
En este inicio rápido se describe cómo enviar y recibir mensajes en una cola de Service Bus, cómo usar [Azure Portal][Azure portal] para crear un espacio de nombres de mensajería y una cola dentro de este, y cómo obtener las credenciales de autorización en ese espacio de nombres. El procedimiento muestra después cómo enviar y recibir mensajes de esta cola mediante la [biblioteca de .NET Standard](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus).

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, asegúrese de que tiene instalados los siguientes elementos:

- Suscripción a Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita][] antes de empezar.
- [Visual Studio 2017 Update 3 (versión 15.3, 26730.01)](https://www.visualstudio.com/vs) o posterior. Use Visual Studio para compilar un ejemplo que envíe mensajes a una cola y los reciba. El ejemplo es para probar la cola que creó mediante PowerShell. 
- [NET Core SDK](https://www.microsoft.com/net/download/windows), versión 2.0 o posterior.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="send-and-receive-messages"></a>Envío y recepción de mensajes

> [!NOTE]
> El ejemplo que se usa en esta sección para enviar y recibir mensajes es un ejemplo de .NET. Para ver ejemplos de envío y recepción de mensajes con otros lenguajes de programación, consulte los [ejemplos de Service Bus](service-bus-samples.md). 
> 
> Para obtener instrucciones paso a paso para enviar y recibir mensajes mediante varios lenguajes de programación, consulte los siguientes inicio rápido:
> - [.NET](service-bus-dotnet-get-started-with-queues.md)
> - [Java](service-bus-java-how-to-use-queues.md)
> - [Node.js con un paquete de Azure o Service Bus](service-bus-nodejs-how-to-use-queues-new-package.md)
> - [Node. js con el paquete azure-sb](service-bus-nodejs-how-to-use-queues.md)
> - [PHP](service-bus-php-how-to-use-queues.md)
> - [Python](service-bus-python-how-to-use-queues.md)
> - [Ruby](service-bus-ruby-how-to-use-queues.md)

Una vez que haya aprovisionado el espacio de nombres y la cola, y disponga de las credenciales necesarias, estará listo para enviar y recibir mensajes. Puede examinar el código en [esta carpeta de ejemplos de GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TopicFilters).

Para ejecutar el código, haga lo siguiente:

1. Clone el [repositorio de GitHub de Service Bus](https://github.com/Azure/azure-service-bus/) mediante la emisión del comando siguiente:

   ```
   git clone https://github.com/Azure/azure-service-bus.git
   ```
3. Vaya a la carpeta de ejemplos `azure-service-bus\samples\DotNet\GettingStarted\BasicSendReceiveQuickStart\BasicSendReceiveQuickStart`.
4. Copie la cadena de conexión y el nombre de la cola que obtuvo en la sección "Obtención de las credenciales de administración".
5.  En un símbolo del sistema, escriba el siguiente comando:

    ```
    dotnet build
    ```
6.  Vaya a la carpeta `bin\Debug\netcoreapp2.0`.
7.  Escriba el siguiente comando para ejecutar el programa. No olvide reemplazar `myConnectionString` por el valor que obtuvo previamente, y `myQueueName` por el nombre de la cola que creó:

    ```shell
    dotnet BasicSendReceiveQuickStart.dll -ConnectionString "myConnectionString" -QueueName "myQueueName"
    ``` 
8. Observe que se envían 10 mensajes a la cola y que, posteriormente, se reciben de ella:

   ![salida del programa](./media/service-bus-quickstart-portal/dotnet.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede usar el portal para quitar el grupo de recursos, el espacio de nombres y la cola.

## <a name="understand-the-sample-code"></a>Descripción del código de ejemplo

Esta sección contiene información más detallada acerca de lo que hace el código de ejemplo. 

### <a name="get-connection-string-and-queue"></a>Obtención de la cadena de conexión y la cola

Tanto la cadena de conexión como el nombre de la cola se usan en el método `Main()` como argumentos de la línea de comandos. `Main()` declara dos variables de cadena que contienen estos valores:

```csharp
static void Main(string[] args)
{
    string ServiceBusConnectionString = "";
    string QueueName = "";

    for (int i = 0; i < args.Length; i++)
    {
        var p = new Program();
        if (args[i] == "-ConnectionString")
        {
            Console.WriteLine($"ConnectionString: {args[i+1]}");
            ServiceBusConnectionString = args[i + 1]; 
        }
        else if(args[i] == "-QueueName")
        {
            Console.WriteLine($"QueueName: {args[i+1]}");
            QueueName = args[i + 1];
        }                
    }

    if (ServiceBusConnectionString != "" && QueueName != "")
        MainAsync(ServiceBusConnectionString, QueueName).GetAwaiter().GetResult();
    else
    {
        Console.WriteLine("Specify -Connectionstring and -QueueName to execute the example.");
        Console.ReadKey();
    }                            
}
```
 
El método `Main()` inicia entonces el bucle de mensajes asincrónicos `MainAsync()`.

### <a name="message-loop"></a>Bucle de mensajes

El método MainAsync() crea un cliente de cola con los argumentos de la línea de comandos, llama a un controlador de mensajes de recepción denominado `RegisterOnMessageHandlerAndReceiveMessages()` y envía el conjunto de mensajes:

```csharp
static async Task MainAsync(string ServiceBusConnectionString, string QueueName)
{
    const int numberOfMessages = 10;
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

    Console.WriteLine("======================================================");
    Console.WriteLine("Press any key to exit after receiving all the messages.");
    Console.WriteLine("======================================================");

    // Register QueueClient's MessageHandler and receive messages in a loop
    RegisterOnMessageHandlerAndReceiveMessages();

    // Send Messages
    await SendMessagesAsync(numberOfMessages);

    Console.ReadKey();

    await queueClient.CloseAsync();
}
```

El método `RegisterOnMessageHandlerAndReceiveMessages()` simplemente establece unas cuantas opciones del controlador de mensajes y, a continuación, llama al método `RegisterMessageHandler()` del cliente de la cola que inicia la recepción:

```csharp
static void RegisterOnMessageHandlerAndReceiveMessages()
{
    // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
        // Set it according to how many messages the application wants to process in parallel.
        MaxConcurrentCalls = 1,

        // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
        // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
        AutoComplete = false
    };

    // Register the function that will process messages
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
} 
```

### <a name="send-messages"></a>Envío de mensajes

Se producen las operaciones de creación y envío de mensajes del método `SendMessagesAsync()`:

```csharp
static async Task SendMessagesAsync(int numberOfMessagesToSend)
{
    try
    {
        for (var i = 0; i < numberOfMessagesToSend; i++)
        {
            // Create a new message to send to the queue
            string messageBody = $"Message {i}";
            var message = new Message(Encoding.UTF8.GetBytes(messageBody));

            // Write the body of the message to the console
            Console.WriteLine($"Sending message: {messageBody}");

            // Send the message to the queue
            await queueClient.SendAsync(message);
        }
    }
    catch (Exception exception)
    {
        Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
    }
}
```

### <a name="process-messages"></a>Procesamiento de mensajes

El método `ProcessMessagesAsync()` confirma, procesa y completa la recepción de los mensajes:

```csharp
static async Task ProcessMessagesAsync(Message message, CancellationToken token)
{
    // Process the message
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

    // Complete the message so that it is not received again.
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
}
```
> [!NOTE]
> Puede administrar los recursos de Service Bus con el [Explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer/). El Explorador de Service Bus permite a los usuarios conectarse a un espacio de nombres de Service Bus y administrar las entidades de mensajería de una forma sencilla. La herramienta dispone de características avanzadas, como la funcionalidad de importación y exportación o la capacidad de probar el tema, las colas, las suscripciones, los servicios de retransmisión, los centros de notificaciones y los centros de eventos. 

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado un espacio de nombres de Service Bus y otros recursos necesarios para enviar y recibir mensajes de una cola. Para más información sobre cómo escribir código para enviar y recibir mensajes, continúe con los tutoriales de la sección **Envío y recepción de mensajes**. 

> [!div class="nextstepaction"]
> [Envío y recepción de mensajes](service-bus-dotnet-get-started-with-queues.md)


[cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[Azure portal]: https://portal.azure.com/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png
