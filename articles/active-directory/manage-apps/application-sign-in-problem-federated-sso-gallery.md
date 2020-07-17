---
title: Problemas al iniciar sesión en una aplicación de la galería de inicio de sesión único federado | Microsoft Docs
description: Instrucciones para resolver errores específicos al iniciar sesión en una aplicación que se ha configurado para un inicio de sesión único federado basado en SAML con Azure AD
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 02/18/2019
ms.author: kenwith
ms.reviewer: luleon, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68dc90cdb096849df17bc25ac185b1239b46ec72
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85413151"
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a>Problemas al iniciar sesión en una aplicación de la galería configurada para inicio de sesión único federado

Para solucionar los problemas de inicio de sesión, se recomienda que siga estas sugerencias para obtener un mejor diagnóstico y automatizar los pasos de resolución:

- Instale la [extensión del explorador seguro de Mis aplicaciones](access-panel-extension-problem-installing.md) para ayudar a Azure Active Directory (Azure AD) a proporcionar mejores diagnósticos y resoluciones al usar la experiencia de pruebas en Azure Portal.
- Reproduzca el error mediante la experiencia de pruebas en la página de configuración de aplicaciones en Azure Portal. Obtenga más información en [Depuración del inicio de sesión único basado en SAML en aplicaciones](../azuread-dev/howto-v1-debug-saml-sso-issues.md).


## <a name="application-not-found-in-directory"></a>No se encontró la aplicación en el directorio

*Error AADSTS70001: No se encontró la aplicación con el identificador "https:\//contoso.com" en el directorio*.

**Causa posible**

El atributo `Issuer` que se envía de la aplicación a Azure AD en la solicitud SAML no coincide con el valor de identificador configurado para la aplicación de Azure AD.

**Resolución**

Asegúrese de que el atributo `Issuer` de la solicitud SAML coincide con el valor del identificador configurado en Azure AD. Si usa la [experiencia de pruebas](../azuread-dev/howto-v1-debug-saml-sso-issues.md) en Azure Portal con la extensión del explorador seguro de Mis aplicaciones, no tiene que seguir estos pasos manualmente.

1.  Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

1.  Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

1.  Escriba **"Azure Active Directory"** en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

1.  Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

1.  Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

    Si no ve la aplicación que desea que aparezca aquí, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

1.  Seleccione la aplicación que desea configurar para el inicio de sesión único.

1.  Cuando se carga la aplicación, abra **Configuración básica de SAML**. Compruebe que el valor del cuadro de texto Identificador coincide con el valor del identificador mostrado en el error.



## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>La dirección de respuesta no coincide con las direcciones de respuesta configuradas para la aplicación

*Error AADSTS50011: La dirección de respuesta "https:\//contoso.com" no coincide con las direcciones de respuesta configuradas para la aplicación*

**Causa posible**

El valor de `AssertionConsumerServiceURL` en la solicitud SAML no coincide con el valor o patrón de la dirección URL de respuesta configurada en Azure AD. El valor de `AssertionConsumerServiceURL` en la solicitud SAML es la dirección URL que aparece en el error.

**Resolución**

Asegúrese de que el valor de `AssertionConsumerServiceURL` en la solicitud SAML coincide con el valor de la dirección URL de respuesta configurada en Azure AD. Si usa la [experiencia de pruebas](../azuread-dev/howto-v1-debug-saml-sso-issues.md) en Azure Portal con la extensión del explorador seguro de Mis aplicaciones, no tiene que seguir estos pasos manualmente.

1.  Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

1.  Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

1.  Escriba **"Azure Active Directory"** en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

1.  Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

1.  Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

    Si no ve la aplicación que desea que aparezca aquí, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

1.  Seleccione la aplicación que desea configurar para el inicio de sesión único.

1.  Cuando se carga la aplicación, abra **Configuración básica de SAML**. Compruebe o actualice el valor en el cuadro de texto Dirección URL de respuesta para que coincida con el valor de `AssertionConsumerServiceURL` en la solicitud SAML.    
    
Después de actualizar el valor de la dirección URL de respuesta en Azure AD y de comprobar que coincide con el valor enviado por la aplicación en la solicitud SAML, podrá iniciar sesión en la aplicación.

## <a name="user-not-assigned-a-role"></a>Usuario no asignado a un rol

*Error AADSTS50105: El usuario con sesión iniciada "brian\@contoso.com" no está asignado a un rol de la aplicación*.

**Causa posible**

El usuario no tiene acceso a la aplicación en Azure AD.

**Resolución**

Para asignar uno o varios usuarios a una aplicación directamente, siga los pasos siguientes. Si usa la [experiencia de pruebas](../azuread-dev/howto-v1-debug-saml-sso-issues.md) en Azure Portal con la extensión del explorador seguro de Mis aplicaciones, no tiene que seguir estos pasos manualmente.

1.  Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global**.

1.  Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

1.  Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

1.  Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

1.  Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

    Si no ve la aplicación que desea que aparezca aquí, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

1.  En la lista de aplicaciones, seleccione la que desea asignar a un usuario.

1.  Cuando se cargue la aplicación, seleccione **Usuarios y grupos** en el menú de navegación izquierdo de la aplicación.

1.  Haga clic en el botón **Agregar** en la parte superior de la lista **Usuarios y grupos** para abrir el panel **Agregar asignación**.

1.  Seleccione el selector **Usuarios y grupos** del panel **Agregar asignación**.

1. En el cuadro de búsqueda **Buscar por nombre o dirección de correo**, escriba el nombre completo o dirección de correo electrónico del usuario que desea agregar.

1. Mantenga el puntero sobre el **usuario** en la lista para que aparezca una **casilla**. Haga clic en la casilla situada junto a la foto o el logotipo del perfil del usuario para agregar ese usuario a la lista **Seleccionados**.

1. **Opcional:** si desea **agregar más de un usuario**, escriba otro nombre completo u otra dirección de correo electrónico en el cuadro de búsqueda **Buscar por nombre o dirección de correo** y haga clic en la casilla para agregar ese usuario a la lista **Seleccionados**.

1. Cuando haya terminado de seleccionar usuarios, haga clic en el botón **Seleccionar** para agregarlos a la lista de usuarios y grupos que se asignarán a la aplicación.

1. **Opcional:** haga clic en el selector **Seleccionar rol** del panel **Agregar asignación** para seleccionar un rol que se asignará a los usuarios que ha seleccionado.

1. Haga clic en el botón **Asignar** para asignar la aplicación a los usuarios seleccionados.

Tras un breve período, los usuarios seleccionados podrán iniciar estas aplicaciones mediante los métodos descritos en la sección de descripción de la solución.

## <a name="not-a-valid-saml-request"></a>No es una solicitud SAML válida

*Error AADSTS75005: La solicitud no es un mensaje de protocolo Saml2 válido.*

**Causa posible**

Azure AD no admite la solicitud SAML enviada por la aplicación para el inicio de sesión único. Algunos de los problemas más comunes son:

-   Faltan campos obligatorios en la solicitud SAML
-   Método codificado de la solicitud SAML

**Resolución**

1. Capture la solicitud SAML. Siga el tutorial de [Cómo depurar el inicio de sesión único basado en SAML en aplicaciones de Azure AD](../azuread-dev/howto-v1-debug-saml-sso-issues.md) para obtener información sobre cómo capturar la solicitud SAML.

1. Póngase en contacto con el proveedor de la aplicación y comparta la información siguiente:

   -   La solicitud SAML

   -   [Los requisitos del protocolo SAML de inicio de sesión único de Azure AD](../develop/single-sign-on-saml-protocol.md)

El proveedor de la aplicación debe validar que admite la implementación de SAML de Azure AD para inicio de sesión único.

## <a name="misconfigured-application"></a>Aplicación mal configurada

*Error AADSTS650056: Aplicación mal configurada. Esto puede deberse a uno de los siguientes motivos: El cliente no ha enumerado ningún permiso en los permisos solicitados en el registro de aplicación del cliente. O bien el administrador no ha dado su consentimiento en el inquilino. Compruebe también el identificador de la aplicación en la solicitud para asegurarse de que coincide con el identificador de la aplicación cliente configurada. Póngase en contacto con su administrador para corregir la configuración o dar su consentimiento en nombre del inquilino.* .

**Causa posible**

El atributo `Issuer` que se envía de la aplicación a Azure AD en la solicitud SAML no coincide con el valor de identificador configurado para la aplicación de Azure AD.

**Resolución**

Asegúrese de que el atributo `Issuer` de la solicitud SAML coincide con el valor del identificador configurado en Azure AD. Si usa la [experiencia de pruebas](../azuread-dev/howto-v1-debug-saml-sso-issues.md) en Azure Portal con la extensión del explorador seguro de Mis aplicaciones, no tiene que seguir estos pasos manualmente:

1.  Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

1.  Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

1.  Escriba **"Azure Active Directory"** en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

1.  Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

1.  Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

    Si no ve la aplicación que desea que aparezca aquí, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

1.  Seleccione la aplicación que desea configurar para el inicio de sesión único.

1.  Cuando se carga la aplicación, abra **Configuración básica de SAML**. Compruebe que el valor del cuadro de texto Identificador coincide con el valor del identificador mostrado en el error.


## <a name="certificate-or-key-not-configured"></a>Certificado o clave no configurados

*Error AADSTS50003: No hay configurada ninguna clave de firma.*

**Causa posible**

El objeto de aplicación está dañado y Azure AD no reconoce el certificado configurado para la aplicación.

**Resolución**

Para eliminar y crear un nuevo certificado, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

1. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

1. Escriba **"Azure Active Directory"** en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

1. Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

1. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

    Si no ve la aplicación que desea que aparezca aquí, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

1. Seleccionar la aplicación que desea configurar para el inicio de sesión único

1. Cuando se cargue la aplicación, haga clic en **Inicio de sesión único** desde el menú de navegación izquierdo de la aplicación.

1. Seleccione **Crear nuevo certificado** en la sección **Certificado de firma de SAML**.

1. Seleccione la fecha de expiración y, a continuación, haga clic en **Guardar**.

1. Active **Activar el certificado nuevo** para reemplazar el certificado activo. Después, haga clic en **Guardar** en la parte superior del panel y en Aceptar para activar el certificado de sustitución.

1. En la sección **Certificado de firma de SAML**, haga clic en **Quitar** para quitar el certificado **no usado**.

## <a name="saml-request-not-present-in-the-request"></a>La solicitud SAML no existe en la solicitud

*Error AADSTS750054: SAMLRequest o SAMLResponse deben existir como parámetros de cadena de consulta en la solicitud HTTP para el enlace de redirección SAML.*

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

1.  Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador**.

2.  Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3.  Escriba **"Azure Active Directory"** en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4.  Seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

5.  Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

    Si no ve la aplicación que desea que aparezca aquí, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones**.

6.  Seleccione la aplicación que desea configurar para el inicio de sesión único.

7.  Cuando se carga la aplicación, abra **Configuración básica de SAML**. En **URL de respuesta (URL del Servicio de consumidor de aserciones)** , elimine las direcciones URL de respuesta no utilizadas o predeterminadas creadas por el sistema. Por ejemplo, `https://127.0.0.1:444/applications/default.aspx`.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Problema al personalizar las notificaciones SAML que se han enviado a una aplicación

Para obtener información sobre cómo personalizar las notificaciones de atributo SAML que se han enviado a su aplicación, vea [Asignación de notificaciones en Azure Active Directory](../develop/active-directory-claims-mapping.md).

## <a name="next-steps"></a>Pasos siguientes

[Cómo depurar el inicio de sesión único basado en SAML en aplicaciones de Azure AD](../azuread-dev/howto-v1-debug-saml-sso-issues.md)
