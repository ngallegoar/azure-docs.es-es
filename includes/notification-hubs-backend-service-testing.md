---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 72e4318bea7245e440db4c7d95bf7f1f38bbe268
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095472"
---
### <a name="send-a-test-notification"></a>Envío de una notificación de prueba

1. En [Postman](https://www.postman.com/downloads/), abra una nueva pestaña.

1. Establezca la solicitud en **POST** y escriba la siguiente dirección:

    ```xml
    https://<app_name>.azurewebsites.net/api/notifications/requests
    ```

1. Si eligió completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional), asegúrese de configurar los encabezados de solicitud para que incluyan el valor **apikey**.

   | Clave                            | Value                          |
   | ------------------------------ | ------------------------------ |
   | apikey                         | <your_api_key>                 |

   > [!NOTE]
   > Esta se puede encontrar en **Config.local_secrets.cs** en el proyecto **PushDemo**.

1. Elija la opción **sin formato** para el **cuerpo** y, a continuación, elija **JSON** en la lista de opciones de formato y, después, incluya algún contenido **JSON** de marcador de posición:

    ```json
    {
        "text": "Message from Postman!",
        "action": "action_a"
    }
    ```

1. Seleccione el botón **Codificar** que aparece en la ventana del botón **Guardar**, en la parte superior derecha. La solicitud debe ser parecida a la del ejemplo siguiente cuando se muestra para **HTML** (dependiendo de si incluyó un encabezado de **apikey**):

    ```html
    POST /api/notifications/requests HTTP/1.1
    Host: https://<app_name>.azurewebsites.net
    apikey: <your_api_key>
    Content-Type: application/json

    {
        "text": "Message from backend service",
        "action": "action_a"
    }
    ```

1. Ejecute la aplicación **PushDemo** en una de las plataformas de destino o en las dos (**Android** e **iOS**).

    > [!NOTE]
    > Si va a realizar las pruebas en **Android** asegúrese de que no ejecutará en **Depuración**, o si esta se ha implementado mediante la ejecución de la aplicación, realice un cierre forzado de esta e iníciela de nuevo desde el iniciador.

1. En la aplicación **PushDemo**, pulse en el botón **Registrar**.

1. De nuevo en **[Postman](https://www.postman.com/downloads)** , cierre la ventana **Generate Code Snippets** (Generar fragmentos de código), si todavía no lo ha hecho, y haga clic en el botón **Enviar**.

1. Compruebe que obtiene una respuesta **200 CORRECTO** en **[Postman](https://www.postman.com/downloads)** y que la alerta aparece en la aplicación indicando que se **ha recibido la acción ActionA**.  

1. Cierre la aplicación **PushDemo** y, después, haga clic de nuevo en el botón **Enviar** en **[Postman](https://www.postman.com/downloads)** .

1. Compruebe que obtiene una respuesta **200 CORRECTO** en **[Postman](https://www.postman.com/downloads)** de nuevo. Compruebe que aparece una notificación en el área de notificaciones de la aplicación **PushDemo** con el mensaje correcto.

1. Pulse en la notificación para confirmar que se abre la aplicación y que aparece la alerta que indica que se **ha recibido la acción ActionA**.

1. De nuevo en **[Postman](https://www.postman.com/downloads)** , modifique el cuerpo de la solicitud anterior para enviar una notificación silenciosa que especifica *action_b* en lugar de *action_a* para el valor **action**.

    ```json
    {
        "action": "action_b",
        "silent": true
    }
    ```

1. Con la aplicación abierta todavía, haga clic en el botón **Enviar** en **[Postman](https://www.postman.com/downloads)** .

1. Compruebe que obtiene una respuesta **200 CORRECTO** en **[Postman](https://www.postman.com/downloads)** y que la alerta aparece en la aplicación indicando que se **ha recibido la acción ActionB** en lugar de **se ha recibido la acción ActionA**.

1. Cierre la aplicación **PushDemo** y, después, haga clic de nuevo en el botón **Enviar** en **[Postman](https://www.postman.com/downloads)** .

1. Compruebe que obtiene una respuesta **200 CORRECTO** en **[Postman](https://www.postman.com/downloads)** y que la notificación silenciosa no aparece en el área de notificación.
