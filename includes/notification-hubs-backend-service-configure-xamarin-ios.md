---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 1dc491084f65bc90397b0897de6b6cfe4f2fd410
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448764"
---
### <a name="configure-infoplist-and-entitlementsplist"></a>Configuración de Info.plist y Entitlements.plist

1. Asegúrese de haber iniciado sesión en la **cuenta de desarrollador de Apple** en **Visual Studio** > **Preferencias...**  > **Publicación** > **Cuentas de desarrollador de Apple**, y que se han descargado el *Certificado* y *Perfil de aprovisionamiento* apropiados. Debe haber creado estos recursos como parte de los pasos anteriores.

1. En **PushDemo.iOS**, abra **Info.plist** y asegúrese de que el valor **BundleIdentifier** coincida con el valor que se usó para el perfil de aprovisionamiento respectivo en el [portal para desarrolladores de Apple](https://developer.apple.com). **BundleIdentifier** tenía el formato ``com.<organization>.PushDemo``.

1. En el mismo archivo, establezca el valor **Versión mínima del sistema** en **13.0**.

    > [!NOTE]
    > En este tutorial solo se admiten los dispositivos que ejecutan **iOS 13.0 y versiones posteriores**, pero se puede ampliar para admitir dispositivos que ejecuten versiones anteriores.

1. Abra **Opciones del proyecto** para **PushDemo.iOS** (haga doble clic en el proyecto).

1. En **Opciones del proyecto**, en **Compilación > Firma de lote para iOS**, asegúrese de que la cuenta de desarrollador esté seleccionada en **Equipo**. A continuación, asegúrese de que la opción, "Automatically manage signing" (Administrar automáticamente la firma) esté seleccionada, y las opciones Certificado de firma y Perfil de aprovisionamiento estén también seleccionadas automáticamente.

    > [!NOTE]
    > Si *Certificado de firma* y *Perfil de aprovisionamiento* no se han seleccionado automáticamente, elija **Aprovisionamiento manual** y, a continuación, haga clic en **Bundle Signing Options** (Opciones de firma de lote). Asegúrese de que su *Equipo* está seleccionado en **Identidad de firma**, y de que el perfil de aprovisionamiento específico de *PushDemo* está seleccionado para **Perfil de aprovisionamiento**, tanto para la configuración de **Depurar** como para la de **Versión**, y compruebe que **iPhone** está seleccionado como **Plataforma** en ambos casos.

1. En **PushDemo.iOS**, abra **Entitlements.plist** y asegúrese de que la casilla **Habilitar notificaciones push** está seleccionada cuando se visualiza en la pestaña **Derechos**. A continuación, asegúrese de que el valor **Entorno APS** está establecida en **desarrollo** cuando se visualiza en la pestaña **Origen**.

### <a name="handle-push-notifications-for-ios"></a>Administración de las notificaciones push para iOS

1. Presione **Ctrl** + **y haga clic** en el proyecto **PushDemo.iOS**, elija **Nueva carpeta** en el menú **Agregar** y, a continuación, haga clic en **Agregar** con *Modelos* como **Nombre de la carpeta**.

1. Presione **Control** + **y haga clic** en la carpeta **Servicios** y, a continuación, elija **Nuevo archivo...** en el menú **Agregar**.

1. Seleccione **General** > **Clase vacía**, escriba *DeviceInstallationService.cs* en **Nombre** y, a continuación, haga clic en **Nuevo** para agregar la siguiente implementación.

    ```csharp
    using System;
    using PushDemo.Models;
    using PushDemo.Services;
    using UIKit;

    namespace PushDemo.iOS.Services
    {
        public class DeviceInstallationService : IDeviceInstallationService
        {
            const int SupportedVersionMajor = 13;
            const int SupportedVersionMinor = 0;

            public string Token { get; set; }

            public bool NotificationsSupported
                => UIDevice.CurrentDevice.CheckSystemVersion(SupportedVersionMajor, SupportedVersionMinor);

            public string GetDeviceId()
                => UIDevice.CurrentDevice.IdentifierForVendor.ToString();

            public DeviceInstallation GetDeviceInstallation(params string[] tags)
            {
                if (!NotificationsSupported)
                    throw new Exception(GetNotificationsSupportError());

                if (string.isNullOrWhitespace(Token))
                    throw new Exception("Unable to resolve token for APNS");

                var installation = new DeviceInstallation
                {
                    InstallationId = GetDeviceId(),
                    Platform = "apns",
                    PushChannel = Token
                };

                installation.Tags.AddRange(tags);

                return installation;
            }

            string GetNotificationsSupportError()
            {
                if (!NotificationsSupported)
                    return $"This app only supports notifications on iOS {SupportedVersionMajor}.{SupportedVersionMinor} and above. You are running {UIDevice.CurrentDevice.SystemVersion}.";

                if (Token == null)
                    return $"This app can support notifications but you must enable this in your settings.";


                return "An error occurred preventing the use of push notifications";
            }
        }
    }
    ```

    > [!NOTE]
    > Esta clase proporciona un identificador único (mediante el uso del valor [UIDevice.IdentifierForVendor](https://docs.microsoft.com/dotnet/api/uikit.uidevice.identifierforvendor?view=xamarin-ios-sdk-12)) y la carga de registro del centro de notificaciones.

1. Agregue una nueva carpeta al proyecto **PushDemo.iOS** denominada *Extensiones* y, a continuación, agregue una **clase vacía** a esa carpeta llamada *NSDataExtensions.cs* con la siguiente implementación.

    ```csharp
    using System.Text;
    using Foundation;

    namespace PushDemo.iOS.Extensions
    {
        internal static class NSDataExtensions
        {
            internal static string ToHexString(this NSData data)
            {
                var bytes = data.ToArray();

                if (bytes == null)
                    return null;

                StringBuilder sb = new StringBuilder(bytes.Length * 2);

                foreach (byte b in bytes)
                    sb.AppendFormat("{0:x2}", b);

                return sb.ToString().ToUpperInvariant();
            }
        }
    }
    ```

1. En **AppDelegate.cs**, asegúrese de que se han agregado los siguientes espacios de nombres en la parte superior del archivo.

    ```csharp
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Foundation;
    using PushDemo.iOS.Extensions;
    using PushDemo.iOS.Services;
    using PushDemo.Services;
    using UIKit;
    using UserNotifications;
    using Xamarin.Essentials;
    ```

1. Agregue propiedades privadas y sus respectivos campos de respaldo para almacenar una referencia a las implementaciones **IPushDemoNotificationActionService**, **INotificationRegistrationService** y **IDeviceInstallationService**.

    ```csharp
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
    ```

1. Agregue el método **RegisterForRemoteNotifications** para registrar la configuración de notificaciones de usuario y, a continuación, para las notificaciones remotas con **APNs**.

    ```csharp
    void RegisterForRemoteNotifications()
    {
        MainThread.BeginInvokeOnMainThread(() =>
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert |
                UIUserNotificationType.Badge |
                UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
        });
    }
    ```

1. Agregue el método **CompleteRegistrationAsync** para establecer el valor de la propiedad `IDeviceInstallationService.Token`. Actualice el registro y almacene en caché el token del dispositivo si se ha actualizado desde que se almacenó por última vez.

    ```csharp
    Task CompleteRegistrationAsync(NSData deviceToken)
    {
        DeviceInstallationService.Token = deviceToken.ToHexString();
        return NotificationRegistrationService.RefreshRegistrationAsync();
    }
    ```

1. Agregue el método **ProcessNotificationActions** para procesar los datos de notificación de **NSDictionary** y llamar condicionalmente a **NotificationActionService.TriggerAction**.

    ```csharp
    void ProcessNotificationActions(NSDictionary userInfo)
    {
        if (userInfo == null)
            return;

        try
        {
            var actionValue = userInfo.ObjectForKey(new NSString("action")) as NSString;

            if (!string.IsNullOrWhiteSpace(actionValue?.Description))
                NotificationActionService.TriggerAction(actionValue.Description);
        }
        catch (Exception ex)
        {
            Debug.WriteLine(ex.Message);
        }
    }
    ```

1. Invalide el método **RegisteredForRemoteNotifications** pasando el argumento **deviceToken** al método **CompleteRegistrationAsync**.

    ```csharp
    public override void RegisteredForRemoteNotifications(
        UIApplication application,
        NSData deviceToken)
        => CompleteRegistrationAsync(deviceToken).ContinueWith((task)
            => { if (task.IsFaulted) throw task.Exception; });
    ```

1. Invalide el método **ReceivedRemoteNotification** pasando el argumento **userInfo** al método **ProcessNotificationActions**.

    ```csharp
    public override void ReceivedRemoteNotification(
        UIApplication application,
        NSDictionary userInfo)
        => ProcessNotificationActions(userInfo);
    ```

1. Invalide el método **FailedToRegisterForRemoteNotifications** para registrar el error.

    ```csharp
    public override void FailedToRegisterForRemoteNotifications(
        UIApplication application,
        NSError error)
        => Debug.WriteLine(error.Description);
    ```

    > [!NOTE]
    > Esto es claramente un marcador de posición. Para escenarios de producción tendrá que implementar el registro y el control de errores apropiados.

1. Actualice el método **FinishedLaunching** para llamar a `Bootstrap.Begin` justo después de la llamada a `Forms.Init` que pasa la implementación específica de la plataforma de **IDeviceInstallationService**.

    ```csharp
    Bootstrap.Begin(() => new DeviceInstallationService());
    ```

1. En el mismo método, solicite autorización condicionalmente y regístrese para notificaciones remotas inmediatamente después de `Bootstrap.Begin`.

    ```csharp
    if (DeviceInstallationService.NotificationsSupported)
    {
        UNUserNotificationCenter.Current.RequestAuthorization(
                UNAuthorizationOptions.Alert |
                UNAuthorizationOptions.Badge |
                UNAuthorizationOptions.Sound,
                (approvalGranted, error) =>
                {
                    if (approvalGranted && error == null)
                        RegisterForRemoteNotifications();
                });
    }
    ```

1. Todavía en **FinishedLaunching**, llame a **ProcessNotificationActions** inmediatamente después de la llamada a `LoadApplication` si el argumento **options** contiene la clave **UIApplication.LaunchOptionsRemoteNotificationKey** que pasa en el objeto **userInfo** resultante.

    ```csharp
    using (var userInfo = options?.ObjectForKey(
        UIApplication.LaunchOptionsRemoteNotificationKey) as NSDictionary)
            ProcessNotificationActions(userInfo);
    ```
