---
title: Integración con Azure SignalR Service
titleSuffix: Azure Digital Twins
description: Obtenga información acerca de cómo transmitir telemetría de Azure Digital Twins a clientes mediante Azure SignalR.
author: dejimarquis
ms.author: aymarqui
ms.date: 09/02/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 38e3526627eb4191643f8bc86b9ce5f49e41a71f
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564413"
---
# <a name="integrate-azure-digital-twins-with-azure-signalr-service"></a>Integración de Azure Digital Twins con Azure SignalR Service

En este artículo, aprenderá a integrar Azure Digital Twins con [Azure SignalR Service](../azure-signalr/signalr-overview.md).

La solución que se describe en este artículo le permitirá enviar datos de telemetría de gemelos digitales a clientes conectados, como una única página web o una aplicación móvil. Como resultado, los clientes se actualizan con métricas y estados en tiempo real de dispositivos IoT sin necesidad de sondear el servidor ni de enviar nuevas solicitudes HTTP para las actualizaciones.

## <a name="prerequisites"></a>Requisitos previos

Estos son los requisitos previos que debe completar antes de continuar:

* Antes de integrar la solución con Azure SignalR Service en este artículo, debe completar el [_**Tutorial: Conexión de una solución de un extremo a otro**_](tutorial-end-to-end.md) de Azure Digital Twins, ya que este procedimiento se basa en él. El tutorial le guiará a través de la configuración de una instancia de Azure Digital Twins que funciona con un dispositivo IoT virtual para desencadenar las actualizaciones de gemelos digitales. En este procedimiento se conectarán esas actualizaciones a una aplicación web de ejemplo mediante Azure SignalR Service.
    - También necesitará el nombre del **tema de Event Grid** que creó en el tutorial.
* Asegúrese de tener [**Node.js**](https://nodejs.org/) instalado en la máquina.

También puede iniciar sesión en [Azure Portal](https://portal.azure.com/) con su cuenta de Azure.

## <a name="solution-architecture"></a>Arquitectura de la solución

Va a adjuntar Azure SignalR Service a Azure Digital Twins a través de la ruta de acceso siguiente. Las secciones A, B y C del diagrama se toman del diagrama de arquitectura del [requisito previo del tutorial de un extremo a otro](tutorial-end-to-end.md); en este procedimiento, tomará este diagrama como base y agregará la sección D.

:::image type="content" source="media/how-to-integrate-azure-signalr/signalr-integration-topology.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo." lightbox="media/how-to-integrate-azure-signalr/signalr-integration-topology.png":::

## <a name="download-the-sample-applications"></a>Descarga de aplicaciones de ejemplo

En primer lugar, descargue las aplicaciones de ejemplo necesarias. Necesitará lo siguiente:
* [**Ejemplos de Azure Digital Twins**](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/): este ejemplo incluye la aplicación *AdtSampleApp*, que contiene dos funciones de Azure para mover datos por una instancia de Azure Digital Twins (puede obtener información detallada sobre este escenario en [*Tutorial: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md)). También contiene una aplicación de ejemplo *DeviceSimulator* que simula un dispositivo IoT y genera un nuevo valor de temperatura cada segundo. 
    - Vaya al vínculo de ejemplo y presione el botón *Descargar archivo ZIP* para descargar una copia del ejemplo en la máquina, como _**Azure_Digital_Twins_samples.zip**_. Descomprima la carpeta.
* [**Ejemplo de aplicación web de integración de SignalR**](https://docs.microsoft.com/samples/azure-samples/digitaltwins-signalr-webapp-sample/digital-twins-samples/): se trata de una aplicación web de React de ejemplo que consumirá datos de telemetría de Azure Digital Twins desde una instancia de Azure SignalR Service.
    -  Vaya al vínculo de ejemplo y presione el botón *Descargar archivo ZIP* para descargar una copia del ejemplo en la máquina, como _**Azure_Digital_Twins_SignalR_integration_web_app_sample.zip**_. Descomprima la carpeta.

[!INCLUDE [Create instance](../azure-signalr/includes/signalr-quickstart-create-instance.md)]

Deje abierto Azure Portal en la ventana del explorador, ya que lo volverá a usar en la sección siguiente.

## <a name="configure-and-run-the-azure-functions-app"></a>Configuración y ejecución de la aplicación de Azure Functions

En esta sección, configurará dos funciones de Azure:
* **negotiate**: una función de desencadenador de HTTP. Esta función usa el enlace de entrada *SignalRConnectionInfo* para generar y devolver información de conexión válida.
* **broadcast**: una función de desencadenador de [Event Grid](../event-grid/overview.md). Recibe datos de telemetría de Azure Digital Twins a través de Event Grid y usa el enlace de salida de la instancia de *SignalR* que creó en el paso anterior para transmitir el mensaje a todas las aplicaciones cliente conectadas.

En primer lugar, vaya al explorador en el que tiene abierto Azure Portal y complete los pasos siguientes para obtener la **cadena de conexión** para la instancia de SignalR que ha configurado. La necesitará para configurar las funciones.
1. Confirme que la instancia de SignalR Service que implementó anteriormente se ha creado correctamente. Para ello, búsquela por su nombre en el cuadro de búsqueda de la parte superior del portal. Seleccione la instancia para abrirla.

1. Seleccione **Claves** en el menú de la instancia para ver las cadenas de conexión para la instancia de SignalR Service.

1. Seleccione el icono para copiar la cadena de conexión principal.

    :::image type="content" source="media/how-to-integrate-azure-signalr/signalr-keys.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo." lightbox="media/how-to-integrate-azure-signalr/signalr-keys.png":::

A continuación, inicie Visual Studio (u otro editor de código de su elección) y abra la solución de código en la carpeta *Azure_Digital_Twins_samples > ADTSampleApp*. A continuación, siga estos pasos para crear las funciones:

1. Cree una nueva clase C# Sharp denominada **SignalRFunctions.cs** en el proyecto *SampleFunctionsApp*.

1. Reemplace el contenido del archivo de clase por el código siguiente:

    ```C#
    using System;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Http;
    using Microsoft.Azure.EventGrid.Models;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Extensions.Http;
    using Microsoft.Azure.WebJobs.Extensions.EventGrid;
    using Microsoft.Azure.WebJobs.Extensions.SignalRService;
    using Microsoft.Extensions.Logging;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    using System.Collections.Generic;
    
    namespace SampleFunctionsApp
    {
        public static class SignalRFunctions
        {
            public static double temperature;
    
            [FunctionName("negotiate")]
            public static SignalRConnectionInfo GetSignalRInfo(
                [HttpTrigger(AuthorizationLevel.Anonymous, "post")] HttpRequest req,
                [SignalRConnectionInfo(HubName = "dttelemetry")] SignalRConnectionInfo connectionInfo)
            {
                return connectionInfo;
            }
    
            [FunctionName("broadcast")]
            public static Task SendMessage(
                [EventGridTrigger] EventGridEvent eventGridEvent,
                [SignalR(HubName = "dttelemetry")] IAsyncCollector<SignalRMessage> signalRMessages,
                ILogger log)
            {
                JObject eventGridData = (JObject)JsonConvert.DeserializeObject(eventGridEvent.Data.ToString());
    
                log.LogInformation($"Event grid message: {eventGridData}");
    
                var patch = (JObject)eventGridData["data"]["patch"][0];
                if (patch["path"].ToString().Contains("/Temperature"))
                {
                    temperature = Math.Round(patch["value"].ToObject<double>(), 2);
                }
    
                var message = new Dictionary<object, object>
                {
                    { "temperatureInFahrenheit", temperature},
                };
        
                return signalRMessages.AddAsync(
                    new SignalRMessage
                    {
                        Target = "newMessage",
                        Arguments = new[] { message }
                    });
            }
        }
    }
    ```

1. En la ventana *Consola del Administrador de paquetes* de Visual Studio o en cualquier ventana de comandos de la máquina en la carpeta *Azure_Digital_Twins_samples\AdtSampleApp\SampleFunctionsApp*, ejecute el siguiente comando para instalar el paquete NuGet `SignalRService` en el proyecto:
    ```cmd
    dotnet add package Microsoft.Azure.WebJobs.Extensions.SignalRService --version 1.2.0
    ```

    Esto debería resolver cualquier problema de dependencia de la clase.

A continuación, publique la función en Azure, siguiendo los pasos descritos en la sección [*Publicación de la aplicación*](tutorial-end-to-end.md#publish-the-app) del tutorial *Conexión de una solución de un extremo a otro*. Puede publicarla en la misma función o servicio de aplicación que usó en el requisito previo del tutorial de un extremo a otro, o bien crear una nueva, si bien se recomienda que utilice la misma para minimizar la duplicación. Finalice también la publicación de la aplicación con los pasos siguientes:
1. Obtenga la **dirección URL del punto de conexión HTTP** de la función *negotiate*. Para ello, vaya a la página [Aplicación de funciones](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites/kind/functionapp) de Azure Portal y seleccione su aplicación de función de la lista. En el menú de la aplicación, seleccione *Funciones* y elija la función *negotiate*.

    :::image type="content" source="media/how-to-integrate-azure-signalr/functions-negotiate.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo.":::

    Pulse *Obtener la dirección URL de la función* y copie el valor **hasta _/api_ (no incluya la última instancia de _/negotiate?_ )** . Utilizará esta dirección más adelante.

    :::image type="content" source="media/how-to-integrate-azure-signalr/get-function-url.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo.":::

1. Por último, agregue la **cadena de conexión** de Azure SignalR anterior a la configuración de la aplicación de función con el siguiente comando de la CLI de Azure. El comando se puede ejecutar en [Azure Cloud Shell](https://shell.azure.com), o localmente si tiene la [CLI de Azure instalada en la máquina](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest):
 
    ```azurecli
    az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "AzureSignalRConnectionString=<your-Azure-SignalR-ConnectionString>"
    ```

    La salida de este comando imprime todas las configuraciones de aplicación configuradas para la función de Azure. Busque `AzureSignalRConnectionString` en la parte inferior de la lista para comprobar que se ha agregado.

    :::image type="content" source="media/how-to-integrate-azure-signalr/output-app-setting.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo.":::

#### <a name="connect-the-function-to-event-grid"></a>Conexión de la función a Event Grid

A continuación, suscriba la función de Azure *broadcast* al **tema de Event Grid** que creó durante el requisito previo del [*Tutorial: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md). Esto permitirá que los datos de telemetría fluyan desde el gemelo *thermostat67* a través del tema de Event Grid hasta la función, que puede transmitirlos a todos los clientes.

Para ello, creará una **suscripción de Event Grid** desde el tema de Event Grid a la función de Azure *broadcast* como punto de conexión.

En [Azure Portal](https://portal.azure.com/), busque el nombre de su tema de Event Grid en la barra de búsqueda superior para ir a él. Seleccione *+ Suscripción de eventos*.

:::image type="content" source="media/how-to-integrate-azure-signalr/event-subscription-1b.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo.":::

En la página *Crear suscripción de eventos*, rellene los campos como se indica a continuación (no se mencionan los campos rellenos de forma predeterminada):
* *DETALLES DE SUSCRIPCIONES DE EVENTOS* > **Nombre**: asigne un nombre a su suscripción de eventos.
* *DETALLES DE PUNTO DE CONEXIÓN* > **Tipo de punto de conexión**: Seleccione *Función de Azure* en las opciones del menú.
* *DETALLES DE PUNTO DE CONEXIÓN* > **Punto de conexión**: Haga clic en el vínculo *Seleccione un punto de conexión*. Se abrirá la ventana *Seleccionar la función de Azure*:
    - Rellene la **Suscripción**, **Grupo de recursos**, **Aplicación de función** y **Función** (*broadcast*). Algunos de estos campos es posible que se rellenen automáticamente después de seleccionar la suscripción.
    - Pulse **Confirmar selección**.

:::image type="content" source="media/how-to-integrate-azure-signalr/create-event-subscription.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo.":::

De nuevo en la página *Crear suscripción de eventos*, pulse **Crear**.

## <a name="configure-and-run-the-web-app"></a>Configuración y ejecución de la aplicación web

En esta sección, verá el resultado en acción. En primer lugar, iniciará la **aplicación de ejemplo del dispositivo simulado** que envía datos de telemetría mediante la instancia de Azure Digital Twins. Después, configurará la **aplicación web del cliente de ejemplo** para conectarse al flujo de Azure SignalR que ha configurado. Tras ello, debería poder ver cómo actualizan los datos la aplicación web de ejemplo en tiempo real.

### <a name="run-the-device-simulator"></a>Ejecución del simulador de dispositivos

Durante el requisito previo del tutorial de un extremo a otro, [ha configurado el simulador de dispositivos](tutorial-end-to-end.md#configure-and-run-the-simulation) para enviar datos mediante una instancia de IoT Hub a la instancia de Azure Digital Twins.

Ahora, lo único que tiene que hacer es iniciar el proyecto del simulador, que se encuentra en *Azure_Digital_Twins_samples > DeviceSimulator > DeviceSimulator.sln*. Si utiliza Visual Studio, puede abrir el proyecto y ejecutarlo con este botón en la barra de herramientas:

:::image type="content" source="media/how-to-integrate-azure-signalr/start-button-simulator.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo.":::

Se abrirá una ventana de la consola y se mostrarán los mensajes de los datos de telemetría de temperatura simulados. Estos se envían mediante la instancia de Azure Digital Twins, donde las funciones de Azure y SignalR los recopilan.

En esta consola no es preciso hacer nada más, solo dejar que se ejecute mientras se completan los pasos siguientes.

### <a name="configure-the-sample-client-web-app"></a>Configuración de la aplicación web cliente de ejemplo

A continuación, configure el **la aplicación web de integración de SignalR de ejemplo** mediante estos pasos:
1. Con Visual Studio o cualquier editor de código de su elección, abra la carpeta _**Azure_Digital_Twins_SignalR_integration_web_app_sample**_ descomprimida que descargó en la sección [*Requisitos previos*](#prerequisites).

1. Abra el archivo *src/App.js* y reemplace la dirección URL de `HubConnectionBuilder` por la dirección URL del punto de conexión HTTP de la función **negotiate** que guardó anteriormente:

    ```javascript
        const hubConnection = new HubConnectionBuilder()
            .withUrl('<URL>')
            .build();
    ```
1. En el *símbolo del sistema para desarrolladores* de Visual Studio o en cualquier ventana de comandos de la máquina, vaya a la carpeta *Azure_Digital_Twins_SignalR_integration_web_app_sample\src*. Ejecute el siguiente comando para instalar los paquetes de nodos dependientes:

    ```cmd
    npm install
    ```

A continuación, defina permisos en la aplicación de función en Azure Portal:
1. En la página [Aplicación de funciones](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites/kind/functionapp) de Azure Portal, seleccione la instancia de su aplicación de función.
1. Desplácese hacia abajo en el menú de la instancia y seleccione *CORS*. En la página CORS, escriba `http://localhost:3000` en el cuadro vacío para agregarlo como origen permitido. Active la casilla *Habilitar Access-Control-Allow-Credentials* y presione *Guardar*.

    :::image type="content" source="media/how-to-integrate-azure-signalr/cors-setting-azure-function.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo.":::

### <a name="see-the-results"></a>Ver los resultados

Para ver los resultados en acción, inicie la **aplicación web de integración de SignalR de ejemplo**. Puede hacerlo desde cualquier ventana de la consola en la ubicación *Azure_Digital_Twins_SignalR_integration_web_app_sample\src*, mediante la ejecución de este comando:

```cmd
npm start
```

Se abrirá una ventana del explorador en la que se ejecutará la aplicación de ejemplo, que muestra un medidor de temperatura visual. Una vez que se ejecute la aplicación, debe empezar a ver los valores de telemetría de temperatura del simulador de dispositivos que se propagan mediante Azure Digital Twins y que refleja la aplicación web en tiempo real.

:::image type="content" source="media/how-to-integrate-azure-signalr/signalr-webapp-output.png" alt-text="Una vista de servicios de Azure en un escenario de un extremo a otro. Muestra los datos que fluyen desde un dispositivo a IoT Hub, a través de una función de Azure (flecha B) hasta una instancia de Azure Digital Twins (sección A), y luego salen a través de Event Grid a otra función de Azure para el procesamiento (flecha C). La sección D muestra los datos que fluyen desde la misma instancia de Event Grid en la flecha C hasta una función de Azure con la etiqueta &quot;broadcast&quot;, que se comunica con otra función de Azure con la etiqueta &quot;negotiate&quot;. Ambas funciones se comunican con dispositivos del equipo.":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos creados en este artículo, siga estos pasos para eliminarlos. 

Con Azure Cloud Shell o la CLI de Azure local, puede eliminar todos los recursos de Azure de un grupo de recursos mediante el comando [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete). Al eliminar el grupo de recursos, también se eliminará lo siguiente:
* La instancia de Azure Digital Twins (del tutorial de un extremo a otro).
* La instancia de IoT Hub y el registro del dispositivo central (del tutorial de un extremo a otro).
* El tema de Event Grid y las suscripciones asociadas.
* La aplicación Azure Functions, incluidas las tres funciones y los recursos asociados, como el almacenamiento.
* La instancia de Azure SignalR.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. 

```azurecli
az group delete --name <your-resource-group>
```

Si va a eliminar la instancia de Azure Digital Twins, también puede eliminar el registro de la aplicación de Azure AD que creó para ella en el tutorial de un extremo a otro mediante este comando:

```azurecli
az ad app delete --id <your-application-ID>
```

Finalmente, elimine las carpetas de ejemplo del proyecto que descargó en la máquina local (*Azure_Digital_Twins_samples.zip* y *Azure_Digital_Twins_SignalR_integration_web_app_sample.zip*).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha configurado funciones de Azure con SignalR para transmitir eventos de telemetría de Azure Digital Twins a una aplicación cliente de ejemplo.

A continuación, puede obtener más información acerca de Azure SignalR Service:
* [*¿Qué es Azure SignalR Service?*](../azure-signalr/signalr-overview.md)

También puede obtener información acerca de la autenticación de Azure SignalR Service con Azure Functions:
* [*Autenticación de Azure SignalR Service*](../azure-signalr/signalr-tutorial-authenticate-azure-functions.md)
