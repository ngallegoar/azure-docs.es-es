---
title: 'Tutorial: Uso de colas de Azure Storage'
description: Un tutorial sobre cómo utilizar Azure Queue service para crear y eliminar colas e insertar, obtener y eliminar mensajes.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/24/2019
ms.service: storage
ms.subservice: queues
ms.topic: tutorial
ms.reviewer: cbrooks
ms.openlocfilehash: 9cbdc5231fdc9f836f300b1a3a81a237a9efc123
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75968193"
---
# <a name="tutorial-work-with-azure-storage-queues"></a>Tutorial: Uso de las colas de Azure Storage

Azure Queue Storage implementa colas basadas en la nube para permitir la comunicación entre los componentes de una aplicación distribuida. Cada cola incluye una lista de los mensajes que un componente emisor puede agregar y un componente receptor puede procesar. Con una cola, la aplicación se puede escalar inmediatamente para satisfacer la demanda. En este artículo se muestran los pasos básicos para trabajar con una cola de Azure Storage.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> - Creación de una cuenta de Azure Storage
> - Creación de la aplicación
> - Agregar compatibilidad con el código asincrónico
> - Creación de una cola
> - Insertar mensajes en una cola
> - Retirar mensajes de la cola
> - Eliminar una cola vacía
> - Buscar argumentos de línea de comandos
> - Compilación y ejecución de la aplicación

## <a name="prerequisites"></a>Prerrequisitos

- Obtenga una copia gratuita del editor de [Visual Studio Code](https://code.visualstudio.com/download) multiplataforma.
- Descargue e instale el [SDK de .NET Core](https://dotnet.microsoft.com/download).
- Si no tiene una suscripción actual a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-an-azure-storage-account"></a>Creación de una cuenta de Azure Storage

Cree primero una cuenta de Azure Storage. Para obtener una guía detallada sobre la creación de una cuenta de almacenamiento, consulte el inicio rápido [Creación de una cuenta de almacenamiento](../common/storage-account-create.md?toc=%2Fazure%2Fstorage%2Fqueues%2Ftoc.json).

## <a name="create-the-app"></a>Creación de la aplicación

Cree una aplicación de .NET Core llamada **QueueApp**. Por motivos de sencillez, esta aplicación enviará y recibirá mensajes a través de la cola.

1. En una ventana de consola (por ejemplo, CMD, PowerShell o CLI de Azure), use el comando `dotnet new` para crear una nueva aplicación de consola con el nombre **QueueApp**. Este comando crea un sencillo proyecto "Hola mundo" de C# con un solo archivo de origen: **Program.cs**.

   ```console
   dotnet new console -n QueueApp
   ```

2. Cambie a la carpeta **QueueApp** recién creada y compile la aplicación para comprobar que todo está bien.

   ```console
   cd QueueApp
   ```

   ```console
   dotnet build
   ```

   Debería ver resultados parecidos al siguiente:

   ```output
   C:\Tutorials>dotnet new console -n QueueApp
   The template "Console Application" was created successfully.

   Processing post-creation actions...
   Running 'dotnet restore' on QueueApp\QueueApp.csproj...
     Restore completed in 155.62 ms for C:\Tutorials\QueueApp\QueueApp.csproj.

   Restore succeeded.

   C:\Tutorials>cd QueueApp

   C:\Tutorials\QueueApp>dotnet build
   Microsoft (R) Build Engine version 16.0.450+ga8dc7f1d34 for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

     Restore completed in 40.87 ms for C:\Tutorials\QueueApp\QueueApp.csproj.
     QueueApp -> C:\Tutorials\QueueApp\bin\Debug\netcoreapp2.1\QueueApp.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:02.40

   C:\Tutorials\QueueApp>_
   ```

## <a name="add-support-for-asynchronous-code"></a>Agregar compatibilidad con el código asincrónico

Como la aplicación usa recursos en la nube, el código se ejecuta de forma asincrónica. No obstante, las palabras clave **async** y **await** de C# no fueron válidas para los métodos **Main** hasta la versión C# 7.1. Puede cambiar fácilmente a ese compilador mediante una marca en el archivo **csproj**.

1. En la línea de comandos del directorio del proyecto, escriba `code .` para abrir Visual Studio Code en el directorio actual. Mantenga la ventana de la línea de comandos abierta. Habrá más comandos que ejecutar más tarde. Si se le pide que agregue los recursos de C# necesarios para compilar y depurar, haga clic en el botón **Sí**.

2. Abra el archivo **QueueApp.csproj** en el editor.

3. Agregue `<LangVersion>7.1</LangVersion>` en el primer **PropertyGroup** del archivo de compilación. Asegúrese de que solo se agrega la etiqueta **LangVersion** ya que **TargetFramework** puede ser diferente dependiendo de la versión de .NET instalada.

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">

     <PropertyGroup>
       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp2.1</TargetFramework>
       <LangVersion>7.1</LangVersion>
     </PropertyGroup>

   ...

   ```

4. Guarde el archivo **QueueApp.csproj**.

5. Abra el archivo de origen **Program.cs** y actualice el método **Main** para que se ejecute de forma asincrónica. Reemplace **void** por un valor devuelto **async Task**.

   ```csharp
   static async Task Main(string[] args)
   ```

6. Guarde el archivo **Program.cs**.

## <a name="create-a-queue"></a>Creación de una cola

1. Instale los paquetes **Microsoft.Azure.Storage.Common** y **Microsoft.Azure.Storage.Queue** en el proyecto con el comando `dotnet add package`. Ejecute los siguientes comandos dotnet desde la carpeta de proyecto en la ventana de la consola.

   ```console
   dotnet add package Microsoft.Azure.Storage.Common
   dotnet add package Microsoft.Azure.Storage.Queue
   ```

2. En la parte superior del archivo **Program.cs**, agregue los siguientes espacios de nombres justo después de la instrucción `using System;`. Esta aplicación utiliza tipos de estos espacios de nombres para conectarse a Azure Storage y trabajar con colas.

   ```csharp
   using System.Threading.Tasks;
   using Microsoft.Azure.Storage;
   using Microsoft.Azure.Storage.Queue;
   ```

3. Guarde el archivo **Program.cs**.

### <a name="get-your-connection-string"></a>Obtención de la cadena de conexión

La biblioteca cliente usa una cadena de conexión para establecer la conexión. La cadena de conexión está disponible en la sección de **configuración** de la cuenta de almacenamiento en Azure Portal.

1. En el explorador web, inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Vaya a la cuenta de almacenamiento en Azure Portal.

3. Seleccione **Claves de acceso**.

4. Haga clic en el botón **Copiar** situado a la derecha del campo **Cadena de conexión**.

![Cadena de conexión](media/storage-tutorial-queues/get-connection-string.png)

La cadena de conexión tiene el siguiente formato:

   ```
   "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net"
   ```

### <a name="add-the-connection-string-to-the-app"></a>Incorporación de la cadena de conexión a la aplicación

Agregue la cadena de conexión a la aplicación para que pueda acceder a la cuenta de almacenamiento.

1. Vuelva a Visual Studio Code.

2. En la clase **Program**, agregue un miembro `private const string connectionString =` para almacenar la cadena de conexión.

3. Después del signo igual, pegue el valor de cadena que copió anteriormente en Azure Portal. El valor **connectionString** será exclusivo para su cuenta.

4. Elimine el código de "Hola mundo" del método **Main**. El código debe ser parecido al siguiente pero con el valor de cadena de conexión exclusivo.

   ```csharp
   namespace QueueApp
   {
       class Program
       {
           private const string connectionString = "DefaultEndpointsProtocol=https; ...";

           static async Task Main(string[] args)
           {
           }
       }
   }
   ```

5. Actualice **Main** para crear un objeto **CloudQueue** que, posteriormente, se pasará a los métodos Send y Receive.

   ```csharp
        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");
        }
   ```

6. Guarde el archivo.

## <a name="insert-messages-into-the-queue"></a>Inserción de mensajes en la cola

Cree un nuevo método para enviar un mensaje a la cola. Agregue el método siguiente a la clase **Program**. Este método obtiene una referencia de cola y, después, crea una nueva cola si esta no existe mediante una llamada a [CreateIfNotExistsAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.createifnotexistsasync). A continuación, agrega el mensaje a la cola mediante una llamada a [AddMessageAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessageasync).

1. Agregue el siguiente método **SendMessageAsync** a la clase **Program**.

   ```csharp
   static async Task SendMessageAsync(CloudQueue theQueue, string newMessage)
   {
       bool createdQueue = await theQueue.CreateIfNotExistsAsync();

       if (createdQueue)
       {
           Console.WriteLine("The queue was created.");
       }

       CloudQueueMessage message = new CloudQueueMessage(newMessage);
       await theQueue.AddMessageAsync(message);
   }
   ```

2. Guarde el archivo.

El mensaje debe estar en un formato que pueda incluirse en una solicitud XML con codificación UTF-8 y puede tener hasta 64 KB de tamaño. Si un mensaje contiene datos binarios, se recomienda codificarlo en Base64.

De forma predeterminada, el tiempo de vida máximo de un mensaje se establece en 7 días. Puede especificar cualquier número positivo para el período de vida de un mensaje. Para agregar un mensaje que no caduque, use `Timespan.FromSeconds(-1)` en la llamada a **AddMessageAsync**.

```csharp
await theQueue.AddMessageAsync(message, TimeSpan.FromSeconds(-1), null, null, null);
```

## <a name="dequeue-messages"></a>Retirar mensajes de la cola

Cree un nuevo método denominado **ReceiveMessageAsync**. Este método recibe un mensaje de la cola mediante una llamada a [GetMessageAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessageasync). Una vez que el mensaje se recibe correctamente, es importante eliminarlo de la cola para que no se procese más de una vez. Una vez recibido el mensaje, elimínelo de la cola mediante una llamada a [DeleteMessageAsync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessageasync).

1. Agregue el siguiente método **ReceiveMessageAsync** a la clase **Program**.

   ```csharp
   static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
   {
       bool exists = await theQueue.ExistsAsync();

       if (exists)
       {
           CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

           if (retrievedMessage != null)
           {
               string theMessage = retrievedMessage.AsString;
               await theQueue.DeleteMessageAsync(retrievedMessage);
               return theMessage;
           }
       }
   }
   ```

2. Guarde el archivo.

## <a name="delete-an-empty-queue"></a>Eliminar una cola vacía

Al final de un proyecto es un procedimiento recomendado identificar si aún necesita los recursos que creó. Los recursos que se dejan en ejecución pueden costarle mucho dinero. Si la cola existe pero está vacía, pregunte al usuario si desea eliminarla.

1. Expanda el método **ReceiveMessageAsync** para que pregunte si desea eliminar la cola vacía.

   ```csharp
   static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
   {
       bool exists = await theQueue.ExistsAsync();

       if (exists)
       {
           CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

           if (retrievedMessage != null)
           {
               string theMessage = retrievedMessage.AsString;
               await theQueue.DeleteMessageAsync(retrievedMessage);
               return theMessage;
           }
           else
           {
               Console.Write("The queue is empty. Attempt to delete it? (Y/N) ");
               string response = Console.ReadLine();

               if (response == "Y" || response == "y")
               {
                   await theQueue.DeleteIfExistsAsync();
                   return "The queue was deleted.";
               }
               else
               {
                   return "The queue was not deleted.";
               }
           }
       }
       else
       {
           return "The queue does not exist. Add a message to the command line to create the queue and store the message.";
       }
   }
   ```

2. Guarde el archivo.

## <a name="check-for-command-line-arguments"></a>Buscar argumentos de línea de comandos

Si no se pasa ningún argumento de línea de comandos a la aplicación, se supone que hay un mensaje para agregar a la cola. Una los argumentos para crear una cadena. Agregue esta cadena a la cola de mensajes mediante una llamada al método **SendMessageAsync** que agregamos anteriormente.

Si no hay ningún argumento de línea de comandos, ejecute una operación de recuperación. Llame al método **ReceiveMessageAsync** para recuperar el primer mensaje de la cola.

Por último, espere la entrada del usuario antes de salir mediante una llamada a **Console.ReadLine**.

1. Expanda el método **Main** para que busque argumentos de línea de comandos y espere la entrada del usuario.

   ```csharp
        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");

            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendMessageAsync(queue, value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveMessageAsync(queue);
                Console.WriteLine($"Received: {value}");
            }

            Console.Write("Press Enter...");
            Console.ReadLine();
        }
   ```

2. Guarde el archivo.

## <a name="complete-code"></a>Código completo

Aquí está la lista de códigos completa de este proyecto.

   ```csharp
   using System;
   using System.Threading.Tasks;
   using Microsoft.Azure.Storage;
   using Microsoft.Azure.Storage.Queue;

   namespace QueueApp
   {
    class Program
    {
        // The string value is broken up for better onscreen formatting
        private const string connectionString = "DefaultEndpointsProtocol=https;" +
                                                "AccountName=<your storage account name>;" +
                                                "AccountKey=<your key>;" +
                                                "EndpointSuffix=core.windows.net";

        static async Task Main(string[] args)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("mystoragequeue");

            if (args.Length > 0)
            {
                string value = String.Join(" ", args);
                await SendMessageAsync(queue, value);
                Console.WriteLine($"Sent: {value}");
            }
            else
            {
                string value = await ReceiveMessageAsync(queue);
                Console.WriteLine($"Received {value}");
            }

            Console.Write("Press Enter...");
            Console.ReadLine();
        }

        static async Task SendMessageAsync(CloudQueue theQueue, string newMessage)
        {
            bool createdQueue = await theQueue.CreateIfNotExistsAsync();

            if (createdQueue)
            {
                Console.WriteLine("The queue was created.");
            }

            CloudQueueMessage message = new CloudQueueMessage(newMessage);
            await theQueue.AddMessageAsync(message);
        }

        static async Task<string> ReceiveMessageAsync(CloudQueue theQueue)
        {
            bool exists = await theQueue.ExistsAsync();

            if (exists)
            {
                CloudQueueMessage retrievedMessage = await theQueue.GetMessageAsync();

                if (retrievedMessage != null)
                {
                    string theMessage = retrievedMessage.AsString;
                    await theQueue.DeleteMessageAsync(retrievedMessage);
                    return theMessage;
                }
                else
                {
                    Console.Write("The queue is empty. Attempt to delete it? (Y/N) ");
                    string response = Console.ReadLine();

                    if (response == "Y" || response == "y")
                    {
                        await theQueue.DeleteIfExistsAsync();
                        return "The queue was deleted.";
                    }
                    else
                    {
                        return "The queue was not deleted.";
                    }
                }
            }
            else
            {
                return "The queue does not exist. Add a message to the command line to create the queue and store the message.";
            }
        }
    }
   }
   ```

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. En la línea de comandos del directorio del proyecto, ejecute el siguiente comando dotnet para compilar el proyecto.

   ```console
   dotnet build
   ```

2. Después de que el proyecto se compile correctamente, ejecute el siguiente comando para agregar el primer mensaje a la cola.

   ```console
   dotnet run First queue message
   ```

Debería ver este resultado:

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter..._
   ```

3. Ejecute la aplicación sin argumentos de línea de comandos para recibir y quitar el primer mensaje de la cola.

   ```console
   dotnet run
   ```

4. Continúe ejecutando la aplicación hasta que se eliminen todos los mensajes. Si la ejecuta más de una vez, recibirá un mensaje de que la cola está vacía y un aviso para que elimine la cola.

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Second queue message
   Sent: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Third queue message
   Sent: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   The queue is empty. Attempt to delete it? (Y/N) Y
   Received: The queue was deleted.
   Press Enter...

   C:\Tutorials\QueueApp>_
   ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

1. Creación de una cola
2. Adición y eliminación de mensajes de una cola
3. Eliminación de una cola de Azure Storage

Consulte el inicio rápido sobre colas de Azure para más información.

> [!div class="nextstepaction"]
> [Inicio rápido sobre colas](storage-quickstart-queues-portal.md)
