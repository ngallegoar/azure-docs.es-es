---
title: Nueva guía de aprendizaje de los registros de aplicaciones de Azure Portal
description: Presenta la nueva experiencia de registro de aplicaciones de Azure Portal.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 10/25/2019
ms.author: marsma
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.openlocfilehash: f72e16afd61d364a2dad634f3502c801f1b99380
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85478151"
---
# <a name="new-azure-portal-app-registration-training-guide"></a>Nueva guía de aprendizaje de los registros de aplicaciones de Azure Portal

La nueva experiencia de [registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de Azure Portal presenta numerosas mejoras. Si está familiarizado con la experiencia de registros de aplicaciones (heredada) de Azure Portal, use esta guía de aprendizaje para empezar a usar la nueva experiencia.

En Azure Active Directory, la nueva experiencia de registro de aplicaciones que se describe aquí está disponible con carácter general (GA). En Azure Active Directory B2C (Azure AD B2C), esta experiencia está en versión preliminar.

## <a name="key-changes"></a>Cambios clave

- Los registros de aplicaciones no son solo una *aplicación web o API* o una aplicación *nativa*. Puede usar el mismo registro de aplicación para todas ellas mediante el registro de los respectivos URI de redirección.

- La experiencia heredada admitía aplicaciones que inician sesión solo con cuentas organizativas (Azure AD). Las aplicaciones se registraban con un solo inquilino. Solo admitían cuentas organizativas del directorio en el que se registraba la aplicación. Podían modificarse para ser multiinquilino y admitir todas las cuentas organizativas. La nueva experiencia le permite registrar aplicaciones que admiten ambas opciones, así como una tercera opción: todas las cuentas de organización así como las cuentas de Microsoft personales.

- La experiencia heredada solo estaba disponible cuando se había iniciado sesión en Azure Portal con una cuenta de organización. Con la nueva experiencia, puede usar cuentas Microsoft personales que no están asociadas a un directorio.

## <a name="list-of-applications"></a>Lista de aplicaciones

La nueva lista de aplicaciones muestra las aplicaciones que se registraron mediante la experiencia de registros de aplicaciones heredada de Azure Portal. Estas aplicaciones inician sesión mediante cuentas de Azure AD. La nueva lista de aplicaciones también muestra las aplicaciones registradas mediante el portal de registro de aplicaciones. Estas aplicaciones inician sesión con Azure AD y cuentas Microsoft personales.

>[!NOTE]
>El portal de registro de aplicaciones está en desuso.

La nueva lista de aplicaciones no tiene una columna **Tipo de aplicación** porque un solo registro de aplicación puede ser de varios tipos. La lista tiene dos columnas adicionales: **Fecha de creación** y **Certificados y secretos**. En **Certificados y secretos** se muestra el estado de las credenciales que se han registrado en la aplicación. Los estados son **Actual**, **Expira próximamente** y **Expirado**.

## <a name="new-app-registration"></a>Nuevo registro de aplicaciones

En la experiencia heredada, para registrar una aplicación era necesario proporcionar lo siguiente: **Nombre**, **Tipo de aplicación** y **URL de inicio de sesión/URI de redirección**. Las aplicaciones que se creaban eran solo aplicaciones de Azure AD de un solo inquilino. Solo admitían cuentas organizativas desde el directorio en el que se registraba la aplicación.

En la nueva experiencia, debe proporcionar un **nombre** para la aplicación y elegir los **tipos de cuenta admitidos**. También puede proporcionar un **URI de redirección**. Si proporciona un URI de redirección, tendrá que especificar si es web o público (móvil y escritorio). Para más información, consulte [Inicio rápido: Registro de una aplicación en la plataforma de identidad de Microsoft](quickstart-register-app.md). En el caso de Azure AD B2C, vea [Registro de una aplicación en Azure Active Directory B2C](../../active-directory-b2c/tutorial-register-applications.md).

## <a name="differences-between-the-application-registration-portal-and-app-registrations-page"></a>Diferencias entre el portal de registro de aplicaciones y la página Registros de aplicaciones

### <a name="the-legacy-properties-page"></a>Página de propiedades heredada

La experiencia heredada tenía una página **Propiedades**. Esta página tenía los siguientes campos:

- **Nombre**
- **Id. de objeto**
- **Identificador de la aplicación**
- **URI de id. de aplicación**
- **Logotipo**
- **URL de página principal**
- **Dirección URL de cierre de sesión**
- **URL de las condiciones del servicio**
- **URL de la declaración de privacidad**
- **Tipo de aplicación**
- **Multiinquilino**

La nueva experiencia no tiene esa página. Aquí es donde puede encontrar la funcionalidad equivalente:

- **Nombre**, **Logotipo**, **URL de página principal**, **URL de las condiciones del servicio** y **URL de la declaración de privacidad** ahora están en la página **Personalización de marca**.
- **Id. de objeto** e **Id. de aplicación (cliente)** se encuentran en la página **Información general**.
- La funcionalidad controlada por el botón **Multiempresa** de la experiencia heredada se reemplazó por la opción **Supported account types** (Tipos de cuenta admitidos) de la página **Autenticación**. Para más información, consulte [Inicio rápido: Modificación de las cuentas compatibles con una aplicación](quickstart-modify-supported-accounts.md).
- **URL de cierre de sesión** ahora está en la página **Autenticación**.
- **Tipo de aplicación** ya no es un campo válido. En su lugar, los URI de redirección, que puede encontrar en la página **Autenticación**, determinan qué tipos de aplicaciones se admiten.
- **URI de id. de aplicación** lo puede encontrar ahora en **Exponer una API**. En la experiencia heredada, esta propiedad se registraba automáticamente con el formato siguiente: `https://{tenantdomain}/{appID}` (por ejemplo, `https://microsoft.onmicrosoft.com/492439af-3282-44c3-b297-45463339544b`). En la nueva experiencia, se genera automáticamente como `api://{appID}`, pero se debe guardar de forma explícita. En inquilinos de Azure AD B2C, se sigue usando el formato `https://{tenantdomain}/{appID}`.

### <a name="reply-urlsredirect-urls"></a>URL de respuesta y URI de redirección

En la experiencia heredada, una aplicación tenía una página **URL de redirección**. En la nueva experiencia, las direcciones URL de respuesta se encuentran en la página **Autenticación** de una aplicación. Ahora se conocen como **URI de redirección**.

El formato de los URI de redirección ha cambiado. Deben estar asociados con un tipo de aplicación: web o pública. Por motivos de seguridad, no se admiten comodines y esquemas `http://`, salvo *http://localhost* .

### <a name="keyscertificates--secrets"></a>Claves y certificados y secretos

En la experiencia heredada, una aplicación tenía una página **Claves**. En la nueva experiencia, su nombre se cambió a **Certificados y secretos**.

Las **claves públicas** ahora se conocen como **certificados**. Las **contraseñas** se conocen ahora como **secretos de cliente**.

### <a name="required-permissionsapi-permissions"></a>Permisos obligatorios y permisos de API

En la experiencia heredada, una aplicación tenía una página **Permisos necesarios**. En la nueva experiencia, el nombre cambió a **Permisos de API**.

Al seleccionar una API en la experiencia heredada, podía elegir entre una pequeña lista de API de Microsoft. También podía buscar a través de las entidades de servicio del inquilino. En la nueva experiencia, puede elegir entre varias pestañas: **API de Microsoft**, **API usadas en mi organización** o **Mis API**. La barra de búsqueda de la pestaña **API usadas en mi organización** busca por entidades de servicio en el inquilino.

> [!NOTE]
> Si la aplicación no está asociada a un inquilino, no verá esta pestaña. Para más información sobre cómo solicitar permisos, consulte [Inicio rápido: Configuración de una aplicación cliente para tener acceso a las API web](quickstart-configure-app-access-web-apis.md).

La experiencia heredada tenía un botón **Conceder permisos** situado en la parte superior de la página **Permisos solicitados**. En la nueva experiencia, la página **Otorgar consentimiento** tiene un botón **Conceder consentimiento del administrador** en la sección **Permisos de API** de una aplicación. Además, hay algunas diferencias en el funcionamiento de los botones.

En la experiencia heredada, la lógica variaba según el usuario que había iniciado sesión y los permisos que se solicitan. La lógica era la siguiente:

- Si solo se solicitaban permisos para el consentimiento del usuario y el usuario que había iniciado sesión no era administrador, el usuario podía conceder consentimiento del usuario para los permisos solicitados.
- Si se solicitaba al menos un permiso que requería el consentimiento del administrador y el usuario que había iniciado sesión no era administrador, el usuario recibía un error al intentar conceder consentimiento.
- Si el usuario que había iniciado sesión era administrador, se concedía el consentimiento del administrador para todos los permisos solicitados.

En la nueva experiencia, solo un administrador puede conceder consentimiento. Cuando un administrador selecciona **Conceder consentimiento del administrador**, el consentimiento del administrador se concede a todos los permisos solicitados.

## <a name="deleting-an-app-registration"></a>Eliminar un registro de aplicación

En la experiencia heredada, solo se podían eliminar aplicaciones de un inquilino. El botón Eliminar estaba deshabilitado para aplicaciones multiempresa. En la nueva experiencia, las aplicaciones se pueden eliminar en cualquier estado, pero debe confirmar la acción. Para más información, consulte [Inicio rápido: Eliminación de una aplicación registrada con la plataforma de identidad de Microsoft](quickstart-remove-app.md).

## <a name="application-manifest"></a>Manifiesto de aplicación

La experiencia heredada y las nuevas usan versiones diferentes para el formato de JSON en el editor de manifiestos. Para más información, consulte [Manifiesto de aplicación de Azure Active Directory](reference-app-manifest.md).

## <a name="new-ui"></a>Nueva interfaz de usuario

La nueva experiencia agrega controles de interfaz de usuario para las siguientes propiedades:

- La página **Autenticación** tiene **flujo de concesión implícito** (`oauth2AllowImplicitFlow`). A diferencia de la experiencia heredada, puede habilitar **Tokens de acceso**, **Tokens de id.** o ambos.
- La página **Exponer una API** contiene **Ámbitos definidos con esta API** (`oauth2Permissions`) y **Aplicaciones cliente autorizadas** (`preAuthorizedApplications`). Para más información sobre cómo configurar una aplicación para que sea una API web y exponer permisos o ámbitos, consulte [Inicio rápido: Configuración de una aplicación para exponer las API web](quickstart-configure-app-expose-web-apis.md).
- La página **Personalización de marca** contiene la opción **Dominio del publicador**. El dominio del publicador se muestra a los usuarios en el [mensaje de consentimiento de la aplicación](application-consent-experience.md). Para más información, vea: [Cómo: Configuración del dominio de editor de una aplicación](howto-configure-publisher-domain.md).

## <a name="limitations"></a>Limitaciones

La nueva experiencia tiene las siguientes limitaciones:

- El formato de los secretos de cliente (contraseñas de aplicación) es diferente que el de la experiencia heredada y puede interrumpir la CLI.
- En la interfaz de usuario no se admite el cambio del valor para las cuentas admitidas. Debe usar el manifiesto de aplicación, a menos que se va a cambiar entre Azure AD de inquilino único y multiempresa.
