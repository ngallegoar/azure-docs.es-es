---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: f0e83548d3ba3b353b471e2e3429a23421aec7b2
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095491"
---
### <a name="create-a-web-project"></a>Creación de un proyecto web

1. En **Visual Studio**, seleccione **Archivo** > **Nueva solución**.

1. Seleccione **.NET Core** > **Aplicación** > **ASP.NET Core** > **API** > **Siguiente**.
  
1. En el cuadro de diálogo **Configure your new ASP.NET Core Web API** (Configure su nueva API web de ASP.NET Core), seleccione la **plataforma de destino** **.NET Core 3.1**.

1. Escriba *PushDemoApi* como **nombre del proyecto** y, luego, seleccione **Crear**.

1. Inicie la depuración (**Comando** + **Entrar**) para probar la aplicación con plantilla.

    > [!NOTE]
    > La aplicación con plantilla está configurada para usar **WeatherForecastController** como *launchUrl*. Esto se establece en **Propiedades** > **launchSettings.json**.  
    >
    > Si aparece un mensaje que le indica que **se ha encontrado un certificado de desarrollo no válido**:
    >
    > 1. Haga clic en **Sí** para aceptar la ejecución de la herramienta "dotnet dev-certs https" para corregirlo. La herramienta "dotnet dev-certs https" le pide que especifique una contraseña para el certificado y la contraseña de la cadena de claves.
    >
    > 1. Haga clic en **Sí** cuando se le pida que **instale y confíe en el nuevo certificado** y, a continuación, escriba la contraseña de la cadena de claves.

1. Expanda la carpeta **Controladores** y, después, elimine **WeatherForecastController.cs**.

1. Elimine **WeatherForecast.cs**.

1. **Control** + **Clic** en el proyecto **PushDemoApi** y, a continuación, elija **Nuevo archivo...** en el menú **Agregar**.

1. Configure los valores de la configuración local con la [herramienta Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-3.1&tabs=linux#secret-manager). Si desacopla los secretos de la solución se asegurará de que no terminen en el control de código fuente. Abra **Terminal** y, a continuación, vaya al directorio del archivo de proyecto y ejecute los siguientes comandos:

    ```bash
    dotnet user-secrets init
    dotnet user-secrets set "NotificationHub:Name" <value>
    dotnet user-secrets set "NotificationHub:ConnectionString" <value>
    ```

    Reemplace los valores de los marcadores de posición por el nombre de su propio centro de notificaciones y por los valores de las cadenas de conexión. Tomó nota de ellos en la sección [Creación de un centro de notificaciones](#create-a-notification-hub). Si no lo hizo, puede buscarlos en [Azure](https://portal.azure.com).

    **NotificationsHub:Name**:  
    Consulte el *nombre* en el resumen de **Información esencial** en la parte superior de **Información general**.  

    **NotificationHub:ConnectionString**:  
    Consulte *DefaultFullSharedAccessSignature* en **Directivas de acceso**.

    > [!NOTE]
    > En escenarios de producción, puede contemplar otras opciones como [Azure Key Vault](https://azure.microsoft.com/services/key-vault) para almacenar de forma segura la cadena de conexión. Por motivos de simplicidad, los secretos se agregarán a la configuración de la aplicación [Azure App Service](https://azure.microsoft.com/services/app-service/).

### <a name="authenticate-clients-using-an-api-key-optional"></a>Autenticación de clientes mediante una clave de API (opcional)

Las claves de API no son tan seguras como los tokens, pero serán suficientes para la finalidad de este tutorial. Una clave de API se puede configurar fácilmente mediante el [middleware de ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/?view=aspnetcore-3.1).

1. Agregue la **clave de API** a los valores de configuración locales.

    ```bash
    dotnet user-secrets set "Authentication:ApiKey" <value>
    ```

    > [!NOTE]
    > Debe reemplazar el valor del marcador de posición por el suyo propio y tomar nota del mismo.

1. **Control** + **Clic** en el proyecto **PushDemoApi**, elija **Nueva carpeta** en el menú **Agregar** y, a continuación, haga clic en **Agregar** con *Autenticación* como **nombre de la carpeta**.

1. **Control** + **Clic** en la carpeta **Autenticación** y, a continuación, elija **Nuevo archivo...** en el menú **Agregar**.

1. Seleccione **General** > **Clase vacía**, escriba *ApiKeyAuthOptions.cs* como **nombre** y, a continuación, haga clic en **Nuevo** para agregar la siguiente implementación.

    ```csharp
    using Microsoft.AspNetCore.Authentication;

    namespace PushDemoApi.Authentication
    {
        public class ApiKeyAuthOptions : AuthenticationSchemeOptions
        {
            public const string DefaultScheme = "ApiKey";
            public string Scheme => DefaultScheme;
            public string ApiKey { get; set; }
        }
    }
    ```

1. Agregue otra **clase vacía** a la carpeta **Autenticación** denominada *ApiKeyAuthHandler.cs* y, a continuación, agregue la siguiente implementación.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Security.Claims;
    using System.Text.Encodings.Web;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Authentication;
    using Microsoft.Extensions.Logging;
    using Microsoft.Extensions.Options;

    namespace PushDemoApi.Authentication
    {
        public class ApiKeyAuthHandler : AuthenticationHandler<ApiKeyAuthOptions>
        {
            const string ApiKeyIdentifier = "apikey";

            public ApiKeyAuthHandler(
                IOptionsMonitor<ApiKeyAuthOptions> options,
                ILoggerFactory logger,
                UrlEncoder encoder,
                ISystemClock clock)
                : base(options, logger, encoder, clock) {}

            protected override Task<AuthenticateResult> HandleAuthenticateAsync()
            {
                string key = string.Empty;

                if (Request.Headers[ApiKeyIdentifier].Any())
                {
                    key = Request.Headers[ApiKeyIdentifier].FirstOrDefault();
                }
                else if (Request.Query.ContainsKey(ApiKeyIdentifier))
                {
                    if (Request.Query.TryGetValue(ApiKeyIdentifier, out var queryKey))
                        key = queryKey;
                }

                if (string.IsNullOrWhiteSpace(key))
                    return Task.FromResult(AuthenticateResult.Fail("No api key provided"));

                if (!string.Equals(key, Options.ApiKey, StringComparison.Ordinal))
                    return Task.FromResult(AuthenticateResult.Fail("Invalid api key."));

                var identities = new List<ClaimsIdentity> {
                    new ClaimsIdentity("ApiKeyIdentity")
                };

                var ticket = new AuthenticationTicket(
                    new ClaimsPrincipal(identities), Options.Scheme);

                return Task.FromResult(AuthenticateResult.Success(ticket));
            }
        }
    }
    ```

    > [!NOTE]
    > Un [controlador de autenticación](https://docs.microsoft.com/aspnet/core/security/authentication/?view=aspnetcore-3.1#authentication-handler) es un tipo que implementa el comportamiento de un esquema, en este caso un esquema de clave de API personalizado.

1. Agregue otra **clase vacía** a la carpeta **Autenticación** denominada *ApiKeyAuthenticationBuilderExtensions.cs* y, a continuación, agregue la siguiente implementación.

    ```csharp
    using System;
    using Microsoft.AspNetCore.Authentication;

    namespace PushDemoApi.Authentication
    {
        public static class AuthenticationBuilderExtensions
        {
            public static AuthenticationBuilder AddApiKeyAuth(
                this AuthenticationBuilder builder,
                Action<ApiKeyAuthOptions> configureOptions)
            {
                return builder
                    .AddScheme<ApiKeyAuthOptions, ApiKeyAuthHandler>(
                        ApiKeyAuthOptions.DefaultScheme,
                        configureOptions);
            }
        }
    }
    ```

    > [!NOTE]
    > Este método de extensión simplifica el código de configuración del middleware en **Startup.cs** facilitando su lectura y seguimiento.

1. En **Startup.cs**, actualice el método **ConfigureServices** para configurar la autenticación de la clave de API con la llamada al método **services.AddControllers**.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers();

        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = ApiKeyAuthOptions.DefaultScheme;
            options.DefaultChallengeScheme = ApiKeyAuthOptions.DefaultScheme;
        }).AddApiKeyAuth(Configuration.GetSection("Authentication").Bind);
    }
    ```

1. También en **Startup.cs**, actualice el método **Configure** para llamar a los métodos de extensión **UseAuthentication** y **UseAuthorization** de **IApplicationBuilder** de la aplicación. Asegúrese de que se llama a esos métodos después de **UseRouting** y antes de **app.UseEndpoints**.

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseHttpsRedirection();

        app.UseRouting();

        app.UseAuthentication();

        app.UseAuthorization();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapControllers();
        });
    }
    ```

    > [!NOTE]
    > La llamada a **UseAuthentication** registra el middleware que usa los esquemas de autenticación previamente registrados (a partir de **ConfigureServices**). Se debe llamar a este antes que a cualquier otro middleware que dependa de la autenticación de los usuarios.

### <a name="add-dependencies-and-configure-services"></a>Incorporación de dependencias y configuración de servicios

ASP.NET Core admite el modelo de diseño de software de [inserción de dependencias (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-3.1), que es una técnica para lograr la [inversión de control (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre las clases y sus dependencias.  

El uso del centro de notificaciones y el [SDK de Notification Hubs para operaciones de back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) se encapsula dentro de un servicio. El servicio se registra y se pone a disposición a través de una abstracción adecuada.

1. **Control** + **Clic** en la carpeta **Dependencias** y, a continuación, elija **Manage NuGet Packages...** (Administrar paquetes NuGet...).

1. Busque **Microsoft.Azure.NotificationHubs** y asegúrese de que está activado.

1. Haga clic en **Agregar paquetes** y, a continuación, haga clic en **Aceptar** cuando se le pida que acepte los términos de licencia.

1. **Control** + **Clic** en el proyecto **PushDemoApi**, elija **Nueva carpeta** en el menú **Agregar** y, a continuación, haga clic en **Agregar** con *Modelos* como **nombre de la carpeta**.

1. **Control** + **Clic** en la carpeta **Modelos** y, a continuación, elija **Nuevo archivo...** en el menú **Agregar**.

1. Seleccione **General** > **Clase vacía**, escriba *PushTemplates.cs* como **nombre** y, a continuación, haga clic en **Nuevo** para agregar la siguiente implementación.

    ```csharp
    namespace PushDemoApi.Models
    {
        public class PushTemplates
        {
            public class Generic
            {
                public const string Android = "{ \"notification\": { \"title\" : \"PushDemo\", \"body\" : \"$(alertMessage)\"}, \"data\" : { \"action\" : \"$(alertAction)\" } }";
                public const string iOS = "{ \"aps\" : {\"alert\" : \"$(alertMessage)\"}, \"action\" : \"$(alertAction)\" }";
            }

            public class Silent
            {
                public const string Android = "{ \"data\" : {\"message\" : \"$(alertMessage)\", \"action\" : \"$(alertAction)\"} }";
                public const string iOS = "{ \"aps\" : {\"content-available\" : 1, \"apns-priority\": 5, \"sound\" : \"\", \"badge\" : 0}, \"message\" : \"$(alertMessage)\", \"action\" : \"$(alertAction)\" }";
            }
        }
    }
    ```

    > [!NOTE]
    > Esta clase contiene las cargas de notificación con tokens para las notificaciones genéricas y silenciosas que requiere este escenario. Las cargas se definen fuera de la [instalación](https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.installation?view=azure-dotnet) para permitir la experimentación sin tener que actualizar las instalaciones existentes a través del servicio. El control de los cambios en las instalaciones de esta forma está fuera del ámbito de este tutorial. Para producción, considere la posibilidad de utilizar [plantillas personalizadas](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).

1. Seleccione **General** > **Clase vacía**, escriba *DeviceInstallation.cs* como **nombre** y, a continuación, haga clic en **Nuevo** para agregar la siguiente implementación.

    ```csharp
    using System.Collections.Generic;
    using System.ComponentModel.DataAnnotations;

    namespace PushDemoApi.Models
    {
        public class DeviceInstallation
        {
            [Required]
            public string InstallationId { get; set; }

            [Required]
            public string Platform { get; set; }

            [Required]
            public string PushChannel { get; set; }

            public IList<string> Tags { get; set; } = Array.Empty<string>();
        }
    }
    ```

1. Agregue otra **clase vacía** a la carpeta **Modelos** denominada *NotificationRequest.cs* y, a continuación, agregue la siguiente implementación.

    ```csharp
    using System;

    namespace PushDemoApi.Models
    {
        public class NotificationRequest
        {
            public string Text { get; set; }
            public string Action { get; set; }
            public string[] Tags { get; set; } = Array.Empty<string>();
            public bool Silent { get; set; }
        }
    }
    ```

1. Agregue otra **clase vacía** a la carpeta **Modelos** denominada *NotificationHubOptions.cs* y, a continuación, agregue la siguiente implementación.

    ```csharp
    using System.ComponentModel.DataAnnotations;

    namespace PushDemoApi.Models
    {
        public class NotificationHubOptions
        {
            [Required]
            public string Name { get; set; }

            [Required]
            public string ConnectionString { get; set; }
        }
    }
    ```

1. Agregue una nueva carpeta al proyecto **PushDemoApi** denominada *Servicios*.

1. Agregue una **interfaz vacía** a la carpeta **Servicios** denominada *INotificationService.cs* y, a continuación, agregue la siguiente implementación.

    ```csharp
    using System.Threading.Tasks;
    using PushDemoApi.Models;

    namespace PushDemoApi.Services
    {
        public interface INotificationService
        {
            Task<bool> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation, CancellationToken token);
            Task<bool> DeleteInstallationByIdAsync(string installationId, CancellationToken token);
            Task<bool> RequestNotificationAsync(NotificationRequest notificationRequest, CancellationToken token);
        }
    }
    ```

1. Agregue una **clase vacía** a la carpeta **Servicios** denominada *NotificationHubsService.cs* y, a continuación, agregue el código siguiente para implementar la interfaz **INotificationService**:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
    using Microsoft.Extensions.Logging;
    using Microsoft.Extensions.Options;
    using PushDemoApi.Models;

    namespace PushDemoApi.Services
    {
        public class NotificationHubService : INotificationService
        {
            readonly NotificationHubClient _hub;
            readonly Dictionary<string, NotificationPlatform> _installationPlatform;
            readonly ILogger<NotificationHubService> _logger;

            public NotificationHubService(IOptions<NotificationHubOptions> options, ILogger<NotificationHubService> logger)
            {
                _logger = logger;
                _hub = NotificationHubClient.CreateClientFromConnectionString(
                    options.Value.ConnectionString,
                    options.Value.Name);

                _installationPlatform = new Dictionary<string, NotificationPlatform>
                {
                    { nameof(NotificationPlatform.Apns).ToLower(), NotificationPlatform.Apns },
                    { nameof(NotificationPlatform.Fcm).ToLower(), NotificationPlatform.Fcm }
                };
            }

            public async Task<bool> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation, CancellationToken token)
            {
                if (string.IsNullOrWhiteSpace(deviceInstallation?.InstallationId) ||
                    string.IsNullOrWhiteSpace(deviceInstallation?.Platform) ||
                    string.IsNullOrWhiteSpace(deviceInstallation?.PushChannel))
                    return false;

                var installation = new Installation()
                {
                    InstallationId = deviceInstallation.InstallationId,
                    PushChannel = deviceInstallation.PushChannel,
                    Tags = deviceInstallation.Tags
                };

                if (_installationPlatform.TryGetValue(deviceInstallation.Platform, out var platform))
                    installation.Platform = platform;
                else
                    return false;

                try
                {
                    await _hub.CreateOrUpdateInstallationAsync(installation, token);
                }
                catch
                {
                    return false;
                }

                return true;
            }

            public async Task<bool> DeleteInstallationByIdAsync(string installationId, CancellationToken token)
            {
                if (string.IsNullOrWhiteSpace(installationId))
                    return false;

                try
                {
                    await _hub.DeleteInstallationAsync(installationId, token);
                }
                catch
                {
                    return false;
                }

                return true;
            }

            public async Task<bool> RequestNotificationAsync(NotificationRequest notificationRequest, CancellationToken token)
            {
                if ((notificationRequest.Silent &&
                    string.IsNullOrWhiteSpace(notificationRequest?.Action)) ||
                    (!notificationRequest.Silent &&
                    (string.IsNullOrWhiteSpace(notificationRequest?.Text)) ||
                    string.IsNullOrWhiteSpace(notificationRequest?.Action)))
                    return false;

                var androidPushTemplate = notificationRequest.Silent ?
                    PushTemplates.Silent.Android :
                    PushTemplates.Generic.Android;

                var iOSPushTemplate = notificationRequest.Silent ?
                    PushTemplates.Silent.iOS :
                    PushTemplates.Generic.iOS;

                var androidPayload = PrepareNotificationPayload(
                    androidPushTemplate,
                    notificationRequest.Text,
                    notificationRequest.Action);

                var iOSPayload = PrepareNotificationPayload(
                    iOSPushTemplate,
                    notificationRequest.Text,
                    notificationRequest.Action);

                try
                {
                    if (notificationRequest.Tags.Length == 0)
                    {
                        // This will broadcast to all users registered in the notification hub
                        await SendPlatformNotificationsAsync(androidPayload, iOSPayload, token);
                    }
                    else if (notificationRequest.Tags.Length <= 20)
                    {
                        await SendPlatformNotificationsAsync(androidPayload, iOSPayload, notificationRequest.Tags, token);
                    }
                    else
                    {
                        var notificationTasks = notificationRequest.Tags
                            .Select((value, index) => (value, index))
                            .GroupBy(g => g.index / 20, i => i.value)
                            .Select(tags => SendPlatformNotificationsAsync(androidPayload, iOSPayload, tags, token));

                        await Task.WhenAll(notificationTasks);
                    }

                    return true;
                }
                catch (Exception e)
                {
                    _logger.LogError(e, "Unexpected error sending notification");
                    return false;
                }
            }

            string PrepareNotificationPayload(string template, string text, string action) => template
                .Replace("$(alertMessage)", text, StringComparison.InvariantCulture)
                .Replace("$(alertAction)", action, StringComparison.InvariantCulture);

            Task SendPlatformNotificationsAsync(string androidPayload, string iOSPayload, CancellationToken token)
            {
                var sendTasks = new Task[]
                {
                    _hub.SendFcmNativeNotificationAsync(androidPayload, token),
                    _hub.SendAppleNativeNotificationAsync(iOSPayload, token)
                };

                return Task.WhenAll(sendTasks);
            }

            Task SendPlatformNotificationsAsync(string androidPayload, string iOSPayload, IEnumerable<string> tags, CancellationToken token)
            {
                var sendTasks = new Task[]
                {
                    _hub.SendFcmNativeNotificationAsync(androidPayload, tags, token),
                    _hub.SendAppleNativeNotificationAsync(iOSPayload, tags, token)
                };

                return Task.WhenAll(sendTasks);
            }
        }
    }
    ```

    > [!NOTE]
    > La expresión de etiqueta que se ha proporcionado a **SendTemplateNotificationAsync** está limitada a 20 etiquetas. Está limitada a 6 para la mayoría de los operadores, pero la expresión solo contiene "OR" (||) en este caso. Si hay más de 20 etiquetas en la solicitud, se deben dividir en varias solicitudes. Consulte la documentación [Expresiones de etiqueta y enrutamiento](https://msdn.microsoft.com/library/azure/Dn530749.aspx?f=255&MSPPError=-2147217396) para obtener más detalles.

1. En **Startup.cs**, actualice el método **ConfigureServices** para agregar **NotificationHubsService** como una implementación de base de datos única de **INotificationService**.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ...

        services.AddSingleton<INotificationService, NotificationHubService>();

        services.AddOptions<NotificationHubOptions>()
            .Configure(Configuration.GetSection("NotificationHub").Bind)
            .ValidateDataAnnotations();
    }
    ```

### <a name="create-the-notifications-api"></a>Creación de la API de notificaciones

1. **Control** + **Clic** en la carpeta **Controladores** y, a continuación, elija **Nuevo archivo...** en el menú **Agregar**.

1. Seleccione **ASP.NET Core** > **Clase de controlador de API web**, escriba *NotificationsController* como **nombre** y, a continuación, haga clic en **Nuevo**.

1. Agregue los siguientes espacios de nombres al principio del archivo.

    ```csharp
    using System.ComponentModel.DataAnnotations;
    using System.Net;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Authorization;
    using Microsoft.AspNetCore.Mvc;
    using PushDemoApi.Models;
    using PushDemoApi.Services;
    ```

1. Actualice el controlador con plantilla para que se derive de **ControllerBase** y se decore con el atributo **ApiController**.

    ```cs
    [ApiController]
    [Route("api/[controller]")]
    public class NotificationsController : ControllerBase
    {
        // Templated methods here
    }
    ```

    > [!NOTE]
    > La clase base **Controller** proporciona compatibilidad con las vistas pero esto no es necesario en este caso y, por tanto, se puede usar **ControllerBase** en su lugar.

1. Si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional), debe decorar **NotificationsController** con el atributo **Authorize** también.

    ```cs
    [Authorize]
    ```

1. Actualice el constructor para que acepte la instancia registrada de **INotificationService** como argumento y asígnela a un miembro de solo lectura.

    ```cs
    readonly INotificationService _notificationService;

    public NotificationsController(INotificationService notificationService)
    {
        _notificationService = notificationService;
    }
    ```

1. En **launchSettings.json** (dentro de la carpeta **Propiedades**), cambie **launchUrl** de `weatherforecast` a *API/notificaciones* para que coincida con la dirección URL especificada en el atributo **RegistrationsController** **Route**.

1. Inicie la depuración (**Comando** + **Entrar**) para comprobar que la aplicación esté funcionando con el nuevo atributo **NotificationsController** y que devuelve un estado **401 No autorizado**.

    > [!NOTE]
    > Puede que Visual Studio no inicie automáticamente la aplicación en el explorador. Va a usar [Postman](https://www.postman.com/downloads) para probar la API a partir de ahora.

1. En una nueva pestaña de **[Postman](https://www.postman.com/downloads)** , establezca la solicitud en **GET** y escriba la dirección siguiente.

    ```bash
    https://localhost:5001/api/notifications
    ```

    > [!NOTE]
    > La dirección localhost debe coincidir con el valor **applicationUrl** que se encuentra en **Propiedades** > **launchSettings.json**. El valor predeterminado debe ser `https://localhost:5001;http://localhost:5000`, pero esto es algo para comprobar si recibe una respuesta 404.

1. Si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional), asegúrese de configurar los encabezados de solicitud para que incluyan el valor **apikey**.

   | Clave                            | Value                          |
   | ------------------------------ | ------------------------------ |
   | apikey                         | <your_api_key>                 |

1. Haga clic en el botón **Enviar**.

    > [!NOTE]
    > Debe recibir el estado **200 CORRECTO** con cierto contenido **JSON**.
    >
    > Si recibe una advertencia de **comprobación de certificado SSL**, puede cambiar la configuración de **[Postman](https://www.postman.com/downloads)** de la comprobación de certificado SSL en **Configuración**.

1. Reemplace los métodos de la clase con plantilla por el siguiente código.

    ```csharp
    [HttpPut]
    [Route("installations")]
    [ProducesResponseType((int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    [ProducesResponseType((int)HttpStatusCode.UnprocessableEntity)]
    public async Task<IActionResult> UpdateInstallation(
        [Required]DeviceInstallation deviceInstallation)
    {
        var success = await _notificationService
            .CreateOrUpdateInstallationAsync(deviceInstallation, HttpContext.RequestAborted);

        if (!success)
            return new UnprocessableEntityResult();

        return new OkResult();
    }

    [HttpDelete()]
    [Route("installations/{installationId}")]
    [ProducesResponseType((int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    [ProducesResponseType((int)HttpStatusCode.UnprocessableEntity)]
    public async Task<ActionResult> DeleteInstallation(
        [Required][FromRoute]string installationId)
    {
        var success = await _notificationService
            .DeleteInstallationByIdAsync(installationId, CancellationToken.None);

        if (!success)
            return new UnprocessableEntityResult();

        return new OkResult();
    }

    [HttpPost]
    [Route("requests")]
    [ProducesResponseType((int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    [ProducesResponseType((int)HttpStatusCode.UnprocessableEntity)]
    public async Task<IActionResult> RequestPush(
        [Required]NotificationRequest notificationRequest)
    {
        if ((notificationRequest.Silent &&
            string.IsNullOrWhiteSpace(notificationRequest?.Action)) ||
            (!notificationRequest.Silent &&
            string.IsNullOrWhiteSpace(notificationRequest?.Text)))
            return new BadRequestResult();

        var success = await _notificationService
            .RequestNotificationAsync(notificationRequest, HttpContext.RequestAborted);

        if (!success)
            return new UnprocessableEntityResult();

        return new OkResult();
    }
    ```

### <a name="create-the-api-app"></a>Creación de la aplicación de API

Ahora va a crear una [aplicación de API](https://azure.microsoft.com/services/app-service/api/) en [Azure App Service](https://docs.microsoft.com/azure/app-service/) para hospedar el servicio de back-end.  

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Haga clic en **Crear un recurso**, busque y elija **Aplicación de API** y, a continuación, haga clic en **Crear**.

1. Actualice los campos siguientes y haga clic en **Crear**.

    **Nombre de la aplicación:**  
    Escriba un nombre único global para la **aplicación de API**.

    **Subscription** (Suscripción):  
    Elija la misma **suscripción** de destino en la que creó el centro de notificaciones.

    **Grupos de recursos:**  
    Elija el mismo **grupo de recursos** en el que creó el centro de notificaciones.

    **Plan de App Service/ubicación:**  
    Cree un nuevo **plan de App Service**.  

    > [!NOTE]
    > Cambie la opción predeterminada a un plan que incluya compatibilidad con **SSL**. De lo contrario, tendrá que realizar los pasos adecuados al trabajar con la aplicación móvil para evitar que se bloqueen las solicitudes **http**.

    **Application Insights:**  
    Mantenga la opción sugerida (se creará un nuevo recurso con ese nombre) o elija un recurso existente.

1. Una vez que se haya aprovisionado la **aplicación de API**, vaya a ese recurso.

1. Tome nota de la propiedad **URL** en el resumen de **Información esencial** que aparece en la parte superior de **Información general**. Esta dirección URL es el *punto de conexión de back-end* que se utilizará más adelante en este tutorial.

    > [!NOTE]
    > La dirección URL utiliza el nombre de la aplicación de API que especificó anteriormente, con el formato `https://<app_name>.azurewebsites.net`.

1. Seleccione **Configuración** en la lista (en **Configuración**).  

1. Para cada una de las opciones siguientes, haga clic en **Nueva configuración de la aplicación** para escribir el **nombre** y un **valor** y, a continuación, haga clic en **Aceptar**.

   | Nombre                               | Value                          |
   | ---------------------------------- | ------------------------------ |
   | `Authentication:ApiKey`            | <api_key_value>                |
   | `NotificationHub:Name`             | <hub_name_value>               |
   | `NotificationHub:ConnectionString` | <hub_connection_string_value>  |

   > [!NOTE]
   > Se trata de la misma configuración que definió anteriormente en la configuración del usuario. Estos valores se pueden copiar. La opción **Authentication:ApiKey** solo es necesaria si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional). En escenarios de producción, puede contemplar otras opciones como [Azure Key Vault](https://azure.microsoft.com/services/key-vault). En este caso, estas opciones se han agregado como configuración de la aplicación por motivos de claridad.

1. Una vez que se haya agregado la configuración de la aplicación, haga clic en **Guardar** y en **Continuar**.

### <a name="publish-the-backend-service"></a>Publicación del servicio back-end

A continuación implementaremos la aplicación en la aplicación de API para que todos los dispositivos puedan acceder a ella.  

1. Cambie la configuración de **Depurar** a **Crear versión** si todavía no lo ha hecho.

1. **Control** + **Clic** en el proyecto **PushDemoApi** y, a continuación, elija **Publicar en Azure...** en el menú **Publicar**.

1. Si se le solicita, siga el flujo de autenticación. Use la cuenta que usó en la sección anterior de [creación de la aplicación de API](#create-the-api-app).

1. Seleccione la **aplicación de API de Azure App Service** que creó anteriormente en la lista como destino de publicación y, a continuación, haga clic en **Publicar**.

Una vez completado el asistente, este publica la aplicación en Azure y, posteriormente, la abre. Tome nota de la **dirección URL** si aún no lo ha hecho. Esta dirección URL es el *punto de conexión de back-end* que se utiliza más adelante en este tutorial.

### <a name="validating-the-published-api"></a>Validación de la API publicada

1. En **[Postman](https://www.postman.com/downloads)** abra una nueva pestaña, establezca la solicitud en **POST** y escriba la dirección siguiente. Reemplace el marcador de posición por la dirección base que anotó en la sección anterior de [publicación del servicio back-end](#publish-the-backend-service).

    ```csharp
    https://<app_name>.azurewebsites.net/api/notifications/installations
    ```

    > [!NOTE]
    > La dirección base debe tener el formato ``https://<app_name>.azurewebsites.net/``.

1. Si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional), asegúrese de configurar los encabezados de solicitud para que incluyan el valor **apikey**.

   | Clave                            | Value                          |
   | ------------------------------ | ------------------------------ |
   | apikey                         | <your_api_key>                 |

1. Elija la opción **sin formato** para el **cuerpo** y, a continuación, elija **JSON** en la lista de opciones de formato y, después, incluya algún contenido **JSON** de marcador de posición:

    ```json
    {}
    ```

1. Haga clic en **Enviar**.

    > [!NOTE]
    > Debe recibir el estado **400 - Solicitud incorrecta** del servicio.

1. Realice de nuevo los pasos del 1 al 4, pero esta vez especificando el punto de conexión de las solicitudes para comprobar que recibe la misma respuesta **400 - Solicitud incorrecta**.

    ```bash
    https://<app_name>.azurewebsites.net/api/notifications/requests
    ```

> [!NOTE]
> Todavía no es posible probar la API mediante datos de solicitud válidos, ya que esto requerirá información específica de la plataforma de la aplicación móvil cliente.
