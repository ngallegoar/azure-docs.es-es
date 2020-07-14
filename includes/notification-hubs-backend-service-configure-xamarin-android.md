---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: de961aa36d690cf03e42707758bc70e5495f692e
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448742"
---
### <a name="validate-package-name-and-permissions"></a>Validación del nombre y los permisos del paquete

1. En **PushDemo.Android**, abra las **opciones del proyecto** y, a continuación, **Aplicación de Android** en la sección **Compilación**.

1. Compruebe que el **nombre del paquete** coincida con el valor que usó en la [consola Firebase](https://console.firebase.google.com) del proyecto **PushDemo**. El **nombre del paquete** tenía el formato ``com.<organization>.pushdemo``.

1. Establezca la opción **Versión de Android mínima** en **Android 8.0 (nivel de API 26)** y la opción **Versión de Android de destino** en el **nivel de API** más reciente.

    > [!NOTE]
    > En este tutorial solo se admiten los dispositivos que ejecutan el **nivel de API 26 o posterior**, pero se puede ampliar para admitir dispositivos que ejecuten versiones anteriores.

1. Asegúrese de que los permisos **INTERNET** y **READ_PHONE_STATE** estén habilitados en **Permisos necesarios**.

1. Haga clic en **Aceptar**

### <a name="add-the-xamarin-google-play-services-base-and-xamarinfirebasemessaging-packages"></a>Incorporación de los paquetes de Xamarin Google Play Services Base y Xamarin.Firebase.Messaging

1. En **PushDemo.Android**, pulse **Control** + **clic** en la carpeta **Paquetes** y, a continuación, elija **Administrar paquetes NuGet...** .

1. Busque **Xamarin.GooglePlayServices.Base** (no **Basement**) y asegúrese de que está seleccionado.

1. Busque **Xamarin.Firebase.Messaging** y asegúrese de que está seleccionado.

1. Haga clic en **Agregar paquetes** y, a continuación, haga clic en **Aceptar** cuando se le pida que acepte los **términos de licencia**.

### <a name="add-the-google-services-json-file"></a>Adición del archivo JSON de Google Services

1. **Control** + **clic** en el proyecto `PushDemo.Android` y, a continuación, elija **Archivo existente...** en el menú **Agregar**.

1. Elija el archivo *google-services.json* que descargó anteriormente al configurar el proyecto **PushDemo** en la [consola Firebase](https://console.firebase.google.com) y, a continuación, haga clic en **Abrir**.

1. Cuando se le solicite, elija **copiar el archivo en el directorio**.

1. **Control** + **clic** en el archivo *google-services.json* desde el proyecto `PushDemo.Android` y, a continuación, asegúrese de que **GoogleServicesJson** esté establecido como la **acción de compilación**.

### <a name="handle-push-notifications-for-android"></a>Administración de las notificaciones push para Android

1. **Control** + **clic** en el proyecto `PushDemo.Android`, elija **Nueva carpeta** en el menú **Agregar** y, a continuación, haga clic en **Agregar** con *Servicios* como **nombre de la carpeta**.

1. **Control** + **clic** en la carpeta **Servicios** y, a continuación, elija **Nuevo archivo...** en el menú **Agregar**.

1. Seleccione **General** > **Clase vacía**, escriba *DeviceInstallationService.cs* como **nombre** y, a continuación, haga clic en **Nuevo** para agregar la siguiente implementación.

    ```csharp
    using System;
    using Android.App;
    using Android.Gms.Common;
    using PushDemo.Models;
    using PushDemo.Services;
    using static Android.Provider.Settings;

    namespace PushDemo.Droid.Services
    {
        public class DeviceInstallationService : IDeviceInstallationService
        {
            public string Token { get; set; }

            public bool NotificationsSupported
                => GoogleApiAvailability.Instance
                    .IsGooglePlayServicesAvailable(Application.Context) == ConnectionResult.Success;

            public string GetDeviceId()
                => Secure.GetString(Application.Context.ContentResolver, Secure.AndroidId);

            public DeviceInstallation GetDeviceInstallation(params string[] tags)
            {
                if (!NotificationsSupported)
                    throw new Exception(GetPlayServicesError());

                if (string.isNullOrWhitespace(Token))
                    throw new Exception("Unable to resolve token for FCM");

                var installation = new DeviceInstallation
                {
                    InstallationId = GetDeviceId(),
                    Platform = "fcm",
                    PushChannel = Token
                };

                installation.Tags.AddRange(tags);

                return installation;
            }

            string GetPlayServicesError()
            {
                int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(Application.Context);

                if (resultCode != ConnectionResult.Success)
                    return GoogleApiAvailability.Instance.IsUserResolvableError(resultCode) ?
                               GoogleApiAvailability.Instance.GetErrorString(resultCode) :
                               "This device is not supported";

                return "An error occurred preventing the use of push notifications";
            }
        }
    }
    ```

    > [!NOTE]
    > Esta clase proporciona un identificador único (mediante [Secure.AndroidId](https://docs.microsoft.com/dotnet/api/android.provider.settings.secure.androidid?view=xamarin-android-sdk-9)) como parte de la carga de registro del centro de notificaciones.

1. Agregue otra **clase vacía** a la carpeta **Servicios** denominada *PushNotificationFirebaseMessagingService.cs* y, a continuación, agregue la siguiente implementación.

    ```csharp
    using Android.App;
    using Android.Content;
    using Firebase.Messaging;
    using PushDemo.Services;

    namespace PushDemo.Droid.Services
    {
        [Service]
        [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
        public class PushNotificationFirebaseMessagingService : FirebaseMessagingService
        {
            IPushDemoNotificationActionService _notificationActionService;
            INotificationRegistrationService _notificationRegistrationService;
            IDeviceInstallationService _deviceInstallationService;

            IPushDemoNotificationActionService NotificationActionService
                => _notificationActionService ??
                    (_notificationActionService =
                    ServiceContainer.Resolve<IPushDemoNotificationActionService>());

            INotificationRegistrationService NotificationRegistrationService
                => _notificationRegistrationService ??
                    (_notificationRegistrationService =
                    ServiceContainer.Resolve<INotificationRegistrationService>());

            IDeviceInstallationService DeviceInstallationService
                => _deviceInstallationService ??
                    (_deviceInstallationService =
                    ServiceContainer.Resolve<IDeviceInstallationService>());

            public override void OnNewToken(string token)
            {
                DeviceInstallationService.Token = token;

                NotificationRegistrationService.RefreshRegistrationAsync()
                    .ContinueWith((task) => { if (task.IsFaulted) throw task.Exception; });
            }

            public override void OnMessageReceived(RemoteMessage message)
            {
                if(message.Data.TryGetValue("action", out var messageAction))
                    NotificationActionService.TriggerAction(messageAction);
            }
        }
    }
    ```

1. En **MainActivity.cs**, asegúrese de que se han agregado los siguientes espacios de nombres en la parte superior del archivo.

    ```csharp
    using System;
    using Android.App;
    using Android.Content;
    using Android.Content.PM;
    using Android.OS;
    using Android.Runtime;
    using Firebase.Iid;
    using PushDemo.Droid.Services;
    using PushDemo.Services;
    ```

1. En **MainActivity.cs**, establezca **LaunchMode** en **SingleTop** para que **MainActivity** no se cree de nuevo cuando se abra.

    ```csharp
    [Activity(
        Label = "PushDemo",
        LaunchMode = LaunchMode.SingleTop,
        Icon = "@mipmap/icon",
        Theme = "@style/MainTheme",
        MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    ```

1. Agregue propiedades privadas y sus respectivos campos de respaldo para almacenar una referencia a las implementaciones **IPushNotificationActionService** y **IDeviceInstallationService**.

    ```csharp
    IPushDemoNotificationActionService _notificationActionService;
    IDeviceInstallationService _deviceInstallationService;

    IPushDemoNotificationActionService NotificationActionService
        => _notificationActionService ??
            (_notificationActionService =
            ServiceContainer.Resolve<IPushDemoNotificationActionService>());

    IDeviceInstallationService DeviceInstallationService
        => _deviceInstallationService ??
            (_deviceInstallationService =
            ServiceContainer.Resolve<IDeviceInstallationService>());
    ```

1. Implemente la interfaz **IOnSuccessListener** para recuperar y almacenar el token de **Firebase**.

    ```csharp
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity, Android.Gms.Tasks.IOnSuccessListener
    {
        ...

        public void OnSuccess(Java.Lang.Object result)
            => DeviceInstallationService.Token =
                result.Class.GetMethod("getToken").Invoke(result).ToString();
    }
    ```

1. Agregue un nuevo método llamado **ProcessNotificationActions** que comprobará si una **intención** determinada tiene un valor adicional denominado *acción*. Desencadene condicionalmente esa acción mediante la implementación de **IPushDemoNotificationActionService**.

    ```csharp
    void ProcessNotificationActions(Intent intent)
    {
        try
        {
            if (intent?.HasExtra("action") == true)
            {
                var action = intent.GetStringExtra("action");

                if (!string.IsNullOrEmpty(action))
                    NotificationActionService.TriggerAction(action);
            }
        }
        catch (Exception ex)
        {
            System.Diagnostics.Debug.WriteLine(ex.Message);
        }
    }
    ```

1. Reemplace el método **OnNewIntent** para que llame al método **ProcessNotificationActions**.

    ```csharp
    protected override void OnNewIntent(Intent intent)
    {
        base.OnNewIntent(intent);
        ProcessNotificationActions(intent);
    }
    ```

    > [!NOTE]
    > Dado que **LaunchMode** para la **actividad** está establecido en **SingleTop**, se enviará una **intención** a la instancia de la **actividad** existente a través del método **OnNewIntent** en lugar de a través del método **OnCreate** y, por tanto, debe controlar una intención entrante en ambos métodos, **OnCreate** y **OnNewIntent**.

1. Actualice el método **OnCreate** para que llame a `Bootstrap.Begin` justo después de la llamada a `base.OnCreate` que pasa la implementación específica de la plataforma de **IDeviceInstallationService**.

    ```csharp
    Bootstrap.Begin(() => new DeviceInstallationService());
    ```

1. En el mismo método llame condicionalmente a **GetInstanceId** en la instancia de **FirebaseApp**, justo después de la llamada a `Bootstrap.Begin`, agregando **MainActivity** como **IOnSuccessListener**.

    ```csharp
    if (DeviceInstallationService.NotificationsSupported)
    {
        FirebaseInstanceId.GetInstance(Firebase.FirebaseApp.Instance)
            .GetInstanceId()
            .AddOnSuccessListener(this);
    }
    ```

1. Todavía en **OnCreate**, llame a **ProcessNotificationActions** justo después de la llamada a `LoadApplication` que está pasando en la **intención** actual.

    ```cs
    ...

    LoadApplication(new App());

    ProcessNotificationActions(Intent);
    ```

> [!NOTE]
> Debe volver a registrar la aplicación cada vez que la ejecute y la detenga en una sesión de depuración para seguir recibiendo notificaciones push.
