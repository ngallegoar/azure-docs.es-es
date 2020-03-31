---
title: Protección de una API mediante OAuth 2.0 con AAD y API Management
titleSuffix: Azure API Management
description: Obtenga información sobre cómo proteger un back-end de API web con Azure Active Directory y API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/21/2019
ms.author: apimpm
ms.openlocfilehash: 8b396b782c1254b3229aeeb8e51b61cc744d6318
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190362"
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Protección de una API mediante OAuth 2.0 con Azure Active Directory API Management

En esta guía se muestra cómo configurar la instancia de Azure API Management para proteger una API mediante el protocolo OAuth 2.0 con Azure Active Directory (Azure AD). 

> [!NOTE]
> Esta característica está disponible en los niveles **Desarrollador**, **Estándar** y **Premium** de API Management.

## <a name="prerequisites"></a>Prerequisites
Para seguir los pasos de este artículo, debe tener:
* Una instancia de API Management
* Una API que se vaya a publicar y que use la instancia de API Management
* Un inquilino de Azure AD

## <a name="overview"></a>Información general

Aquí se muestra una introducción rápida de los pasos:

1. Registre una aplicación (aplicación back-end) en Azure AD que represente la API.
2. Registre otra aplicación (aplicación cliente) en Azure AD que represente una aplicación de cliente que deba llamar a la API.
3. En Azure AD, conceda permisos para que la aplicación cliente llame a la aplicación back-end.
4. Configure la consola del desarrollador para llamar a la API con la autorización de usuario OAuth 2.0.
5. Agregue la directiva **validate-jwt** para validar el token de OAuth para todas las solicitudes entrantes.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>Registro de una aplicación en Azure AD que represente la API

Para proteger una API con Azure AD, el primer paso es registrar una aplicación en Azure AD que la represente. 

1. Vaya a [Azure Portal](https://portal.azure.com) para registrar la aplicación. Busque y seleccione **Registros de aplicaciones**.

1. Seleccione **Nuevo registro**. 

1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación: 
    - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, *backend-app*. 
    - En la sección **Tipos de cuenta admitidos**, seleccione una opción que se ajuste a su escenario. 

1. Deje vacía la sección **URI de redireccionamiento**.

1. Seleccione **Registrar** para crear la aplicación. 

1. En la página **Información general** de la aplicación, busque el valor de **Id. de aplicación (cliente)** y regístrelo para usarlo más tarde.

1. Seleccione **Exponer una API** y establezca la opción **URI de id. de aplicación** con el valor predeterminado. Anote este valor para más adelante.

1. Seleccione el botón **Agregar un ámbito** para mostrar la página **Agregar un ámbito**. Después, cree un ámbito compatible con la API (por ejemplo, `Files.Read`). Por último, seleccione el botón **Agregar ámbito** para crear el ámbito. Repita este paso para agregar todos los ámbitos compatibles con la API.

1. Cuando se creen los ámbitos, anótelos para usarlos en un paso posterior. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Registro de otra aplicación en Azure AD que represente una aplicación cliente

Todas las aplicaciones cliente que llamen a la API deben registrarse también como aplicación en Azure AD. En este ejemplo, la aplicación cliente es la consola del desarrollador en el portal para desarrolladores de API Management. A continuación se indica cómo registrar otra aplicación en Azure AD que represente la consola del desarrollador.

1. Vaya a [Azure Portal](https://portal.azure.com) para registrar la aplicación. Busque y seleccione **Registros de aplicaciones**.

1. Seleccione **Nuevo registro**.

1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación: 
    - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, *client-app*. 
    - En la sección **Tipos de cuenta admitidos**, seleccione **Cuentas en cualquier directorio organizativo (cualquier directorio de Azure AD: multiinquilino)** . 

1. En la sección **URI de redirección**, seleccione `Web` e introduzca la dirección URL `https://contoso5.portal.azure-api.net/signin`.

1. Seleccione **Registrar** para crear la aplicación. 

1. En la página **Información general** de la aplicación, busque el valor de **Id. de aplicación (cliente)** y regístrelo para usarlo más tarde.

Ahora, cree un secreto de cliente para esta aplicación que se usará en un paso posterior.

1. En la lista de páginas de la aplicación cliente, seleccione **Certificados y secretos** y **Nuevo secreto de cliente**.

1. En **Agregar un secreto de cliente**, proporcione una **descripción**. Elija cuando debe expirar la clave y seleccione **Agregar**.

Cuando se cree el secreto, anote el valor de clave para usarlo en un paso posterior. 

## <a name="grant-permissions-in-azure-ad"></a>Concesión de permisos en Azure AD

Ahora que hemos registrado dos aplicaciones que representan la API y la consola del desarrollador, es necesario conceder permisos para que la aplicación cliente pueda llamar a la aplicación back-end.  

1. Vaya a [Azure Portal](https://portal.azure.com) para conceder permisos a la aplicación cliente. Busque y seleccione **Registros de aplicaciones**.

1. Seleccione la aplicación cliente. Después, en la lista de páginas de la aplicación, seleccione **Permisos de API**.

1. Seleccione **Agregar un permiso**.

1. En **Seleccionar una API**, seleccione **Mis API** y busque y seleccione la aplicación de back-end.

1. En **Permisos delegados**, seleccione los permisos adecuados para la aplicación de back-end y, luego, seleccione **Agregar permisos**.

1. De manera opcional, en la página **Permisos de API**, seleccione **Conceder consentimiento del administrador para \<nombre-de-inquilino>** para conceder el consentimiento en nombre de todos los usuarios de este directorio. 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>Habilitación de la autorización de usuario OAuth 2.0 en la consola del desarrollador

En este punto ya se han creado nuestras aplicaciones en Azure AD y se han concedido los permisos pertinentes para que la aplicación cliente pueda llamar a la aplicación back-end. 

En este ejemplo, la consola del desarrollador es la aplicación cliente. En los pasos siguientes se describe la habilitación de la autorización de usuario OAuth 2.0 en la consola del desarrollador. 

1. En Azure Portal, vaya a la instancia de API Management.

1. Seleccione **OAuth 2.0** > **Agregar**.

1. Proporcione un **Nombre para mostrar** y una **Descripción**.

1. Como **URL de la página de registro de cliente**, escriba un valor de marcador de posición como `http://localhost`. La **dirección URL de la página de registro de cliente** señala a una página que los usuarios pueden utilizar para crear y configurar sus propias cuentas para proveedores de OAuth 2.0 que admiten esto. En este ejemplo, los usuarios no crean ni configuran sus propias cuentas, por lo que se usa un marcador de posición en su lugar.

1. En **Tipos de concesión de autorización**, seleccione **Código de autorización**.

1. Especifique la **URL del punto de conexión de autorización** y la **URL del punto de conexión de token**. Recupere estos valores desde la página **Puntos de conexión** del inquilino de Azure AD. Vaya a la página **Registros de aplicaciones** de nuevo y seleccione **Puntos de conexión**.


1. Copie el **punto de conexión de autorización de OAuth 2.0** y péguelo en el cuadro de texto **URL de punto de conexión de autorización**. Seleccione **POST** en Método de solicitud de autorización.

1. Copie el **punto de conexión de token de OAuth 2.0** y péguelo en el cuadro de texto **URL de punto de conexión del token**. 

    >[!IMPORTANT]
    > Puede usar puntos de conexión **v1** o **v2**. Sin embargo, el paso siguiente será distinto según la versión que elija. Se recomienda usar puntos de conexión v2. 

1. Si usa puntos de conexión **v1**, agregue un parámetro de cuerpo denominado **resource**. Como valor de este parámetro, use el **id. de aplicación** de la aplicación de back-end. 

1. Si usa puntos de conexión **v2**, use el ámbito que creó para la aplicación de back-end en el campo **Ámbito predeterminado**.

1. A continuación, especifique las credenciales del cliente. Estas son las credenciales para la aplicación cliente.

1. Como **Id. de cliente**, use el **id. de aplicación** de la aplicación cliente.

1. Como **secreto de cliente**, use la clave que creó para la aplicación cliente. 

1. Inmediatamente después del secreto del cliente se encuentra **redirect_url** como tipo de concesión de código de autorización. Anote esta dirección URL.

1. Seleccione **Crear**.

1. Vuelva a la aplicación cliente y seleccione **Autenticación**.

1. En **URI de redirección**, seleccione el tipo como **Web**, pegue el valor de **redirect_url** en **URI de redirección** y, a continuación, guarde.

Ahora que ha configurado un servidor de autorización de OAuth 2.0, la consola del desarrollador puede obtener tokens de acceso de Azure AD. 

El paso siguiente consiste en habilitar la autorización de usuario de OAuth 2.0 para la API. Esto permite que la consola del desarrollador sepa que debe obtener un token de acceso en nombre del usuario antes de realizar llamadas a la API.

1. Vaya a la instancia de API Management y a **API**.

2. Seleccione la API que desea proteger. Por ejemplo, puede usar `Echo API`.

3. Vaya a **Configuración**.

4. En **Seguridad**, elija **OAuth 2.0** y seleccione el servidor OAuth 2.0 que se configuró anteriormente. 

5. Seleccione **Guardar**.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Llamada correcta a la API desde el portal para desarrolladores

> [!NOTE]
> Esta sección no es aplicable al nivel de **consumo**, que no es compatible con el portal para desarrolladores.

Ahora que la autorización de usuario OAuth 2.0 está habilitada en la API, la consola del desarrollador obtendrá un token de acceso en nombre del usuario antes de llamar a la API.

1. Vaya a cualquier operación de la API en el portal para desarrolladores y, a continuación, seleccione **Probar**. Aparecerá la consola del desarrollador.

2. Tenga en cuenta el nuevo elemento de la sección **Autorización** correspondiente al servidor de autorización que acaba de agregar.

3. Seleccione **Código de autorización** de la lista desplegable de autorizaciones y se le pedirá que inicie sesión en el inquilino de Azure AD. Si ya ha iniciado sesión con la cuenta, es posible que no se le indique nada.

4. Al iniciar sesión correctamente, un encabezado `Authorization` se agregará a la solicitud con un token de acceso de Azure AD. El siguiente es un token de ejemplo (con codificación Base64):

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

5. Seleccione **Enviar** y podrá llamar a la API correctamente.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configurar una directiva de validación de JWT para autorizar previamente las solicitudes

En este momento, si un usuario intenta realizar una llamada desde la consola del desarrollador, se le pide al usuario que inicie sesión. La consola del desarrollador obtiene un token de acceso en nombre del usuario e incluye el token en la solicitud realizada a la API.

Sin embargo, ¿qué ocurre si alguien llama a la API sin un token o con uno no válido? Por ejemplo, si intenta llamar a la API sin el encabezado `Authorization`, la llamada continuará de todos modos. La razón es que API Management no valida el token de acceso en este momento. Simplemente pasa el encabezado `Authorization` a la API de back-end.

La directiva de [validación de JWT](api-management-access-restriction-policies.md#ValidateJWT) se puede usar para autorizar previamente las solicitudes en API Management, al validarse los tokens de acceso de cada solicitud entrante. Si una solicitud no tiene un token válido, API Management la bloquea. Por ejemplo, agregue la directiva siguiente a la sección de la directiva `<inbound>` de `Echo API`. Comprueba la notificación de audiencia en un token de acceso y devuelve un mensaje de error si el token no es válido. Para información sobre cómo configurar directivas, consulte el artículo sobre [edición o establecimiento de directivas](set-edit-policies.md).

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```
> [!NOTE]
> Esta dirección URL de `openid-config` corresponde al punto de conexión v1. Para el punto de conexión v2 `openid-config`, utilice `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`.

## <a name="build-an-application-to-call-the-api"></a>Compilación de una aplicación para llamar a la API

En esta guía, se utiliza la consola del desarrollador en API Management como la aplicación de cliente de ejemplo para llamar a `Echo API` protegido por OAuth 2.0. Para más información sobre cómo compilar una aplicación e implementar OAuth 2.0, consulte [Ejemplos de código de Azure Active Directory](../active-directory/develop/sample-v2-code.md).

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre [Escenarios de autenticación para Azure AD](../active-directory/develop/authentication-scenarios.md).
* Consulte más [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre la administración de API.
* Para conocer otras formas de proteger el servicio back-end, consulte [Autenticación de certificado mutua](api-management-howto-mutual-certificates.md).

* [Creación de una instancia del servicio de API Management](get-started-create-service-instance.md).

* [Administración de su primera API en Administración de API de Azure](import-and-publish.md).
