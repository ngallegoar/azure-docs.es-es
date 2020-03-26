---
title: 'Tutorial: Implementación de Azure Functions como módulos en Azure IoT Edge'
description: En este tutorial, va a desarrollar una función de Azure como módulo IoT Edge que luego implementará en un dispositivo perimetral.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/25/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1f4b3387375b52b28600590a099884277f3de63e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78943030"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules"></a>Tutorial: Implementación de Azure Functions como módulos IoT Edge

Azure Functions se puede usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos de Azure IoT Edge. En este tutorial, se detallan los pasos para crear e implementar una función de Azure que filtra los datos de sensor en el dispositivo IoT Edge simulado. Utilizará el dispositivo de IoT Edge simulado que creó en las guías de inicio rápido para implementar Azure IoT Edge en un dispositivo simulado con [Windows](quickstart.md) o [Linux](quickstart-linux.md). En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Usar Visual Studio Code para crear una función de Azure.
> * Usar VS Code y Docker para crear una imagen de Docker y publicarla en el registro de contenedor.
> * Implementar el módulo desde el registro de contenedor en el dispositivo IoT Edge.
> * Ver los datos filtrados.

<center>

![Diagrama: Tutorial sobre la arquitectura, la fase y la implementación del módulo de función](./media/tutorial-deploy-function/functions-architecture.png)
</center>

>[!NOTE]
>Los módulos de Azure Functions en Azure IoT Edge se encuentran en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La función de Azure que creó en este tutorial filtra los datos sobre la temperatura generados por el dispositivo. La función solo envía mensajes a los niveles superiores de Azure IoT Hub si la temperatura sobrepasa el umbral especificado.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar este tutorial, debe haber realizado el anterior para configurar el entorno de desarrollo de contenedores de Linux: [Desarrollo de módulos IoT Edge para dispositivos Linux](tutorial-develop-for-linux.md). Al completar ese tutorial, se deben cumplir los siguientes requisitos previos:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure.
* Un [dispositivo Linux que ejecute Azure IoT Edge](quickstart-linux.md).
* Un registro de contenedor, como [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) configurado con [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [CE de Docker](https://docs.docker.com/install/) configurado para ejecutar contenedores de Linux.

Para desarrollar un módulo de IoT Edge en Azure Functions, instale los siguientes requisitos previos adicionales en la máquina de desarrollo:

* [Extensión de C# para Visual Studio Code (con tecnología de OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [El SDK de .NET Core 2.1](https://www.microsoft.com/net/download).

## <a name="create-a-function-project"></a>Creación de un proyecto de aplicación de una función

Azure IoT Tools para Visual Studio Code que ha instalado en los requisitos previos proporciona funcionalidades de administración, así como algunas plantillas de código. En esta sección se usa Visual Studio Code para crear una solución IoT Edge que contiene una función de Azure.

### <a name="create-a-new-project"></a>Creación de un nuevo proyecto

Cree una plantilla de solución de función de C# que pueda personalizar con su propio código.

1. Abra Visual Studio Code en el equipo de desarrollo.

2. Para abrir la paleta de comandos de VS Code, seleccione **Ver** > **Paleta de comandos**.

3. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge). Para crear la solución, siga las indicaciones de la paleta de comandos.

   | Campo | Value |
   | ----- | ----- |
   | Seleccionar carpeta | Elija la ubicación en el equipo de desarrollo en la que VS Code creará los archivos de la solución. |
   | Proporcionar un nombre de la solución | Escriba un nombre descriptivo para la solución, como **FunctionSolution** o acepte el valor predeterminado. |
   | Seleccionar plantilla del módulo | Seleccione **Azure Functions: C#** . |
   | Proporcionar un nombre de módulo | Asigne al módulo el nombre **CSharpFunction**. |
   | Proporcionar repositorio de imágenes de Docker del módulo | Un repositorio de imágenes incluye el nombre del registro de contenedor y el nombre de la imagen de contenedor. La imagen de contenedor se rellena previamente a partir del último paso. Reemplace **localhost:5000** por el valor del servidor de inicio de sesión del registro de contenedor de Azure. Puede recuperar el servidor de inicio de sesión de la página de información general del registro de contenedor en Azure Portal. La cadena final se parece a \<nombre del Registro\>.azurecr.io/CSharpFunction. |

   ![Especificación del repositorio de imágenes de Docker](./media/tutorial-deploy-function/repository.png)

### <a name="add-your-registry-credentials"></a>Adición de las credenciales del Registro

El archivo del entorno almacena las credenciales del registro de contenedor y las comparte con el runtime de IoT Edge. El entorno de ejecución necesita estas credenciales para extraer las imágenes privadas e insertarlas en el dispositivo IoT Edge.

1. En el explorador de VS Code, abra el archivo .env.
2. Actualice los campos con los valores de **nombre de usuario** y **contraseña** que ha copiado del Registro de contenedor de Azure.
3. Guarde este archivo.

### <a name="select-your-target-architecture"></a>Selección de la arquitectura de destino

Actualmente, Visual Studio Code puede desarrollar módulos de C para dispositivos Linux AMD64 y Linux ARM32v7. Debe seleccionar qué arquitectura tiene como destino con cada solución, dado que el contenedor se crea y ejecuta de manera diferente para cada tipo de arquitectura. La opción predeterminada es Linux AMD64.

1. Abra la paleta de comandos y busque **Azure IoT Edge: Set Default Target Platform for Edge Solution** (Establecer la plataforma de destino predeterminada para la solución perimetral), o bien seleccione el icono de acceso directo en la barra lateral en la parte inferior de la ventana.

2. En la paleta de comandos, seleccione la arquitectura de destino en la lista de opciones. Para este tutorial, usamos una máquina virtual Ubuntu como dispositivo IoT Edge, por lo que mantendrá el valor predeterminado **amd64**.

### <a name="update-the-module-with-custom-code"></a>Actualización del módulo con código personalizado

Vamos a agregar código adicional para que el módulo procese los mensajes en el perímetro antes de reenviarlos a IoT Hub.

1. En Visual Studio Code, abra **modules** > **CSharpFunction** > **CSharpFunction.cs**.

1. Reemplace el contenido del archivo **CSharpFunction.cs** por el código siguiente. Este código recibe datos de telemetría sobre la temperatura ambiente y la de la máquina y solo reenvía el mensaje a IoT Hub si la temperatura de la máquina está por encima de un umbral definido.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.IO;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Client;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.EdgeHub;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;
   using Newtonsoft.Json;

   namespace Functions.Samples
   {
       public static class CSharpFunction
       {
           [FunctionName("CSharpFunction")]
           public static async Task FilterMessageAndSendMessage(
               [EdgeHubTrigger("input1")] Message messageReceived,
               [EdgeHub(OutputName = "output1")] IAsyncCollector<Message> output,
               ILogger logger)
           {
               const int temperatureThreshold = 20;
               byte[] messageBytes = messageReceived.GetBytes();
               var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

               if (!string.IsNullOrEmpty(messageString))
               {
                   logger.LogInformation("Info: Received one non-empty message");
                   // Get the body of the message and deserialize it.
                   var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

                   if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                   {
                       // Send the message to the output as the temperature value is greater than the threshold.
                       using (var filteredMessage = new Message(messageBytes))
                       {
                            // Copy the properties of the original message into the new Message object.
                            foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                            {filteredMessage.Properties.Add(prop.Key, prop.Value);}
                            // Add a new property to the message to indicate it is an alert.
                            filteredMessage.Properties.Add("MessageType", "Alert");
                            // Send the message.
                            await output.AddAsync(filteredMessage);
                            logger.LogInformation("Info: Received and transferred a message with temperature above the threshold");
                       }
                   }
               }
           }
       }
       //Define the expected schema for the body of incoming messages.
       class MessageBody
       {
           public Machine machine {get; set;}
           public Ambient ambient {get; set;}
           public string timeCreated {get; set;}
       }
       class Machine
       {
           public double temperature {get; set;}
           public double pressure {get; set;}
       }
       class Ambient
       {
           public double temperature {get; set;}
           public int humidity {get; set;}
       }
   }
   ```

1. Guarde el archivo.

## <a name="build-your-iot-edge-solution"></a>Compilación de la solución de IoT Edge

En la sección anterior se ha creado una solución de IoT Edge y se ha modificado **CSharpFunction** para filtrar los mensajes que notifiquen una temperatura de la máquina inferior al umbral aceptable. Ahora, tiene que compilar la solución como una imagen de contenedor e insertarla en el registro de contenedor.

En esta sección, proporciona las credenciales para su registro de contenedor por segunda vez (la primera fue en el archivo **.env** de su solución IoT Edge) iniciando sesión de forma local desde su máquina de desarrollo para que Visual Studio Code pueda insertar imágenes en su registro.

1. Abra el terminal integrado de VS Code, para lo que debe seleccionar **View** > **Terminal** (Ver > Terminal).

2. Inicie sesión en su registro de contenedor, para lo que debe escribir el comando siguiente en el terminal integrado. Use el nombre de usuario y el servidor de inicio de sesión que copió anteriormente de Azure Container Registry.

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```

    Cuando se le pida la contraseña, pegue la contraseña (no se podrá ver en la ventana de terminal) del registro de contenedor y presione **Entrar**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

3. En el explorador de VS Code, haga clic con el botón derecho en el archivo deployment.template.json y seleccione **Build and Push IoT Edge solution** (Compilar e insertar solución de IoT Edge).

Cuando le indica a Visual Studio Code que compile la solución, esta herramienta primero toma la información de la plantilla de implementación y genera un archivo deployment.json en una nueva carpeta denominada **config**. Después, ejecuta dos comandos en el terminal integrado: `docker build` y `docker push`. El comando build compila el código e incluye las funciones en un contenedor. Después, el comando push inserta el código en el registro de contenedor que se especificó al inicializar la solución.

## <a name="view-your-container-image"></a>Visualización de una imagen de contenedor

Visual Studio Code genera un mensaje de éxito cuando la imagen de contenedor se inserta en el registro de contenedor. Si quiere confirmar por su cuenta que la operación es correcta, puede ver la imagen en el registro.

1. En Azure Portal, vaya al registro de contenedor de Azure.
2. Seleccione **Repositorios**.
3. Verá que el repositorio **csharpfunction** se muestra en la lista. Seleccione este repositorio para ver más detalles.
4. En la sección **Etiquetas**, verá la etiqueta **0.0.1-amd64**. Esta etiqueta refleja la versión y la plataforma de la imagen que ha creado. Estos valores se establecen en el archivo module.json, en la carpeta CSharpFunction.

## <a name="deploy-and-run-the-solution"></a>Implementación y ejecución de la solución

Puede usar Azure Portal para implementar el módulo de función en un dispositivo de IoT Edge, como hizo en las guías de inicio rápido. También puede implementar y supervisar los módulos desde dentro de Visual Studio Code. En las secciones siguientes, se usa Azure IoT Tools para VS Code que se indicó en los requisitos previos. Si no lo ha hecho aún, instale la extensión ahora.

1. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub).

2. Haga clic con el botón derecho en el nombre del dispositivo IoT Edge y seleccione **Create Deployment for IoT Edge device** (Crear una implementación para un dispositivo individual).

3. Vaya a la carpeta de la solución que contiene **CSharpFunction**. Abra la carpeta config, seleccione el archivo **deployment.json** y, a continuación, haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge).

4. Actualice la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Debería ver el nuevo **CSharpFunction** en ejecución junto con el módulo **SimulatedTemperatureSensor**, así como **$edgeAgent** y **$edgeHub**. Los nuevos módulos pueden tardar unos instantes en mostrarse. El dispositivo IoT Edge debe recuperar su nueva información de implementación de IoT Hub, iniciar los nuevos contenedores y, a continuación, notificar el estado a IoT Hub.

   ![Visualización de los módulos implementados en VS Code](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-the-generated-data"></a>Visualización de los datos generados

Puede ver todos los mensajes que llegan a su centro de IoT ejecutando **Azure IoT Hub: Start Monitoring Built-in Event Endpoint** (Iniciar supervisión del punto de conexión del evento integrado) en la paleta de comandos.

También puede filtrar la vista para ver todos los mensajes que llegan al centro de IoT desde un dispositivo específico. Haga clic con el botón derecho en la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) y seleccione **Start Monitoring Built-in Event Endpoint** (Iniciar la supervisión de punto de conexión de eventos integrado).

Para detener la supervisión de mensajes, ejecute el comando **Azure IoT Hub: Stop Monitoring Built-in Event Endpoint** (Detener supervisión del punto de conexión del evento integrado) en la paleta de comandos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba.

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un módulo de Azure Functions que contiene código para filtrar datos sin procesar generados por el dispositivo IoT Edge. Cuando esté listo para compilar sus propios módulos, puede obtener más información sobre cómo [desarrollar con Azure IoT Edge para Visual Studio Code](how-to-vs-code-develop-module.md).

Puede continuar con los siguientes tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Búsqueda de promedios mediante una ventana flotante en Azure Stream Analytics](tutorial-deploy-stream-analytics.md)
