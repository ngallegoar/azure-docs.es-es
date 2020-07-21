---
author: mikeparker104
ms.author: miparker
ms.date: 07/07/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 9a8b76c0887cff498edd90aedf5cc6eb674fc7e5
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156325"
---
### <a name="configure-the-runner-target-and-infoplist"></a>Configuración del destino de Runner e Info.plist

1. En **Visual Studio Code**, con la tecla **Control** pulsada, **haga clic** en la carpeta **ios** y, después, elija **Abrir en Xcode**.

1. En **Xcode**, haga clic en **Runner** (el **xcodeproj** de la parte superior, no la carpeta), seleccione el destino de **Runner** y, a continuación, haga clic en **Signing & Capabilities** (Firma y funcionalidades). Con la configuración de compilación **All** (Todo) seleccionada, seleccione su cuenta de desarrollador para el **equipo**. Asegúrese de que la opción, "Automatically manage signing" (Administrar automáticamente la firma) esté seleccionada, y las opciones Certificado de firma y Perfil de aprovisionamiento estén también seleccionadas automáticamente.

    > [!NOTE]
    > Si no ve el nuevo valor de Perfil de aprovisionamiento, intente actualizar los perfiles para la Identidad de firma desde **Xcode** > **Preferencias** > **Cuenta** y, a continuación, seleccione el botón **Download Manual Profiles** (Descargar perfiles manuales) para descargar los perfiles.

1. Haga clic en **+ Capability** (+ Funcionalidad) y busque **Push Notifications** (Notificaciones push). **Haga doble clic** en **Push Notifications** (Notificaciones push) para agregar esta funcionalidad.

1. Abra **Info.plist** y establezca el valor **Versión mínima del sistema** en **13.0**.

    > [!NOTE]
    > En este tutorial solo se admiten los dispositivos que ejecutan **iOS 13.0 y versiones posteriores**, pero se puede ampliar para admitir dispositivos que ejecuten versiones anteriores.

1. Abra **Runner.entitlements** y asegúrese de que el valor **Entorno APS** está establecido en **desarrollo**.

### <a name="handle-push-notifications-for-ios"></a>Administración de las notificaciones push para iOS

1. Con la tecla **Control** pulsada, **haga clic** en la carpeta **Runner** (en el proyecto de Runner), seleccione **New Group** (Nuevo grupo) y use **Services** como nombre.

1. Con la tecla **Control** pulsada, **haga clic** en la carpeta **Services** y seleccione **New File...** (Nuevo archivo). A continuación, elija **Swift File** (Archivo de Swift) y haga clic en **Next** (Siguiente). Especifique **DeviceInstallationService** como nombre y haga clic en **Create** (Crear).

1. Implemente **DeviceInstallationService.swift** mediante el código siguiente.

    ```swift
    import Foundation

    class DeviceInstallationService {

        enum DeviceRegistrationError: Error {
            case notificationSupport(message: String)
        }

        var token : Data? = nil

        let DEVICE_INSTALLATION_CHANNEL = "com.<your_organization>.pushdemo/deviceinstallation"
        let GET_DEVICE_ID = "getDeviceId"
        let GET_DEVICE_TOKEN = "getDeviceToken"
        let GET_DEVICE_PLATFORM = "getDevicePlatform"

        private let deviceInstallationChannel : FlutterMethodChannel

        var notificationsSupported : Bool {
            get {
                if #available(iOS 13.0, *) {
                    return true
                }
                else {
                    return false
                }
            }
        }

        init(withBinaryMessenger binaryMessenger : FlutterBinaryMessenger) {
            deviceInstallationChannel = FlutterMethodChannel(name: DEVICE_INSTALLATION_CHANNEL, binaryMessenger: binaryMessenger)
            deviceInstallationChannel.setMethodCallHandler(handleDeviceInstallationCall)
        }

        func getDeviceId() -> String {
            return UIDevice.current.identifierForVendor!.description
        }

        func getDeviceToken() throws -> String {
            if(!notificationsSupported) {
                let notificationSupportError = getNotificationsSupportError()
                throw DeviceRegistrationError.notificationSupport(message: notificationSupportError)
            }

            if (token == nil) {
                throw DeviceRegistrationError.notificationSupport(message: "Unable to resolve token for APNS.")
            }

            return token!.reduce("", {$0 + String(format: "%02X", $1)})
        }

        func getDevicePlatform() -> String {
            return "apns"
        }

        private func handleDeviceInstallationCall(call: FlutterMethodCall, result: @escaping FlutterResult) {
            switch call.method {
            case GET_DEVICE_ID:
                result(getDeviceId())
            case GET_DEVICE_TOKEN:
                getDeviceToken(result: result)
            case GET_DEVICE_PLATFORM:
                result(getDevicePlatform())
            default:
                result(FlutterMethodNotImplemented)
            }
        }

        private func getDeviceToken(result: @escaping FlutterResult) {
            do {
                let token = try getDeviceToken()
                result(token)
            }
            catch let error {
                result(FlutterError(code: "UNAVAILABLE", message: error.localizedDescription, details: nil))
            }
        }

        private func getNotificationsSupportError() -> String {

            if (!notificationsSupported) {
                return "This app only supports notifications on iOS 13.0 and above. You are running \(UIDevice.current.systemVersion)"
            }

            return "An error occurred preventing the use of push notifications."
        }
    }
    ```

    > [!NOTE]
    > Esta clase implementa el homólogo específico de la plataforma para el canal `com.<your_organization>.pushdemo/deviceinstallation`. Esto está definido en la parte de Flutter de la aplicación en **DeviceInstallationService.dart**. En este caso, las llamadas se realizan desde el código común al host nativo. Asegúrese de reemplazar **<su_organización>** por el nombre de su propia organización siempre que se use este código.
    >
    > Esta clase proporciona un identificador único (mediante el uso del valor [UIDevice.IdentifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)) como parte de la carga de registro del centro de notificaciones.

1. Agregue otro **archivo de Swift** a la carpeta **Services** denominado *NotificationRegistrationService* y, a continuación, agregue el siguiente código.

    ```swift
    import Foundation

    class NotificationRegistrationService {

        let NOTIFICATION_REGISTRATION_CHANNEL = "com.<your_organization>.pushdemo/notificationregistration"
        let REFRESH_REGISTRATION = "refreshRegistration"

        private let notificationRegistrationChannel : FlutterMethodChannel

        init(withBinaryMessenger binaryMessenger : FlutterBinaryMessenger) {
           notificationRegistrationChannel = FlutterMethodChannel(name: NOTIFICATION_REGISTRATION_CHANNEL, binaryMessenger: binaryMessenger)
        }

        func refreshRegistration() {
            notificationRegistrationChannel.invokeMethod(REFRESH_REGISTRATION, arguments: nil)
        }
    }
    ```

    > [!NOTE]
    > Esta clase implementa el homólogo específico de la plataforma para el canal `com.<your_organization>.pushdemo/notificationregistration`. Esto está definido en la parte de Flutter de la aplicación en **NotificationRegistrationService.dart**. En este caso, las llamadas se realizan desde el host nativo al código común. Una vez más, recuerde reemplazar **<su_organización>** por el nombre de su propia organización siempre que se use este código.

1. Agregue otro **archivo de Swift** a la carpeta **Services** denominado *NotificationActionService* y, a continuación, agregue el siguiente código.

    ```swift
    import Foundation

    class NotificationActionService {

        let NOTIFICATION_ACTION_CHANNEL = "com.<your_organization>.pushdemo/notificationaction"
        let TRIGGER_ACTION = "triggerAction"
        let GET_LAUNCH_ACTION = "getLaunchAction"

        private let notificationActionChannel: FlutterMethodChannel

        var launchAction: String? = nil

        init(withBinaryMessenger binaryMessenger: FlutterBinaryMessenger) {
            notificationActionChannel = FlutterMethodChannel(name: NOTIFICATION_ACTION_CHANNEL, binaryMessenger: binaryMessenger)
            notificationActionChannel.setMethodCallHandler(handleNotificationActionCall)
        }

        func triggerAction(action: String) {
           notificationActionChannel.invokeMethod(TRIGGER_ACTION, arguments: action)
        }

        private func handleNotificationActionCall(call: FlutterMethodCall, result: @escaping FlutterResult) {
            switch call.method {
            case GET_LAUNCH_ACTION:
                result(launchAction)
            default:
                result(FlutterMethodNotImplemented)
            }
        }
    }
    ```

    > [!NOTE]
    > Esta clase implementa el homólogo específico de la plataforma para el canal `com.<your_organization>.pushdemo/notificationaction`. Esto está definido en la parte de Flutter de la aplicación en **NotificationActionService.dart**. En este caso, se pueden realizar llamadas en ambas direcciones. Asegúrese de reemplazar **<su_organización>** por el nombre de su propia organización siempre que se use este código.

1. En **AppDelegate.swift**, agregue variables para almacenar una referencia a los servicios que ha creado anteriormente.

    ```swift
    var deviceInstallationService : DeviceInstallationService?
    var notificationRegistrationService : NotificationRegistrationService?
    var notificationActionService : NotificationActionService?
    ```

1. Agregue una función llamada **processNotificationActions** para procesar los datos de notificación. Desencadene condicionalmente esa acción o almacénela para su uso posterior si la acción se procesa durante el inicio de la aplicación.

    ```swift
    func processNotificationActions(userInfo: [AnyHashable : Any], launchAction: Bool = false) {
        if let action = userInfo["action"] as? String {
            if (launchAction) {
                notificationActionService?.launchAction = action
            }
            else {
                notificationActionService?.triggerAction(action: action)
            }
        }
    }
    ```

1. Invalide la función **didRegisterForRemoteNotificationsWithDeviceToken** estableciendo el valor de **token** para **DeviceInstallationService**. A continuación, llame a **refreshRegistration** en **NotificationRegistrationService**.

    ```swift
    override func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
      deviceInstallationService?.token = deviceToken
      notificationRegistrationService?.refreshRegistration()
    }
    ```

1. Invalide la función **didReceiveRemoteNotification** pasando el argumento **userInfo** a la función **processNotificationActions**.

    ```swift
    override func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any]) {
        processNotificationActions(userInfo: userInfo)
    }
    ```

1. Invalide la función **didFailToRegisterForRemoteNotificationsWithError** para registrar el error.

    ```swift
    override func application(_ application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: Error) {
        print(error);
    }
    ```

    > [!NOTE]
    > Esto es claramente un marcador de posición. Para escenarios de producción tendrá que implementar el registro y el control de errores apropiados.

1. En **didFinishLaunchingWithOptions**, cree una instancia de las variables **deviceInstallationService**, **notificationRegistrationService**y **notificationActionService**.

    ```swift
    let controller : FlutterViewController = window?.rootViewController as! FlutterViewController

    deviceInstallationService = DeviceInstallationService(withBinaryMessenger: controller.binaryMessenger)
    notificationRegistrationService = NotificationRegistrationService(withBinaryMessenger: controller.binaryMessenger)
    notificationActionService = NotificationActionService(withBinaryMessenger: controller.binaryMessenger)
    ```

1. En la misma función, solicite autorización condicionalmente y regístrese para notificaciones remotas.

    ```swift
    if #available(iOS 13.0, *) {
      UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
          (granted, error) in

          if (granted)
          {
              DispatchQueue.main.async {
                  let pushSettings = UIUserNotificationSettings(types: [.alert, .sound, .badge], categories: nil)
                  application.registerUserNotificationSettings(pushSettings)
                  application.registerForRemoteNotifications()
              }
          }
      }
    }
    ```

1. Si **launchOptions** contiene la clave **remoteNotification**, llame a **processNotificationActions** al final de la función **didFinishLaunchingWithOptions**. Pase el objeto **userInfo** resultante y use *true* para el argumento *launchAction*. Un valor *true* denota que la acción se procesa durante el inicio de la aplicación.

    ```swift
    if let userInfo = launchOptions?[.remoteNotification] as? [AnyHashable : Any] {
        processNotificationActions(userInfo: userInfo, launchAction: true)
    }
    ```
