---
author: alexeystrakh
ms.author: alstrakh
ms.date: 06/11/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: c8271cc4de558ec5c97f41d6a070f71a9fc49cd2
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060462"
---
### <a name="create-the-react-native-solution"></a>Creación de la solución React Native

1. En `Terminal`, actualice las herramientas de entorno necesarias para trabajar con React Native mediante los siguientes comandos:

    ```bash
    # install node
    brew install node
    # or update
    brew update node
    # install watchman
    brew install watchman
    # or update
    brew upgrade watchman
    # install cocoapods
    sudo gem install cocoapods
    ```

1. En `Terminal`, ejecute el siguiente comando, para desinstalar la CLI de `React Native` si la tiene instalada. Use `npx` para acceder automáticamente a la versión más reciente de la CLI de React Native disponible:

    ```bash
    npm uninstall -g react-native-cli
    ```

    > [!NOTE]
    > React Native tiene una interfaz de línea de comandos integrada. En lugar de instalar y administrar una versión específica de la CLI globalmente, se recomienda acceder a la versión actual en tiempo de ejecución mediante `npx`, que se incluye con Node.js. Con `npx react-native <command>`, se descargará y ejecutará la versión estable actual de la CLI en el momento en que se ejecute el comando.

1. Vaya a la carpeta de proyectos donde desea crear la nueva aplicación. Use la plantilla basada en Typescript especificando el parámetro `--template`:

    ```bash
    # init new project with npx
    npx react-native init PushDemo --template react-native-template-typescript
    ```

1. Ejecute el servidor Metro, que compila lotes de JavaScript y supervisa las actualizaciones de código para aplicarlas a los lotes en tiempo real:

    ```bash
    cd PushDemo
    npx react-native start
    ```

1. Ejecute la aplicación iOS para comprobar la configuración. Antes de ejecutar el siguiente comando, asegúrese de que ha iniciado un simulador de iOS o de que ha conectado un dispositivo iOS:

    ```bash
    npx react-native run-ios
    ```

1. Ejecute la aplicación Android para comprobar la configuración. Son necesarios algunos pasos adicionales para configurar un dispositivo o un emulador de Android de modo que pueda acceder al servidor Metro de React Native. Los siguientes comandos generan el lote de JavaScript inicial para Android y lo colocan en la carpeta de recursos.

    ```bash
    # create assets folder for the bundle
    mkdir android/app/scr/main/assets
    # build the bundle
    npx react-native bundle --platform android --dev true --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res
    # enable ability for sim to access the localhost
    adb reverse tcp:8081 tcp:8081
    ```

    Este script se implementará previamente con la versión inicial de la aplicación. Una vez implementado, configure el emulador o el dispositivo para acceder al servidor Metro especificando la dirección IP del servidor. Ejecute el siguiente comando para compilar y ejecutar la aplicación Android:

    ```bash
    npx react-native run-android
    ```

    Una vez en la aplicación, presione `CMD+M` (emulador) o agite el dispositivo para rellenar la configuración del desarrollador, vaya a `Settings` > `Change Bundle Location` y especifique la dirección IP del servidor Metro con el puerto predeterminado: `<metro-server-ip-address>:8081`.

1. En el archivo `App.tsx`, aplique cualquier cambio en el diseño de página, guárdelo y haga que el cambio se refleje automáticamente en las aplicaciones iOS y Android.

    > [!NOTE]
    > En la [documentación oficial](https://reactnative.dev/docs/environment-setup) está disponible una detallada guía de instalación del entorno de desarrollo.

### <a name="install-required-packages"></a>Instalación de los paquetes requeridos

Para que este ejemplo funcione, necesita los tres paquetes siguientes:

1. [Notificaciones push de React Native para iOS](https://www.npmjs.com/package/@react-native-community/push-notification-ios) - [Proyecto en GitHub](https://github.com/react-native-community/push-notification-ios)

    Este paquete se creó cuando PushNotificationIOS se separó del servicio básico de React Native. El paquete implementa de forma nativa notificaciones push para iOS y proporciona una interfaz de React Native para el acceso. Ejecute el siguiente comando para instalar el paquete:

    ```bash
    yarn add @react-native-community/push-notification-ios
    ```

1. [Notificaciones push de React Native multiplataforma](https://www.npmjs.com/package/react-native-push-notification)

    Este paquete implementa notificaciones locales y remotas en iOS y Android en modo multiplataforma. Ejecute el siguiente comando para instalar el paquete:

    ```bash
    yarn add react-native-push-notification
    ```

1. [Paquete de información sobre dispositivos](https://www.npmjs.com/package/react-native-device-info) El paquete proporciona información sobre un dispositivo en tiempo de ejecución. Con él podrá definir un identificador de dispositivo, que se usará para registrarse para la notificación push. Ejecute el siguiente comando para instalar el paquete:

    ```bash
    yarn add react-native-device-info
    ```

### <a name="implement-the-cross-platform-components"></a>Implementación de componentes multiplataforma

1. Cree e implemente `DemoNotificationHandler`:

    ```typescript
    import PushNotification from 'react-native-push-notification';

    class DemoNotificationHandler {
      private _onRegister: any;
      private _onNotification: any;

      onNotification(notification: any) {
        console.log('NotificationHandler:', notification);

        if (typeof this._onNotification === 'function') {
          this._onNotification(notification);
        }
      }

      onRegister(token: any) {
        console.log('NotificationHandler:', token);

        if (typeof this._onRegister === 'function') {
          this._onRegister(token);
        }
      }

      attachTokenReceived(handler: any) {
        this._onRegister = handler;
      }

      attachNotificationReceived(handler: any) {
        this._onNotification = handler;
      }
    }

    const handler = new DemoNotificationHandler();

    PushNotification.configure({
      onRegister: handler.onRegister.bind(handler),
      onNotification: handler.onNotification.bind(handler),
      permissions: {
        alert: true,
        badge: true,
        sound: true,
      },
      popInitialNotification: true,
      requestPermissions: true,
    });

    export default handler;
    ```

1. Cree e implemente `DemoNotificationService`:

    ```typescript
    import PushNotification from 'react-native-push-notification';
    import DemoNotificationHandler from './DemoNotificationHandler';

    export default class DemoNotificationService {
      constructor(onTokenReceived: any, onNotificationReceived: any) {
        DemoNotificationHandler.attachTokenReceived(onTokenReceived);
        DemoNotificationHandler.attachNotificationReceived(onNotificationReceived);
        PushNotification.getApplicationIconBadgeNumber(function(number: number) {
          if(number > 0) {
            PushNotification.setApplicationIconBadgeNumber(0);
          }
        });
      }

      checkPermissions(cbk: any) {
        return PushNotification.checkPermissions(cbk);
      }

      requestPermissions() {
        return PushNotification.requestPermissions();
      }

      cancelNotifications() {
        PushNotification.cancelLocalNotifications();
      }

      cancelAll() {
        PushNotification.cancelAllLocalNotifications();
      }

      abandonPermissions() {
        PushNotification.abandonPermissions();
      }
    }
    ```

1. Cree e implemente `DemoNotificationRegistrationService`:

    ```typescript
    export default class DemoNotificationService {
        constructor(
            readonly apiUrl: string,
            readonly apiKey: string) {
        }

    async registerAsync(request: any): Promise<Response> {
            const method = 'PUT';
            const registerApiUrl = `${this.apiUrl}/notifications/installations`;
            const result = await fetch(registerApiUrl, {
                method: method,
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                    'apiKey': this.apiKey
                },
                body: JSON.stringify(request)
            });

            this.validateResponse(registerApiUrl, method, request, result);
            return result;
        }

        async deregisterAsync(deviceId: string): Promise<Response> {
            const method = 'DELETE';
            const deregisterApiUrl = `${this.apiUrl}/notifications/installations/${deviceId}`;
            const result = await fetch(deregisterApiUrl, {
                method: method,
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                    'apiKey': this.apiKey
                }
            });

            this.validateResponse(deregisterApiUrl, method, null, result);
            return result;
        }

        private validateResponse(requestUrl: string, method: string, requestPayload: any, response: Response) {
            console.log(`Request: ${method} ${requestUrl} => ${JSON.stringify(requestPayload)}\nResponse: ${response.status}`);
            if (!response || response.status != 200) {
                throw `HTTP error ${response.status}: ${response.statusText}`;
            }
        }
    }
    ```

1. Configure la aplicación. Abra `package.json` y agregue la siguiente definición de script:

    ```json
    "configure": "cp .app.config.tsx src/config/AppConfig.tsx"
    ```

    Después, ejecute este script, que copiará la configuración predeterminada en la carpeta `config`.

    ```bash
    yarn configure
    ```

    Como paso final, actualice el archivo de configuración copiado en el paso anterior con la información de acceso de la API. Especifique los parámetros `apiKey` y `apiUrl`:

    ```typescript
    module.exports = {
        appName: "PushDemo",
        env: "production",
        apiUrl: "https://<azure-push-notifications-api-url>/api/",
        apiKey: "<api-auth-key>",
    };
    ```

### <a name="implement-the-cross-platform-ui"></a>Implementación de una interfaz de usuario multiplataforma

1. Defina el diseño de página.

    ```typescript
    <View style={styles.container}>
      {this.state.isBusy &&
        <ActivityIndicator></ActivityIndicator>
      }
      <View style={styles.button}>
        <Button title="Register" onPress={this.onRegisterButtonPress.bind(this)} disabled={this.state.isBusy} />
      </View>
      <View style={styles.button}>
        <Button title="Deregister" onPress={this.onDeregisterButtonPress.bind(this)} disabled={this.state.isBusy} />
      </View>
    </View>
    ```

1. Aplique estilos.

    ```css
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        alignItems: "center",
        justifyContent: 'flex-end',
        margin: 50,
      },
      button: {
        margin: 5,
        width: "100%",
      }
    });
    ```

1. Inicialice el componente de página.

    ```typescript
      state: IState;
      notificationService: DemoNotificationService;
      notificationRegistrationService: DemoNotificationRegistrationService;
      deviceId: string;

      constructor(props: any) {
        super(props);
        this.deviceId = DeviceInfo.getUniqueId();
        this.state = {
          status: "Push notifications registration status is unknown",
          registeredOS: "",
          registeredToken: "",
          isRegistered: false,
          isBusy: false,
        };

        this.notificationService = new DemoNotificationService(
          this.onTokenReceived.bind(this),
          this.onNotificationReceived.bind(this),
        );

        this.notificationRegistrationService = new DemoNotificationRegistrationService(
          Config.apiUrl,
          Config.apiKey,
        );
      }
    ```

1. Defina controladores de clic de botón.

    ```typescript
      async onRegisterButtonPress() {
        if (!this.state.registeredToken || !this.state.registeredOS) {
          Alert.alert("The push notifications token wasn't received.");
          return;
        }

        let status: string = "Registering...";
        let isRegistered = this.state.isRegistered;
        try {
          this.setState({ isBusy: true, status });
          const pnPlatform = this.state.registeredOS == "ios" ? "apns" : "fcm";
          const pnToken = this.state.registeredToken;
          const request = {
            installationId: this.deviceId,
            platform: pnPlatform,
            pushChannel: pnToken,
            tags: []
          };
          const response = await this.notificationRegistrationService.registerAsync(request);
          status = `Registered for ${this.state.registeredOS} push notifications`;
          isRegistered = true;
        } catch (e) {
          status = `Registration failed: ${e}`;
        }
        finally {
          this.setState({ isBusy: false, status, isRegistered });
        }
      }

      async onDeregisterButtonPress() {
        if (!this.notificationService)
          return;

        let status: string = "Deregistering...";
        let isRegistered = this.state.isRegistered;
        try {
          this.setState({ isBusy: true, status });
          await this.notificationRegistrationService.deregisterAsync(this.deviceId);
          status = "Deregistered from push notifications";
          isRegistered = false;
        } catch (e) {
          status = `Deregistration failed: ${e}`;
        }
        finally {
          this.setState({ isBusy: false, status, isRegistered });
        }
      }
    ```

1. Administre los registros de tokens y notificaciones push recibidos.

    ```typescript
      onTokenReceived(token: any) {
        console.log(`Received a notification token on ${token.os}`);
        this.setState({ registeredToken: token.token, registeredOS: token.os, status: `The push notifications token has been received.` });

        if (this.state.isRegistered && this.state.registeredToken && this.state.registeredOS) {
          this.onRegisterButtonPress();
        }
      }

      onNotificationReceived(notification: any) {
        console.log(`Received a push notification on ${this.state.registeredOS}`);
        this.setState({ status: `Received a push notification...` });

        if (notification.data.message) {
          Alert.alert(AppConfig.appName, `${notification.data.action} action received`);
        }
      }
    };
    ```