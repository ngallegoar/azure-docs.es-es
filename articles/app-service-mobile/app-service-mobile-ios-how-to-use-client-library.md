---
title: Uso del SDK de iOS
description: Uso del SDK de iOS para Aplicaciones móviles de Azure
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 1bf8f8e198f6c4a4a0af308262cd830685698a80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226508"
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Uso de la biblioteca de cliente de iOS para Aplicaciones móviles de Azure

[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Información general
En esta guía se muestra cómo realizar algunas tareas comunes a través del [SDK de iOS de Azure Mobile Apps][1]. Si está familiarizado con Aplicaciones móviles de Azure, complete primero [Creación de una aplicación de Apache Cordova] para crear un back-end, crear una tabla y descargar un proyecto de Xcode para iOS pregenerada. En esta guía, nos centramos en el SDK de iOS de cliente. Para obtener más información sobre el SDK de servidor para el back-end, consulte los procedimientos del SDK de servidor.

## <a name="reference-documentation"></a>Documentación de referencia

La documentación de referencia para el SDK de cliente iOS se encuentra aquí: [Referencia de cliente de iOS de Azure Mobile Apps][2].

## <a name="supported-platforms"></a>Plataformas compatibles

El SDK de iOS admite proyectos de Objective-C, Swift 2.2 y Swift 2.3 para iOS 8.0 o posterior.

La autenticación de flujo de servidor utiliza una vista web para la interfaz de usuario presentada.  Si el dispositivo no puede presentar una interfaz de usuario de vista web, hay que utilizar otros métodos de autenticación que están fuera del ámbito del producto.  
Por tanto, este SDK no es adecuado para dispositivos de tipo reloj o con restricciones similares.

## <a name="setup-and-prerequisites"></a><a name="Setup"></a>Configuración y requisitos previos

En esta guía se asume que ha creado un back-end con una tabla. En esta guía se asume que la tabla tiene el mismo esquema que las tablas de dichos tutoriales. En esta guía también se supone que en el código se hace referencia a `MicrosoftAzureMobile.framework` e importa `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

## <a name="how-to-create-client"></a><a name="create-client"></a>Procedimientos: Creación del cliente

Para obtener acceso al back-end de Aplicaciones móviles de Azure en el proyecto, cree un `MSClient`. Reemplace `AppUrl` por la dirección URL de la aplicación. Puede dejar `gatewayURLString` y `applicationKey` vacías. Si configura una puerta de enlace para la autenticación, rellene `gatewayURLString` con la dirección URL de la puerta de enlace.

**Objective-C**:

```objc
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**SWIFT**:

```swift
let client = MSClient(applicationURLString: "AppUrl")
```

## <a name="how-to-create-table-reference"></a><a name="table-reference"></a>Procedimientos: Creación de una referencia de tabla

Para acceder a los datos o actualizarlos, cree una referencia a la tabla de back-end. Reemplace `TodoItem` por el nombre de la tabla.

**Objective-C**:

```objc
MSTable *table = [client tableWithName:@"TodoItem"];
```

**SWIFT**:

```swift
let table = client.tableWithName("TodoItem")
```

## <a name="how-to-query-data"></a><a name="querying"></a>Procedimientos: Consultar datos

Para crear una consulta de base de datos, consulte el objeto `MSTable` . La consulta siguiente obtiene todos los elementos de `TodoItem` y registra el texto de cada elemento.

**Objective-C**:

```objc
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occurred
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```swift
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="how-to-filter-returned-data"></a><a name="filtering"></a>Procedimientos: Filtro de datos devueltos

Para filtrar los resultados, hay muchas opciones disponibles.

Para filtrar mediante un predicado, use un `NSPredicate` y `readWithPredicate`. Los siguientes filtros devolvieron datos para buscar solo los elementos Todo incompletos.

**Objective-C**:

```objc
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```swift
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="how-to-use-msquery"></a><a name="query-object"></a>Procedimientos: Uso de MSQuery

Para realizar una consulta compleja (como de ordenación y paginación), cree un objeto `MSQuery` directamente o mediante un predicado:

**Objective-C**:

```objc
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**SWIFT**:

```swift
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery` permite controlar varios comportamientos de consulta.

* Especificar el orden de los resultados
* Limitar qué campos se devuelven
* Limitar el número de registros que se devolverán
* Especificar el recuento total en la respuesta
* Especificar parámetros de la cadena de consulta personalizados en la solicitud
* Aplicar funciones adicionales

Ejecutar una consulta `MSQuery` llamando a `readWithCompletion` en el objeto.

## <a name="how-to-sort-data-with-msquery"></a><a name="sorting"></a>Procedimientos: Ordenación de datos con MSQuery

Para ordenar los resultados, echemos un vistazo a un ejemplo. Para ordenar por orden ascendente el campo text y, luego, por orden descendente el campo complete, invoque `MSQuery` de la siguiente manera:

**Objective-C**:

```objc
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```swift
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="how-to-limit-fields-and-expand-query-string-parameters-with-msquery"></a><a name="selecting"></a><a name="parameters"></a>Instrucciones: Limitación de campos y expansión de los parámetros de cadena de consulta con MSQuery

Para limitar los campos que se devolverán en una consulta, especifique los nombres de los campos en la propiedad **selectFields** . En este ejemplo solamente se devuelven los campos de texto y aquellos que se hayan rellenado:

**Objective-C**:

```objc
query.selectFields = @[@"text", @"complete"];
```

**SWIFT**:

```swift
query.selectFields = ["text", "complete"]
```

Para incluir parámetros de cadena de consulta adicionales en la solicitud al servidor (por ejemplo, porque los utiliza un script de servidor personalizado), introduzca `query.parameters` :

**Objective-C**:

```objc
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**SWIFT**:

```swift
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="how-to-configure-page-size"></a><a name="paging"></a>Procedimientos: Configuración del tamaño de página

Con Azure Mobile Apps, el tamaño de página controla el número de registros que se extraen de las tablas de back-end al mismo tiempo. Luego, una llamada a los datos de `pull` enviaría dichos datos en lotes, basándose en este tamaño de página, hasta que no haya más registros para extraer.

Es posible configurar un tamaño de página con **MSPullSettings** tal como se muestra a continuación. El tamaño de página predeterminado es 50 y en el ejemplo siguiente se cambia a 3.

Para mejorar el rendimiento es posible configurar otro tamaño de página. Si tiene un gran número de registros de datos pequeños, un tamaño de página elevado reduce el número de idas y vueltas del servidor.

Este valor controla el tamaño de página en el cliente. Si el cliente pide un tamaño de página mayor que el que admite el back-end de Mobile Apps, se limita al máximo que el back-end está configurado para admitir.

Este valor es también el *número* de registros de datos, no el *tamaño en bytes*.

Si aumenta el tamaño de página del cliente, también debe aumentar el tamaño de página en el servidor. Consulte ["Instrucciones: Cómo ajustar el tamaño de paginación de la tabla"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) para saber qué pasos debe dar para hacerlo.

**Objective-C**:

```objc
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```

**SWIFT**:

```swift
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="how-to-insert-data"></a><a name="inserting"></a>Procedimientos: Inserción de datos

Para insertar una nueva fila en la tabla, cree un elemento `NSDictionary` e invoque `table insert`. Si el [esquema dinámico] está habilitado, el back-end móvil de Azure App Service genera automáticamente columnas nuevas basadas en `NSDictionary`.

Si no se proporciona `id` , el backend genera automáticamente un nuevo identificador único. Proporcione su propio `id` para utilizar direcciones de correo electrónico, nombres de usuario o sus propios valores personalizados como Id. Proporcionar su propio ID puede facilitar las combinaciones y la lógica de la base de datos de tipo empresarial.

`result` contiene el nuevo elemento insertado. En función de la lógica del servidor, puede tener datos modificados o adicionales en comparación con lo que se pasó al servidor.

**Objective-C**:

```objc
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```swift
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="how-to-modify-data"></a><a name="modifying"></a>Procedimientos: Modificación de datos

Para actualizar una fila existente, modifique un elemento y llame a `update`:

**Objective-C**:

```objc
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```swift
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Como alternativa, proporcione el identificador de fila y el campo actualizado:

**Objective-C**:

```objc
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```swift
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Como mínimo, debe establecerse el atributo `id` al realizar actualizaciones.

## <a name="how-to-delete-data"></a><a name="deleting"></a>Procedimientos: Eliminación de datos

Para eliminar un elemento, invoque `delete` con el elemento:

**Objective-C**:

```objc
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```swift
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

O bien elimínelo proporcionando un identificador de fila:

**Objective-C**:

```objc
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```swift
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Como mínimo, el atributo `id` debe establecerse a la hora de efectuar eliminaciones.

## <a name="how-to-call-custom-api"></a><a name="customapi"></a>Procedimientos: Llamada a una API personalizada

Con una API personalizada, puede exponer cualquier funcionalidad de back-end. No necesita asignar a una operación de tabla. No solo obtendrá más control sobre la mensajería, también podrá leer o establecer encabezados y cambiar el formato del cuerpo de la respuesta.

Para invocar a una API personalizada, llame a `MSClient.invokeAPI`. El contenido de la solicitud y la respuesta se tratan como JSON. Para usar otros tipos de soportes físicos, [use la otra sobrecarga de `invokeAPI`][5].  Para realizar una solicitud `GET` en lugar de una solicitud `POST`, establezca el parámetro `HTTPMethod` en `"GET"` y el parámetro `body` en `nil` (puesto que las solicitudes GET no tienen cuerpos de mensaje). Si la API personalizada es compatible con otros verbos HTTP, cambie `HTTPMethod` de acuerdo a ello.

**Objective-C**:

```objc
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**SWIFT**:

```swift
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <a name="how-to-register-push-templates-to-send-cross-platform-notifications"></a><a name="templates"></a>Procedimientos: Registro de plantillas push para enviar notificaciones entre plataformas

Para registrar plantillas, pase las plantillas con el método **client.push registerDeviceToken** en la aplicación cliente.

**Objective-C**:

```objc
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**SWIFT**:

```swift
client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
    if let err = error {
        print("ERROR ", err)
    }
})
```

Las plantillas serán de tipo NSDictionary y pueden contener varias plantillas con el formato siguiente:

**Objective-C**:

```objc
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**SWIFT**:

```swift
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Por seguridad, se eliminan todas las etiquetas de la solicitud.  Para agregar etiquetas a las instalaciones o plantillas dentro de las instalaciones, vea [Trabajar con el SDK del servidor back-end de .NET para Azure Mobile Apps][4].  Para enviar notificaciones mediante estas plantillas registradas, trabaje con las [API de Notification Hubs][3].

## <a name="how-to-handle-errors"></a><a name="errors"></a>Procedimientos: Gestión de errores

Al realizar una llamada a un back-end móvil de Azure App Service, el bloque de finalización contiene un parámetro `NSError` . En caso de producirse un error, este parámetro no será nulo. En su código, debe marcar este parámetro y administrar el error según sea necesario, como se muestra en los fragmentos de código anteriores.

El archivo [`<WindowsAzureMobileServices/MSError.h>`][6] define las constantes `MSErrorResponseKey`, `MSErrorRequestKey` y `MSErrorServerItemKey`. Para obtener más datos relacionados con el error:

**Objective-C**:

```objc
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**SWIFT**:

```swift
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Además, el archivo define constantes para cada código de error:

**Objective-C**:

```objc
if (error.code == MSErrorPreconditionFailed) {
```

**SWIFT**:

```swift
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="how-to-authenticate-users-with-the-active-directory-authentication-library"></a><a name="adal"></a>Procedimientos: Autenticación de usuarios con la biblioteca de autenticación de Active Directory

Puede utilizar la biblioteca de autenticación de Active Directory (ADAL) para iniciar la sesión de los usuarios en su aplicación con Azure Active Directory. Se prefiere la autenticación de flujo de cliente mediante un SDK de proveedor de identidades al uso del método `loginWithProvider:completion:` .  Este tipo de autenticación proporciona una experiencia de usuario más nativa y permite realizar más personalizaciones.

1. Configure su back-end de aplicación móvil para el inicio de sesión en AAD siguiendo el tutorial [Configuración de la aplicación de App Service para usar el inicio de sesión de Azure Active Directory][7] . Asegúrese de completar el paso opcional de registrar una aplicación cliente nativa. Para iOS, se recomienda que el URI de redireccionamiento tenga el formato `<app-scheme>://<bundle-id>`. Para más información, consulte la [guía de inicio rápido de iOS para ADAL][8].
2. Instale ADAL mediante Cocoapods. Edite el podfile para incluir la siguiente definición; sustituya **YOUR-PROJECT** por el nombre de su proyecto de Xcode:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   y el POD:

        pod 'ADALiOS'

3. Mediante el Terminal, ejecute `pod install` desde el directorio que contiene el proyecto y abra el área de trabajo del Xcode generado (no el proyecto).
4. Agregue el siguiente código a la aplicación, según el lenguaje que esté utilizando. En cada uno, realice estas sustituciones:

   * Reemplace **INSERT-AUTHORITY-HERE** por el nombre del inquilino en el que aprovisionó la aplicación. El formato debe ser https://login.microsoftonline.com/contoso.onmicrosoft.com. Este valor se puede copiar de la pestaña Dominio de Azure Active Directory en [Azure Portal].
   * Reemplace **INSERT-RESOURCE-ID-HERE** por el Id. de cliente del back-end de la aplicación móvil. El Id. de cliente en la pestaña **Opciones avanzadas** de **Configuración de Azure Active Directory** en el portal.
   * Reemplace **INSERT-CLIENT-ID-HERE** por el Id. de cliente que copió de la aplicación cliente nativa.
   * Reemplace **INSERT-REDIRECT-URI-HERE** por el punto de conexión */.auth/login/done* del sitio, mediante el esquema HTTPS. El valor debería parecerse al siguiente: *https://contoso.azurewebsites.net/.auth/login/done* .

**Objective-C**:

```objc
#import <ADALiOS/ADAuthenticationContext.h>
#import <ADALiOS/ADAuthenticationSettings.h>
// ...
- (void) authenticate:(UIViewController*) parent
            completion:(void (^) (MSUser*, NSError*))completionBlock;
{
    NSString *authority = @"INSERT-AUTHORITY-HERE";
    NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
    NSString *clientId = @"INSERT-CLIENT-ID-HERE";
    NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
    ADAuthenticationError *error;
    ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
    authContext.parentController = parent;
    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:resourceId
                                    clientId:clientId
                                redirectUri:redirectUri
                            completionBlock:^(ADAuthenticationResult *result) {
                                if (result.status != AD_SUCCEEDED)
                                {
                                    completionBlock(nil, result.error);;
                                }
                                else
                                {
                                    NSDictionary *payload = @{
                                                            @"access_token" : result.tokenCacheStoreItem.accessToken
                                                            };
                                    [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                }
                            }];
}
```

**SWIFT**:

```swift
// add the following imports to your bridging header:
//        #import <ADALiOS/ADAuthenticationContext.h>
//        #import <ADALiOS/ADAuthenticationSettings.h>

func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
    let authority = "INSERT-AUTHORITY-HERE"
    let resourceId = "INSERT-RESOURCE-ID-HERE"
    let clientId = "INSERT-CLIENT-ID-HERE"
    let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
    var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
    let authContext = ADAuthenticationContext(authority: authority, error: error)
    authContext.parentController = parent
    ADAuthenticationSettings.sharedInstance().enableFullScreen = true
    authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
            if result.status != AD_SUCCEEDED {
                completion(nil, result.error)
            }
            else {
                let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                client.loginWithProvider("aad", token: payload, completion: completion)
            }
        }
}
```

## <a name="how-to-authenticate-users-with-the-facebook-sdk-for-ios"></a><a name="facebook-sdk"></a>Procedimientos: Autenticación de usuarios con SDK de Facebook para iOS

Puede usar el SDK de Facebook para iOS para que los usuarios inicien sesión en su aplicación con Facebook.  Es preferible usar la autenticación de flujo de cliente al método `loginWithProvider:completion:` .  Este tipo de autenticación proporciona una experiencia de usuario más nativa y permite realizar más personalizaciones.

1. Configure el back-end de aplicación móvil para el inicio de sesión en Facebook siguiendo el tutorial [Configuración de la aplicación de App Service para usar el inicio de sesión de Facebook][9].
2. Instale el SDK de Facebook para iOS siguiendo la documentación [SDK de Facebook para iOS: primeros pasos][10]. En lugar de crear una aplicación, puede agregar la plataforma iOS en el registro existente.
3. La documentación de Facebook incluye algún código de Objective-C en el delegado de la aplicación. Si utiliza **Swift**, puede usar las siguientes traducciones para AppDelegate.swift:

    ```swift
    // Add the following import to your bridging header:
    //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
        // Add any custom logic here.
        return true
    }

    func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
        let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
        // Add any custom logic here.
        return handled
    }
    ```
4. Además de agregar `FBSDKCoreKit.framework` al proyecto, también puede agregar una referencia a `FBSDKLoginKit.framework` de la misma manera.
5. Agregue el siguiente código a la aplicación, según el lenguaje que esté utilizando.

    **Objective-C**:

    ```objc
    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
                completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
            logInWithReadPermissions: @[@"public_profile"]
            fromViewController:parent
            handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
                if (error) {
                    completionBlock(nil, error);
                } else if (result.isCancelled) {
                    completionBlock(nil, error);
                } else {
                    NSDictionary *payload = @{
                                            @"access_token":result.token.tokenString
                                            };
                    [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
                }
            }];
    }
    ```

    **SWIFT**:

    ```swift
    // Add the following imports to your bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }
    ```

## <a name="how-to-authenticate-users-with-twitter-fabric-for-ios"></a><a name="twitter-fabric"></a>Procedimientos: Autenticación de usuarios con Fabric de Twitter para iOS

Puede usar Fabric para iOS para que los usuarios inicien sesión en su aplicación con Twitter. La autenticación de flujo de cliente es preferible al uso del método `loginWithProvider:completion:` , ya que proporciona una experiencia de usuario más nativa y permite realizar más personalizaciones.

1. Configure su back-end de aplicación móvil para el inicio de sesión en Twitter siguiendo el tutorial [Configuración de la aplicación Servicio de aplicaciones para usar el inicio de sesión de Twitter](../app-service/configure-authentication-provider-twitter.md) .
2. Agregue Fabric al proyecto siguiendo el documento [Fabric for iOS - Getting Started] y configurando TwitterKit.

   > [!NOTE]
   > De forma predeterminada, Fabric creará automáticamente una aplicación de Twitter. Puede evitar que se cree registrando la clave de usuario y el secreto de consumidor que creó anteriormente mediante los fragmentos de código siguientes.    Asimismo, puede reemplazar los valores de clave de usuario y de secreto de consumidor que proporcione al Servicio de aplicaciones por los valores que aparecen en [Fabric Dashboard]. Si elige esta opción, asegúrese de establecer la dirección URL de devolución de llamada en un valor de marcador de posición, como `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Si decide utilizar los secretos que creó anteriormente, agregue el siguiente código al delegado de la aplicación:

    **Objective-C**:

    ```objc
    #import <Fabric/Fabric.h>
    #import <TwitterKit/TwitterKit.h>
    // ...
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
        [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
        [Fabric with:@[[Twitter class]]];
        // Add any custom logic here.
        return YES;
    }
    ```

    **SWIFT**:

    ```swift
    import Fabric
    import TwitterKit
    // ...
    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
        Fabric.with([Twitter.self])
        // Add any custom logic here.
        return true
    }
    ```

3. Agregue el siguiente código a la aplicación, según el lenguaje que esté utilizando.

    **Objective-C**:

    ```objc
    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }
    ```

    **SWIFT**:

    ```swift
    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }
    ```

## <a name="how-to-authenticate-users-with-the-google-sign-in-sdk-for-ios"></a><a name="google-sdk"></a>Procedimientos: Autenticación de usuarios con el SDK de inicio de sesión de Google para iOS

Puede usar el SDK de inicio de sesión de Google para iOS para que los usuarios inicien sesión en su aplicación con una cuenta de Google.  Google anunció recientemente cambios en sus directivas de seguridad de OAuth.  Estos cambios obligarán a usar el SDK de Google en el futuro.

1. Configure su back-end de aplicación móvil para el inicio de sesión en Google siguiendo el tutorial [Configuración de la aplicación Servicio de aplicaciones para usar el inicio de sesión de Google](../app-service/configure-authentication-provider-google.md) .
2. Instale el SDK de Google para iOS siguiendo la documentación de [Inicio de sesión de Google para iOS: Empiece a integrar](https://developers.google.com/identity/sign-in/ios/start-integrating). Puede omitir la sección Authenticate with a Backend Server (Autenticar con un servidor back-end).
3. Agregue el siguiente código al método `signIn:didSignInForUser:withError:` del delegado según el lenguaje que esté utilizando.

    **Objective-C**:
    ```objc
    NSDictionary *payload = @{
                                @"id_token":user.authentication.idToken,
                                @"authorization_code":user.serverAuthCode
                                };

    [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
        // ...
    }];
    ```

    **SWIFT**:

    ```swift
    let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
    client.loginWithProvider("google", token: payload) { (user, error) in
        // ...
    }
    ```

4. Asegúrese de agregar también lo siguiente a `application:didFinishLaunchingWithOptions:` en el delegado de la aplicación, reemplazando "SERVER_CLIENT_ID" por el mismo identificador que usó para configurar el Servicio de aplicaciones en el paso 1.

    **Objective-C**:

    ```objc
    [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
    ```

     **SWIFT**:

    ```swift
    GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"
    ```

5. Agregue el siguiente código a la aplicación en un UIViewController que permita implementar el protocolo `GIDSignInUIDelegate` según el lenguaje que esté utilizando.  La sesión se cierra antes de volver a iniciar sesión y, aunque no es necesario que vuelva a escribir sus credenciales, verá un cuadro de diálogo de consentimiento.  Solo llame a este método si el token de sesión ha expirado.

   **Objective-C**:

    ```objc
    #import <Google/SignIn.h>
    // ...
    - (void)authenticate
    {
            [GIDSignIn sharedInstance].uiDelegate = self;
            [[GIDSignIn sharedInstance] signOut];
            [[GIDSignIn sharedInstance] signIn];
    }
    ```

   **SWIFT**:

    ```swift
    // ...
    func authenticate() {
        GIDSignIn.sharedInstance().uiDelegate = self
        GIDSignIn.sharedInstance().signOut()
        GIDSignIn.sharedInstance().signIn()
    }
    ```

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Creación de una aplicación de Apache Cordova]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode
[Azure Portal]: https://portal.azure.com/
[Handling Expired Tokens]: https://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: https://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: https://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Esquema dinámico]: https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/dynamic-schema
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: https://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: https://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: https://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Fabric Dashboard]: https://www.fabric.io/home
[Fabric for iOS - Getting Started]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: https://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: https://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: ../app-service/configure-authentication-provider-aad.md
[8]:../active-directory/develop/quickstart-v2-ios.md
[9]: ../app-service/configure-authentication-provider-facebook.md
[10]: https://developers.facebook.com/docs/ios/getting-started
