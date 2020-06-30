---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 343fef941870b6deaad777fac9b9d258d454aa2b
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095481"
---
#### <a name="no-response-from-the-backend-service"></a>No hay respuesta del servicio de back-end

Cuando realice pruebas localmente, asegúrese de que el servicio de back-end se esté ejecutando y de que esté usando el puerto correcto.

Si realiza las pruebas en la **aplicación de API de Azure**, compruebe que el servicio se esté ejecutando y que se haya implementado e iniciado sin errores.

Asegúrese de comprobar que ha especificado la dirección base correctamente en **[Postman](https://www.postman.com/downloads)** o en la configuración de la aplicación móvil al realizar las pruebas mediante el cliente. La dirección base debe ser `https://<api_name>.azurewebsites.net/` o `https://localhost:5001/` al realizar las pruebas de forma local.

#### <a name="not-receiving-notifications-on-android-after-starting-or-stopping-a-debug-session"></a>No se reciben notificaciones en Android después de iniciar o detener una sesión de depuración

Asegúrese de registrarse de nuevo después de iniciar o detener una sesión de depuración. El depurador hará que se genere un nuevo token de **Firebase**. También se debe actualizar la instalación del centro de notificaciones.

#### <a name="receiving-a-401-status-code-from-the-backend-service"></a>Se recibe un código de estado 401 del servicio de back-end

Compruebe que se configura el encabezado de la solicitud **apikey** y que este valor coincide con el que había configurado para el servicio de back-end.

Si recibe este error al realizar pruebas localmente, asegúrese de que el valor de clave que definió en la configuración de cliente coincide con el valor de configuración de usuario **Authentication:ApiKey** que usa la [API](#create-the-api-app).

Si está realizando las pruebas con una **aplicación de API**, asegúrese de que el valor de clave en el archivo de configuración del cliente coincide con la configuración de la aplicación **Authentication:ApiKey** que está utilizando en la [aplicación de API](#create-the-api-app).

> [!NOTE]
> Si ha creado o cambiado este valor después de haber implementado el servicio de back-end, debe reiniciar el servicio para que surta efecto.

Si eligió no completar la sección [Autenticación de clientes mediante una clave de API](#authenticate-clients-using-an-api-key-optional), asegúrese de que no aplicó el atributo **Authorize** a la clase **NotificationsController**.

#### <a name="receiving-a-404-status-code-from-the-backend-service"></a>Se recibe un código de estado 404 del servicio de back-end

Compruebe que el punto de conexión y el método de solicitud HTTP son correctos. Por ejemplo, los puntos de conexión deben ser:

- **[PUT]** `https://<api_name>.azurewebsites.net/api/notifications/installations`
- **[DELETE]** `https://<api_name>.azurewebsites.net/api/notifications/installations/<installation_id>`
- **[POST]** `https://<api_name>.azurewebsites.net/api/notifications/requests`

O cuando se realizan las pruebas de manera local:

- **[PUT]** `https://localhost:5001/api/notifications/installations`
- **[DELETE]** `https://localhost:5001/api/notifications/installations/<installation_id>`
- **[POST]** `https://localhost:5001/api/notifications/requests`

Al especificar la dirección base en la aplicación cliente, asegúrese de que finaliza con un `/`. La dirección base debe ser `https://<api_name>.azurewebsites.net/` o `https://localhost:5001/` al realizar las pruebas de forma local.

#### <a name="unable-to-register-and-a-notification-hub-error-message-is-displayed"></a>No se puede realizar el registro y aparece un mensaje de error del centro de notificaciones

Compruebe que el dispositivo de prueba tenga conectividad de red. A continuación, determine el código de estado de la respuesta http estableciendo un punto de interrupción para que inspeccione el valor de propiedad **StatusCode** en **HttpResponse**.

Revise las sugerencias de solución de problemas anteriores, si procede, según el código de estado.

Establezca un punto de interrupción en las líneas que devuelven estos códigos de estado específicos para la API correspondiente. Después, intente llamar al servicio de back-end al depurar localmente.

Compruebe que el servicio de back-end funciona según lo previsto a través de **[Postman](https://www.postman.com/downloads)** con la carga adecuada. Use la carga real creada por el código de cliente para la plataforma en cuestión.

Revise las secciones de configuración específicas de la plataforma para asegurarse de que no se ha omitido ningún paso. Compruebe que se están resolviendo los valores adecuados para las variables `installation id` y `token` de la plataforma adecuada.

#### <a name="unable-to-resolve-an-id-for-the-device-error-message-is-displayed"></a>No se puede resolver un identificador para el mensaje de error del dispositivo

Revise las secciones de configuración específicas de la plataforma para asegurarse de que no se ha omitido ningún paso.
