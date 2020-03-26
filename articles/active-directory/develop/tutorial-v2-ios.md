---
title: 'Tutorial de MSAL para iOS y macOS: Plataforma de identidad de Microsoft | Azure'
description: Aprenda la forma en que las aplicaciones de iOS y macOS (Swift) pueden llamar a una API que requiera tokens de acceso mediante la Plataforma de identidad de Microsoft.
services: active-directory
documentationcenter: dev-center-name
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 08/30/2019
ms.author: jmprieur
ms.reviewer: oldalton
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: abf083aacbdc643d780a8061b405752f36e27e45
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79129939"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-or-macos-app"></a>Inicio de sesión de los usuarios y llamada a Microsoft Graph desde una aplicación para iOS o macOS

En este tutorial, aprenderá a integrar una aplicación para iOS o macOS con la plataforma de identidad de Microsoft. La aplicación iniciará la sesión de un usuario, obtendrá un token de acceso para llamar a Microsoft Graph API y realizará una solicitud a Microsoft Graph API.  

Cuando haya completado la guía, la aplicación podrá aceptar inicios de sesión de cuentas Microsoft personales (como outlook.com, live.com y otras), y cuentas profesionales y educativas de cualquier empresa u organización que utilice Azure Active Directory.

>[!NOTE]
> Si es la primera vez que usa la plataforma de identidad de Microsoft, es aconsejable que comience con [Inicio de sesión de los usuarios y llamada a Microsoft Graph API desde una aplicación de iOS o macOS](quickstart-v2-ios.md).

## <a name="how-this-tutorial-works"></a>Funcionamiento de este tutorial

![Muestra cómo funciona la aplicación de ejemplo generada por este tutorial](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

La aplicación de este tutorial iniciará la sesión de los usuarios y obtendrá datos en su nombre.  A estos datos se accederá a través de una API protegida (Microsoft Graph API en este caso) que requiere autorización y que está protegida por la plataforma de identidad de Microsoft.

Más concretamente:

* La aplicación iniciará la sesión del usuario mediante un explorador o Microsoft Authenticator.
* El usuario final aceptará los permisos que la aplicación ha solicitado.
* Se emitirá un token de acceso a la aplicación para Microsoft Graph API.
* El token de acceso se incluirán en la solicitud HTTP a la API web.
* Se procesa la respuesta de Microsoft Graph.

Este ejemplo utiliza la Biblioteca de autenticación de Microsoft (MSAL) para implementar la autenticación. MSAL renovará automáticamente los tokens, proporcionará el inicio de sesión único entre las otras aplicaciones del dispositivo y administrará las cuentas.

Este tutorial es aplicable a las aplicaciones para iOS y macOS. Tenga en cuenta que algunos pasos son diferentes entre esas dos plataformas. 

## <a name="prerequisites"></a>Prerrequisitos

- En esta guía se requiere la versión 10.x o posterior de XCode para compilar la aplicación. Puede descargar XCode desde el [sitio web de iTunes](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "URL de descarga de XCode").
- Biblioteca de autenticación de Microsoft ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Puede usar un administrador de dependencias o agregar la biblioteca manualmente. Las instrucciones siguientes muestran cómo hacerlo.

En este tutorial, vamos a crear un nuevo proyecto. Si desea descargar el tutorial completado en su lugar, descargue el código:
- [Código de ejemplo de iOS](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)
- [Código de ejemplo de macOS](https://github.com/Azure-Samples/active-directory-macOS-swift-native-v2/archive/master.zip)

## <a name="create-a-new-project"></a>Creación de un nuevo proyecto

1. Abra Xcode y seleccione **Create a new Xcode project** (Crear un nuevo proyecto en Xcode).
2. En el caso de las aplicaciones para iOS, seleccione **iOS** > **Aplicación de vista única** y seleccione **Siguiente**.
3. En el caso de las aplicaciones para macOS, seleccione **macOS** > **Aplicación de Cocoa** y seleccione **Siguiente**.
4. Especifique un nombre de producto.
5. En **Lenguaje**, seleccione **Swift** y seleccione **Siguiente**.
6. Seleccione una carpeta para crear la aplicación y haga clic en **Crear**.

## <a name="register-your-application"></a>Registrar su aplicación

1. Vaya a [Azure Portal](https://aka.ms/MobileAppReg).
2. Abra la [hoja de Registros de aplicaciones](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) y haga clic en **+Nuevo registro**.
3. Escriba un **nombre** para ka aplicación y, después, sin configurar un URI de redirección, haga clic en **Registrar**.
4. En la sección **Administrar** del panel que aparece, seleccione **Autenticación**.

5. Haga clic en **Probar la nueva experiencia**  cerca de la parte superior de la pantalla para abrir el registro de aplicaciones y, después, haga clic en **+Nuevo registro** >  **+ Agregar una plataforma** > **iOS**.
    - Escriba el identificador de agrupación del proyecto. Si descargó el código, es `com.microsoft.identitysample.MSALiOS`. Si va a crear su propio proyecto, seleccione el proyecto en Xcode y abra la pestaña **General**. El identificador de la agrupación aparece en la sección **Identidad**.
    - Tenga en cuenta que para macOS también debe usar la experiencia de iOS. 
6. Haga clic en `Configure` y guarde el valor de **Configuración de MSAL**  que aparece en la página **Configuración de iOS** para que pueda escribirlo cuando configure la aplicación más tarde.  Haga clic en **Done**(Listo).

## <a name="add-msal"></a>Adición de MSAL

Elija una de las siguientes formas de instalar la biblioteca de MSAL en su aplicación:

### <a name="cocoapods"></a>CocoaPods

1. Si usa [CocoaPods](https://cocoapods.org/), instale `MSAL`, para lo que primero debe crear un archivo vacío llamado `podfile` en la misma carpeta que el archivo `.xcodeproj` del proyecto. Agregue lo siguiente a `podfile`:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL'
   end
   ```

2. Reemplace `<your-target-here>` por el nombre del proyecto.
3. En una ventana de terminal, vaya a la carpeta que contiene el archivo `podfile` que ha creado y ejecute `pod install` para instalar la biblioteca MSAL.
4. Cierre Xcode y abra `<your project name>.xcworkspace` para volver a cargar el proyecto en Xcode.

### <a name="carthage"></a>Carthage

Si usa [Carthage](https://github.com/Carthage/Carthage), instale `MSAL`, para lo que debe agregarlo a `Cartfile`:

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

En una ventana de terminal, en el mismo directorio que el archivo `Cartfile` actualizado, ejecute el siguiente comando para que Carthage actualice las dependencias del proyecto.

iOS:

```bash
carthage update --platform iOS
```

macOS:

```bash
carthage update --platform macOS
```

### <a name="manually"></a>Manualmente

También puede usar un submódulo de Git o consultar la versión más reciente y usarla como marco en su aplicación.

## <a name="add-your-app-registration"></a>Adición del registro de aplicaciones

A continuación, agregaremos el registro de aplicaciones al código. 

En primer lugar, agregue la siguiente instrucciones de importación al principio de los archivos `ViewController.swift` y `AppDelegate.swift`:

```swift
import MSAL
```

Después agregue el siguiente código a `ViewController.swift`, antes de `viewDidLoad()`:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/" // the Microsoft Graph endpoint
let kScopes: [String] = ["https://graph.microsoft.com/user.read"] // request permission to read the profile of the signed-in user
let kAuthority = "https://login.microsoftonline.com/common" // this authority allows a personal Microsoft account and a work or school account in any organization’s Azure AD tenant to sign in
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
var webViewParameters : MSALWebviewParameters?
```

El único valor que debe modificar arriba es el asignado a `kClientID` como [Identificador de aplicación](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#application-id-client-id). Este valor forma parte de los datos de Configuración de MSAL que guardó al principio de este tutorial para registrar la aplicación en Azure Portal.

## <a name="for-ios-only-configure-url-schemes"></a>Solo para iOS, configure los esquemas de dirección URL.

En este paso, registrará `CFBundleURLSchemes` para que se pueda redirigir al usuario a la aplicación después de iniciar sesión. Por cierto, `LSApplicationQueriesSchemes` también permite a la aplicación hacer uso de Microsoft Authenticator.

En Xcode, abra `Info.plist` como un archivo de código fuente y agregue lo siguiente en la sección `<dict>`. Reemplace `[BUNDLE_ID]` por el valor que usó en Azure Portal que, si descargó el código, es `com.microsoft.identitysample.MSALiOS`. Si va a crear su propio proyecto, seleccione el proyecto en Xcode y abra la pestaña **General**. El identificador de la agrupación aparece en la sección **Identidad**.

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauthv2</string>
    <string>msauthv3</string>
</array>
```

## <a name="for-macos-only-configure-app-sandbox"></a>Solo para macOS, configure App Sandbox.

1. Vaya a la configuración del proyecto de Xcode > **pestaña Funcionalidad** > **App Sandbox**
2. Active la casilla **Conexiones salientes (cliente)** . 

## <a name="create-your-apps-ui"></a>Creación de la interfaz de usuario de la aplicación

Ahora cree una interfaz de usuario que incluya un botón para llamar a Microsoft Graph API, otro para cerrar la sesión y una vista de texto para parte de la salida agregando el siguiente código a la clase `ViewController`:

### <a name="ios-ui"></a>Interfaz de usuario de iOS

```swift
var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = UIButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
        callGraphButton.setTitleColor(.blue, for: .normal)
        callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
        callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
        // Add sign out button
        signOutButton = UIButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.setTitle("Sign Out", for: .normal)
        signOutButton.setTitleColor(.blue, for: .normal)
        signOutButton.setTitleColor(.gray, for: .disabled)
        signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = UITextView()
        loggingText.isUserInteractionEnabled = false
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: 10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
    }
```

### <a name="macos-ui"></a>Interfaz de usuario de macOS

```swift

var callGraphButton: NSButton!
var loggingText: NSTextView!
var signOutButton: NSButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = NSButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.title = "Call Microsoft Graph API"
        callGraphButton.target = self
        callGraphButton.action = #selector(callGraphAPI(_:))
        callGraphButton.bezelStyle = .rounded
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 30.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 34.0).isActive = true
        
        // Add sign out button
        signOutButton = NSButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.title = "Sign Out"
        signOutButton.target = self
        signOutButton.action = #selector(signOut(_:))
        signOutButton.bezelStyle = .texturedRounded
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 34.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = NSTextView()
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: -10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: -10.0).isActive = true
        loggingText.widthAnchor.constraint(equalToConstant: 500.0).isActive = true
        loggingText.heightAnchor.constraint(equalToConstant: 300.0).isActive = true
    }
```

A continuación, también en la clase `ViewController`, reemplace el método `viewDidLoad()` por:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.updateLogging(text: "Unable to create Application Context \(error)")
        }
    }
```

## <a name="use-msal"></a>Uso de MSAL

### <a name="initialize-msal"></a>Inicializar MSAL

Agregue el siguiente método `initMSAL` a la clase `ViewController`:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.updateLogging(text: "Unable to create authority URL")
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
        self.initWebViewParams()
    }
```

Agregue lo siguiente después del método `initMSAL` a la clase `ViewController`.

### <a name="ios-code"></a>Código para iOS:

```swift
func initWebViewParams() {
        self.webViewParameters = MSALWebviewParameters(parentViewController: self)
    }
```

### <a name="macos-code"></a>Código para macOS:

```swift
func initWebViewParams() {
        self.webViewParameters = MSALWebviewParameters()
        self.webViewParameters?.webviewType = .wkWebView
    }
```

### <a name="for-ios-only-handle-the-sign-in-callback"></a>Solo para iOS, controle la devolución de llamada del inicio de sesión

Abra el archivo `AppDelegate.swift` . Para controlar la devolución de llamada después de iniciar sesión, agregue `MSALPublicClientApplication.handleMSALResponse` a la clase `appDelegate` de la siguiente forma:

```swift
// Inside AppDelegate...
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
}

```

**Si usa Xcode 11**, debe colocar la devolución de llamada de MSAL en `SceneDelegate.swift` en su lugar.
Si admite UISceneDelegate y UIApplicationDelegate para la compatibilidad con sistemas operativos iOS anteriores, la devolución de llamada de MSAL debe colocarse en ambos archivos.

```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        
        guard let urlContext = URLContexts.first else {
            return
        }
        
        let url = urlContext.url
        let sourceApp = urlContext.options.sourceApplication
        
        MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApp)
    }
```

#### <a name="acquire-tokens"></a>Adquisición de tokens

Ahora, podemos implementar la lógica de procesamiento de la interfaz de usuario de la aplicación y obtener los tokens interactivamente a través de MSAL.

MSAL expone dos métodos principales para obtener tokens: `acquireTokenSilently()` y `acquireTokenInteractively()`: 

- `acquireTokenSilently()` intenta iniciar la sesión de un usuario y obtener tokens sin interacción del usuario, siempre que haya una cuenta.

- `acquireTokenInteractively()` siempre muestra la interfaz de usuario al intentar iniciar la sesión del usuario. Puede utilizar las cookies de sesión en el explorador o una cuenta de Microsoft Authenticator para proporcionar una experiencia de inicio de sesión único interactivo.

Agregue el siguiente código a la clase `ViewController`:

```swift
    @objc func callGraphAPI(_ sender: AnyObject) {
        
        guard let currentAccount = self.currentAccount() else {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            acquireTokenInteractively()
            return
        }
        
        acquireTokenSilently(currentAccount)
    }

    func currentAccount() -> MSALAccount? {
        
        guard let applicationContext = self.applicationContext else { return nil }
        
        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        
        do {
            let cachedAccounts = try applicationContext.allAccounts()
            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }
        } catch let error as NSError {
            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }
        
        return nil
    }
```

#### <a name="get-a-token-interactively"></a>Obtención de un token interactivamente

El código siguiente obtiene un token por primera vez mediante la creación de un objeto `MSALInteractiveTokenParameters` y la llamada a `acquireToken`. A continuación, agregará código que:

1. Crea `MSALInteractiveTokenParameters` con ámbitos.
2. Llama a `acquireToken()` con los parámetros creados.
3. Controla los errores. Para más información, consulte [la guía de control de errores de MSAL para iOS y macOS](msal-handling-exceptions.md).
4. Controla el caso de éxito.

Agregue el siguiente código a la clase `ViewController` .

```swift
func acquireTokenInteractively() {
        
    guard let applicationContext = self.applicationContext else { return }
    guard let webViewParameters = self.webViewParameters else { return }
        
    // #1
    let parameters = MSALInteractiveTokenParameters(scopes: kScopes, webviewParameters: webViewParameters)
        
    // #2
    applicationContext.acquireToken(with: parameters) { (result, error) in
            
        // #3
        if let error = error {
                
            self.updateLogging(text: "Could not acquire token: \(error)")
            return
        }
            
        guard let result = result else {
                
            self.updateLogging(text: "Could not acquire token: No result returned")
            return
        }
            
        // #4
        self.accessToken = result.accessToken
        self.updateLogging(text: "Access token is \(self.accessToken)")
        self.updateSignOutButton(enabled: true)
        self.getContentWithToken()
    }
}    
```


#### <a name="get-a-token-silently"></a>Obtención de un token en silencio

Para adquirir un token actualizado en modo silencioso, agregue el código siguiente a la clase`ViewController`. Crea un objeto `MSALSilentTokenParameters` y llama a `acquireTokenSilent()`:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        guard let applicationContext = self.applicationContext else { return }
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in    
            if let error = error {
                let nsError = error as NSError
                if (nsError.domain == MSALErrorDomain) {
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>Llamar a Microsoft Graph API 

Una vez que tenga un token, la aplicación puede usarlo este valor en el encabezado de HTTP para realizar una solicitud autorizada a Microsoft Graph:

| clave del encabezado    | value                 |
| ------------- | --------------------- |
| Authorization | Portador \<token_de_acceso> |

Agregue el siguiente código a la clase `ViewController`:

```swift
    func getContentWithToken() {        
        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
               
        URLSession.shared.dataTask(with: request) { data, response, error in
               
        if let error = error {
            self.updateLogging(text: "Couldn't get graph result: \(error)")
            return
        }
               
        guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
               
        self.updateLogging(text: "Couldn't deserialize result JSON")
            return
        }
               
        self.updateLogging(text: "Result from Graph: \(result))")
        
        }.resume()
    }
```

Consulte [Microsoft Graph API](https://graph.microsoft.com) para más información acerca de Microsoft Graph API.

### <a name="use-msal-for-sign-out"></a>Uso de MSAL para cierre de sesión

A continuación, agregue compatibilidad para el cierre de sesión.

> [!Important]
> Si cierra sesión con MSAL, se elimina de esta aplicación toda la información conocida sobre un usuario, pero el usuario seguirá teniendo una sesión activa en su dispositivo. Si el usuario intenta iniciar sesión nuevamente, puede ver la interfaz de usuario de inicio de sesión, pero es posible que no tenga que volver a escribir las credenciales debido a que la sesión del dispositivo sigue activa.

Para agregar la funcionalidad de cierre de sesión, agregue el siguiente código a la clase `ViewController`. Este método recorre en iteración todas las cuentas y las quita:

```swift 
@objc func signOut(_ sender: AnyObject) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            try applicationContext.remove(account)
            self.updateLogging(text: "")
            self.updateSignOutButton(enabled: false)
            self.accessToken = ""
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>Habilitación del almacenamiento en caché de tokens

De forma predeterminada, MSAL almacena en caché los tokens de la aplicación en el llavero de iOS o macOS. 

Para habilitar el almacenamiento en caché de tokens:
1. Asegúrese de que la aplicación está firmada correctamente.
2. Vaya a la configuración del proyecto de Xcode > **Pestaña Capabilities** (Funcionalidades) > **Enable Keychain Sharing** (Habilitar uso compartido de la cadena de claves)
3. Haga clic en **+** y, en **Keychain Groups** (Grupos de llaveros), escriba el valor siguiente: 3.a Para iOS, escriba `com.microsoft.adalcache` 3.b Para macOS, escriba `com.microsoft.identity.universalstorage`.

### <a name="add-helper-methods"></a>Adición de métodos auxiliares
Agregue los siguientes métodos auxiliares a la clase `ViewController` para completar el ejemplo.

### <a name="ios-ui"></a>Interfaz de usuario de iOS:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
```

### <a name="macos-ui"></a>Interfaz de usuario de macOS:

```swift
func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.string = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.string = text
            }
        }
    }
```



### <a name="multi-account-applications"></a>Aplicaciones de varias cuentas

Esta aplicación se ha creado para un escenario de una única cuenta. MSAL también admite escenarios de varias cuentas, pero requiere algún trabajo adicional por parte de las aplicaciones. Deberá crear una interfaz de usuario para ayudar a los usuarios a seleccionar qué cuenta desean usar para cada acción que requiera tokens. Como alternativa, la aplicación puede implementar un método heurístico para seleccionar qué cuenta se usará mediante el método `getAccounts()`.

## <a name="test-your-app"></a>Prueba de la aplicación

### <a name="run-locally"></a>Ejecución en modo local

Cree e implemente la aplicación en un emulador o dispositivo de prueba. Debe ser capaz de iniciar sesión y obtener tokens para Azure AD o cuentas personales de Microsoft.

La primera vez que un usuario inicie sesión en la aplicación, Microsoft Identity le pedirá que acepte los permisos solicitados.  Aunque la mayoría de los usuarios pueden dar su consentimiento, algunos inquilinos de Azure AD tienen el consentimiento del usuario deshabilitado, lo que requiere que los administradores den su consentimiento en nombre de todos los usuarios. Para admitir este escenario, registre los ámbitos de la aplicación en Azure Portal.

Después de iniciar sesión, esta aplicación mostrará los datos devueltos desde el punto de conexión `/me` de Microsoft Graph.

## <a name="get-help"></a>Obtener ayuda

Visite [Ayuda y soporte técnico](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) si tienes algún problema con este tutorial o con la plataforma de identidad de Microsoft.

Ayúdenos a mejorar la Plataforma de identidad de Microsoft. Rellene una breve encuesta de dos preguntas y háganos saber su opinión.

> [!div class="nextstepaction"]
> [Encuesta sobre la Plataforma de identidad de Microsoft](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
