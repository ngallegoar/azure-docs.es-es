---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 40bc1c8c3d578e35b6689124f60f82d8ea85f0f2
ms.sourcegitcommit: e04a66514b21019f117a4ddb23f22c7c016da126
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85112078"
---
### <a name="create-the-xamarinforms-solution"></a>Creación de la solución Xamarin.Forms

1. En **Visual Studio**, cree una nueva solución **Xamarin.Forms** mediante **Aplicación de Forms en blanco** como plantilla y escriba *PushDemo* como **nombre del proyecto**.

    > [!NOTE]
    > En el cuadro de diálogo **Configuración de Aplicación de Forms en blanco**, asegúrese de que el **identificador de la organización** coincide con el valor que ha utilizado anteriormente y de que están seleccionados los destinos de **Android** e **iOS**.

1. **Control** + **Clic** en la solución *PushDemo* y, a continuación, elija **Actualizar paquetes NuGet**.
1. **Control** + **Clic** en la solución *PushDemo* y, a continuación, elija **Administrar paquetes NuGet...** .
1. Busque **Newtonsoft.Json** y asegúrese de que está seleccionado.
1. Haga clic en **Agregar paquetes** y, a continuación, haga clic en **Aceptar** cuando se le pida que acepte los términos de licencia.
1. Compile y ejecute la aplicación en cada plataforma de destino (**Comando** + **Entrar**) para probar las ejecuciones de aplicaciones con plantilla en los dispositivos.

### <a name="implement-the-cross-platform-components"></a>Implementación de componentes multiplataforma

1. **Control** + **Clic** en el proyecto **PushDemo**, elija **Nueva carpeta** en el menú **Agregar** y, a continuación, haga clic en **Agregar** con *Modelos* como **nombre de la carpeta**.

1. **Control** + **Clic** en la carpeta **Modelos** y, a continuación, elija **Nuevo archivo...** en el menú **Agregar**.

1. Seleccione **General** > **Clase vacía**, escriba *DeviceInstallation.cs* y, a continuación, agregue la siguiente implementación.

    ```csharp
    using System.Collections.Generic;
    using Newtonsoft.Json;

    namespace PushDemo.Models
    {
        public class DeviceInstallation
        {
            [JsonProperty("installationId")]
            public string InstallationId { get; set; }

            [JsonProperty("platform")]
            public string Platform { get; set; }

            [JsonProperty("pushChannel")]
            public string PushChannel { get; set; }

            [JsonProperty("tags")]
            public List<string> Tags { get; set; } = new List<string>();
        }
    }
    ```

1. Agregue una **enumeración vacía** a la carpeta **Modelos** denominada *PushDemoAction.cs* con la siguiente implementación.

    ```csharp
    namespace PushDemo.Models
    {
        public enum PushDemoAction
        {
            ActionA,
            ActionB
        }
    }
    ```

1. Agregue una nueva carpeta al proyecto **PushDemo** denominada *Servicios* y, a continuación, agregue una **clase vacía** a esa carpeta llamada *ServiceContainer.cs* con la siguiente implementación.

     ```csharp
    using System;
    using System.Collections.Generic;

    namespace PushDemo.Services
    {
        public static class ServiceContainer
        {
            static readonly Dictionary<Type, Lazy<object>> services
                = new Dictionary<Type, Lazy<object>>();

            public static void Register<T>(Func<T> function)
                => services[typeof(T)] = new Lazy<object>(() => function());

            public static T Resolve<T>()
                => (T)Resolve(typeof(T));

            public static object Resolve(Type type)
            {
                {
                    if (services.TryGetValue(type, out var service))
                        return service.Value;

                    throw new KeyNotFoundException($"Service not found for type '{type}'");
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Se trata de una versión reducida de la clase [ServiceContainer](https://github.com/xamcat/mobcat-library/blob/master/MobCAT/ServiceContainer.cs) del repositorio [XamCAT](https://github.com/xamcat/mobcat-library). Se usará como un contenedor de inversión de control ligero.

1. Agregue una **interfaz vacía** a la carpeta **Servicios** denominada *IDeviceInstallationService.cs* y, a continuación, agregue el siguiente código.

    ```csharp
    using PushDemo.Models;

    namespace PushDemo.Services
    {
        public interface IDeviceInstallationService
        {
            string Token { get; set; }
            bool NotificationsSupported { get; }
            string GetDeviceId();
            DeviceInstallation GetDeviceInstallation(params string[] tags);
        }
    }
    ```

    > [!NOTE]
    > Cada destino implementará y arrancará posteriormente esta interfaz para proporcionar la funcionalidad específica de la plataforma y la información de **DeviceInstallation** que necesita el servicio back-end.

1. Agregue otra **interfaz vacía** a la carpeta **Servicios** denominada *INotificationRegistrationService.cs* y, a continuación, agregue el siguiente código.  

    ```csharp
    using System.Threading.Tasks;

    namespace PushDemo.Services
    {
        public interface INotificationRegistrationService
        {
            Task DeregisterDeviceAsync();
            Task RegisterDeviceAsync(params string[] tags);
            Task RefreshRegistrationAsync();
        }
    }
    ```

    > [!NOTE]
    > Este controlará la interacción entre el cliente y el servicio back-end.

1. Agregue otra **interfaz vacía** a la carpeta **Servicios** denominada *INotificationActionService.cs* y, a continuación, agregue el siguiente código.  

    ```csharp
    namespace PushDemo.Services
    {
        public interface INotificationActionService
        {
            void TriggerAction(string action);
        }
    }
    ```

    > [!NOTE]
    > Este se usa como un mecanismo sencillo para centralizar el control de las acciones de notificación.

1. Agregue una **interfaz vacía** a la carpeta **Servicios** denominada *IPushDemoNotificationActionService.cs* que se deriva de *INotificationActionService* con la siguiente implementación.  

    ```csharp
    using System;
    using PushDemo.Models;

    namespace PushDemo.Services
    {
        public interface IPushDemoNotificationActionService : INotificationActionService
        {
            event EventHandler<PushDemoAction> ActionTriggered;
        }
    }
    ```

    > [!NOTE]
    > Este tipo es específico de la aplicación **PushDemo** y utiliza la enumeración **PushDemoAction** para identificar la acción que se desencadena de forma fuertemente tipada.

1. Agregue una **clase vacía** a la carpeta **Servicios** denominada *NotificationRegistrationService.cs* que implemente *INotificationRegistrationService* con el siguiente código.

    ```csharp
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    using PushDemo.Models;
    using Xamarin.Essentials;

    namespace PushDemo.Services
    {
        public class NotificationRegistrationService : INotificationRegistrationService
        {
            const string CachedTagsKey = "cached_tags";
            const string RequestUrl = "api/notifications/installations";

            string _baseApiUrl;
            HttpClient _client;
            IDeviceInstallationService _deviceInstallationService;

            public NotificationRegistrationService(string baseApiUri, string apiKey)
            {
                _client = new HttpClient();
                _client.DefaultRequestHeaders.Add("Accept", "application/json");
                _client.DefaultRequestHeaders.Add("apikey", apiKey);

                _baseApiUrl = baseApiUri;
            }

            IDeviceInstallationService DeviceInstallationService
                => _deviceInstallationService ??
                    (_deviceInstallationService = ServiceContainer.Resolve<IDeviceInstallationService>());

            public Task DeregisterDeviceAsync()
            {
                var deviceId = DeviceInstallationService?.GetDeviceId();

                if (string.IsNullOrWhiteSpace(deviceId))
                    throw new Exception("Unable to resolve an ID for the device.");

                SecureStorage.Remove(CachedTagsKey);

                return SendAsync(HttpMethod.Delete, $"{RequestUrl}/{deviceId}");
            }

            public async Task RegisterDeviceAsync(params string[] tags)
            {
                var deviceInstallation = DeviceInstallationService?.GetDeviceInstallation(tags);

                if (deviceInstallation == null)
                    throw new Exception($"Unable to get device installation information.");

                if (string.IsNullOrWhiteSpace(deviceInstallation.InstallationId))
                    throw new Exception($"No {nameof(deviceInstallation.InstallationId)} value for {nameof(DeviceInstallation)}");

                if (string.IsNullOrWhiteSpace(deviceInstallation.Platform))
                    throw new Exception($"No {nameof(deviceInstallation.Platform)} value for {nameof(DeviceInstallation)}");

                if (string.IsNullOrWhiteSpace(deviceInstallation.PushChannel))
                    throw new Exception($"No {nameof(deviceInstallation.PushChannel)} value for {nameof(DeviceInstallation)}");

                await SendAsync<DeviceInstallation>(HttpMethod.Put, RequestUrl, deviceInstallation)
                    .ConfigureAwait(false);

                var serializedTags = JsonConvert.SerializeObject(tags);
                await SecureStorage.SetAsync(CachedTagsKey, serializedTags);
            }

            public async Task RefreshRegistrationAsync()
            {
                var serializedTags = await SecureStorage.GetAsync(CachedTagsKey)
                    .ConfigureAwait(false);

                if (string.IsNullOrWhiteSpace(serializedTags))
                    return;

                var tags = JsonConvert.DeserializeObject<string[]>(serializedTags);

                await RegisterDeviceAsync(tags);
            }

            async Task SendAsync<T>(HttpMethod requestType, string requestUri, T obj)
            {
                string serializedContent = null;

                await Task.Run(() => serializedContent = JsonConvert.SerializeObject(obj))
                    .ConfigureAwait(false);

                await SendAsync(requestType, requestUri, serializedContent);
            }

            async Task SendAsync(
                HttpMethod requestType,
                string requestUri,
                string jsonRequest = null)
            {
                var request = new HttpRequestMessage(requestType, new Uri($"{_baseApiUrl}{requestUri}"));

                if (jsonRequest != null)
                    request.Content = new StringContent(jsonRequest, Encoding.UTF8, "application/json");

                var response = await _client.SendAsync(request).ConfigureAwait(false);

                response.EnsureSuccessStatusCode();
            }
        }
    }
    ```

    > [!NOTE]
    > El argumento **apiKey** solo es necesario si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional).

1. Agregue una **clase vacía** a la carpeta **Servicios** denominada *PushDemoNotificationActionService.cs* que implemente *IPushDemoNotificationActionService* con el siguiente código.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using PushDemo.Models;

    namespace PushDemo.Services
    {
        public class PushDemoNotificationActionService : IPushDemoNotificationActionService
        {
            readonly Dictionary<string, PushDemoAction> _actionMappings = new Dictionary<string, PushDemoAction>
            {
                { "action_a", PushDemoAction.ActionA },
                { "action_b", PushDemoAction.ActionB }
            };

            public event EventHandler<PushDemoAction> ActionTriggered = delegate { };

            public void TriggerAction(string action)
            {
                if (!_actionMappings.TryGetValue(action, out var pushDemoAction))
                    return;

                List<Exception> exceptions = new List<Exception>();

                foreach (var handler in ActionTriggered?.GetInvocationList())
                {
                    try
                    {
                        handler.DynamicInvoke(this, pushDemoAction);
                    }
                    catch (Exception ex)
                    {
                        exceptions.Add(ex);
                    }
                }

                if (exceptions.Any())
                    throw new AggregateException(exceptions);
            }
        }
    }
    ```

1. Agregue una **clase vacía** al proyecto **PushDemo** llamada *Config.cs* con la siguiente implementación.  

    ```csharp
    namespace PushDemo
    {
        public static partial class Config
        {
            public static string ApiKey = "API_KEY";
            public static string BackendServiceEndpoint = "BACKEND_SERVICE_ENDPOINT";
        }
    }
    ```

    > [!NOTE]
    > Se utiliza como una forma sencilla de mantener los secretos fuera del control de código fuente. Puede reemplazar estos valores como parte de una compilación automatizada o invalidarlos mediante una clase parcial local. Esto lo hará en el paso siguiente.
    >
    > El campo **ApiKey** solo es necesario si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional).

1. Agregue una **clase vacía** al proyecto **PushDemo** llamada esta vez *Config.local_secrets.cs* con la siguiente implementación.  

    ```csharp
    namespace PushDemo
    {
        public static partial class Config
        {
            static Config()
            {
                ApiKey = "<your_api_key>";
                BackendServiceEndpoint = "<your_api_app_url>";
            }
        }
    }
    ```

    > [!NOTE]
    > Reemplace los valores del marcador de posición por los suyos propios. Debe anotarlos al crear el servicio back-end. La dirección URL de la **aplicación de API** debe ser ``https://<api_app_name>.azurewebsites.net/``. No olvide agregar ``*.local_secrets.*`` al archivo gitignore para evitar la confirmación de este archivo.
    >
    > El campo **ApiKey** solo es necesario si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional).

1. Agregue una **clase vacía** al proyecto **PushDemo** llamada *Bootstrap.cs* con la siguiente implementación.  

    ```csharp
    using System;
    using PushDemo.Services;

    namespace PushDemo
    {
        public static class Bootstrap
        {
            public static void Begin(Func<IDeviceInstallationService> deviceInstallationService)
            {
                ServiceContainer.Register(deviceInstallationService);

                ServiceContainer.Register<IPushDemoNotificationActionService>(()
                    => new PushDemoNotificationActionService());

                ServiceContainer.Register<INotificationRegistrationService>(()
                    => new NotificationRegistrationService(
                        Config.BackendServiceEndpoint,
                        Config.ApiKey));
            }
        }
    }
    ```

    > [!NOTE]
    > Cada plataforma llamará al método **Begin** cuando la aplicación se inicie pasando una implementación específica de la plataforma de **IDeviceInstallationService**.
    >
    > El argumento de constructor **NotificationRegistrationService** **apiKey** solo es necesario si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional).

### <a name="implement-the-cross-platform-ui"></a>Implementación de una interfaz de usuario multiplataforma

1. En el proyecto **PushDemo**, abra **MainPage.xaml** y reemplace el control **StackLayout** por lo siguiente.

    ```xml
    <StackLayout VerticalOptions="EndAndExpand"  
                 HorizontalOptions="FillAndExpand"
                 Padding="20,40">
        <Button x:Name="RegisterButton"
                Text="Register"
                Clicked="RegisterButtonClicked" />
        <Button x:Name="DeregisterButton"
                Text="Deregister"
                Clicked="DeregisterButtonClicked" />
    </StackLayout>
    ```

1. Ahora, en **MainPage.xaml.cs**, agregue un campo de respaldo **de solo lectura** para almacenar una referencia a la implementación **INotificationRegistrationService**.

    ```csharp
    readonly INotificationRegistrationService _notificationRegistrationService;
    ```

1. En el constructor **MainPage**, resuelva la implementación **INotificationRegistrationService** con **ServiceContainer** y asígnela al campo de respaldo *_notificationRegistrationService_*.

    ```csharp
    public MainPage()
    {
        InitializeComponent();

        _notificationRegistrationService =
            ServiceContainer.Resolve<INotificationRegistrationService>();
    }
    ```

1. Implemente los controladores de eventos para los eventos **Clicked** de los botones **RegisterButton** y **DeregisterButton** que llaman a los métodos **Register**/**Deregister** correspondientes.

    ```csharp
    void RegisterButtonClicked(object sender, EventArgs e)
        => _notificationRegistrationService.RegisterDeviceAsync().ContinueWith((task)
            => { ShowAlert(task.IsFaulted ?
                    task.Exception.Message :
                    $"Device registered"); });

    void DeregisterButtonClicked(object sender, EventArgs e)
        => _notificationRegistrationService.DeregisterDeviceAsync().ContinueWith((task)
            => { ShowAlert(task.IsFaulted ?
                    task.Exception.Message :
                    $"Device deregistered"); });

    void ShowAlert(string message)
        => MainThread.BeginInvokeOnMainThread(()
            => DisplayAlert("PushDemo", message, "OK").ContinueWith((task)
                => { if (task.IsFaulted) throw task.Exception; }));
    ```

1. Ya en **App.xaml.cs**, asegúrese de que se hace referencia a los siguientes espacios de nombres.

    ```csharp
    using PushDemo.Models;
    using PushDemo.Services;
    using Xamarin.Essentials;
    using Xamarin.Forms;
    ```

1. Implemente el controlador de eventos para el evento **IPushDemoNotificationActionService** **ActionTriggered**.

    ```csharp
    void NotificationActionTriggered(object sender, PushDemoAction e)
        => ShowActionAlert(e);

    void ShowActionAlert(PushDemoAction action)
        => MainThread.BeginInvokeOnMainThread(()
            => MainPage?.DisplayAlert("PushDemo", $"{action} action received", "OK")
                .ContinueWith((task) => { if (task.IsFaulted) throw task.Exception; }));
    ```

1. En el constructor **App**, resuelva la implementación de **IPushNotificationActionService** mediante **ServiceContainer** y suscríbase al evento **IPushDemoNotificationActionService** **ActionTriggered**.

    ```csharp
    public App()
    {
        InitializeComponent();

        ServiceContainer.Resolve<IPushDemoNotificationActionService>()
            .ActionTriggered += NotificationActionTriggered;

        MainPage = new MainPage();
    }
    ```

    > [!NOTE]
    > Esto es simplemente para mostrar la recepción y propagación de las acciones con notificaciones push. Normalmente, estas se tratarían de forma silenciosa, por ejemplo, yendo a una vista específica o actualizando algunos datos en lugar de mostrar una alerta a través de la **página** raíz, **MainPage** en este caso.
