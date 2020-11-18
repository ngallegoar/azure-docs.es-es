---
title: Problemas al iniciar sesión en aplicaciones configuradas con inicio de sesión único basado en SAML
description: Instrucciones para resolver errores específicos al iniciar sesión en una aplicación que se ha configurado para un inicio de sesión único federado basado en SAML con Azure Active Directory
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: troubleshooting
ms.date: 02/18/2019
ms.author: kenwith
ms.reviewer: luleon, asteen
ms.custom: contperfq2
ms.openlocfilehash: e75669c70c67d55c94642a0f6dbe3c9dbc3376e6
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94651556"
---
# <a name="problems-signing-in-to-saml-based-single-sign-on-configured-apps"></a>Problemas al iniciar sesión en aplicaciones configuradas con inicio de sesión único basado en SAML
Para solucionar estos problemas de inicio de sesión, se recomienda que siga los siguientes consejos para obtener un mejor diagnóstico y automatizar los pasos de resolución:

- Instale la [extensión del explorador seguro de Mis aplicaciones](./access-panel-deployment-plan.md) para ayudar a Azure Active Directory (Azure AD) a proporcionar mejores diagnósticos y resoluciones al usar la experiencia de pruebas en Azure Portal.
- Reproduzca el error mediante la experiencia de pruebas en la página de configuración de aplicaciones en Azure Portal. Obtenga más información en [Depuración del inicio de sesión único basado en SAML en aplicaciones](./debug-saml-sso-issues.md).

Si usa la [experiencia de pruebas](./debug-saml-sso-issues.md) de Azure Portal con la extensión del explorador seguro de Aplicaciones, no tiene que seguir los siguientes pasos manualmente para abrir la página de configuración del inicio de sesión único basado en SAML.

Para abrir la página de configuración del inicio de sesión único basado en SAML:
1.  Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.
1.  Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.
1.  Escriba **"Azure Active Directory"** en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.
1.  Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.
1.  Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.
    Si no ve la aplicación que desea que aparezca aquí, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.
1.  Seleccione la aplicación que desea configurar para el inicio de sesión único.
1. Cuando se cargue la aplicación, seleccione **Inicio de sesión único** desde el menú de navegación izquierdo de la aplicación.
1. Seleccione Inicio de sesión único basado en SAML.

## <a name="application-not-found-in-directory"></a>No se encontró la aplicación en el directorio
`Error AADSTS70001: Application with Identifier 'https:\//contoso.com' was not found in the directory.`

**Causa posible**

El atributo `Issuer` que se envía de la aplicación a Azure AD en la solicitud SAML no coincide con el valor de identificador configurado para la aplicación de Azure AD.

**Resolución**

Asegúrese de que el atributo `Issuer` de la solicitud SAML coincide con el valor del identificador configurado en Azure AD.

En la página de configuración del inicio de sesión único basado en SAML, en la sección **Configuración básica de SAML**, compruebe que el valor del cuadro de texto Identificador coincide con el valor del identificador que aparece en el error.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>La dirección de respuesta no coincide con las direcciones de respuesta configuradas para la aplicación
`Error AADSTS50011: The reply address 'https:\//contoso.com' does not match the reply addresses configured for the application.`

**Causa posible**

El valor de `AssertionConsumerServiceURL` en la solicitud SAML no coincide con el valor o patrón de la dirección URL de respuesta configurada en Azure AD. El valor de `AssertionConsumerServiceURL` en la solicitud SAML es la dirección URL que aparece en el error.

**Resolución**

Asegúrese de que el valor de `AssertionConsumerServiceURL` en la solicitud SAML coincide con el valor de la dirección URL de respuesta configurada en Azure AD. 

Compruebe o actualice el valor en el cuadro de texto Dirección URL de respuesta para que coincida con el valor de `AssertionConsumerServiceURL` en la solicitud SAML.   
    
Después de actualizar el valor de la dirección URL de respuesta en Azure AD y de comprobar que coincide con el valor enviado por la aplicación en la solicitud SAML, podrá iniciar sesión en la aplicación.

## <a name="user-not-assigned-a-role"></a>Usuario no asignado a un rol
`Error AADSTS50105: The signed in user 'brian\@contoso.com' is not assigned to a role for the application.`

**Causa posible**

El usuario no tiene acceso a la aplicación en Azure AD.

**Resolución**

Para asignar uno o varios usuarios a una aplicación directamente, consulte [Inicio rápido: Asignación de usuarios a una aplicación](add-application-portal-assign-users.md).

## <a name="not-a-valid-saml-request"></a>No es una solicitud SAML válida
`Error AADSTS75005: The request is not a valid Saml2 protocol message.`

**Causa posible**

Azure AD no admite la solicitud SAML enviada por la aplicación para el inicio de sesión único. Algunos de los problemas más comunes son:
- Faltan campos obligatorios en la solicitud SAML
- Método codificado de la solicitud SAML

**Resolución**

1. Capture la solicitud SAML. Siga el tutorial de [Cómo depurar el inicio de sesión único basado en SAML en aplicaciones de Azure AD](./debug-saml-sso-issues.md) para obtener información sobre cómo capturar la solicitud SAML.
1. Póngase en contacto con el proveedor de la aplicación y comparta la información siguiente:
    - La solicitud SAML
    - [Los requisitos del protocolo SAML de inicio de sesión único de Azure AD](../develop/single-sign-on-saml-protocol.md)

El proveedor de la aplicación debe validar que admite la implementación de SAML de Azure AD para inicio de sesión único.

## <a name="misconfigured-application"></a>Aplicación mal configurada
`Error AADSTS650056: Misconfigured application. This could be due to one of the following: The client has not listed any permissions in the requested permissions in the client's application registration. Or, The admin has not consented in the tenant. Or, Check the application identifier in the request to ensure it matches the configured client application identifier. Please contact your admin to fix the configuration or consent on behalf of the tenant.`

**Causa posible**

El atributo `Issuer` que se envía de la aplicación a Azure AD en la solicitud SAML no coincide con el valor de identificador configurado para la aplicación de Azure AD.

**Resolución**

Asegúrese de que el atributo `Issuer` de la solicitud SAML coincide con el valor del identificador configurado en Azure AD. 

Compruebe que el valor del cuadro de texto Identificador coincide con el valor del identificador mostrado en el error.

## <a name="certificate-or-key-not-configured"></a>Certificado o clave no configurados
`Error AADSTS50003: No signing key configured.`

**Causa posible**

El objeto de aplicación está dañado y Azure AD no reconoce el certificado configurado para la aplicación.

**Resolución**

Para eliminar y crear un nuevo certificado, siga estos pasos:
1. En la pantalla de configuración del inicio de sesión único basado en SAML, seleccione **Crear nuevo certificado** en la sección **Certificado de firma de SAML**.
1. Seleccione la fecha de expiración y, a continuación, haga clic en **Guardar**.
1. Active **Activar el certificado nuevo** para reemplazar el certificado activo. Después, haga clic en **Guardar** en la parte superior del panel y en Aceptar para activar el certificado de sustitución.
1. En la sección **Certificado de firma de SAML**, haga clic en **Quitar** para quitar el certificado **no usado**.

## <a name="saml-request-not-present-in-the-request"></a>La solicitud SAML no existe en la solicitud
`Error AADSTS750054: SAMLRequest or SAMLResponse must be present as query string parameters in HTTP request for SAML Redirect binding.`

**Causa posible**

Azure AD no podía identificar la solicitud SAML dentro de los parámetros de dirección URL en la solicitud HTTP. Esto puede ocurrir si la aplicación no usa el enlace de redirección HTTP para enviar la solicitud SAML a Azure AD.

**Resolución**

La aplicación debe enviar la solicitud SAML codificada en el encabezado de ubicación mediante el enlace de redirección HTTP. Para más información sobre cómo implementarla, lea la sección Enlace de redirección HTTP del [documento de especificaciones del protocolo SAML](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf).

## <a name="azure-ad-is-sending-the-token-to-an-incorrect-endpoint"></a>Azure AD envía el token a un punto de conexión incorrecto
**Causa posible**

Durante el inicio de sesión único, si la solicitud de inicio de sesión no contiene una dirección URL de respuesta explícita (URL del Servicio de consumidor de aserciones), Azure AD seleccionará cualquiera de las direcciones URL de respuesta configuradas para esa aplicación. Incluso si la aplicación tiene una dirección URL de respuesta explícita configurada, el usuario puede ser redirigido a https://127.0.0.1:444. 

Cuando la aplicación se agregó como una aplicación que no es de la galería, Azure Active Directory creó esta dirección URL de respuesta como un valor predeterminado. Este comportamiento ha cambiado y Azure Active Directory ya no agrega esta dirección URL de forma predeterminada. 

**Resolución**

Elimine las direcciones URL de respuesta no utilizadas configuradas para la aplicación.

En la página de configuración del inicio de sesión único basado en SAML, en la **URL de respuesta (URL del Servicio de consumidor de aserciones)** , elimine las direcciones URL de respuesta no utilizadas o predeterminadas creadas por el sistema. Por ejemplo, `https://127.0.0.1:444/applications/default.aspx`.


## <a name="authentication-method-by-which-the-user-authenticated-with-the-service-doesnt-match-requested-authentication-method"></a>El método de autenticación mediante el cual se autenticó el usuario en el servicio no coincide con el método de autenticación solicitado.
`Error: AADSTS75011 Authentication method by which the user authenticated with the service doesn't match requested authentication method 'AuthnContextClassRef'. `

**Causa posible**

`RequestedAuthnContext` está en la solicitud SAML. Esto significa que la aplicación espera el elemento `AuthnContext` especificado por `AuthnContextClassRef`. Sin embargo, el usuario ya se ha autenticado antes de tener acceso a la aplicación y el elemento `AuthnContext` (método de autenticación) usado para esa autenticación anterior es diferente del que se solicita. Por ejemplo, se produjo un acceso de usuario federado a mis aplicaciones y WIA. El elemento `AuthnContextClassRef` será `urn:federation:authentication:windows`. AAD no realizará una solicitud de autenticación nueva, utilizará el contexto de autenticación que pasó a través del IdP (ADFS o cualquier otro servicio de federación en este caso). Por lo tanto, se producirá un error de coincidencia si la aplicación solicita algo diferente de `urn:federation:authentication:windows`. Otro escenario es cuando se usó MultiFactor: `'X509, MultiFactor`.

**Resolución**


`RequestedAuthnContext` es un valor opcional. A continuación, si es posible, pregunte a la aplicación si se puede quitar.

Otra opción consiste en asegurarse de que se respetará `RequestedAuthnContext`. Esto se realizará solicitando una nueva autenticación. Al hacerlo, cuando se procese la solicitud SAML, se realizará una nueva autenticación y se respetará `AuthnContext`. Para solicitar una nueva autenticación, la solicitud de SAML contiene el valor `forceAuthn="true"`. 



## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Problema al personalizar las notificaciones SAML que se han enviado a una aplicación
Para obtener información sobre cómo personalizar las notificaciones de atributo SAML que se han enviado a su aplicación, vea [Asignación de notificaciones en Azure Active Directory](../develop/active-directory-claims-mapping.md).

## <a name="errors-related-to-misconfigured-apps"></a>Errores relacionados con aplicaciones mal configuradas
Compruebe que las configuraciones en el portal coinciden con lo que tiene en la aplicación. En concreto, compare el identificador del cliente o aplicación, las direcciones URL de respuesta, las claves y secretos de cliente, y el URI del identificador de la aplicación.

Compare el recurso del que está solicitando acceso mediante código con los permisos configurados en la pestaña **Recursos necesarios** para asegurarse de que solo se solicitan los recursos que ha configurado.

## <a name="next-steps"></a>Pasos siguientes
- [Serie de guías de inicio rápido sobre la administración de aplicaciones](add-application-portal-assign-users.md)
- [Cómo depurar el inicio de sesión único basado en SAML en aplicaciones de Azure AD](./debug-saml-sso-issues.md)
- [Los requisitos del protocolo SAML de inicio de sesión único de Azure AD](../develop/single-sign-on-saml-protocol.md)