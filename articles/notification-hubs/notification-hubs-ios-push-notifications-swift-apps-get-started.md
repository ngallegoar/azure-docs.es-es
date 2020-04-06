---
title: Envío de notificaciones push a aplicaciones iOS basadas en Swift que usan Azure Notification Hubs | Microsoft Docs
description: Aprenda a enviar notificaciones push a aplicaciones iOS basadas en Swift que usen Azure Notification Hubs.
services: notification-hubs
documentationcenter: ios
author: mikeparker104
manager: femila
editor: jwargo
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 05/21/2019
ms.author: miparker
ms.reviewer: jowargo
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: a721c519c7a836e20455c6f1887bcfa7b52951f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336633"
---
# <a name="tutorial-send-push-notifications-to-swift-ios-apps-using-notification-hubs-rest-api"></a>Tutorial: Envío de notificaciones push a aplicaciones iOS basadas en Swift mediante la API REST de Notification Hubs

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

En este tutorial, usa Azure Notification Hubs para enviar notificaciones push a una aplicación iOS basada en Swift con la [API de REST](/rest/api/notificationhubs/). Además, crea una aplicación iOS vacía que recibe notificaciones push mediante [Apple Push Notification service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).

Este tutorial le guía por los siguientes pasos:

> [!div class="checklist"]
> * Generar el archivo de solicitud de firma de certificado.
> * Solicitar notificaciones push a la aplicación.
> * Crear un perfil de aprovisionamiento para la aplicación.
> * Crear un Centro de notificaciones
> * Configurar un centro de notificaciones con la información de APNs.
> * Conectar la aplicación iOS a un centro de notificaciones.
> * Probar la solución.

El código completo de este tutorial se puede encontrar en [GitHub](https://github.com/xamcat/mobcat-samples/tree/master/notification_hub_rest).

## <a name="prerequisites"></a>Prerrequisitos

Para continuar, necesita:

- Consultar la [Introducción a Azure Notification Hubs](notification-hubs-push-notification-overview.md) si no está familiarizado con el servicio.
- Obtener información sobre [registros e instalación](notification-hubs-push-notification-registration-management.md).
- Una [cuenta de desarrollador de Apple](https://developer.apple.com) activa.
- Mac con Xcode, junto con un certificado de desarrollador válido instalado en el Llavero.
- Un dispositivo iPhone físico donde pueda ejecutar y depurar, ya que no puede probar las notificaciones push con el simulador.
- El dispositivo iPhone físico registrado en el [portal de Apple](https://developer.apple.com) y asociado con el certificado.
- Una [suscripción a Azure](https://portal.azure.com) donde pueda crear y administrar los recursos.

Incluso si no tiene ninguna experiencia previa con el desarrollo en iOS, podrá seguir los pasos para crear este ejemplo de primeros principios. No obstante, resulta beneficioso estar familiarizado con los conceptos siguientes:

- Compilar aplicaciones de iOS con Xcode y Swift.
- Configurar una instancia de [Azure Notification Hubs](notification-hubs-ios-apple-push-notification-apns-get-started.md) para iOS.
- El [portal para desarrolladores de Apple](https://developer.apple.com) y [Azure Portal](https://portal.azure.com).

> [!NOTE]
> El centro de notificaciones se configurará para usar el modo de autenticación **Espacio aislado** únicamente. No debe usar este modo de autenticación para las cargas de trabajo de producción.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-a-notification-hub"></a>Conectar la aplicación iOS a un centro de notificaciones

En esta sección, compilará la aplicación de iOS que se conectará al centro de notificaciones.  

### <a name="create-an-ios-project"></a>Crear un proyecto para iOS

1. En XCode, cree un nuevo proyecto iOS y seleccione la plantilla **Single View Application** (Aplicación de vista sencilla).

1. Al configurar las opciones para el nuevo proyecto:

   1. Especifique **Product Name** (Nombre de producto) (PushDemo) y **Organization Identifier** (Identificador de la organización) (`com.<organization>`) que usó cuando estableció **Bundle Identifier** (Identificador de paquete) en el portal para desarrolladores de Apple.

   1. Elija **Team** (Equipo) para el que se configuró el **App ID** (Id. de aplicación).

   1. Establezca **language** (lenguaje) en **Swift**.

   1. Seleccione **Next** (Siguiente).

1. Cree una nueva carpeta llamada **SupportingFiles**.

1. Crear un nuevo archivo p-list denominado **devsettings.plist** en la carpeta **SupportingFiles**. No olvide agregar esta carpeta a su archivo **gitignore** de modo que no se confirme al trabajar con un repositorio de Git. En una aplicación de producción, probablemente establecería condicionalmente estos secretos como parte de un proceso de compilación automatizado. Dicha configuración no se trata en este tutorial.

1. Actualice **devsettings.plist** para incluir las siguientes entradas de configuración con sus propios valores desde el centro de notificaciones que ha aprovisionado:

   | Clave                            | Tipo                     | Value                     |
   |--------------------------------| -------------------------| --------------------------|
   | notificationHubKey             | String                   | \<hubKey>                  |
   | notificationHubKeyName         | String                   | \<hubKeyName>              |
   | notificationHubName            | String                   | \<hubName>                 |
   | notificationHubNamespace       | String                   | \<hubNamespace>            |

   Para encontrar los valores necesarios, desplácese hasta el recurso del centro de notificaciones en Azure Portal. En concreto, los valores de **notificationHubName** y **notificationHubNamespace** están en la esquina superior derecha del resumen **Información esencial** dentro de la página **Información general**.

   ![Resumen de Información esencial de Notification Hubs](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   También puede encontrar los valores de **notificationHubKeyName** y **notificationHubKey** en **Directivas de acceso** y seleccionando la **Directiva de acceso** correspondiente, como `DefaultFullSharedAccessSignature`. Después de eso, copie de **Cadena de conexión primaria** el valor con prefijo `SharedAccessKeyName=` para `notificationHubKeyName`, y el valor con prefijo `SharedAccessKey=` para `notificationHubKey`.

   La cadena de conexión debe tener el siguiente formato:

   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Por simplicidad, especifique `DefaultFullSharedAccessSignature` de modo que pueda usar el token para enviar notificaciones. En la práctica, `DefaultListenSharedAccessSignature` sería una mejor opción para situaciones donde solo quiere recibir notificaciones.

1. En **Project Navigator** (Navegador de proyectos), seleccione **Nombre del proyecto** y, luego, seleccione la pestaña **General**.

1. Busque **Identidad** y, luego, establezca el valor **Identificador del lote** para que coincida con `com.<organization>.PushDemo`, que es el valor usado para el **Id. de aplicación** de un paso anterior.

1. Busque **Signing & Capabilities** (Firma y funcionalidades) y, luego, seleccione el **Equipo** adecuado para la **cuenta de desarrollador de Apple**. El valor **Equipo** debe coincidir con el que creó los certificados y perfiles.

1. Xcode debe desplegar automáticamente el valor de **Perfil de aprovisionamiento** adecuado según en el **Identificador de lote**. Si no ve el nuevo valor de **Perfil de aprovisionamiento**, intente actualizar los perfiles para la **Identidad de firma** desde **Xcode** > **Preferencias** > **Cuenta** y, a continuación, seleccione el botón **Download Manual Profiles** (Descargar perfiles manuales) para descargar los perfiles.

1. En la pestaña **Signing & Capabilities** (Firma y funcionalidades), haga clic en el botón **+ Capability** (+ Funcionalidad) y pulse dos veces en **Push Notifications** (Notificaciones push) en la lista para asegurarse de que las **notificaciones push** están habilitadas.

1. Abra el archivo **AppDelegate.swift** para implementar el protocolo **UNUserNotificationCenterDelegate** y agregue el código siguiente a la parte superior de la clase:

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

    Usará estos miembros más adelante. En concreto, usará el miembro **tags** como parte del registro con una **plantilla personalizada**. Para más información sobre las etiquetas, consulte [Etiquetas para registros](notification-hubs-tags-segment-push-message.md) y [Registros de plantillas](notification-hubs-templates-cross-platform-push-messages.md).

1. En el mismo archivo, agregue el código siguiente a la función **didFinishLaunchingWithOptions**:

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

    Este código recupera los valores de configuración de **devsettings.plist**, establece la clase **AppDelegate** como el delegado **UNUserNotificationCenter**, solicita autorización para las notificaciones push y, luego, llama a **registerForRemoteNotifications**.

    Por simplicidad, el código es compatible con *iOS 10 y versiones posteriores únicamente*. Puede agregar compatibilidad con versiones anteriores del sistema operativo de manera condicional si usa las API y los enfoques correspondientes como lo haría normalmente.

1. En el mismo archivo, agregue las funciones siguientes:

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

    El código usa los valores de **installationId** y **pushChannel** para registrarse en el centro de notificaciones. En este caso, usa **UIDevice.current.identifierForVendor** para proporcionar un valor único para identificar el dispositivo y, luego, da formato a **deviceToken** para proporcionar el valor de  **pushChannel** deseado. La función **showAlert** existe simplemente para mostrar texto de mensaje con fines de demostración.

1. Aún en el archivo **AppDelegate.swift**, agregue las funciones **willPresent** y **didReceive** a **UNUserNotificationCenterDelegate**. Estas funciones muestran una alerta cuando se les notifica que una aplicación se ejecuta en primer plano o en segundo plano, respectivamente.

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

1. Agregue instrucciones de impresión al final de la función **didRegisterForRemoteNotificationsWithDeviceToken** para verificar que se están asignando valores a **installationId** y **pushChannel**.

    ```swift
    print(installationId)
    print(pushChannel)
    ```

1. Cree las carpetas **Models**, **Services** y **Utilities** para los componentes básicos que agregará al proyecto más adelante.

1. Compruebe que el proyecto se compila y se ejecuta en un dispositivo físico. Las notificaciones push no se pueden probar con el simulador.

### <a name="create-models"></a>Crear modelos

En este paso, creará un conjunto de modelos para representar las cargas de la [API de REST de Notification Hubs](/rest/api/notificationhubs/) y para almacenar los datos necesarios del token de firma de acceso compartido (SAS).

1. Agregue un nuevo archivo Swift denominado **PushTemplate.swift** a la carpeta **Models**. Este modelo proporciona una estructura que representa el **BODY** de una plantilla individual como parte de la carga **DeviceInstallation**.

    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String

        init(withBody body : String) {
            self.body = body
        }
    }
    ```

1. Agregue un nuevo archivo Swift denominado **DeviceInstallation.swift** a la carpeta **Models**. Este archivo define una estructura que representa la carga para crear o actualizar una **instalación de dispositivo**. Agregue el siguiente código al archivo:

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

1. Agregue un nuevo archivo Swift denominado **TokenData.swift** a la carpeta **Models**. Este modelo se usará para almacenar un token de SAS junto con la expiración.

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

### <a name="generate-a-sas-token"></a>Generar un token de SAS

Notification Hubs usa la misma infraestructura de seguridad que Azure Service Bus. Para llamar a la API de REST, deberá [generar mediante programación un token de SAS](/rest/api/eventhub/generate-sas-token) que pueda usarse en el encabezado de **Autorización** de la solicitud.  

El token resultante tendrá el formato siguiente:

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

El propio proceso implica los mismos seis pasos principales:  

1. Calcular la expiración en formato de [tiempo UNIX](https://en.wikipedia.org/wiki/Unix_time), es decir, el número de segundos transcurridos desde la medianoche (hora universal coordinada) del 1 de enero de 1970.
1. Aplicar formato a **ResourceUrl** que representa el recurso al que intenta acceder de modo que esté en minúscula y codificado por porcentaje. **ResourceUrl** tiene el formato `'https://<namespace>.servicebus.windows.net/<hubName>'`.
1. Preparar **StringToSign**, que tiene el formato `'<UrlEncodedResourceUrl>\n<ExpiryEpoch>'`.
1. Procesar y codificar en Base64 la **firma** con el hash HMAC-SHA256 del valor **StringToSign**. El valor hash se usa con la parte de la **clave** de la **cadena de conexión** para la **Regla de autorización** correspondiente.
1. Formatear la **firma** codificada en Base64 para que esté codificado por porcentaje.
1. Generar el token en el formato esperado al usar los valores de **UrlEncodedSignature**, **ExpiryEpoch**, **KeyName** y **UrlEncodedResourceUrl**.

Consulte la [documentación de Azure Service Bus](../service-bus-messaging/service-bus-sas.md) para una introducción más completa de la firma de acceso compartido y cómo la usan Azure Service Bus y Notification Hubs.

A los efectos de este ejemplo de Swift, va a usar la biblioteca de código abierto **CommonCrypto** de Apple para ayudar con el hash de la firma. Dado que es una biblioteca de C, no puede acceder desde Swift tal cual. Puede poner la biblioteca a disposición usando un encabezado puente.

Para agregar y configurar el encabezado puente:

1. En Xcode, seleccione **File** > **New** > **File** > **Header File** (Archivo>Nuevo>Archivo>Archivo de encabezado). Asigne al archivo de encabezado el nombre **BridgingHeader.h**.

1. Edite el archivo para importar **CommonHMAC.h**:

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

1. Actualice **Build Settings** (Configuración de compilación) del destino para que haga referencia al encabezado puente:

   1. Pulse el objeto **PushDemo** y desplácese hacia abajo hasta la sección **Swift Compiler** (Compilador de Swift).

   1. Asegúrese de que la opción**Install Objective-C Compatibility Header** (Instalar encabezado de compatibilidad de Objective-C) está establecida en **Yes** (Sí).

   1. Escriba la ruta de acceso de archivo `'<ProjectName>/BridgingHeader.h'` en la opción **Objective-C bridging Header** (Encabezado puente de Objective-C). Esta es la ruta de acceso de archivo al encabezado puente.

   Si no encuentra estas opciones, asegúrese de tener seleccionada la vista **All** (Todos) en lugar de **Basic** (Básica) o **Customized** (Personalizada).

   Hay muchas bibliotecas de contenedores de código abierto de terceros disponibles que pueden facilitar el uso de **CommonCrypto**. Sin embargo, la explicación de estas bibliotecas está fuera del ámbito de este artículo.

1. Agregue un nuevo archivo Swift denominado **TokenUtility.swift** dentro de la carpeta **Utilities** y agregue el código siguiente:

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

   Como se describió anteriormente, la función **getSasToken** organiza los pasos generales necesarios para preparar el token. Más adelante en este tutorial, el servicio de instalación llamará a la función.

   La función **getSasToken** llama a las otras dos funciones: **sha256HMac** para procesar la firma, y **urlEncodedString** para codificar la cadena de dirección URL asociada. La función **urlEncodedString** es necesaria, ya que no es posible lograr el resultado esperado si usa la función integrada **addingPercentEncoding**.

   El [SDK de iOS de Azure Storage](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) es un excelente ejemplo de cómo enfocar estas operaciones en Objective-C. Puede encontrar más información sobre los tokens de SAS de Azure Service Bus en la [documentación de Azure Service Bus](../service-bus-messaging/service-bus-sas.md).

1. En **AppDelegate.swift**, agregue el código siguiente a la función *didRegisterForRemoteNotificationsWithDeviceToken* para comprobar que **TokenUtility.getSasToken** genera un token válido.
    
    ```swift
    let baseAddress = "https://<notificaitonHubNamespace>.servicebus.windows.net/<notifiationHubName>"

    let tokenData = TokenUtility.getSasToken(forResourceUrl: baseAddress,
                                                withKeyName: self.notificationHubKeyName!,
                                                andKey: self.notificationHubKey!)
    
    print(tokenData.token)
    ```

    No olvide reemplazar los valores del marcador de posición de la cadena **baseAddress** por los suyos propios.

### <a name="verify-the-sas-token"></a>Verificar el token de SAS

Antes de implementar el servicio de instalación en el cliente, compruebe que nuestra aplicación genere correctamente el token de SAS mediante la utilidad HTTP de su elección. Para los fines de este tutorial, nuestra herramienta elegida será **Postman**.

Anote los valores de **installationId** y **token** generados por la aplicación.

Siga estos pasos para llamar a la API de **instalaciones**:

1. En **Postman**, abra una nueva pestaña.

1. Establezca la solicitud en **GET** y especifique la siguiente dirección:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
    ```

1. Configure los encabezados de solicitud como sigue:

   | Clave           | Value            |
   | ------------- | ---------------- |
   | Content-Type  | application/json |
   | Authorization | \<sasToken>       |
   | x-ms-version  | 2015-01          |

1. Seleccione el botón **Code** (Codificar) que aparece en la superior derecha bajo el botón **Save** (Guardar). La solicitud debe tener un aspecto similar al del siguiente ejemplo:

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

1. Seleccione el botón **Enviar**.

En este momento, no existe ningún registro para el valor **installationId** especificado. La comprobación debe tener una respuesta "404 No encontrado" en lugar de una respuesta "401 No autorizado". Este resultado debe confirmar que se ha aceptado el token de SAS.

### <a name="implement-the-installation-service-class"></a>Implementar la clase de servicio de instalación

A continuación implementaremos el contenedor básico en torno a la [API de REST de instalaciones](/rest/api/notificationhubs/create-overwrite-installation).  

Agregue un nuevo archivo de Swift denominado **NotificationRegistrationService.swift** bajo la carpeta **Services** y, luego, agregue el código siguiente a este archivo:

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

Los detalles necesarios se proporcionan como parte de la inicialización. Las etiquetas y plantillas se pasan opcionalmente a la función **register** para formar parte de la carga de **instalación de dispositivo** de JSON.  

La función **register** llama a las otras funciones privadas para preparar la solicitud. Una vez que se recibe una respuesta, se llama a la finalización e indica si el registro se realizó correctamente.  

La función **getBaseAddress** construye el punto de conexión de la solicitud. La construcción usa los parámetros *espacio de nombres* y *nombre* del centro de notificaciones que se proporcionaron durante la inicialización.  

La función **getSasToken** comprueba si el token almacenado actualmente es válido. Si el token no es válido, la función llama a **TokenUtility** para generar un nuevo token y, luego, lo almacena antes de devolver un valor.

Por último, **encodeToJson** convierte los objetos del modelo correspondiente en JSON para su uso como parte del cuerpo de la solicitud.

### <a name="invoke-the-notification-hubs-rest-api"></a>Invocar la API de REST de Notification Hubs

El último paso es actualizar **AppDelegate** usar **NotificationRegistrationService** para registrarse en nuestro **NotificationHub**.

1. Abra **AppDelegate.swift** y agregue una variable de nivel de clase para almacenar una referencia a **NotificationRegistrationService** y el archivo **PushTemplate** genérico:

    ```swift
    var registrationService : NotificationRegistrationService?
    let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")
    ```

1. En el mismo archivo, actualice la función **didRegisterForRemoteNotificationsWithDeviceToken** para inicializar **NotificationRegistrationService** con los parámetros necesarios y, luego, llame a la función **register**.

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

    Por simplicidad, va a usar un par de instrucciones de impresión para actualizar la ventana de salida con el resultado de la operación de **register**.

1. Ahora compile y ejecute la aplicación en un dispositivo físico. Debería ver "Registered" (Registrado) en la ventana de salida.

## <a name="test-the-solution"></a>Probar la solución

En esta etapa, nuestra aplicación se ha registrado en **NotificationHub** y puede recibir notificaciones push. En Xcode, detenga el depurador y cierre la aplicación si se está ejecutando actualmente. A continuación, compruebe que los detalles de **Instalación del dispositivo** sean los previstos y que la aplicación ahora puede recibir notificaciones push.  

### <a name="verify-the-device-installation"></a>Verificar la instalación del dispositivo

Ahora puede hacer la misma solicitud que hizo anteriormente con **Postman** para [verificar el token de SAS](#verify-the-sas-token). Suponiendo que el token de SAS no ha expirado, la respuesta ahora debe incluir los detalles de instalación que proporcionó, por ejemplo, las etiquetas y plantillas.

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

Si ha expirado el **token de SAS** anterior, puede agregar un **punto de interrupción** a la **línea 24** de la clase **TokenUtility** para obtener un nuevo **token de SAS** y actualizar el encabezado **Authorization** con el nuevo valor.

### <a name="send-a-test-notification-azure-portal"></a>Enviar una notificación de prueba (Azure Portal)

La forma más rápida de probar que ahora puede recibir notificaciones es ir al centro de notificaciones en Azure Portal:

1. En Azure Portal, vaya a la pestaña **Información general** en el centro de notificaciones.

1. Seleccione **Envío de prueba**, que está arriba del resumen de **Información esencial** en la esquina superior izquierda de la ventana del portal:

    ![Botón Envío de prueba del resumen de Información esencial de Notification Hubs](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)

1. Elija **Plantilla personalizada** en la lista **Plataformas**.

1. Escriba **12345** en **Enviar a expresión de etiqueta**. Había especificado esta etiqueta anteriormente en la instalación.

1. De manera opcional, edite el **mensaje** en la carga de JSON:

    ![Envío de prueba de Notification Hubs](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)

1. Seleccione **Enviar**. El portal debería indicar si la notificación se envió correctamente al dispositivo:

    ![Resultados de Envío de prueba de Notification Hubs](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    Suponiendo que la aplicación no se ejecuta en primer plano, también verá una notificación en el **Centro de notificaciones** en el dispositivo. Al pulsar la notificación, debería abrirse la aplicación y mostrarse la alerta.

    ![Ejemplo de notificación recibida](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-mail-carrier"></a>Envío de una notificación de prueba (operador de correo)

Puede enviar notificaciones a través de la [API de REST](/rest/api/notificationhubs/) usando **Postman**, que puede ser una manera más práctica para probar.

1. En **Postman**, abra una nueva pestaña.

1. Establezca la solicitud en **POST** y escriba la siguiente dirección:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
    ```

1. Configure los encabezados de solicitud como sigue:

   | Clave                            | Value                          |
   | ------------------------------ | ------------------------------ |
   | Content-Type                   | application/json;charset=utf-8 |
   | Authorization                  | \<sasToken>                     |
   | ServiceBusNotification-Format  | template                       |
   | Etiquetas                           | "12345"                        |

1. Configure el **CUERPO** de la solicitud para usar **RAW - JSON (application.json)** con la carga de JSON siguiente:

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

1. Seleccione el botón **Code** (Codificar) que aparece en la superior derecha bajo el botón **Save** (Guardar) en esquina superior derecha de la ventana. La solicitud debe tener un aspecto similar al del siguiente ejemplo:

    ```html
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

1. Seleccione el botón **Enviar**.

Debe obtener un código de estado correcto **201 Creado** y recibir la notificación en el dispositivo cliente.

## <a name="next-steps"></a>Pasos siguientes
Ahora tiene una aplicación básica de iOS basada en Swift conectada a un centro de notificaciones a través de la [API de REST](/rest/api/notificationhubs/), y puede enviar y recibir notificaciones. Para más información, consulte los siguientes artículos.

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
