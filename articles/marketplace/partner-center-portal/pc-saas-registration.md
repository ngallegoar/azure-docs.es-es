---
title: 'Registro de una aplicación SaaS: Azure Marketplace'
description: Aprenda a usar Azure Portal para registrar una aplicación SaaS y recibir un token de seguridad de Azure Active Directory.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dsindona
ms.openlocfilehash: b3c20d25917d66cba8ae3d811eddaa6455b87722
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792962"
---
# <a name="register-a-saas-application"></a>Registro de una aplicación SaaS

En este artículo se explica cómo registrar una aplicación SaaS mediante Microsoft [Azure Portal](https://portal.azure.com/).  Después de un registro correcto, recibirá un token de seguridad de Azure Active Directory (Azure AD) que puede usar para acceder a las API de suministro SaaS.  Para más información sobre Azure AD, consulte [¿Qué es la autenticación?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios)

## <a name="service-to-service-authentication-flow"></a>Flujo de autenticación de servicio a servicio

El siguiente diagrama muestra el flujo de suscripción de un nuevo cliente y cuándo se utilizan estas API:

![Flujo de la API de la oferta de SaaS](./media/saas-offer-publish-api-flow-v1.png)

Azure no impone ninguna restricción en la autenticación que el servicio SaaS expone a los usuarios finales. Sin embargo, la autenticación con las API de suministro SaaS se realiza con un token de seguridad de Azure AD, normalmente obtenido al registrar la aplicación SaaS a través de Azure Portal. 

## <a name="register-an-azure-ad-secured-app"></a>Registrar una aplicación con protección de Azure AD

Cualquier aplicación que quiera usar las funciones de Azure AD debe registrarse primero en un inquilino de Azure AD. Este proceso de registro implica proporcionar a Azure AD los detalles de la aplicación, como la dirección URL donde se encuentra, la dirección URL para enviar respuestas cuando se autentica un usuario, el identificador URI que identifica la aplicación, etc.  Para registrar una aplicación nueva mediante Azure Portal, realice los pasos siguientes:

1.  Inicie sesión en [Azure Portal](https://portal.azure.com/).
2.  Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
3.  En el panel de navegación izquierdo, haga clic en el servicio **Azure Active Directory**, haga clic en **Registros de aplicaciones** y haga clic en **Nuevo registro de aplicación**.

    ![Registros de aplicaciones SaaS de AD](./media/saas-offer-app-registration-v1.png)

4.  En la página Crear, escriba la información de registro de la aplicación:
    -   **Name**: Especifique un nombre de aplicación significativo.
    -   **Tipo de aplicación**: 
        - Seleccione **Nativo** para las [aplicaciones cliente](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) que están instaladas localmente en un dispositivo. Esta configuración se utiliza para [clientes nativos](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#native-client) públicos de OAuth.
        - Seleccione **Aplicación web/API** para las [aplicaciones cliente](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) y las [aplicaciones de recursos/API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#resource-server) que están instaladas en un servidor seguro. Esta configuración se utiliza para [clientes web](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#web-client) confidenciales de OAuth y [clientes basados en agente de usuario](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client) públicos.
        La misma aplicación también puede exponer tanto un cliente como un recurso o API.
    -   **URL de inicio de sesión**: Para aplicaciones web o aplicaciones de la API, proporcione la dirección URL base de la aplicación. Por ejemplo, **http://localhost:31544** podría ser la dirección URL de una aplicación web que se ejecuta en la máquina local. Los usuarios utilizan esta dirección URL para iniciar sesión en una aplicación cliente web.
    -   **URI de redirección**: Para aplicaciones nativas, proporcione el identificador URI que utiliza Azure AD para devolver las respuestas de los tokens. Escriba un valor específico para la aplicación, por ejemplo, **http://MyFirstAADApp** .

        ![Registros de aplicaciones SaaS de AD](./media/saas-offer-app-registration-v1-2.png)

        Para obtener ejemplos específicos para aplicaciones web o aplicaciones nativas, consulte las guías de inicio rápido de configuraciones que están disponibles en la sección *Introducción* de la [Guía de desarrolladores de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).

5.  Cuando haya terminado, haga clic en **Crear**. Azure AD asigna un *identificador de aplicación* único a la aplicación y le lleva a la página de registro principal de la aplicación. En función de que se trate de una aplicación web o nativa, se proporcionan opciones diferentes para agregar funcionalidades adicionales a la aplicación.

>[!Note]
>De forma predeterminada, la aplicación recién registrada está configurada para permitir solo el inicio de sesión en la aplicación a los usuarios del mismo inquilino.

## <a name="using-the-azure-ad-security-token"></a>Usar el token de seguridad de Azure AD

Una vez que se ha registrado la aplicación, puede solicitar un token de seguridad de Azure AD mediante programación.  Se espera que el anunciante use este token y haga una solicitud para resolverlo.  Al usar las distintas API de suministro, el parámetro de consulta del token se encuentra en la dirección URL cuando se redirige al usuario al sitio web SaaS desde Azure.  Este token es válido durante una hora únicamente.  Además, debe descodificar como dirección URL el valor del token del explorador antes de usarlo.

Para más información sobre estos tokens, consulte [Tokens de acceso de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/access-tokens).


### <a name="get-a-token-based-on-the-azure-ad-app"></a>Obtención de un token basado en la aplicación de Azure AD

Método HTTP

`POST`

*Request URL* (URL de la solicitud)

**https://login.microsoftonline.com/ *{tenantId}* /oauth2/token**

*Parámetro de URI*

|  **Nombre de parámetro**  | **Obligatorio**  | **Descripción**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| tenantId             | True          | Identificador de inquilino de la aplicación de AAD registrada   |
|  |  |  |


*Encabezado de solicitud*

|  **Nombre de encabezado**  | **Obligatorio** |  **Descripción**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  Content-Type     | True         | Tipo de contenido asociado a la solicitud. El valor predeterminado es `application/x-www-form-urlencoded`.  |
|  |  |  |


*Cuerpo de la solicitud*

| **Nombre de propiedad**   | **Obligatorio** |  **Descripción**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | True         | Tipo de concesión. El valor predeterminado es `client_credentials`.                    |
|  Client_id          | True         |  Identificador de cliente o aplicación asociado a la aplicación de Azure AD.                  |
|  client_secret      | True         |  Contraseña asociada a la aplicación de Azure AD.                               |
|  Resource           | True         |  Recurso de destino para el que se solicita el token. El valor predeterminado es `62d94f6c-d599-489b-a797-3e10e42fbe22`. |
|  |  |  |


*Respuesta*

|  **Nombre**  | **Tipo**       |  **Descripción**    |
| ---------- | -------------  | ------------------- |
| 200 OK    | TokenResponse  | La solicitud se realizó correctamente   |
|  |  |  |

*TokenResponse*

Ejemplo de token de respuesta:

``` json
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "62d94f6c-d599-489b-a797-3e10e42fbe22",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }               
```

## <a name="next-steps"></a>Pasos siguientes

La aplicación protegida por Azure AD ahora puede usar la [API de suministro de SaaS versión 2](./pc-saas-fulfillment-api-v2.md).
