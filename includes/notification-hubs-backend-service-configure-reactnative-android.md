---
author: alexeystrakh
ms.author: alstrakh
ms.date: 06/11/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 9330488debb0b7611fdd63656e4c555566c749dd
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060467"
---
### <a name="configure-required-android-packages"></a>Configuración de los paquetes Android necesarios

El paquete se [vincula automáticamente](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) al compilar la aplicación. Tendrá que seguir algunos pasos adicionales para completar el proceso de configuración.

### <a name="configure-android-manifest"></a>Configuración del manifiesto de Android

En el archivo "android/app/src/main/AndroidManifest.xml", compruebe el nombre del paquete, los permisos y los servicios necesarios. Asegúrese de que ha registrado los receptores `RNPushNotificationPublisher` y `RNPushNotificationBootEventReceiver`, y de que ha registrado el servicio `RNPushNotificationListenerService`. Los metadatos de las notificaciones se pueden usar para personalizar la apariencia de las notificaciones push.

  ```xml
  <manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="YOUR_PACKAGE_NAME">

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>

        <application
          android:name=".MainApplication"
          android:label="@string/app_name"
          android:usesCleartextTraffic="true"
          android:icon="@mipmap/ic_launcher"
          android:roundIcon="@mipmap/ic_launcher_round"
          android:allowBackup="false"
          android:theme="@style/AppTheme">

          <meta-data  android:name="com.dieam.reactnativepushnotification.notification_channel_name"
                      android:value="PushDemo Channel"/>
          <meta-data  android:name="com.dieam.reactnativepushnotification.notification_channel_description"
                      android:value="PushDemo Channel Description"/>
          <meta-data  android:name="com.dieam.reactnativepushnotification.notification_foreground"
                      android:value="true"/>
          <meta-data  android:name="com.dieam.reactnativepushnotification.notification_color"
                      android:resource="@android:color/white"/>

          <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationPublisher" />
          <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationBootEventReceiver">
              <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
              </intent-filter>
          </receiver>

          <service
              android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationListenerService"
              android:exported="false" >
              <intent-filter>
                  <action android:name="com.google.firebase.MESSAGING_EVENT" />
              </intent-filter>
          </service>

          <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:configChanges="keyboard|keyboardHidden|orientation|screenSize|uiMode"
            android:launchMode="singleTask"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
          </activity>
          <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
        </application>

  </manifest>
  ```

### <a name="configure-google-services"></a>Configuración de Google Services

En "android/app/build.gradle", registre Google Services:

```gradle
dependencies {
  ...
  implementation 'com.google.firebase:firebase-analytics:17.3.0'
  ...
}

apply plugin: 'com.google.gms.google-services'
```

Copie el archivo "google-services.json" que descargó durante la instalación de FCM en la carpeta del proyecto "android/app/".

### <a name="handle-push-notifications-for-android"></a>Administración de las notificaciones push para Android

Configuró el servicio `RNPushNotificationListenerService` existente para administrar las notificaciones push de Android entrantes. Este servicio se registró anteriormente en el manifiesto de aplicación. Procesa las notificaciones entrantes y las envía por proxy a la parte de React Native multiplataforma. No se necesita realizar ningún paso adicional.
