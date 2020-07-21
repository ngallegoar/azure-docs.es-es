---
author: mikeparker104
ms.author: miparker
ms.date: 07/07/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 11bd05231aa3b5b336400d2434e63e57807a7e69
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156335"
---
### <a name="add-the-google-services-json-file"></a>Adición del archivo JSON de Google Services

1. Con la tecla **Control** pulsada, **haga clic** en la carpeta **android** y, a continuación, elija **Open in Android Studio** (Abrir en Android Studio). A continuación, cambie a la vista **Project** (Proyecto), si aún no está abierta.

1. Busque el archivo *google-services.json* que ha descargado anteriormente al configurar el proyecto **PushDemo** en la [consola Firebase](https://console.firebase.google.com). Después, arrástrelo al directorio raíz del módulo de la **aplicación** (**android** > **android** > **aplicación**).

### <a name="configure-build-settings-and-permissions"></a>Configuración de los valores y permisos de la compilación

1. Cambie la vista **Project** (Proyecto) a **Android**.

1. Abra **AndroidManifest.xml** y agregue los permisos **INTERNET** y **READ_PHONE_STATE** después del elemento **application** antes de cerrar la etiqueta **</manifest>** .

    ```xml
    <manifest>
        <application>...</application>
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    </manifest>
    ```

### <a name="add-the-firebase-sdks"></a>Adición de los SDK de Firebase

1. En **Android Studio**, abra el archivo de nivel de proyecto **build.gradle**, que se encuentra en **Gradle Scripts** > **build.gradle (Project: android)** [Scripts de Gradle > build.gradle (Proyecto: android)]. Asegúrese de que tiene la ruta de clases `com.google.gms:google-services' en el nodo ``buildscript`` > **dependencies** (dependencias).

    ```json
    buildscript {

      repositories {
        // Check that you have the following line (if not, add it):
        google()  // Google's Maven repository
      }

      dependencies {
        // ...

        // Add the following line:
        classpath 'com.google.gms:google-services:4.3.3'  // Google Services plugin
      }
    }

    allprojects {
      // ...

      repositories {
        // Check that you have the following line (if not, add it):
        google()  // Google's Maven repository
        // ...
      }
    }
    ```

    > [!NOTE]
    > Asegúrese de hacer referencia a la versión más reciente según las instrucciones proporcionadas en la [consola de Firebase](https://console.firebase.google.com) al crear el **proyecto de Android**.

1. En el archivo **build.gradle** de nivel de aplicación (**Gradle Scripts** > **build.gradle (Module: app)** ) [Scripts de Gradle > build.gradle (Módulo: aplicación)], aplique el **complemente de Gradle de Google Services**. Aplique el complemento directamente encima del nodo **android**.

    ```json
    // ...

    // Add the following line:
    apply plugin: 'com.google.gms.google-services'  // Google Services plugin

    android {
      // ...
    }
    ```

1. En el mismo archivo, en el nodo **dependencies** (dependencias), agregue la dependencia para la biblioteca **Cloud Messaging** de Android.

    ```json
    dependencies {
        // ...
        implementation 'com.google.firebase:firebase-messaging:20.2.0'
    }
    ```

    > [!NOTE]
    > Asegúrese de hacer referencia a la versión más reciente según la [documentación del cliente Cloud Messaging de Android](https://firebase.google.com/docs/cloud-messaging/android/client).

1. Guarde los cambios y, a continuación, haga clic en el botón **Sync Now** (Sincronizar ahora), desde la ventana de la barra de herramientas, o en **Sync Project with Gradle Files** (Sincronizar proyecto con archivos de Gradle).

### <a name="handle-push-notifications-for-android"></a>Administración de las notificaciones push para Android

1. En **Android Studio**, con la tecla **Control** pulsada, **haga clic** en la carpeta del paquete **com.<su_organización>.pushdemo** (**app** > **src** > **main** > **kotlin**) y seleccione **Package** (Paquete) en el menú **New** (Nuevo). Escriba **services** como nombre y, a continuación, presione la tecla **Retorno**.

1. Con la tecla **Control** pulsada, **haga clic** en la carpeta **services** y seleccione la clase **Kotlin File/Class** en el menú **New** (Nuevo). Escriba **DeviceInstallationService** como nombre y, a continuación, presione la tecla **Retorno**.

1. Implemente **DeviceInstallationService** mediante el código siguiente.

    ```kotlin
    package com.<your_organization>.pushdemo.services

    import android.annotation.SuppressLint
    import android.content.Context
    import android.provider.Settings.Secure
    import com.google.android.gms.common.ConnectionResult
    import com.google.android.gms.common.GoogleApiAvailability
    import io.flutter.embedding.engine.FlutterEngine
    import io.flutter.plugin.common.MethodCall
    import io.flutter.plugin.common.MethodChannel

    @SuppressLint("HardwareIds")
    class DeviceInstallationService {

        companion object {
            const val DEVICE_INSTALLATION_CHANNEL = "com.<your_organization>.pushdemo/deviceinstallation"
            const val GET_DEVICE_ID = "getDeviceId"
            const val GET_DEVICE_TOKEN = "getDeviceToken"
            const val GET_DEVICE_PLATFORM = "getDevicePlatform"
        }

        private var context: Context
        private var deviceInstallationChannel : MethodChannel

        val playServicesAvailable
            get() = GoogleApiAvailability.getInstance().isGooglePlayServicesAvailable(context) == ConnectionResult.SUCCESS

        constructor(context: Context, flutterEngine: FlutterEngine) {
            this.context = context
            deviceInstallationChannel = MethodChannel(flutterEngine.dartExecutor.binaryMessenger, DEVICE_INSTALLATION_CHANNEL)
            deviceInstallationChannel.setMethodCallHandler { call, result -> handleDeviceInstallationCall(call, result) }
        }

        fun getDeviceId() : String
            = Secure.getString(context.applicationContext.contentResolver, Secure.ANDROID_ID)

        fun getDeviceToken() : String {
            if(!playServicesAvailable) {
                throw Exception(getPlayServicesError())
            }

            // TODO: Revisit once we have created the PushNotificationsFirebaseMessagingService
            val token = "Placeholder_Get_Value_From_FirebaseMessagingService_Implementation"

            if (token.isNullOrBlank()) {
                throw Exception("Unable to resolve token for FCM.")
            }

            return token
        }

        fun getDevicePlatform() : String = "fcm"

        private fun handleDeviceInstallationCall(call: MethodCall, result: MethodChannel.Result) {
            when (call.method) {
                GET_DEVICE_ID -> {
                    result.success(getDeviceId())
                }
                GET_DEVICE_TOKEN -> {
                    getDeviceToken(result)
                }
                GET_DEVICE_PLATFORM -> {
                    result.success(getDevicePlatform())
                }
                else -> {
                    result.notImplemented()
                }
            }
        }

        private fun getDeviceToken(result: MethodChannel.Result) {
            try {
                val token = getDeviceToken()
                result.success(token)
            }
            catch (e: Exception) {
                result.error("ERROR", e.message, e)
            }
        }

        private fun getPlayServicesError(): String {
            val resultCode = GoogleApiAvailability.getInstance().isGooglePlayServicesAvailable(context)

            if (resultCode != ConnectionResult.SUCCESS) {
                return if (GoogleApiAvailability.getInstance().isUserResolvableError(resultCode)){
                    GoogleApiAvailability.getInstance().getErrorString(resultCode)
                } else {
                    "This device is not supported"
                }
            }

            return "An error occurred preventing the use of push notifications"
        }
    }
    ```

    > [!NOTE]
    > Esta clase implementa el homólogo específico de la plataforma para el canal `com.<your_organization>.pushdemo/deviceinstallation`. Esto está definido en la parte de Flutter de la aplicación en **DeviceInstallationService.dart**. En este caso, las llamadas se realizan desde el código común al host nativo. Asegúrese de reemplazar **<su_organización>** por el nombre de su propia organización siempre que se use este código.
    >
    > Esta clase proporciona un identificador único (mediante [Secure.AndroidId](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)) como parte de la carga de registro del centro de notificaciones.

1. Agregue otra clase **Kotlin File/Class** a la carpeta **services** denominada *NotificationRegistrationService* y, a continuación, agregue el siguiente código.

    ```kotlin
    package com.<your_organization>.pushdemo.services

    import io.flutter.embedding.engine.FlutterEngine
    import io.flutter.plugin.common.MethodChannel

    class NotificationRegistrationService {

        companion object {
            const val NOTIFICATION_REGISTRATION_CHANNEL = "com.<your_organization>.pushdemo/notificationregistration"
            const val REFRESH_REGISTRATION = "refreshRegistration"
        }

        private var notificationRegistrationChannel : MethodChannel

        constructor(flutterEngine: FlutterEngine) {
            notificationRegistrationChannel = MethodChannel(flutterEngine.dartExecutor.binaryMessenger, NotificationRegistrationService.NOTIFICATION_REGISTRATION_CHANNEL)
        }

        fun refreshRegistration() {
            notificationRegistrationChannel.invokeMethod(REFRESH_REGISTRATION, null)
        }
    }
    ```

    > [!NOTE]
    > Esta clase implementa el homólogo específico de la plataforma para el canal `com.<your_organization>.pushdemo/notificationregistration`. Esto está definido en la parte de Flutter de la aplicación en **NotificationRegistrationService.dart**. En este caso, las llamadas se realizan desde el host nativo al código común. Una vez más, recuerde reemplazar **<su_organización>** por el nombre de su propia organización siempre que se use este código.

1. Agregue otra clase **Kotlin File/Class** a la carpeta **services** denominada *NotificationActionService* y, a continuación, agregue el siguiente código.

    ```kotlin
    package com.<your_organization>.pushdemo.services

    import io.flutter.embedding.engine.FlutterEngine
    import io.flutter.plugin.common.MethodCall
    import io.flutter.plugin.common.MethodChannel

    class NotificationActionService {
        companion object {
            const val NOTIFICATION_ACTION_CHANNEL = "com.<your_organization>.pushdemo/notificationaction"
            const val TRIGGER_ACTION = "triggerAction"
            const val GET_LAUNCH_ACTION = "getLaunchAction"
        }

        private var notificationActionChannel : MethodChannel
        var launchAction : String? = null

        constructor(flutterEngine: FlutterEngine) {
            notificationActionChannel = MethodChannel(flutterEngine.dartExecutor.binaryMessenger, NotificationActionService.NOTIFICATION_ACTION_CHANNEL)
            notificationActionChannel.setMethodCallHandler { call, result -> handleNotificationActionCall(call, result) }
        }

        fun triggerAction(action: String) {
            notificationActionChannel.invokeMethod(NotificationActionService.TRIGGER_ACTION, action)
        }

        private fun handleNotificationActionCall(call: MethodCall, result: MethodChannel.Result) {
            when (call.method) {
                NotificationActionService.GET_LAUNCH_ACTION -> {
                    result.success(launchAction)
                }
                else -> {
                    result.notImplemented()
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Esta clase implementa el homólogo específico de la plataforma para el canal `com.<your_organization>.pushdemo/notificationaction`. Esto está definido en la parte de Flutter de la aplicación en **NotificationActionService.dart**. En este caso, se pueden realizar llamadas en ambas direcciones. Asegúrese de reemplazar **<su_organización>** por el nombre de su propia organización siempre que se use este código.

1. Agregue otra clase **Kotlin File/Class** al paquete **com.<su_organización>.pushdemo** denominado *PushNotificationsFirebaseMessagingService* y, a continuación, implemente con el código siguiente.

    ```kotlin
    package com.<your_organization>.pushdemo

    import android.os.Handler
    import android.os.Looper
    import com.google.firebase.messaging.FirebaseMessagingService
    import com.google.firebase.messaging.RemoteMessage
    import com.<your_organization>.pushdemo.services.NotificationActionService
    import com.<your_organization>.pushdemo.services.NotificationRegistrationService

    class PushNotificationsFirebaseMessagingService : FirebaseMessagingService() {

        companion object {
            var token : String? = null
            var notificationRegistrationService : NotificationRegistrationService? = null
            var notificationActionService : NotificationActionService? = null
        }

        override fun onNewToken(token: String) {
            PushNotificationsFirebaseMessagingService.token = token
            notificationRegistrationService?.refreshRegistration()
        }

        override fun onMessageReceived(message: RemoteMessage) {
            message.data.let {
                Handler(Looper.getMainLooper()).post {
                    notificationActionService?.triggerAction(it.getOrDefault("action", null))
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Esta clase es responsable de controlar las notificaciones cuando la aplicación se ejecuta en primer plano. Llamará condicionalmente a **triggerAction** en **NotificationActionService** si se incluye una acción en la carga de notificación que se recibe en **onMessageReceived**. Esto también llamará a **refreshRegistration** en **NotificationRegistrationService** cuando el token de **Firebase** se vuelva a generar invalidando la función **onNewToken**.
    >
    > Una vez más, recuerde reemplazar **<su_organización>** por el nombre de su propia organización siempre que se use este código.

1. En **AndroidManifest.xml** (**app** > **src** > **main**), agregue **PushNotificationsFirebaseMessagingService** al final del elemento **application** con el filtro de intención `com.google.firebase.MESSAGING_EVENT`.

    ```xml
    <manifest>
        <application>
            <!-- EXISTING MANIFEST CONTENT -->
             <service
                android:name="com.<your_organization>.pushdemo.PushNotificationsFirebaseMessagingService"
                android:exported="false">
                <intent-filter>
                    <action android:name="com.google.firebase.MESSAGING_EVENT" />
                </intent-filter>
            </service>
        </application>
    </manifest>
    ```

1. En **DeviceInstallationService**, asegúrese de que las siguientes importaciones estén presentes en la parte superior del archivo.

    ```kotlin
    package com.<your_organization>.pushdemo
    import com.<your_organization>.pushdemo.services.PushNotificationsFirebaseMessagingService
    ```

    > [!NOTE]
    > Reemplace **<su_organización>** por su propio valor de organización.

1. Actualice el texto del marcador de posición *Placeholder_Get_Value_From_FirebaseMessagingService_Implementation* para obtener el valor del token de **PushNotificationFirebaseMessagingService**.

    ```kotlin
    fun getDeviceToken() : String {
        if(!playServicesAvailable) {
            throw Exception(getPlayServicesError())
        }

        // Get token from the PushNotificationsFirebaseMessagingService.token field.
        val token = PushNotificationsFirebaseMessagingService.token

        if (token.isNullOrBlank()) {
            throw Exception("Unable to resolve token for FCM.")
        }

        return token
    }
    ```

1. En **MainActivity**, asegúrese de que las siguientes importaciones estén presentes en la parte superior del archivo.

    ```kotlin
    package com.<your_organization>.pushdemo

    import android.content.Intent
    import android.os.Bundle
    import com.google.android.gms.tasks.OnCompleteListener
    import com.google.firebase.iid.FirebaseInstanceId
    import com.<your_organization>.pushdemo.services.DeviceInstallationService
    import com.<your_organization>.pushdemo.services.NotificationActionService
    import com.<your_organization>.pushdemo.services.NotificationRegistrationService
    import io.flutter.embedding.android.FlutterActivity
    ```

    > [!NOTE]
    > Reemplace **<su_organización>** por su propio valor de organización.

1. Agregue una variable para almacenar una referencia a **DeviceInstallationService**.

    ```kotlin
    private lateinit var deviceInstallationService: DeviceInstallationService
    ```

1. Agregue una función llamada **processNotificationActions** que comprobará si una **intención** tiene un valor adicional denominado **acción**. Desencadene condicionalmente esa acción o almacénela para su uso posterior si la acción se procesa durante el inicio de la aplicación.

    ```kotlin
     private fun processNotificationActions(intent: Intent, launchAction: Boolean = false) {
        if (intent.hasExtra("action")) {
            var action = intent.getStringExtra("action");

            if (action.isNotEmpty()) {
                if (launchAction) {
                    PushNotificationsFirebaseMessagingService.notificationActionService?.launchAction = action
                }
                else {
                    PushNotificationsFirebaseMessagingService.notificationActionService?.triggerAction(action)
                }
            }
        }
    }
    ```

1. Reemplace la función **onNewIntent** para que llame a **processNotificationActions**.

    ```kotlin
    override fun onNewIntent(intent: Intent) {
        super.onNewIntent(intent)
        processNotificationActions(intent)
    }
    ```

    > [!NOTE]
    > Dado que **LaunchMode** para **MainActivity** está establecido en **SingleTop**, se enviará una **intención** a la instancia de la **actividad** existente mediante la función **onNewIntent** en lugar de mediante la función **onCreate** y, por tanto, debe controlar una **intención** entrante en ambas funciones, **onCreate** y **onNewIntent**.

1. Invalide la función **onCreate** y establezca **deviceInstallationService** en una nueva instancia de **DeviceInstallationService**.

    ```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        flutterEngine?.let {
            deviceInstallationService = DeviceInstallationService(context, it)
        }
    }
    ```

1. Establezca las propiedades **notificationActionService** y **notificationRegistrationService** en **PushNotificationFirebaseMessagingServices**.

    ```kotlin
    flutterEngine?.let {
      deviceInstallationService = DeviceInstallationService(context, it)
      PushNotificationsFirebaseMessagingService.notificationActionService = NotificationActionService(it)
      PushNotificationsFirebaseMessagingService.notificationRegistrationService = NotificationRegistrationService(it)
    }
    ```

1. En la misma función, llame condicionalmente a **FirebaseInstanceId.getInstance().instanceId**. Implemente **OnCompleteListener** para establecer el valor de **token** resultante en **PushNotificationFirebaseMessagingService** antes de llamar a **refreshRegistration**.

    ```kotlin
    if(deviceInstallationService?.playServicesAvailable) {
        FirebaseInstanceId.getInstance().instanceId
            .addOnCompleteListener(OnCompleteListener { task ->
                if (!task.isSuccessful)
                    return@OnCompleteListener

                PushNotificationsFirebaseMessagingService.token = task.result?.token
                PushNotificationsFirebaseMessagingService.notificationRegistrationService?.refreshRegistration()
            })
    }
    ```

1. Aún en **onCreate**, llame a **processNotificationActions** al final de la función. Use *true* para el argumento *launchAction* para indicar que esta acción se procesa durante el inicio de la aplicación.

    ```kotlin
    processNotificationActions(this.intent, true)
    ```

> [!NOTE]
> Debe volver a registrar la aplicación cada vez que la ejecute y la detenga en una sesión de depuración para seguir recibiendo notificaciones push.
