---
title: 'Adición de conectores de API a flujos de autoservicio de registro: Azure AD'
description: Configurar una API Web para usarla en un flujo de usuario.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e0498a2015b75221763ab5fdd4f6e94428922bd6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85386749"
---
# <a name="add-an-api-connector-to-a-user-flow"></a>Adición de un conector de API a un flujo de usuario

Para usar un [conector de API](api-connectors-overview.md), primero debe crear el conector de API y, después, habilitarlo en un flujo de usuario.

## <a name="create-an-api-connector"></a>Creación de un conector de API

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador de Azure AD.
2. En **Servicios de Azure**, seleccione **Azure Active Directory**.
3. En el menú de la izquierda, seleccione **External Identities**.
4. Seleccione **All API connectors (Preview)** (Todos los conectores de API [versión preliminar]) y, después, seleccione **New API connector** (Nuevo conector de API).

   ![Adición de un conector de API nuevo](./media/self-service-sign-up-add-api-connector/api-connector-new.png)

5. Escriba el nombre para mostrar de la llamada. Por ejemplo, **Comprobar el estado de aprobación**.
6. Proporcione el valor de **Dirección URL del punto de conexión** de la llamada API.
7. Indique la información de autenticación de la API.

   - Actualmente solo se admite la autenticación básica. Si desea usar una API sin autenticación básica con fines de desarrollo, solo tiene que escribir un **Nombre de usuario** y una **Contraseña** ficticios que la API pueda omitir. Si se va a usar con una función de Azure con una clave de API, puede incluir el código como un parámetro de consulta en **Dirección URL del punto de conexión** (por ejemplo, https[]()://contoso.azurewebsites.net/api/endpoint<b>?code=0123456789</b>).

   ![Adición de un conector de API nuevo](./media/self-service-sign-up-add-api-connector/api-connector-config.png)

8. Seleccione las notificaciones que desea enviar a la API.
9. Seleccione las notificaciones que vaya a recibir de la API.

   ![Establecimiento de notificaciones del conector de API](./media/self-service-sign-up-add-api-connector/api-connector-claims.png)

10. Seleccione **Guardar**.

## <a name="enable-the-api-connector-in-a-user-flow"></a>Habilitación del conector de API en un flujo de usuario

Siga estos pasos para agregar el conector de API a un flujo de usuario de autoservicio de registro.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador de Azure AD.
2. En **Servicios de Azure**, seleccione **Azure Active Directory**.
3. En el menú de la izquierda, seleccione **External Identities**.
4. Seleccione **Flujos de usuario (versión preliminar)** y, después, seleccione el flujo de usuario para el que desea habilitar el conector de API.
5. Seleccione **Conectores de API** y, después, seleccione los puntos de conexión de API que desea invocar en los pasos siguientes del flujo de usuario:

   - **Después de iniciar sesión con un proveedor de identidades**
   - **Antes de crear el usuario**

   ![Adición de API al flujo de usuario](./media/self-service-sign-up-add-api-connector/api-connectors-user-flow-select.png)

6. Seleccione **Guardar**.

Obtenga información sobre [dónde se puede habilitar un conector de API en un flujo de usuario](api-connectors-overview.md#where-you-can-enable-an-api-connector-in-a-user-flow).

## <a name="request-sent-to-the-api"></a>Solicitud enviada a la API

Un conector de API se materializa como una solicitud HTTP POST y envía las notificaciones seleccionadas como pares clave-valor en un cuerpo JSON. La respuesta también debe tener el encabezado HTTP `Content-Type: application/json`. Los atributos se serializan de forma similar a los atributos de usuario de Microsoft Graph. <!--# TODO: Add link to MS Graph or create separate reference.-->

### <a name="example-request"></a>Solicitud de ejemplo

```http
POST <API-endpoint>
Content-type: application/json

{
 "email_address": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ //Sent for Google and Facebook identity providers
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "postalCode": "33971",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

De forma predeterminada, en todas las solicitudes se envía la notificación de **configuración regional de UI ("ui_locales")** . Proporciona la configuración regional de un usuario y la puede usar la API para devolver respuestas internacionalizadas. No aparece en el panel de configuración de API.

Si una notificación para enviar no tiene un valor en el momento en que se llama al punto de conexión de la API, la notificación no se enviará a la API.

Se pueden crear atributos personalizados para el usuario con el formato **extensión_\<extensions-app-id>_nombreAtributo**. La API esperará recibir las notificaciones con este mismo formato serializado. La API puede devolver notificaciones con o sin `<extensions-app-id>`. Para más información acerca de los atributos personalizados, consulte cómo [definir atributos personalizados para flujos de autoservicio de registro](user-flow-add-custom-attributes.md).

> [!TIP] 
> Las notificaciones de [**identidades ("identities")** ](https://docs.microsoft.com/graph/api/resources/objectidentity?view=graph-rest-1.0) y **dirección de correo electrónico ("email_address")** se pueden usar para identificar a un usuario antes de que tenga una cuenta en el inquilino. La notificación "identities" se envía cuando un usuario se autentica con Google o Facebook, y "email_address" se envía siempre.

## <a name="expected-response-types-from-the-web-api"></a>Tipos de respuesta esperados de la API web

Cuando la API web recibe una solicitud HTTP de Azure AD durante un flujo de usuario, puede devolver estas respuestas:

- [Respuesta de continuación](#continuation-response)
- [Respuesta de bloqueo](#blocking-response)
- [Respuesta de error de validación](#validation-error-response)

### <a name="continuation-response"></a>Respuesta de continuación

Una respuesta de continuación indica que el flujo de usuario debe continuar con el paso siguiente. En una respuesta de continuación, la API puede devolver notificaciones.

Si la API devuelve una notificación que está seleccionada como **Claim to receive** (Notificación para recibir), la notificación hace lo siguiente:

- Rellena previamente los campos de entrada en la página de recopilación de atributos, si el conector de API se invoca antes de que se presente la página.
- Invalida cualquier valor que ya se haya asignado a la notificación.
- Asigna un valor a la notificación si anteriormente era NULL.

#### <a name="example-of-a-continuation-response"></a>Ejemplo de una respuesta de continuación

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue",
    "postalCode": "12349", // return claim
    "extension_<extensions-app-id>_CustomAttribute": "value" // return claim
}
```

| Parámetro                                          | Tipo              | Obligatorio | Descripción                                                                                                                                                                                                                                                                            |
| -------------------------------------------------- | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| version                                            | String            | Sí      | La versión de la API.                                                                                                                                                                                                                                                                |
| action                                             | String            | Sí      | El valor debe ser `Continue`.                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | No       | Los valores se pueden almacenar en el directorio si están seleccionados como **Claim to receive** (Notificación para recibir) en la configuración del conector de API y **User attribute** (Atributo de usuario) de un flujo de usuario. Los valores se pueden devolver en el token si están seleccionados como **Application claim** (Notificación de aplicación).                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | No       | Opcionalmente, la notificación devuelta puede no contener `_<extensions-app-id>_`. Los valores se almacenan almacenar en el directorio si están seleccionados como **Claim to receive** (Notificación para recibir) en la configuración del conector de API y **User attribute** (Atributo de usuario) de un flujo de usuario. Los atributos personalizados no se pueden devolver en el token. |

### <a name="blocking-response"></a>Respuesta de bloqueo

Una respuesta de bloqueo termina el flujo de usuario. La API puede emitirla intencionadamente para detener el flujo de usuario y mostrar una página de bloqueo al usuario. La página de bloqueo muestra el mensaje `userMessage` proporcionado por la API.

El siguiente es un ejemplo de la respuesta de bloqueo:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "There was a problem with your request. You are not able to sign up at this time.",
    "code": "CONTOSO-BLOCK-00"
}

```

| Parámetro   | Tipo   | Obligatorio | Descripción                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| version     | String | Sí      | La versión de la API.                                                    |
| action      | String | Sí      | El valor debe ser `ShowBlockPage`.                                              |
| userMessage | String | Sí      | Mensaje que se va a mostrar al usuario.                                            |
| código        | String | No       | Código de error. Se puede usar con fines de depuración. No se muestra al usuario. |

#### <a name="end-user-experience-with-a-blocking-response"></a>Finaliza la experiencia del usuario con una respuesta de bloqueo

![Página de bloqueo de ejemplo](./media/api-connectors-overview/blocking-page-response.png)

### <a name="validation-error-response"></a>Respuesta de error de validación

Una llamada API invocada después de una página de recopilación de atributos puede devolver una respuesta de error de validación. Al hacerlo, el flujo de usuario permanece en la página de recopilación de atributos y se muestra `userMessage` al usuario. El usuario puede editar el formulario y volver a enviarlo. Este tipo de respuesta se puede usar para la validación de datos de entrada.

#### <a name="example-of-a-validation-error-response"></a>Ejemplo de una respuesta de error de validación

```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code.",
    "code": "CONTOSO-VALIDATION-00"
}
```

| Parámetro   | Tipo    | Obligatorio | Descripción                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| version     | String  | Sí      | La versión de la API.                                                    |
| action      | String  | Sí      | El valor debe ser `ValidationError`.                                           |
| status      | Entero | Sí      | Debe ser un valor `400` para una respuesta ValidationError.                        |
| userMessage | String  | Sí      | Mensaje que se va a mostrar al usuario.                                            |
| código        | String  | No       | Código de error. Se puede usar con fines de depuración. No se muestra al usuario. |

#### <a name="end-user-experience-with-a-validation-error-response"></a>Finaliza la experiencia del usuario con una respuesta de error de validación

![Página de validación de ejemplo](./media/api-connectors-overview/validation-error-postal-code.png)

### <a name="integration-with-azure-functions"></a>Integración con Azure Functions
Puede usar un desencadenador HTTP en Azure Functions como una forma sencilla de crear una API para usarla con el conector de API. Puede usar la función de Azure para, [por ejemplo](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts), crear la lógica de validación y limitar los registros a dominios específicos. También puede llamar e invocar otras API web, almacenes de usuarios o servicios en la nube.

## <a name="next-steps"></a>Pasos siguientes

<!-- - Learn [where you can enable an API connector](api-connectors-overview.md#where-you-can-enable-an-api-connector-in-a-user-flow) -->
- Obtenga información sobre cómo [agregar un flujo de trabajo personalizado de aprobación al autoservicio de registro](self-service-sign-up-add-approvals.md).
- Vea una introducción a los [ejemplos de inicio rápido de Azure Functions](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts).
<!-- - Learn how to [use API connectors to verify a user identity](code-samples-self-service-sign-up.md#identity-verification) -->
