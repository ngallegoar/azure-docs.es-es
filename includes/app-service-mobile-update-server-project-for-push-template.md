---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 675ad278cb8bdc0ced4eff3bd77572f44c9808fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "68857496"
---
En esta sección, se actualiza el código del proyecto de back-end de Mobile Apps existente con el objetivo de enviar una notificación push cada vez que se agrega un nuevo elemento. Este proceso funciona con la característica de [plantillas](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) de Azure Notification Hubs, que permite inserciones multiplataforma. Los diversos clientes se registran para recibir notificaciones push mediante plantillas, y una solo notificación push puede acceder a todas las plataformas de cliente.

Elija uno de los procedimientos siguientes que se ajuste al tipo de proyecto de back-end&mdash;: [back-end de .NET](#dotnet) o [back-end de Node.js](#nodejs).

### <a name="net-back-end-project"></a><a name="dotnet"></a>Proyecto de back-end de .NET

1. En Visual Studio, haga clic con el botón derecho en el proyecto de servidor. A continuación, seleccione **Administrar paquetes NuGet**. Busque `Microsoft.Azure.NotificationHubs` y, después, seleccione **Instalar**. Este proceso instala la biblioteca de Notification Hubs para enviar notificaciones desde el back-end.
2. En el proyecto de servidor, abra **Controladores** > **TodoItemController.cs**. Luego, agregue las siguientes instrucciones using:

    ```csharp
    using System.Collections.Generic;
    using Microsoft.Azure.NotificationHubs;
    using Microsoft.Azure.Mobile.Server.Config;
    ```

3. En el método **PostTodoItem**, agregue el código siguiente después de la llamada a **InsertAsync**:  

    ```csharp
    // Get the settings for the server project.
    HttpConfiguration config = this.Configuration;
    MobileAppSettingsDictionary settings =
        this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

    // Get the Notification Hubs credentials for the mobile app.
    string notificationHubName = settings.NotificationHubName;
    string notificationHubConnection = settings
        .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

    // Create a new Notification Hub client.
    NotificationHubClient hub = NotificationHubClient
    .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

    // Send the message so that all template registrations that contain "messageParam"
    // receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string,string> templateParams = new Dictionary<string,string>();
    templateParams["messageParam"] = item.Text + " was added to the list.";

    try
    {
        // Send the push notification and log the results.
        var result = await hub.SendTemplateNotificationAsync(templateParams);

        // Write the success result to the logs.
        config.Services.GetTraceWriter().Info(result.State.ToString());
    }
    catch (System.Exception ex)
    {
        // Write the failure result to the logs.
        config.Services.GetTraceWriter()
            .Error(ex.Message, null, "Push.SendAsync Error");
    }
    ```

    Este proceso envía una notificación de plantilla que contiene el archivo item.text cuando se inserta un nuevo elemento.

4. Vuelva a publicar el proyecto de servidor.

### <a name="nodejs-back-end-project"></a><a name="nodejs"></a>Proyecto de back-end de Node.js

1. Configure el proyecto de back-end.
2. Reemplace el código existente en el archivo todoitem.js por el siguiente:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
    promises = require('azure-mobile-apps/src/utilities/promises'),
    logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    table.insert(function (context) {
    // For more information about the Notification Hubs JavaScript SDK,
    // see https://aka.ms/nodejshubs.
    logger.info('Running TodoItem.insert');

    // Define the template payload.
    var payload = '{"messageParam": "' + context.item.text + '" }';  

    // Execute the insert. The insert returns the results as a promise.
    // Do the push as a post-execute action within the promise flow.
    return context.execute()
        .then(function (results) {
            // Only do the push if configured.
            if (context.push) {
                // Send a template notification.
                context.push.send(null, payload, function (error) {
                    if (error) {
                        logger.error('Error while sending push notification: ', error);
                    } else {
                        logger.info('Push notification sent successfully!');
                    }
                });
            }
            // Don't forget to return the results from the context.execute().
            return results;
        })
        .catch(function (error) {
            logger.error('Error while running context.execute: ', error);
        });
    });

    module.exports = table;  
    ```

    Este proceso envía una notificación de plantilla que contiene el archivo item.text cuando se inserta un nuevo elemento.

3. Cuando edite el archivo en el equipo local, vuelva a publicar el proyecto de servidor.
