---
title: archivo de inclusión
description: archivo de inclusión
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: a7e543dcad9ad1b016d1244451cd87cda5ad7492
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67440208"
---
1. Descargue los inicios rápidos del SDK de cliente para estas plataformas:
    
    [iOS (Objective-C)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS)  
    [iOS (Swift)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS-Swift)  
    [Android (Java)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/android)  
    [Xamarin.iOS](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.iOS)  
    [Xamarin.Android](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.android)  
    [Xamarin.Forms](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.forms)  
    [Cordova](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/cordova)  
    [Windows (C#)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/windows-uwp-cs)  

    > [!NOTE]
    > Si usa el proyecto de iOS, debe descargar "azuresdk-iOS-\*.zip" de la [versión más reciente de GitHub](https://github.com/Azure/azure-mobile-apps-ios-client/releases/latest). Descomprima y agregue el archivo `MicrosoftAzureMobile.framework` a la raíz del proyecto.
    >

2. Tendrá que agregar una conexión de base de datos o conectarse a una conexión existente. Primero, determine si podrá crear un almacén de datos o usar uno existente.

    - **Crear un nuevo almacén de datos**: si va a crear un almacén de datos, eche un vistazo a esta guía de inicio rápido:

        [Inicio rápido: Introducción a las bases de datos únicas en Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-quickstart-guide)

    - **Origen de datos existente**: Siga estas instrucciones si quiere usar una conexión de base de datos existente

        1. Formato de cadena de conexión de SQL Database: `Data Source=tcp:{your_SQLServer},{port};Initial Catalog={your_catalogue};User ID={your_username};Password={your_password}`

           **{your_SQLServer}** : nombre del servidor; lo encontrará en la página de información general de la base de datos y suele tener el formato "nombre_de_servidor.database.windows.net".
            **{port}** : el puerto suele ser 1433.
            **{your_catalogue}** : nombre de la base de datos.
            **{your_username}** : nombre de usuario para acceder a la base de datos.
            **{your_password}** : contraseña para acceder a la base de datos.

            [Más información sobre el formato de cadena de conexión SQL](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)

        2. Agregue la cadena de conexión a su **aplicación móvil**. En App Service, puede administrar las cadenas de conexión para la aplicación mediante la opción **Configuración** del menú.

            Para agregar una cadena de conexión:

            1. Haga clic en la pestaña **Configuración de la aplicación**.

            2. Haga clic en **[+] Nueva cadena de conexión**.

            3. Deberá proporcionar valores para **Nombre**, **Valor** y **Tipo** para la cadena de conexión.

            4. En **Nombre**, escriba `MS_TableConnectionString`

            5. El valor debe ser la cadena de conexión que formó en el paso anterior.

            6. Si va a agregar una cadena de conexión a una base de datos de SQL Azure, elija **SQLAzure** en **tipo**.

3. Azure Mobile Apps tiene varios SDK para los back-ends de .NET y Node.js.

   - **Back-end de Node.js**
    
     Si va a usar la aplicación de inicio rápido de Node.js, siga estas instrucciones.

     1. En Azure Portal, vaya a **Tablas fáciles** y verá esta pantalla.
      
        ![Nodo de tablas fáciles](./media/app-service-mobile-configure-new-backend/node-easy-tables.png)

     2. Asegúrese de que la cadena de conexión SQL ya está agregada en la pestaña **Configuración**. Después, active la casilla **Reconozco que esta acción sobrescribirá el contenido del sitio web** y haga clic en el botón **Crear tabla TodoItem**.
     
        ![Configuración del nodo de tablas fáciles](./media/app-service-mobile-configure-new-backend/node-easy-tables-configuration.png)

     3. En **Tablas fáciles**, haga clic en el botón **+ Agregar**.
    
        ![Botón Agregar del nodo de tablas fáciles](./media/app-service-mobile-configure-new-backend/node-easy-tables-add.png)

     4. Cree una tabla `TodoItem` con acceso anónimo.
      
        ![Agregar tabla del nodo de tablas fáciles](./media/app-service-mobile-configure-new-backend/node-easy-tables-table-add.png)

   - **Back-end de .NET**
    
        Si va a usar la aplicación de inicio rápido de .NET, siga estas instrucciones.

        1. Descargue el proyecto de servidor .NET de Azure Mobile Apps desde el [repositorio azure-mobile-apps-quickstarts](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet/Quickstart).

        2. Compile el proyecto de servidor de .NET localmente en Visual Studio.

        3. En Visual Studio, abra el Explorador de soluciones, haga clic con el botón derecho en el proyecto `ZUMOAPPNAMEService`, elija **Publicar** y verá la ventana `Publish to App Service`. Si está trabajando en Mac, puede ver otras formas de implementar la aplicación [aquí](https://docs.microsoft.com/azure/app-service/deploy-local-git).
        
           ![Publicación de Visual studio](./media/app-service-mobile-configure-new-backend/visual-studio-publish.png)

        4. Seleccione **App Service** como destino de publicación y haga clic en **Seleccionar existente**. Después, seleccione **Publicar** en la parte inferior de la ventana.

        5. Primero tendrá que iniciar sesión en Visual Studio con su suscripción de Azure. Seleccione `Subscription` y `Resource Group` y, luego, el nombre de la aplicación. Cuando esté listo, haga clic en **Aceptar**. Se implementará el proyecto de servidor de .NET que tiene localmente en el back-end de App Service. Cuando termine la implementación, se le redirigirá a `http://{zumoappname}.azurewebsites.net/` en el explorador.                   