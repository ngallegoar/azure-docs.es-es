---
title: Envío de notificaciones push a aplicaciones iOS basadas en Swift mediante Azure Notification Hubs y las API REST
description: En este tutorial aprenderá a usar Azure Notification Hubs y las API REST para enviar notificaciones push a dispositivos iOS.
author: sethmanheim
ms.author: sethm
ms.date: 06/19/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/03/2020
ms.openlocfilehash: 861011874c63f4c5d18ce33b107bbe9a80359045
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2020
ms.locfileid: "85127475"
---
# <a name="tutorial-send-push-notifications-to-swift-ios-apps-using-notification-hubs-rest-apis"></a>Tutorial: Envío de notificaciones push a aplicaciones iOS basadas en Swift mediante las API REST de Notification Hubs

Este tutorial muestra cómo puede utilizar Azure Notification Hubs para enviar notificaciones push a una aplicación iOS mediante API REST.

En este tutorial se describen los pasos siguientes:

- Cree una aplicación iOS de ejemplo.
- Conecte la aplicación iOS a Azure Notification Hubs.
- Pruebe el envío de las notificaciones push.
- Compruebe que la aplicación recibe notificaciones.

El código completo de este tutorial se puede descargar [en GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/v3-preview2/Samples).

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Mac con [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532), junto con un certificado de desarrollador válido instalado en el Llavero.

- Un iPhone o iPad con iOS versión 10 o posterior.

- El dispositivo físico registrado en el [portal de Apple](https://developer.apple.com/) y asociado con el certificado.

Antes de continuar, asegúrese de seguir el tutorial anterior sobre cómo empezar a usar [Azure Notification Hubs para aplicaciones iOS](https://go.microsoft.com/fwlink/?linkid=2129801), para instalar y configurar las credenciales push en el centro de notificaciones. Aunque no tenga ninguna experiencia previa con el desarrollo de iOS, debería poder seguir estos pasos.

> [!NOTE]
> Debido a los requisitos de configuración de las notificaciones push, debe implementar y probar estas en un dispositivo iOS físico (iPhone o iPad), en lugar de hacerlo en el emulador de iOS.

En las secciones siguientes, compilará una aplicación iOS que se conecta al centro de notificaciones.

## <a name="create-an-ios-project"></a>Crear un proyecto para iOS

1. En Xcode, cree un nuevo proyecto iOS y seleccione la plantilla **Single View Application** (Aplicación de vista única).

2. Al configurar las opciones para el nuevo proyecto:
   - Especifique el valor de **Product Name** (Nombre de producto) (PushDemo) y **Organization Identifier** (Identificador de la organización) (`com.<organization>`) que usó cuando estableció **Bundle Identifier** (Identificador de agrupación) en el portal para desarrolladores de Apple.
   - Elija el mismo valor en **Team** (Equipo) que para el que configuró el valor de **App ID** (Identificador de la aplicación).
   - Establezca el valor de **language** (Lenguaje) en **Swift**.
   - Seleccione  **Siguiente**.

3. Cree una nueva carpeta llamada **SupportingFiles**.

4. Crear un nuevo archivo p-list denominado **devsettings.plist** en la carpeta  **SupportingFiles**. No olvide agregar esta carpeta a su archivo **gitignore** de modo que no se confirme al trabajar con un repositorio de Git. En una aplicación de producción, establecería condicionalmente estos secretos como parte de un proceso de compilación automatizado. Esta configuración no se trata en este tutorial.

5. Actualice **devsettings.plist** para incluir las siguientes entradas de configuración con sus propios valores desde el centro de notificaciones que ha aprovisionado:

   | **Clave**                  | **Tipo** | **Valor**        |
   | ------------------------ | -------- | ---------------- |
   | notificationHubKey       | String   | `<hubKey>`       |
   | notificationHubKeyName   | String   | `<hubKeyName>`   |
   | notificationHubName      | String   | `<hubName>`      |
   | notificationHubNamespace | String   | `<hubNamespace>` |

   Para encontrar los valores necesarios, desplácese hasta el recurso del centro de notificaciones en Azure Portal. En concreto, los valores de **notificationHubName** y **notificationHubNamespace** están en la esquina superior derecha del resumen **Información esencial** dentro de la página **Información general**.

   :::image type="content" source="media/ios-sdk-swift-rest/image1.png" alt-text="Valores requeridos":::

   También puede buscar los valores de **notificationHubKeyName** y **notificationHubKey** yendo a **Directivas de acceso** y seleccionando el valor de **Directiva de acceso** correspondiente, como **DefaultFullSharedAccessSignature**. Después, copie del campo **Cadena de conexión principal** el valor con el prefijo `SharedAccessKeyName=` para **notificationHubKeyName** y el valor con el prefijo `SharedAccessKey=` para **notificationHubKey**. La cadena de conexión debe tener el siguiente formato:

   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Para simplificar, especifique **DefaultFullSharedAccessSignature**, de modo que pueda usar el token para enviar notificaciones. En la práctica, **DefaultListenSharedAccessSignature** es una opción mejor para las situaciones en las que solo desea recibir notificaciones.

6. En **Project Navigator**(Explorador de proyectos), seleccione el valor de **Project Name** (Nombre de proyecto) y, a continuación, seleccione la pestaña **General**.

7. Busque **Identity** (Identidad) y, luego, establezca el valor de **Bundle Identifier** (Identificador de la agrupación) para que coincida con `com.<organization>.PushDemo`, que es el valor usado para el **identificador de aplicación** de un paso anterior.

8. Busque **Signing & Capabilities** (Firma y funcionalidades) y, luego, seleccione el valor de **Team** (Equipo) adecuado para la  **cuenta de desarrollador de Apple**. El valor de **Team** (Equipo) debe coincidir con el del equipo en el que creó los certificados y perfiles.

9. Xcode debe desplegar automáticamente el valor de **Provisioning Profile** (Perfil de aprovisionamiento) adecuado en función del valor de **Bundle Identifier** (Identificador de agrupación). Si no ve el nuevo valor de **Provisioning Profile** (Perfil de aprovisionamiento), intente actualizar los perfiles para el valor correspondiente de **Signing Identity** (Identidad de firma) seleccionando **Xcode**, después **Preferences** (Preferencias), **Account** (Cuenta) y, a continuación, el botón **Download Manual Profiles** (Descargar perfiles manuales) para descargar los perfiles.

10. Todavía en la pestaña **Signing & Capabilities** (Firma y funcionalidades), haga clic en el botón **+ Capability** (+ Funcionalidad) y pulse dos veces en **Push Notifications** (Notificaciones push) en la lista para asegurarse de que las **notificaciones push** están habilitadas.

11. Abra el archivo **AppDelegate.swift** para implementar el protocolo **UNUserNotificationCenterDelegate** y agregue el código siguiente a la parte superior de la clase:

    ```swift
    @UIApplicationMain
    class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {

       ...

       var configValues: NSDictionary?
       var notificationHubNamespace : String?
       var notificationHubName : String?
       var notificationHubKeyName : String?
       var notificationHubKey : String?
       let tags = ["12345"]

       ...

    }
    ```

    Usará estos miembros más adelante. En concreto, usará el miembro **tags** como parte del registro con una **plantilla personalizada**. Para más información sobre las etiquetas, consulte [Etiquetas para registros](notification-hubs-tags-segment-push-message.md) y  [Registros de plantilla](notification-hubs-templates-cross-platform-push-messages.md).

12. En el mismo archivo, agregue el código siguiente a la función **didFinishLaunchingWithOptions**:

    ```swift
    if let path = Bundle.main.path(forResource: "devsettings", ofType: "plist") {
       if let configValues = NSDictionary(contentsOfFile: path) {
           self.notificationHubNamespace = configValues["notificationHubNamespace"] as? String
           self.notificationHubName = configValues["notificationHubName"] as? String
           self.notificationHubKeyName = configValues["notificationHubKeyName"] as? String
           self.notificationHubKey = configValues["notificationHubKey"] as? String
       }
    }

    if #available(iOS 10.0, *){
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
            (granted, error) in

            if (granted)
            {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
    }

    return true
    ```

    Este código recupera los valores de configuración de **devsettings.plist**, establece la clase **AppDelegate** como el delegado **UNUserNotificationCenter**, solicita autorización para las notificaciones push y, luego, llama a **registerForRemoteNotifications**.

    Para simplificar, el código solo es compatible con iOS 10 y versiones posteriores. Puede agregar compatibilidad con versiones anteriores de iOS de manera condicional si usa las API y los enfoques correspondientes como lo haría normalmente.

13. En el mismo archivo, agregue el siguiente código:

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})
    }

    func showAlert(withText text : String) {
        let alertController = UIAlertController(title: "PushDemo", message: text, preferredStyle: UIAlertControllerStyle.alert)
        alertController.addAction(UIAlertAction(title: "OK", style: UIAlertActionStyle.default,handler: nil))
        let keyWindow = UIApplication.shared.windows.filter {$0.isKeyWindow}.first
        keyWindow?.rootViewController?.present(alertController, animated: true, completion: nil)
    }
    ```

    Este código usa los valores **installationId** y **pushChannel** para registrarse en el centro de notificaciones. En este caso, usa **UIDevice.current.identifierForVendor** para proporcionar un valor único para identificar el dispositivo y, luego, da formato a **deviceToken** para proporcionar el valor de **pushChannel** deseado. La función **showAlert** existe simplemente para mostrar texto de mensaje con fines de demostración.

14. Todavía en el archivo **AppDelegate.swift**, agregue las funciones **willPresent** y **didReceive** a **UNUserNotificationCenterDelegate**. Estas funciones muestran una alerta cuando se les notifica que una aplicación se ejecuta en primer plano o en segundo plano.

    ```swift
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter,
        willPresent notification: UNNotification,
        withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        showAlert(withText: notification.request.content.body)
    }

    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter,
        didReceive response: UNNotificationResponse,
        withCompletionHandler completionHandler: @escaping () -> Void) {
        showAlert(withText: response.notification.request.content.body)
    }
    ```

15. Agregue instrucciones `print` al final de la función **didRegisterForRemoteNotificationsWithDeviceToken** para verificar que se están asignando valores a **installationId** y **pushChannel**.

    ```swift
    print(installationId)
    print(pushChannel)
    ```

16. Cree las carpetas  **Models**, **Services** y **Utilities** para los componentes básicos que agregará al proyecto más adelante.

17. Compruebe que el proyecto se compila y se ejecuta en un dispositivo físico. Las notificaciones push no se pueden probar con el simulador.

## <a name="create-models"></a>Crear modelos

En este paso, creará un conjunto de modelos para representar las cargas de la [API REST de Notification Hubs](/rest/api/notificationhubs/) y para almacenar los datos necesarios del token de firma de acceso compartido (SAS).

1. Agregue un nuevo archivo Swift denominado **PushTemplate.swift** a la carpeta **Models**. Este modelo define una estructura que representa el elemento **BODY** de una plantilla individual como parte de la carga de **DeviceInstallation**.

    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String

        init(withBody body : String) {
            self.body = body
        }
    }
    ```

2. Agregue un nuevo archivo Swift denominado **DeviceInstallation.swift** a la carpeta **Models**. Este archivo define una estructura que representa la carga para crear o actualizar una **instalación de dispositivo**. Agregue el siguiente código al archivo:

    ```swift
    import Foundation

    struct DeviceInstallation : Codable {
        let installationId : String
        let pushChannel : String
        let platform : String = "apns"
        var tags : [String]
        var templates : Dictionary<String, PushTemplate>

        init(withInstallationId installationId : String, andPushChannel pushChannel : String) {
            self.installationId = installationId
            self.pushChannel = pushChannel
            self.tags = [String]()
            self.templates = Dictionary<String, PushTemplate>()
        }
    }
    ```

3. Agregue un nuevo archivo Swift denominado **TokenData.swift** a la carpeta **Models**. Este modelo se usará para almacenar un token de SAS junto con su expiración. Agregue el siguiente código al archivo:

    ```swift
    import Foundation

    struct TokenData {

        let token : String
        let expiration : Int

        init(withToken token : String, andTokenExpiration expiration : Int) {
            self.token = token
            self.expiration = expiration
        }
    }
    ```

## <a name="generate-a-sas-token"></a>Generar un token de SAS

Notification Hubs usa la misma infraestructura de seguridad que Azure Service Bus. Para llamar a la API REST, [genere mediante programación un token de SAS](/rest/api/eventhub/generate-sas-token) que se pueda usar en el encabezado de **autorización** de la solicitud.

El token resultante tendrá el formato siguiente:

```xml
SharedAccessSignature sig=\<UrlEncodedSignature\>\&se=\<ExpiryEpoch\>\&skn=\<KeyName\>\&sr=\<UrlEncodedResourceUri\> |
```

El propio proceso implica los mismos seis pasos:

1. Calcular la expiración en formato de [tiempo UNIX](https://en.wikipedia.org/wiki/Unix_time); es decir, el número de segundos transcurridos desde la medianoche (hora universal coordinada) del 1 de enero de 1970.

2. Aplicar formato al valor de **ResourceUrl** que representa el recurso al que intenta acceder de modo que esté en minúscula y codificado por porcentaje.  **ResourceUrl** tiene el formato `https://<namespace>.servicebus.windows.net/<hubName>`.

3. Preparar **StringToSign**, que tiene el formato `<UrlEncodedResourceUrl>\n<ExpiryEpoch>`.

4. Procesar y codificar en Base64 el valor de **Signature** (Firma) mediante el hash HMAC-SHA256 del valor de **StringToSign**. El valor hash se usa con la parte  **Key** (Clave) de **Connection String** (Cadena de conexión) para el valor de **Authorization Rule** (Regla de autorización) correspondiente.

5. Dar formato al valor de  **Signature** (Firma) codificado en Base64 para que esté codificado por porcentaje.

6. Generar el token en el formato esperado mediante los valores de **UrlEncodedSignature**,  **ExpiryEpoch**,  **KeyName** y **UrlEncodedResourceUrl**.

Consulte la [documentación de Azure Service Bus](../service-bus-messaging/service-bus-sas.md) para una introducción más completa de la firma de acceso compartido y cómo la usan Azure Service Bus y Notification Hubs.

A los efectos de este ejemplo de Swift, usará la biblioteca de código abierto **CommonCrypto** de Apple para ayudar con el hash de la firma. Dado que es una biblioteca de C, no puede acceder desde Swift tal cual. Puede poner la biblioteca a disposición usando un encabezado puente.

Para agregar y configurar el encabezado puente:

1. En Xcode, seleccione **File** (Archivo), **New** (Nuevo), después  **File** (Archivo) y, a continuación, **Header File** (Archivo de encabezado). Asigne al archivo de encabezado el nombre **BridgingHeader.h**.

2. Edite el archivo para importar **CommonHMAC.h**:

   ```swift
   #import <CommonCrypto/CommonHMAC.h>

   #ifndef BridgingHeader_h
   #define BridgingHeader_h

   #endif /* BridgingHeader_h */
   ```

3. Actualice **Build Settings** (Configuración de compilación) del destino para que haga referencia al encabezado puente:

   1. Seleccione el proyecto **PushDemo** y desplácese hacia abajo hasta la sección  **Swift Compiler** (Compilador de Swift).

   2. Asegúrese de que la opción **Install Objective-C Compatibility Header** (Instalar encabezado de compatibilidad de Objective-C) está establecida en **Yes** (Sí).

   3. Escriba la ruta de acceso de archivo `<ProjectName>/BridgingHeader.h` en la opción **Objective-C bridging Header** (Encabezado puente de Objective-C). Esta es la ruta de acceso al encabezado puente.

   Si no encuentra estas opciones, asegúrese de tener seleccionada la vista **All** (Todos) en lugar de **Basic** (Básica) o **Customized** (Personalizada).

   Hay muchas bibliotecas de contenedores de código abierto de terceros disponibles que pueden facilitar el uso de **CommonCrypto**. Sin embargo, la explicación de esas bibliotecas está fuera del ámbito de este artículo.

4. Agregue un nuevo archivo Swift denominado **TokenUtility.swift** dentro de la carpeta **Utilities** y agregue el código siguiente:

   ```swift
   import Foundation

   struct TokenUtility {
      typealias Context = UnsafeMutablePointer<CCHmacContext>

      static func getSasToken(forResourceUrl resourceUrl : String, withKeyName keyName : String, andKey key : String, andExpiryInSeconds expiryInSeconds : Int = 3600) -> TokenData {
        let expiry = (Int(NSDate().timeIntervalSince1970) + expiryInSeconds).description
        let encodedUrl = urlEncodedString(withString: resourceUrl)
        let stringToSign = "\(encodedUrl)\n\(expiry)"
        let hashValue = sha256HMac(withData: stringToSign.data(using: .utf8)!, andKey: key.data(using: .utf8)!)
        let signature = hashValue.base64EncodedString(options: .init(rawValue: 0))
        let encodedSignature = urlEncodedString(withString: signature)
        let sasToken = "SharedAccessSignature sr=\(encodedUrl)&sig=\(encodedSignature)&se=\(expiry)&skn=\(keyName)"
        let tokenData = TokenData(withToken: sasToken, andTokenExpiration: expiryInSeconds)

        return tokenData
      }

      private static func sha256HMac(withData data : Data, andKey key : Data) -> Data {
         let context = Context.allocate(capacity: 1)
         CCHmacInit(context, CCHmacAlgorithm(kCCHmacAlgSHA256), (key as NSData).bytes, size_t((key as NSData).length))
         CCHmacUpdate(context, (data as NSData).bytes, (data as NSData).length)
         var hmac = Array<UInt8>(repeating: 0, count: Int(CC_SHA256_DIGEST_LENGTH))
         CCHmacFinal(context, &hmac)

         let result = NSData(bytes: hmac, length: hmac.count)
         context.deallocate()

         return result as Data
      }

      private static func urlEncodedString(withString stringToConvert : String) -> String {
         var encodedString = ""
         let sourceUtf8 = (stringToConvert as NSString).utf8String
         let length = strlen(sourceUtf8)

         let charArray: [Character] = [ ".", "-", "_", "~", "a", "z", "A", "Z", "0", "9"]
         let asUInt8Array = String(charArray).utf8.map{ Int8($0) }

         for i in 0..<length {
             let currentChar = sourceUtf8![i]

             if (currentChar == asUInt8Array[0] || currentChar == asUInt8Array[1] || currentChar == asUInt8Array[2] || currentChar == asUInt8Array[3] ||
                (currentChar >= asUInt8Array[4] && currentChar <= asUInt8Array[5]) ||
                (currentChar >= asUInt8Array[6] && currentChar <= asUInt8Array[7]) ||
                (currentChar >= asUInt8Array[8] && currentChar <= asUInt8Array[9])) {
                 encodedString += String(format:"%c", currentChar)
             }
             else {
                 encodedString += String(format:"%%%02x", currentChar)
             }
         }

         return encodedString
      }
   }
   ```

   Esta utilidad encapsula la lógica responsable de generar el token de SAS.

   Como se describió anteriormente, la función **getSasToken** organiza los pasos generales necesarios para preparar el token. Más adelante en este tutorial, el servicio de instalación llamará a la función.

   La función **getSasToken** llama a las otras dos funciones: **sha256HMac** para procesar la firma, y **urlEncodedString** para codificar la cadena de dirección URL asociada. La función **urlEncodedString** es necesaria, ya que no es posible lograr el resultado esperado si usa la función integrada **addingPercentEncoding**.

   El [SDK de iOS de Azure Storage](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) es un excelente ejemplo de cómo enfocar estas operaciones en Objective-C. Puede encontrar más información sobre los tokens de SAS de Azure Service Bus en la  [documentación de Azure Service Bus](../service-bus-messaging/service-bus-sas.md).

5. En **AppDelegate.swift**, agregue el código siguiente a la función `didRegisterForRemoteNotificationsWithDeviceToken` para comprobar que el método **TokenUtility.getSasToken** está generando un token válido.

   ```swift
   let baseAddress = "https://<notificaitonHubNamespace>.servicebus.windows.net/<notifiationHubName>"

   let tokenData = TokenUtility.getSasToken(forResourceUrl: baseAddress,
                                            withKeyName: self.notificationHubKeyName!,
                                            andKey: self.notificationHubKey!)

   print(tokenData.token)
   ```

   No olvide reemplazar los marcadores de posición de la cadena **baseAddress** por sus propios valores.

## <a name="verify-the-sas-token"></a>Verificar el token de SAS

Antes de implementar el servicio de instalación en el cliente, compruebe que la aplicación genere correctamente el token de SAS mediante una utilidad HTTP. Para los fines de este tutorial, nuestra herramienta elegida es **Postman**.

Tome nota de los valores de  **installationId** y **token** generados por la aplicación.

Siga estos pasos para llamar a la API de **instalaciones**:

1. En **Postman**, abra una nueva pestaña.
2. Establezca la solicitud en **GET** y especifique la siguiente dirección:

   ```xml
   https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
   ```

3. Configure los encabezados de solicitud como sigue:

   | **Clave**       | **Valor**        |
   | ------------- | ---------------- |
   | Content-Type  | application/json |
   | Authorization | \<sasToken\>     |
   | x-ms-version  | 2015-01          |

4. Seleccione el botón **Code** (Codificar) que aparece en la superior derecha bajo el botón  **Save** (Guardar). La solicitud debe tener un aspecto similar al del siguiente ejemplo:

   ```xml
   GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
   Host: <namespace>.servicebus.windows.net
   Content-Type: application/json
   Authorization: <sasToken>
   x-ms-version: 2015-01
   Cache-Control: no-cache
   Postman-Token: <postmanToken>
   ```

5. Seleccione el botón **Send** (Enviar).

En este momento, no existe ningún registro para el valor **installationId** especificado. La comprobación debe tener una respuesta "404 No encontrado" en lugar de una respuesta "401 No autorizado". Este resultado debe confirmar que se ha aceptado el token de SAS.

## <a name="implement-the-installation-service-class"></a>Implementar la clase de servicio de instalación

A continuación, implemente un contenedor básico en torno a la [API REST de instalaciones](/rest/api/notificationhubs/create-overwrite-installation).

Agregue un nuevo archivo Swift denominado **NotificationRegistrationService.swift** bajo la carpeta **Services** y, luego, agregue el código siguiente a este archivo:

```swift
import Foundation

class NotificationRegistrationService {
    private let tokenizedBaseAddress: String = "https://%@.servicebus.windows.net/%@"
    private let tokenizedCreateOrUpdateInstallationRequest = "/installations/%@?api-version=%@"
    private let session = URLSession(configuration: URLSessionConfiguration.default)
    private let apiVersion = "2015-01"
    private let jsonEncoder = JSONEncoder()
    private let defaultHeaders: [String : String]
    private let installationId : String
    private let pushChannel : String
    private let hubNamespace : String
    private let hubName : String
    private let keyName : String
    private let key : String
    private var tokenData : TokenData? = nil
    private var tokenExpiryDate : Date? = nil

    init(withInstallationId installationId : String,
            andPushChannel pushChannel : String,
            andHubNamespace hubNamespace : String,
            andHubName hubName : String,
            andKeyName keyName : String,
            andKey key: String) {
        self.installationId = installationId
        self.pushChannel = pushChannel
        self.hubNamespace = hubNamespace
        self.hubName = hubName
        self.keyName = keyName
        self.key = key
        self.defaultHeaders = ["Content-Type": "application/json", "x-ms-version": apiVersion]
    }

    func register(
        withTags tags : [String]? = nil,
        andTemplates templates : Dictionary<String, PushTemplate>? = nil,
        completeWith completion: ((_ result: Bool) -> ())? = nil) {

        var deviceInstallation = DeviceInstallation(withInstallationId: installationId, andPushChannel: pushChannel)

        if let tags = tags {
            deviceInstallation.tags = tags
        }

        if let templates = templates {
            deviceInstallation.templates = templates
        }

        if let deviceInstallationJson = encodeToJson(deviceInstallation) {
            let sasToken = getSasToken()
            let requestUrl = String.init(format: tokenizedCreateOrUpdateInstallationRequest, installationId, apiVersion)
            let apiEndpoint = "\(getBaseAddress())\(requestUrl)"

            var request = URLRequest(url: URL(string: apiEndpoint)!)
            request.httpMethod = "PUT"

            for (key,value) in self.defaultHeaders {
                request.addValue(value, forHTTPHeaderField: key)
            }

            request.addValue(sasToken, forHTTPHeaderField: "Authorization")
            request.httpBody = Data(deviceInstallationJson.utf8)

            (self.session.dataTask(with: request) { dat, res, err in
                if let completion = completion {
                        completion(err == nil &&
                        (res as! HTTPURLResponse).statusCode == 200)
                }
            }).resume()
        }
    }

    private func getBaseAddress() -> String {
        return String.init(format: tokenizedBaseAddress, hubNamespace, hubName)
    }

    private func getSasToken() -> String {
        if (tokenData == nil ||
            tokenExpiryDate == nil ||
            Date() >= tokenExpiryDate!) {

            self.tokenData = TokenUtility.getSasToken(
                forResourceUrl: getBaseAddress(),
                withKeyName: self.keyName,
                andKey: self.key)

            self.tokenExpiryDate = Date(timeIntervalSinceNow: -(5 * 60))
                .addingTimeInterval(TimeInterval(tokenData!.expiration))
        }

        return (tokenData?.token)!
    }

    private func encodeToJson<T : Encodable>(_ object: T) -> String? {
        do {
            let jsonData = try jsonEncoder.encode(object)
            if let jsonString = String(data: jsonData, encoding: .utf8) {
                return jsonString
            } else {
                return nil
            }
        }
        catch {
            return nil
        }
    }
}
```

Los detalles necesarios se proporcionan como parte de la inicialización. Las etiquetas y plantillas se pasan opcionalmente a la función **register** para formar parte de la carga de **Device Installation** (Instalación de dispositivo) de JSON.

La función **register** llama a las otras funciones privadas para preparar la solicitud. Una vez que se recibe una respuesta, se llama a la finalización e indica si el registro se realizó correctamente.

La función **getBaseAddress** construye el punto de conexión de la solicitud. La construcción usa los parámetros *namespace* y *name* del centro de notificaciones que se proporcionaron durante la inicialización.

La función **getSasToken** comprueba si el token almacenado actualmente es válido. Si el token no es válido, la función llama a **TokenUtility** para generar un nuevo token y, luego, lo almacena antes de devolver un valor.

Por último, **encodeToJson** convierte los objetos del modelo correspondiente en JSON para su uso como parte del cuerpo de la solicitud.

## <a name="invoke-the-notification-hubs-rest-api"></a>Invocar la API de REST de Notification Hubs

El último paso es actualizar **AppDelegate** para usar **NotificationRegistrationService** para registrarse con **NotificationHub**.

1. Abra **AppDelegate.swift** y agregue una variable de nivel de clase para almacenar una referencia a **NotificationRegistrationService** y el archivo **PushTemplate** genérico:

   ```swift
   var registrationService : NotificationRegistrationService?
   let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")
   ```

2. En el mismo archivo, actualice la función **didRegisterForRemoteNotificationsWithDeviceToken** para inicializar **NotificationRegistrationService** con los parámetros necesarios y, luego, llame a la función **register**.

   ```swift
   func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    let installationId = (UIDevice.current.identifierForVendor?.description)!
    let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})

    // Initialize the Notification Registration Service
    self.registrationService = NotificationRegistrationService(
        withInstallationId: installationId,
        andPushChannel: pushChannel,
        andHubNamespace: notificationHubNamespace!,
        andHubName: notificationHubName!,
        andKeyName: notificationHubKeyName!,
        andKey: notificationHubKey!)

    // Call register, passing in the tags and template parameters
    self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
        if !result {
            print("Registration issue")
        } else {
            print("Registered")
          }
       }
   }
   ```

   Para simplificar, el código utiliza instrucciones `print` para actualizar la ventana de salida con el resultado de la operación **register**.

3. Compile y ejecute la aplicación en un dispositivo físico. Debería ver **Registered** (Registrado) en la ventana de salida.

## <a name="test-the-solution"></a>Probar la solución

En este momento, la aplicación se ha registrado con **NotificationHub** y puede recibir notificaciones push. En Xcode, detenga el depurador y cierre la aplicación si se está ejecutando actualmente. A continuación, compruebe que los detalles de **Device Installation** (Instalación del dispositivo) sean los previstos y que la aplicación ya puede recibir notificaciones push.

### <a name="verify-the-device-installation"></a>Verificar la instalación del dispositivo

Ahora puede hacer la misma solicitud que hizo anteriormente con **Postman** para [verificar el token de SAS](notification-hubs-ios-push-notifications-swift-apps-get-started.md#verify-the-sas-token). Suponiendo que el token de SAS no ha expirado, la respuesta ahora debe incluir los detalles de instalación que proporcionó, por ejemplo, las etiquetas y plantillas.

```json
{
    "installationId": "<installationId>",
    "pushChannel": "<pushChannel>",
    "pushChannelExpired": false,
    "platform": "apns",
    "expirationTime": "9999-12-31T23:59:59.9999999Z",
    "tags": [
        "12345"
    ],
    "templates": {
        "genericTemplate": {
            "body": "{\"aps\":{\"alert\":\"$(message)\"}}",
            "tags": [
                "genericTemplate"
            ]
        }
    }
}
```

Si ha expirado el token de SAS anterior, puede agregar un punto de interrupción a la línea 24 de la clase **TokenUtility** para obtener un nuevo token de SAS y actualizar el encabezado **Authorization** (Autorización) con el nuevo valor.

### <a name="send-a-test-notification-azure-portal"></a>Enviar una notificación de prueba (Azure Portal)

La forma más rápida de probar que ahora puede recibir notificaciones es ir al centro de notificaciones en Azure Portal:

1. En Azure Portal, vaya a la pestaña **Información general** en el centro de notificaciones.

2. Seleccione **Envío de prueba**, que está arriba del resumen de **Información esencial** en la esquina superior izquierda de la ventana del portal:

   :::image type="content" source="media/ios-sdk-swift-rest/image2.png" alt-text="Botón Envío de prueba del resumen de Información esencial de Notification Hubs":::

3. Elija **Plantilla personalizada** en la lista  **Plataformas**.

4. Escriba **12345** en **Enviar a expresión de etiqueta**. Había especificado esta etiqueta anteriormente en la instalación.

5. De manera opcional, edite el **mensaje** en la carga de JSON:

   :::image type="content" source="media/ios-sdk-swift-rest/image3.png" alt-text="Envío de prueba de Notification Hubs":::

6. Seleccione **Enviar**. El portal debería indicar si la notificación se envió correctamente al dispositivo:

   :::image type="content" source="media/ios-sdk-swift-rest/image4.png" alt-text="Resultado del envío de prueba":::

   Suponiendo que la aplicación no se ejecuta en primer plano, también verá una notificación en el **Centro de notificaciones** en el dispositivo. Al pulsar la notificación, debería abrirse la aplicación y mostrarse la alerta.

   :::image type="content" source="media/ios-sdk-swift-rest/image5.png" alt-text="Notificación de prueba":::

### <a name="send-a-test-notification-mail-carrier"></a>Envío de una notificación de prueba (operador de correo)

Puede enviar notificaciones a través de la [API REST](/rest/api/notificationhubs/) usando **Postman**, que puede ser una manera más práctica para probar.

1. En **Postman**, abra una nueva pestaña.

2. Establezca la solicitud en **POST** y escriba la siguiente dirección:

   ```xml
   https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
   ```

3. Configure los encabezados de solicitud como sigue:

   | Clave                           | Value                          |
   | ----------------------------- | ------------------------------ |
   | Content-Type                  | application/json;charset=utf-8 |
   | Authorization                 | \<sasToken\>                   |
   | ServiceBusNotification-Format | template                       |
   | Etiquetas                          | "12345"                        |

4. Configure la sección **BODY** (Cuerpo) de la solicitud para usar **RAW - JSON (application.json)**  con la carga de JSON siguiente:

   ```json
   {
   "message" : "Hello from Postman!"
   }
   ```

5. Seleccione el botón **Code** (Codificar) que aparece bajo el botón **Save** (Guardar) en la esquina superior derecha de la ventana. La solicitud debe tener un aspecto similar al del siguiente ejemplo:

   ```xml
   POST /<hubName>/messages/?api-version=2015-01 HTTP/1.1
   Host: <namespace>.servicebus.windows.net
   Content-Type: application/json;charset=utf-8.
   ServiceBusNotification-Format: template
   Tags: "12345"
   Authorization: <sasToken>
   Cache-Control: no-cache
   Postman-Token: <postmanToken>

   {
       "message" : "Hello from Postman!"
   }
   ```

6. Seleccione el botón **Send** (Enviar).

Debe obtener un código de estado correcto **201 Creado** y recibir la notificación en el dispositivo cliente.

## <a name="next-steps"></a>Pasos siguientes

Ahora tiene una aplicación iOS básica basada en Swift conectada a un centro de notificaciones a través de la  [API REST de Notification Hubs](/rest/api/notificationhubs/), y puede enviar y recibir notificaciones. Para aprender más sobre cómo enviar notificaciones a dispositivos específicos, pase al siguiente tutorial:

- [Tutorial: Envío de notificaciones push a dispositivos específicos](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

Para más información, consulte los siguientes artículos.

- [Introducción a Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [API de REST de Notification Hubs](/rest/api/notificationhubs/)
- [SDK de Notification Hubs para operaciones de back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [SDK de Notification Hubs en GitHub](https://github.com/Azure/azure-notificationhubs)
- [Registro en el back-end de aplicación](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Administración de registros](notification-hubs-push-notification-registration-management.md)
- [Trabajo con etiquetas](notification-hubs-tags-segment-push-message.md)
- [Trabajo con plantillas personalizadas](notification-hubs-templates-cross-platform-push-messages.md)
- [Control de acceso de Service Bus con Firmas de acceso compartido](../service-bus-messaging/service-bus-sas.md)
- [Generar tokens de SAS mediante programación](/rest/api/eventhub/generate-sas-token)
- [Seguridad de Apple: cifrado común](https://developer.apple.com/security/)
- [Tiempo UNIX](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
