---
title: 'Tutorial: Creación de una aplicación de chat de Blazor Server: Azure SignalR'
description: En este tutorial obtendrá información sobre cómo crear y modificar una aplicación Blazor Server con Azure SignalR Service.
author: JialinXin
ms.service: signalr
ms.topic: tutorial
ms.date: 09/09/2020
ms.author: jixin
ms.openlocfilehash: ea55762aa40360d8eea94223a030f08aad504206
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485399"
---
# <a name="tutorial-build-a-blazor-server-chat-app"></a>Tutorial: Compilación de una aplicación de chat de Blazor Server

En este tutorial se muestra cómo crear y modificar una aplicación Blazor Server. Aprenderá a:

> [!div class="checklist"]
> * Crear una sala de chat sencilla con una aplicación Blazor Server.
> * Modificar los componentes Razor.
> * Usar el control de eventos y el enlace de datos en los componentes.
> * Implementar rápidamente en Azure App Service con Visual Studio.
> * Migrar SignalR local a Azure SignalR Service.

## <a name="prerequisites"></a>Requisitos previos
* Instalación del [SDK para .NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0) (Versión >= 3.0.100)
* Instalación de [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) (Versión >= 16.3)
> La versión preliminar de Visual Studio 2019 también funciona e incluye el lanzamiento de la plantilla de aplicación Blazor Server más reciente, que tiene como destino la versión más reciente de .NET Core.

[¿Tiene problemas? Háganoslo saber.](https://aka.ms/asrs/qsblazor)

## <a name="build-a-local-chat-room-in-blazor-server-app"></a>Creación de una sala de chat local en una aplicación Blazor Server

Desde la versión 16.2.0 de Visual Studio 2019, Azure SignalR Service está integrado en el proceso de publicación de una aplicación web y administrar las dependencias entre la aplicación web y SignalR Service es mucho más cómodo. Puede tener la experiencia de trabajar con SignalR local en el entorno local de desarrollo y trabajar con Azure SignalR Service para Azure App Service al mismo tiempo sin cambios en el código.

1. Creación de una aplicación Blazor de chat
   
   En Visual Studio, elija Crear un nuevo proyecto-> Aplicación Blazor > (asigne un nombre a la aplicación y elija una carpeta) -> Aplicación Blazor Server. Asegúrese de que ya ha instalado el SDK para .NET Core 3.0+ para que Visual Studio reconozca correctamente la plataforma de destino.

   [ ![En crear un nuevo proyecto, se seleccionan las plantillas de la aplicación Blazor.](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-create.png) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-create.png#lightbox)
   
   O bien, ejecute cmd:
   ```dotnetcli
   dotnet new blazorserver -o BlazorChat
   ```
   
1. Agregue el archivo `BlazorChatSampleHub.cs` para implementar un elemento `Hub` para el chat.
   
   ```cs
   using System;
   using System.Threading.Tasks;
   using Microsoft.AspNetCore.SignalR;
   
   namespace BlazorChat
   {
       public class BlazorChatSampleHub : Hub
       {
           public const string HubUrl = "/chat";
   
           public async Task Broadcast(string username, string message)
           {
               await Clients.All.SendAsync("Broadcast", username, message);
           }
   
           public override Task OnConnectedAsync()
           {
               Console.WriteLine($"{Context.ConnectionId} connected");
               return base.OnConnectedAsync();
           }
   
           public override async Task OnDisconnectedAsync(Exception e)
           {
               Console.WriteLine($"Disconnected {e?.Message} {Context.ConnectionId}");
               await base.OnDisconnectedAsync(e);
           }
       }
   }
   ```
   
1. Agregue un punto de conexión para el centro en `Startup.Configure()`.
   
   ```cs
   app.UseEndpoints(endpoints =>
   {
       endpoints.MapBlazorHub();
       endpoints.MapFallbackToPage("/_Host");
       endpoints.MapHub<BlazorChatSampleHub>(BlazorChatSampleHub.HubUrl);
   });
   ```
   
1. Instale el paquete `Microsoft.AspNetCore.SignalR.Client` para usar el cliente de SignalR.

   ```dotnetcli
   dotnet add package Microsoft.AspNetCore.SignalR.Client --version 3.1.7
   ```

1. Cree el archivo `ChatRoom.razor` en la carpeta `Pages` para implementar el cliente de SignalR. Siga los pasos que se indican a continuación o simplemente copie el archivo [ChatRoom.razor](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/BlazorChat/Pages/ChatRoom.razor).

   1. Agregue un vínculo a la página y una referencia.
      
      ```razor
      @page "/chatroom"
      @inject NavigationManager navigationManager
      @using Microsoft.AspNetCore.SignalR.Client;
      ```

   1. Agregue código al nuevo cliente de SignalR para enviar y recibir mensajes.
      
      ```razor
      @code {
          // flag to indicate chat status
          private bool _isChatting = false;
          
          // name of the user who will be chatting
          private string _username;
      
          // on-screen message
          private string _message;
          
          // new message input
          private string _newMessage;
          
          // list of messages in chat
          private List<Message> _messages = new List<Message>();
          
          private string _hubUrl;
          private HubConnection _hubConnection;
      
          public async Task Chat()
          {
              // check username is valid
              if (string.IsNullOrWhiteSpace(_username))
              {
                  _message = "Please enter a name";
                  return;
              };
      
              try
              {
                  // Start chatting and force refresh UI.
                  _isChatting = true;
                  await Task.Delay(1);

                  // remove old messages if any
                  _messages.Clear();
         
                  // Create the chat client
                  string baseUrl = navigationManager.BaseUri;
      
                  _hubUrl = baseUrl.TrimEnd('/') + BlazorChatSampleHub.HubUrl;
      
                  _hubConnection = new HubConnectionBuilder()
                      .WithUrl(_hubUrl)
                      .Build();
      
                  _hubConnection.On<string, string>("Broadcast", BroadcastMessage);
      
                  await _hubConnection.StartAsync();
      
                  await SendAsync($"[Notice] {_username} joined chat room.");
              }
              catch (Exception e)
              {
                  _message = $"ERROR: Failed to start chat client: {e.Message}";
                  _isChatting = false;
              }
          }
      
          private void BroadcastMessage(string name, string message)
          {
              bool isMine = name.Equals(_username, StringComparison.OrdinalIgnoreCase);
      
              _messages.Add(new Message(name, message, isMine));
      
              // Inform blazor the UI needs updating
              StateHasChanged();
          }
      
          private async Task DisconnectAsync()
          {
              if (_isChatting)
              {
                  await SendAsync($"[Notice] {_username} left chat room.");
      
                  await _hubConnection.StopAsync();
                  await _hubConnection.DisposeAsync();
      
                  _hubConnection = null;
                  _isChatting = false;
              }
          }
      
          private async Task SendAsync(string message)
          {
              if (_isChatting && !string.IsNullOrWhiteSpace(message))
              {
                  await _hubConnection.SendAsync("Broadcast", _username, message);
      
                  _newMessage = string.Empty;
              }
          }
      
          private class Message
          {
              public Message(string username, string body, bool mine)
              {
                  Username = username;
                  Body = body;
                  Mine = mine;
              }
      
              public string Username { get; set; }
              public string Body { get; set; }
              public bool Mine { get; set; }
      
              public bool IsNotice => Body.StartsWith("[Notice]");
      
              public string CSS => Mine ? "sent" : "received";
          }
      }
      ```

   1. Agregue la parte de representación antes de `@code` para que la interfaz de usuario interactúe con el cliente de SignalR.
      
      ```razor
      <h1>Blazor SignalR Chat Sample</h1>
      <hr />
      
      @if (!_isChatting)
      {
          <p>
              Enter your name to start chatting:
          </p>
      
          <input type="text" maxlength="32" @bind="@_username" />
          <button type="button" @onclick="@Chat"><span class="oi oi-chat" aria-hidden="true"></span> Chat!</button>
      
          // Error messages
          @if (_message != null)
          {
              <div class="invalid-feedback">@_message</div>
              <small id="emailHelp" class="form-text text-muted">@_message</small>
          }
      }
      else
      {
          // banner to show current user
          <div class="alert alert-secondary mt-4" role="alert">
              <span class="oi oi-person mr-2" aria-hidden="true"></span>
              <span>You are connected as <b>@_username</b></span>
              <button class="btn btn-sm btn-warning ml-md-auto" @onclick="@DisconnectAsync">Disconnect</button>
          </div>
          // display messages
          <div id="scrollbox">
              @foreach (var item in _messages)
              {
                  @if (item.IsNotice)
                  {
                      <div class="alert alert-info">@item.Body</div>
                  }
                  else
                  {
                      <div class="@item.CSS">
                          <div class="user">@item.Username</div>
                          <div class="msg">@item.Body</div>
                      </div>
                  }
              }
              <hr />
              <textarea class="input-lg" placeholder="enter your comment" @bind="@_newMessage"></textarea>
              <button class="btn btn-default" @onclick="@(() => SendAsync(_newMessage))">Send</button>
          </div>
      }
      ```

1. Actualice `NavMenu.razor` para insertar un menú de entrada para la sala de chat en `NavMenuCssClass` como REST.

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="chatroom">
           <span class="oi oi-chat" aria-hidden="true"></span> Chat room
       </NavLink>
   </li>
   ```
   
1. Actualice `site.css` para optimizar las vistas de burbujas del área del gráfico. Anexe el código siguiente al final.

   ```css
   /* improved for chat text box */
   textarea {
       border: 1px dashed #888;
       border-radius: 5px;
       width: 80%;
       overflow: auto;
       background: #f7f7f7
   }
   
   /* improved for speech bubbles */
   .received, .sent {
       position: relative;
       font-family: arial;
       font-size: 1.1em;
       border-radius: 10px;
       padding: 20px;
       margin-bottom: 20px;
   }
   
   .received:after, .sent:after {
       content: '';
       border: 20px solid transparent;
       position: absolute;
       margin-top: -30px;
   }
   
   .sent {
       background: #03a9f4;
       color: #fff;
       margin-left: 10%;
       top: 50%;
       text-align: right;
   }
   
   .received {
       background: #4CAF50;
       color: #fff;
       margin-left: 10px;
       margin-right: 10%;
   }
   
   .sent:after {
       border-left-color: #03a9f4;
       border-right: 0;
       right: -20px;
   }
   
   .received:after {
       border-right-color: #4CAF50;
       border-left: 0;
       left: -20px;
   }
   
   /* div within bubble for name */
   .user {
       font-size: 0.8em;
       font-weight: bold;
       color: #000;
   }
   
   .msg {
       /*display: inline;*/
   }
   ```

1. Pulse <kbd>F5</kbd> para ejecutar la aplicación. Podrá conversar como se indica a continuación.

   [ ![Se muestra una animación de chat entre dos interlocutores. Ambos interlocutores se saludan.](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat.gif) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat.gif#lightbox)
   
[¿Tiene problemas? Háganoslo saber.](https://aka.ms/asrs/qsblazor)

## <a name="publish-to-azure"></a>Publicar en Azure

   Hasta ahora, la aplicación Blazor trabaja en SignalR local y, cuando se implementa en Azure App Service, se recomienda usar [Azure SignalR Service](/aspnet/core/signalr/scale?view=aspnetcore-3.1#azure-signalr-service), que permite escalar verticalmente una aplicación Blazor Server a un gran número de conexiones de SignalR simultáneas. Además, los centros de datos de alto rendimiento y alcance global del servicio SignalR ayudan significativamente a reducir la latencia ocasionada por la geografía.

> [!IMPORTANT]
> En una aplicación Blazor Server, los estados de la interfaz de usuario se mantienen en el lado servidor, lo que significa que se requiere la permanencia del servidor en este caso. Si hay un único servidor de aplicaciones, la permanencia del servidor está asegurada por diseño. Sin embargo, si hay varios servidores de aplicaciones, existe la posibilidad de que la negociación y la conexión del cliente puedan ir a distintos servidores y conducir a errores en la interfaz de usuario de la aplicación Blazor. Por lo tanto, debe habilitar la permanencia del servidor como se indica a continuación en `appsettings.json`:
> ```json
> "Azure:SignalR:ServerStickyMode": "Required"
> ```

1. Haga clic con el botón derecho en el proyecto y seleccione `Publish` (Publicar).

   * Destino: Azure
   * Destino específico: se admiten todos los tipos de **Azure App Service**.
   * App Service: cree una nueva o seleccione una instancia de App Service existente.

   [ ![La animación muestra la selección de Azure como destino y, a continuación, Azure App Service como destino específico.](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-profile.gif) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-profile.gif#lightbox)

1. Agregue la dependencia de Azure SignalR Service.

   Una vez creado el perfil de publicación, puede ver un mensaje recomendado en **Dependencias del servicio**. Haga clic en **Configurar** para crear una nueva o seleccione una instancia de Azure SignalR Service existente en el panel.

   [ ![Al publicar, se resalta el vínculo para configurar.](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-dependency.png) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-dependency.png#lightbox)

   La dependencia de servicio realizará las siguientes acciones para permitir que la aplicación cambie automáticamente a Azure SignalR Service cuando se ejecuta en Azure.

   * Actualizar [`HostingStartupAssembly`](/aspnet/core/fundamentals/host/platform-specific-configuration?view=aspnetcore-3.1) para usar Azure SignalR Service.
   * Agregar la referencia del paquete NuGet de Azure SignalR Service.
   * Actualizar las propiedades del perfil para guardar la configuración de dependencias.
   * Configurar el almacén de secretos en función de su elección.
   * Agregar la configuración de `appsettings` para que el destino de la aplicación seleccionado sea Azure SignalR Service.

   [ ![En el resumen de los cambios, se usan las casillas para seleccionar todas las dependencias.](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-dependency-summary.png) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-dependency-summary.png#lightbox)

1. Publicación de la aplicación

   Ahora está listo para publicar. Y se abrirá automáticamente la página después de que se complete la publicación. 
   > [!NOTE]
   > Es posible que no funcione inmediatamente la primera vez que se visite la página debido a la latencia de inicio de la implementación de Azure App Service. Pruebe a actualizar la página para dar algún tiempo.
   > Además, puede usar el modo de depurador del explorador con <kbd>F12</kbd> para validar que el tráfico ya se ha redireccionado a Azure SignalR Service.

   [ ![El ejemplo de chat de SignalR para Blazor muestra un cuadro de texto para indicar el nombre y un botón de chat para iniciar un chat.](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-azure.png) ](media/signalr-tutorial-build-blazor-server-chat-app/blazor-chat-azure.png#lightbox)
   
[¿Tiene problemas? Háganoslo saber.](https://aka.ms/asrs/qsblazor)

## <a name="further-topic-enable-azure-signalr-service-in-local-development"></a>Tema adicional: Habilitación de Azure SignalR Service en el desarrollo local

1. Agregue una referencia al SDK de Azure SignalR.

   ```dotnetcli
   dotnet add package Microsoft.Azure.SignalR --version 1.5.1
   ```

1. Agregue una llamada a Azure SignalR Service en `Startup.ConfigureServices()`.

   ```cs
   public void ConfigureServices(IServiceCollection services)
   {
       ...
       services.AddSignalR().AddAzureSignalR();
       ...
   }
   ```

1. Configure el valor de `ConnectionString` de Azure SignalR Service en `appsetting.json` o con la herramienta [Administrador de secretos](/aspnet/core/security/app-secrets?tabs=visual-studio&view=aspnetcore-3.1#secret-manager).

> [!NOTE]
> El paso 2 se puede reemplazar mediante el uso de [`HostingStartupAssembly`](/aspnet/core/fundamentals/host/platform-specific-configuration?view=aspnetcore-3.1) en el SDK de SignalR.
> 
> 1. Agregue la configuración para activar Azure SignalR Service en `appsetting.json`.
>    ```js
>    "Azure": {
>      "SignalR": {
>        "Enabled": true,
>        "ServerStickyMode": "Required",
>        "ConnectionString": <your-connection-string>
>      }
>    }
>    ```
> 
> 1. Asigne el ensamblado de inicio del hospedaje para que utilice el SDK de Azure SignalR. Edite `launchSettings.json` y agregue una configuración como la que se muestra a continuación dentro de `environmentVariables`.
>    ```js
>    "environmentVariables": {
>        ...,
>        "ASPNETCORE_HOSTINGSTARTUPASSEMBLIES": "Microsoft.Azure.SignalR"
>      }
>    ```

[¿Tiene problemas? Háganoslo saber.](https://aka.ms/asrs/qsblazor)

## <a name="clean-up-resources"></a>Limpieza de recursos

Para limpiar los recursos creados en este tutorial, elimine el grupo de recursos mediante Azure Portal.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una sala de chat sencilla con una aplicación Blazor Server.
> * Modificar los componentes Razor.
> * Usar el control de eventos y el enlace de datos en los componentes.
> * Implementar rápidamente en Azure App Service con Visual Studio.
> * Migrar SignalR local a Azure SignalR Service.

Más información sobre la alta disponibilidad.
> [!div class="nextstepaction"]
> [Resistencia y recuperación ante desastres](signalr-concept-disaster-recovery.md)

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a ASP.NET Core Blazor](/aspnet/core/blazor)
