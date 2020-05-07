---
title: 'Publicación de aplicaciones cliente nativas: Azure AD | Microsoft Docs'
description: Explica cómo habilitar las aplicaciones cliente nativas para comunicarse con el conector del proxy de la aplicación de Azure AD para proporcionar acceso remoto seguro a las aplicaciones locales.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro, has-adal-ref
ms.collection: M365-identity-device-management
ms.openlocfilehash: a649a6fab1fe85efc4edcfd2d3151ab85302101b
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610282"
---
# <a name="how-to-enable-native-client-applications-to-interact-with-proxy-applications"></a>Habilitación de las aplicaciones cliente nativas para interactuar con el proxy de aplicaciones

Puede usar Azure Active Directory (Azure AD) Application Proxy para publicar aplicaciones web, pero también se puede utilizar para publicar las aplicaciones cliente nativas que se configuran con la Biblioteca de autenticación de Azure AD (ADAL). Las aplicaciones cliente nativas son distintas de las aplicaciones web porque se instalan en un dispositivo, mientras a las aplicaciones web se accede mediante un explorador.

Para admitir aplicaciones cliente nativas, Application Proxy acepta tokens emitidos por Azure AD que se envían en el encabezado. El servicio Application Proxy realiza la autenticación para los usuarios. Esta solución no utiliza tokens de aplicación para la autenticación.

![Relación entre los usuarios finales, Azure AD y las aplicaciones publicadas](./media/application-proxy-configure-native-client-application/richclientflow.png)

Utilice la biblioteca de Autenticación de Azure AD, que se encarga de la autenticación y admite muchos de los entornos de cliente, para publicar aplicaciones nativas. El proxy de la aplicación se adapta a la [aplicación nativa para el escenario de Web API](../azuread-dev/native-app.md).

Este artículo le guiará por los cuatro pasos para publicar una aplicación nativa con el proxy de aplicación y la biblioteca de Autenticación de Azure AD.

## <a name="step-1-publish-your-proxy-application"></a>Paso 1: Publique su aplicación de proxy

Publique su aplicación de proxy al igual que haría con cualquier otra aplicación y asigne a los usuarios el acceso a la aplicación. Para más información, consulte [Publicación de aplicaciones mediante el proxy de aplicación de Azure AD](application-proxy-add-on-premises-application.md).

## <a name="step-2-register-your-native-application"></a>Paso 2: Registre la aplicación nativa

Ahora debe registrar la aplicación en Azure AD:

1. Inicie sesión en el [portal de Azure Active Directory](https://aad.portal.azure.com/). Aparecerá el **panel** del **Centro de administración de Azure Active Directory**.
1. En la barra lateral, seleccione **Azure Active Directory**. Se mostrará la página de introducción de **Azure Active Directory**.
1. En la barra lateral de introducción de Azure AD, seleccione **Registros de aplicaciones**. Aparece la lista de todos los registros de aplicaciones.
1. Seleccione **Nuevo registro**. Se muestra la página **Registrar una aplicación**.

   ![Creación de un nuevo registro de aplicaciones en Azure Portal](./media/application-proxy-configure-native-client-application/create.png)

1. En el encabezado **Nombre**, especifique un nombre para mostrar a los usuarios en la aplicación.
1. En el encabezado **Tipos de cuentas admitidos**, seleccione un nivel de acceso mediante estas instrucciones:

   - Para seleccionar como destino únicamente las cuentas que son internas de su organización, seleccione **Solo las cuentas de este directorio organizativo**.
   - Seleccione **Cuentas en cualquier directorio organizativo** si desea tener como destino todos los clientes de negocios y del sector educativo.
   - Para establecer como destino el mayor conjunto posible de clientes, seleccione **Cuentas en cualquier directorio organizativo y cuentas Microsoft personales**.

1. En el encabezado **URI de redireccionamiento**, seleccione **Cliente público (móvil y de escritorio)** y, a continuación, escriba el URI de redireccionamiento de su aplicación.
1. Seleccione y lea las **directivas de la plataforma de Microsoft** y, a continuación, seleccione **Registrar**. Se crea el registro de aplicación y se muestra la página de introducción.

Para obtener más información sobre cómo crear un registro de aplicación, consulte [Integración de aplicaciones con Azure Active Directory](../develop/quickstart-register-app.md).

## <a name="step-3-grant-access-to-your-proxy-application"></a>Paso 3: Conceda acceso a la aplicación de proxy

Ahora que ha registrado su aplicación nativa, puede darle acceso a otras aplicaciones de su directorio, en este caso, para tener acceso a la aplicación de proxy. Para permitir que la aplicación nativa se exponga en el proxy de aplicación:

1. En la barra lateral de la nueva página de registro de aplicaciones, seleccione **Permisos de API**. Se muestra la página **Permisos de API** del nuevo registro de aplicaciones.
1. Seleccione **Agregar un permiso**. Aparece la página **Solicitud de permisos de API**.
1. En el valor **Seleccionar una API**, elija **API usadas en mi organización**. Aparece una lista que contiene las aplicaciones del directorio que exponen las API.
1. Escriba en el cuadro de búsqueda o desplácese para buscar el proxy de aplicación que publicó en [Paso 1: Publique su aplicación de proxy](#step-1-publish-your-proxy-application) y, a continuación, seleccione el proxy de aplicación.
1. En el encabezado **¿Qué tipo de permiso necesita la aplicación web?** , seleccione el tipo de permiso. Si su aplicación nativa necesita acceder a la API de proxy de aplicación como el usuario que inició sesión, elija **Permisos delegados**.
1. En el encabezado **Seleccionar permisos**, seleccione el permiso deseado y seleccione **Agregar permisos**. La página **Permisos de API** de su aplicación nativa ahora muestra el proxy de aplicación y el permiso de API que ha agregado.

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Paso 4: Editar la biblioteca de autenticación de Active Directory

Edite el código de la aplicación nativa en el contexto de autenticación de Active Directory Authentication Library (ADAL) para incluir el siguiente texto:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = await authContext.AcquireTokenAsync("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

La información necesaria del código de ejemplo puede encontrarse en el portal de Azure AD:

| Información necesaria | Cómo encontrarla en el portal de Azure AD |
| --- | --- |
| \<Identificador de inquilino> | **Azure Active Directory** > **Propiedades** > **Id. de directorio** |
| \<Dirección URL externa del proxy de aplicación> | **Aplicaciones empresariales** > *su aplicación de proxy* > **Proxy de aplicación** > **Dirección URL externa** |
| \<Identificador de aplicación de la aplicación nativa> | **Aplicaciones empresariales** > *su aplicación nativa* > **Propiedades** > **Id. de aplicación** |
| \<URI de redireccionamiento de la aplicación nativa> | **Azure Active Directory** > **Registros de aplicaciones** > *su aplicación nativa* > **URI de redirección** |
| \<Dirección URL de la API de aplicación de proxy> | **Azure Active Directory** > **Registros de aplicaciones** > *su aplicación nativa* > **Permisos de API** > **Nombre de permisos/API** |

Cuando se edita la ADAL con estos parámetros, los usuarios podrán autenticarse en las aplicaciones cliente nativas incluso si están fuera de la red corporativa.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el flujo de la aplicación nativa, consulte [Aplicaciones nativas en Azure Active Directory](../azuread-dev/native-app.md).

Obtenga información sobre la configurar el [inicio de sesión único en aplicaciones de Azure Active Directory](what-is-single-sign-on.md#choosing-a-single-sign-on-method).
