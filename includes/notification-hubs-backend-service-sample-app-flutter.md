---
author: mikeparker104
ms.author: miparker
ms.date: 07/07/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 2f8cd5e554980225850780cc474106c3dc69288a
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156336"
---
### <a name="create-the-flutter-solution"></a>Creación de la solución Flutter

1. Abra una nueva instancia de **Visual Studio Code**.

1. Abra la **Paleta de comandos** (**Mayús** + **Comando** + **P**).

1. Seleccione el comando **Flutter: New Project** (Flutter: Nuevo proyecto) y presione **Entrar**.

1. Escriba *push_demo* en el campo **Nombre del proyecto** y seleccione un valor para **Ubicación del proyecto**.

1. Cuando se le pida, elija **Get Packages** (Obtener paquetes).

1. Presione la tecla **Control** y **haga clic** en la carpeta **kotlin** (en **app** > **src** > **main**) y elija **Mostrar en Finder**. A continuación, cambie el nombre de las carpetas secundarias (en la carpeta **kotlin**) a ```com```, ```<your_organization>``` y ```pushdemo``` respectivamente.

    > [!NOTE]
    > Al usar la plantilla de **Visual Studio Code**, de forma predeterminada estas carpetas son **com**, **example**, **<nombre_del_proyecto>** . Suponiendo que se usa **mobcat** como **organización**, la estructura de las carpetas debería aparecer como:
    >
    > - kotlin
    >     - com
    >         - mobcat
    >             - pushdemo

1. De nuevo en **Visual Studio Code**, actualice el valor de **applicationId** de **android** > **app** > **build.gradle** a `com.<your_organization>.pushdemo`.

    > [!NOTE]
    > Debe usar su propio nombre de organización para el marcador de posición *<su_organización>* . Por ejemplo, al usar **mobcat** como organización se generará un valor *com.mobcat.pushdemo* de **nombre de paquete**.

1. Actualice el atributo **package** en los archivos **AndroidManifest.xml**, en **src** > **debug**, **src** > **main** y **src** > **profile** respectivamente. Asegúrese de que los valores coinciden con el de **applicationId** que usó en el paso anterior.

    ```xml
    <manifest
        xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.<your_organization>.pushdemo>">
        ...
    </manifest>
    ```

1. Actualice el atributo ```android:label``` del archivo **AndroidManifest.xml** de **src** > **main** a *PushDemo*. A continuación, agregue el atributo ```android:allowBackup``` directamente en ```android:label``` y establezca su valor en **false**.

    ```xml
    <application
        android:name="io.flutter.app.FlutterApplication"
        android:label="PushDemo"
        android:allowBackup="false"
        android:icon="@mipmap/ic_launcher">
        ...
    </application>
    ```

1. Abra el archivo de nivel de aplicación **build.gradle** (**Android** > **app** > **build.gradle**) y actualice *compileSdkVersion* (de la sección **Android**) para usar la API **29**. A continuación, actualice los valores *minSdkVersion* y *targetSdkVersion* (de la sección **defaultConfig**) a **26** y **29** respectivamente.

    > [!NOTE]
    > En este tutorial solo se admiten los dispositivos que ejecutan el **nivel de API 26 o posterior**, pero se puede ampliar para admitir dispositivos que ejecuten versiones anteriores.

1. Presione la tecla **Control** y **haga clic** en la carpeta **ios**, después, elija **Abrir en Xcode**.

1. En **Xcode**, haga clic en **Runner** (Ejecutor) (en **xcodeproj** de la parte superior, no en la carpeta). A continuación, seleccione el destino **Runner** (Ejecutor) y la pestaña **General**. Con la configuración de compilación **All** (Todo) seleccionada, actualice el valor de **Bundle Identifier** (Identificador del lote) a `com.<your_organization>.PushDemo`.

    > [!NOTE]
    > Debe usar su propio nombre de organización para el marcador de posición *<su_organización>* . Por ejemplo, al usar **mobcat** como organización se generará un valor *com.mobcat.PushDemo* de **Bundle Identifier** (Identificador del lote).

1. Haga clic en **info.plist** y actualice el valor de **Bundle name** (Nombre del lote) a **PushDemo**.

1. Cierre **Xcode** y vuelva a **Visual Studio Code**.

1. De nuevo en **Visual Studio Code**, abra **pubspec.yaml** y agregue [http](https://pub.dev/packages/http) y [flutter_secure_storage](https://pub.dev/packages/flutter_secure_storage) **Dart packages** (Paquetes de Dart) como dependencias. A continuación, guarde el archivo y haga clic en **Get Packages** (Obtener paquetes) cuando se le solicite.

    ```yaml
    dependencies:
      flutter:
        sdk: flutter

      http: ^0.12.1
      flutter_secure_storage: ^3.3.3
    ```

1. En **Terminal**, cambie el directorio a la carpeta **ios** (para el proyecto de Flutter). A continuación, ejecute el comando [**pod install**](https://guides.cocoapods.org/using/getting-started.html#installation) para instalar nuevos pods (obligatorio para el paquete [flutter_secure_storage](https://pub.dev/packages/flutter_secure_storage)).

1. Presione la tecla **Control** y **haga clic** en la carpeta **lib**, después, elija **Archivo nuevo** en el menú con *main_page.dart* como nombre de archivo. Después agregue el siguiente código.

    ```kotlin
    import 'package:flutter/material.dart';

    class MainPage extends StatefulWidget {
      @override
      _MainPageState createState() => _MainPageState();
    }

    class _MainPageState extends State<MainPage> {
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          body: SafeArea(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: <Widget>[],
            )
          )
        );
      }
    }
    ```

1. En **main.dart**, reemplace el código de plantilla por el siguiente.

    ```dart
    import 'package:flutter/material.dart';
    import 'package:push_demo/main_page.dart';

    final navigatorKey = GlobalKey<NavigatorState>();

    void main() => runApp(MaterialApp(home: MainPage(), navigatorKey: navigatorKey));
    ```

1. En **Terminal**, compile y ejecute la aplicación en cada plataforma de destino para probar las ejecuciones de aplicaciones con plantilla en los dispositivos. Asegúrese de que los dispositivos compatibles estén conectados.

    ```bash
    flutter run
    ```

### <a name="implement-the-cross-platform-components"></a>Implementación de componentes multiplataforma

1. Presione la tecla **Control** y **haga clic** en la carpeta **lib**, después, elija **Nueva carpeta** en el menú con *models* como **Nombre de la carpeta**.

1. Presione la tecla **Control** y **haga clic** en la carpeta **models**, después, elija **Archivo nuevo** en el menú con *device_installation.dart* como nombre de archivo. Después, agregue el siguiente código.

    ```dart
    class DeviceInstallation {
        final String deviceId;
        final String platform;
        final String token;
        final List<String> tags;

        DeviceInstallation(this.deviceId, this.platform, this.token, this.tags);

        DeviceInstallation.fromJson(Map<String, dynamic> json)
          : deviceId = json['installationId'],
            platform = json['platform'],
            token = json['pushChannel'],
            tags = json['tags'];

        Map<String, dynamic> toJson() =>
        {
          'installationId': deviceId,
          'platform': platform,
          'pushChannel': token,
          'tags': tags,
        };
    }
    ```

1. Agregue un nuevo archivo a la carpeta **models** llamado *push_demo_action.dart* que defina la enumeración de las acciones que se admiten en este ejemplo.

    ```dart
    enum PushDemoAction {
      actionA,
      actionB,
    }
    ```

1. Agregue una nueva carpeta al proyecto denominada **services** y, a continuación, agregue un archivo nuevo a esa carpeta llamado *device_installation_service.dart* con la siguiente implementación.

    ```dart
    import 'package:flutter/services.dart';

    class DeviceInstallationService {
      static const deviceInstallation = const MethodChannel('com.<your_organization>.pushdemo/deviceinstallation');
      static const String getDeviceIdChannelMethod = "getDeviceId";
      static const String getDeviceTokenChannelMethod = "getDeviceToken";
      static const String getDevicePlatformChannelMethod = "getDevicePlatform";

      Future<String> getDeviceId() {
        return deviceInstallation.invokeMethod(getDeviceIdChannelMethod);
      }

      Future<String> getDeviceToken() {
        return deviceInstallation.invokeMethod(getDeviceTokenChannelMethod);
      }

      Future<String> getDevicePlatform() {
        return deviceInstallation.invokeMethod(getDevicePlatformChannelMethod);
      }
    }
    ```

    > [!NOTE]
    > Debe usar su propio nombre de organización para el marcador de posición *<su_organización>* . Por ejemplo, al usar **mobcat** como organización se generará un nombre *com.mobcat.pushdemo/deviceinstallation* de [MethodChannel](https://api.flutter.dev/flutter/services/MethodChannel-class.html).
    >
    > Esta clase encapsula el trabajo con la plataforma nativa subyacente para adquirir los detalles de instalación de dispositivos necesarios. [MethodChannel](https://api.flutter.dev/flutter/services/MethodChannel-class.html) facilita la comunicación asincrónica bidireccional con las plataformas nativas subyacentes. El homólogo específico de la plataforma para este canal se creará en los pasos posteriores.

1. Agregue otro archivo a esa carpeta llamado *notification_action_service.dart* con la siguiente implementación.

    ```dart
    import 'package:flutter/services.dart';
    import 'dart:async';
    import 'package:push_demo/models/push_demo_action.dart';

    class NotificationActionService {
      static const notificationAction =
          const MethodChannel('com.<your_organization>.pushdemo/notificationaction');
      static const String triggerActionChannelMethod = "triggerAction";
      static const String getLaunchActionChannelMethod = "getLaunchAction";

      final actionMappings = {
        'action_a' : PushDemoAction.actionA,
        'action_b' : PushDemoAction.actionB
      };

      final actionTriggeredController = StreamController.broadcast();

      NotificationActionService() {
        notificationAction
            .setMethodCallHandler(handleNotificationActionCall);
      }

      Stream get actionTriggered => actionTriggeredController.stream;

      Future<void> triggerAction({action: String}) async {

        if (!actionMappings.containsKey(action)) {
          return;
        }

        actionTriggeredController.add(actionMappings[action]);
      }

      Future<void> checkLaunchAction() async {
        final launchAction = await notificationAction.invokeMethod(getLaunchActionChannelMethod) as String;

        if (launchAction != null) {
          triggerAction(action: launchAction);
        }
      }

      Future<void> handleNotificationActionCall(MethodCall call) async {
        switch (call.method) {
          case triggerActionChannelMethod:
            return triggerAction(action: call.arguments as String);
          default:
            throw MissingPluginException();
            break;
        }
      }
    }
    ```

    > [!NOTE]
    > Se usará como mecanismo sencillo para centralizar el control de las acciones de notificación, de modo que se puedan controlar mediante distintas plataformas con una enumeración fuertemente tipada. El servicio permite que la plataforma nativa subyacente desencadene una acción si se especifica una en la carga de notificación. También permite al código común comprobar retrospectivamente si se especificó una acción durante el inicio de la aplicación cuando Flutter esté ya listo para procesarla. Por ejemplo, cuando la aplicación se inicia al pulsar en una notificación del centro de notificaciones.

1. Agregue un archivo nuevo a la carpeta **services** llamado *notification_registration_service.dart* con la siguiente implementación.

    ```dart
    import 'dart:convert';
    import 'package:flutter/services.dart';
    import 'package:http/http.dart' as http;
    import 'package:push_demo/services/device_installation_service.dart';
    import 'package:push_demo/models/device_installation.dart';
    import 'package:flutter_secure_storage/flutter_secure_storage.dart';

    class NotificationRegistrationService {
      static const notificationRegistration =
          const MethodChannel('com.<your_organization>.pushdemo/notificationregistration');

      static const String refreshRegistrationChannelMethod = "refreshRegistration";
      static const String installationsEndpoint = "api/notifications/installations";
      static const String cachedDeviceTokenKey = "cached_device_token";
      static const String cachedTagsKey = "cached_tags";

      final deviceInstallationService = DeviceInstallationService();
      final secureStorage = FlutterSecureStorage();

      String baseApiUrl;
      String apikey;

      NotificationRegistrationService(this.baseApiUrl, this.apikey) {
        notificationRegistration
            .setMethodCallHandler(handleNotificationRegistrationCall);
      }

      String get installationsUrl => "$baseApiUrl$installationsEndpoint";

      Future<void> deregisterDevice() async {
        final cachedToken = await secureStorage.read(key: cachedDeviceTokenKey);
        final serializedTags = await secureStorage.read(key: cachedTagsKey);

        if (cachedToken == null || serializedTags == null) {
          return;
        }

        var deviceId = await deviceInstallationService.getDeviceId();

        if (deviceId.isEmpty) {
          throw "Unable to resolve an ID for the device.";
        }

        var response = await http
            .delete("$installationsUrl/$deviceId", headers: {"apikey": apikey});

        if (response.statusCode != 200) {
          throw "Deregister request failed: ${response.reasonPhrase}";
        }

        await secureStorage.delete(key: cachedDeviceTokenKey);
        await secureStorage.delete(key: cachedTagsKey);
      }

      Future<void> registerDevice(List<String> tags) async {
        try {
          final deviceId = await deviceInstallationService.getDeviceId();
          final platform = await deviceInstallationService.getDevicePlatform();
          final token = await deviceInstallationService.getDeviceToken();

          final deviceInstallation =
              DeviceInstallation(deviceId, platform, token, tags);

          final response = await http.put(installationsUrl,
              body: jsonEncode(deviceInstallation),
              headers: {"apikey": apikey, "Content-Type": "application/json"});

          if (response.statusCode != 200) {
            throw "Register request failed: ${response.reasonPhrase}";
          }

          final serializedTags = jsonEncode(tags);

          await secureStorage.write(key: cachedDeviceTokenKey, value: token);
          await secureStorage.write(key: cachedTagsKey, value: serializedTags);
        } on PlatformException catch (e) {
          throw e.message;
        } catch (e) {
          throw "Unable to register device: $e";
        }
      }

      Future<void> refreshRegistration() async {
        final currentToken = await deviceInstallationService.getDeviceToken();
        final cachedToken = await secureStorage.read(key: cachedDeviceTokenKey);
        final serializedTags = await secureStorage.read(key: cachedTagsKey);

        if (currentToken == null ||
            cachedToken == null ||
            serializedTags == null ||
            currentToken == cachedToken) {
          return;
        }

        final tags = jsonDecode(serializedTags);

        return registerDevice(tags);
      }

      Future<void> handleNotificationRegistrationCall(MethodCall call) async {
        switch (call.method) {
          case refreshRegistrationChannelMethod:
            return refreshRegistration();
          default:
            throw MissingPluginException();
            break;
        }
      }
    }
    ```

    > [!NOTE]
    > Esta clase encapsula el uso de **DeviceInstallationService** y las solicitudes al servicio back-end para realizar las acciones de registro, anulación del registro y actualización de registros necesarias. El argumento **apiKey** solo es necesario si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional).

1. Agregue un nuevo archivo a la carpeta **lib** denominado *config.dart* con la siguiente implementación.

    ```dart
    class Config {
      static String apiKey = "API_KEY";
      static String backendServiceEndpoint = "BACKEND_SERVICE_ENDPOINT";
    }
    ```

    > [!NOTE]
    > Esta es una forma sencilla de definir secretos de la aplicación. Reemplace los valores del marcador de posición por los suyos propios. Debe anotarlos al crear el servicio back-end. La **dirección URL de la aplicación de API** debe ser `https://<api_app_name>.azurewebsites.net/`. El miembro **apiKey** solo es necesario si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional).
    >
    > Asegúrese de agregarlo al archivo gitignore para evitar la confirmación de estos secretos en el control de código fuente.

### <a name="implement-the-cross-platform-ui"></a>Implementación de una interfaz de usuario multiplataforma

1. En **main_page.dart**, reemplace la función **build** por lo siguiente.

    ```dart
    @override
    Widget build(BuildContext context) {
    return Scaffold(
        body: Padding(
          padding: const EdgeInsets.symmetric(horizontal: 20.0, vertical: 40.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.end,
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: <Widget>[
              FlatButton(
                child: Text("Register"),
                onPressed: registerButtonClicked,
              ),
              FlatButton(
                child: Text("Deregister"),
                onPressed: deregisterButtonClicked,
              ),
            ],
          ),
        ),
      );
    }
    ```

1. Agregue las importaciones necesarias a la parte superior del archivo **main_page.dart**.

    ```dart
    import 'package:push_demo/services/notification_registration_service.dart';
    import 'config.dart';
    ```

1. Agregue un campo a la clase **_MainPageState** para almacenar una referencia a **NotificationRegistrationService**.

    ```dart
    final notificationRegistrationService = NotificationRegistrationService(Config.backendServiceEndpoint, Config.apiKey);
    ```

1. En la clase **_MainPageState**, implemente controladores para los eventos **onPressed** de los botones **Register** (Registrar) y **Deregistrer** (Anular registro). Llame a los métodos **Register**/**Deregister** correspondientes y muestre una alerta con el resultado.

    ```dart
    void registerButtonClicked() async {
        try {
          await notificationRegistrationService.registerDevice(List<String>());
          await showAlert(message: "Device registered");
        }
        catch (e) {
          await showAlert(message: e);
        }
      }

      void deregisterButtonClicked() async {
        try {
          await notificationRegistrationService.deregisterDevice();
          await showAlert(message: "Device deregistered");
        }
        catch (e) {
          await showAlert(message: e);
        }
      }

      Future<void> showAlert({ message: String }) async {
        return showDialog<void>(
          context: context,
          barrierDismissible: false,
          builder: (BuildContext context) {
            return AlertDialog(
              title: Text('PushDemo'),
              content: SingleChildScrollView(
                child: ListBody(
                  children: <Widget>[
                    Text(message),
                  ],
                ),
              ),
              actions: <Widget>[
                FlatButton(
                  child: Text('OK'),
                  onPressed: () {
                    Navigator.of(context).pop();
                  },
                ),
              ],
            );
          },
        );
      }
    ```

1. Ahora, en **main.dart**, asegúrese de que las siguientes importaciones estén presentes en la parte superior del archivo.

    ```dart
    import 'package:flutter/material.dart';
    import 'package:push_demo/models/push_demo_action.dart';
    import 'package:push_demo/services/notification_action_service.dart';
    import 'package:push_demo/main_page.dart';
    ```

1. Declare una variable para almacenar la referencia a una instancia de **NotificationActionService** e inicialícela.

    ```dart
    final notificationActionService = NotificationActionService();
    ```

1. Agregue funciones para controlar la presentación de una alerta cuando se desencadene una acción.

    ```dart
    void notificationActionTriggered(PushDemoAction action) {
      showActionAlert(message: "${action.toString().split(".")[1]} action received");
    }

    Future<void> showActionAlert({ message: String }) async {
      return showDialog<void>(
        context: navigatorKey.currentState.overlay.context,
        barrierDismissible: false,
        builder: (BuildContext context) {
          return AlertDialog(
            title: Text('PushDemo'),
            content: SingleChildScrollView(
              child: ListBody(
                children: <Widget>[
                  Text(message),
                ],
              ),
            ),
            actions: <Widget>[
              FlatButton(
                child: Text('OK'),
                onPressed: () {
                  Navigator.of(context).pop();
                },
              ),
            ],
          );
        },
      );
    }
    ```

1. Actualice la función **main** para observar la secuencia de **NotificationActionService** **actionTriggered** y busque las acciones capturadas durante el inicio de la aplicación.

    ```dart
    void main() async {
      runApp(MaterialApp(home: MainPage(), navigatorKey: navigatorKey,));
      notificationActionService.actionTriggered.listen((event) { notificationActionTriggered(event as PushDemoAction); });
      await notificationActionService.checkLaunchAction();
    }
    ```

    > [!NOTE]
    > Esto es simplemente para mostrar la recepción y propagación de las acciones con notificaciones push. Normalmente, estas se tratarían de forma silenciosa, por ejemplo, yendo a una vista específica o actualizando algunos datos en lugar de mostrar una alerta en este caso.
