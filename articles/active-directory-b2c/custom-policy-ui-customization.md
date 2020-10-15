---
title: Personalización de la interfaz de usuario de la aplicación con una directiva personalizada
titleSuffix: Azure AD B2C
description: Obtenga información acerca de cómo personalizar una interfaz de usuario mediante una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: cb833ff35dae4fe1c0c27204ec66fa6b4cdb82c7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85388891"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Al completar los pasos de este artículo, creará una directiva personalizada de registro e inicio de sesión con su marca y apariencia. Con Azure Active Directory B2C (Azure AD B2C), controlará prácticamente todo el contenido HTML y CSS que se presenta a los usuarios. Cuando se usa una directiva personalizada, la personalización de la interfaz del usuario se configura en XML en lugar de con controles de Azure Portal.

## <a name="prerequisites"></a>Prerrequisitos

Realice los pasos del artículo [Introducción a las directivas personalizadas](custom-policy-get-started.md). Debe tener una directiva personalizada activa para registrar e iniciar sesión de cuentas locales.

[!INCLUDE [active-directory-b2c-html-how-to](../../includes/active-directory-b2c-html-how-to.md)]

### <a name="4-modify-the-extensions-file"></a>4. Modificación del archivo de extensiones

Para configurar la personalización de la interfaz de usuario, copie **ContentDefinition** y sus elementos secundarios del archivo base en el archivo de extensiones.

1. Abra el archivo base de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/` **`TrustFrameworkBase.xml`** </em>. Este archivo base es uno de los archivos de directivas incluidos en el paquete de inicio de directivas personalizadas, que debería haber obtenido en el requisito previo, [Introducción a las directivas personalizadas en Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom).
1. Busque y copie todo el contenido del elemento **ContentDefinitions**.
1. Abra el archivo de extensión. Por ejemplo, *TrustFrameworkExtensions.xml*. Busque el elemento **BuildingBlocks**. Si el elemento no existe, agréguelo.
1. Pegue todo el contenido del elemento **ContentDefinitions** que ha copiado como elemento secundario del elemento **BuildingBlocks**.
1. Busque el elemento **ContentDefinition** que contenga `Id="api.signuporsignin"` en el XML que ha copiado.
1. Cambie el valor de **LoadUri** a la dirección URL del archivo HTML que cargó en el almacenamiento. Por ejemplo, `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.

    La directiva personalizada debería ser similar al fragmento de código siguiente:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

1. Guarde el archivo de extensiones.

### <a name="5-upload-and-test-your-updated-custom-policy"></a>5. Carga y prueba de la directiva personalizada actualizada

#### <a name="51-upload-the-custom-policy"></a>5.1 Carga de la directiva personalizada

1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Busque y seleccione **Azure AD B2C**.
1. En **Directivas**, seleccione **Identity Experience Framework**.
1. Seleccione **Cargar directiva personalizada**.
1. Cargue el archivo de extensiones que cambió anteriormente.

#### <a name="52-test-the-custom-policy-by-using-run-now"></a>5.2 Prueba de la directiva personalizada con **Ejecutar ahora**

1. Seleccione la directiva cargada y, a continuación, **Ejecutar ahora**.
1. Debería poder registrarse con una dirección de correo electrónico.

[!INCLUDE [active-directory-b2c-html-templates](../../includes/active-directory-b2c-html-templates.md)]

## <a name="configure-dynamic-custom-page-content-uri"></a>Configuración del URI de contenido de página personalizada dinámica

Con las directivas personalizadas de Azure AD B2C, puede enviar un parámetro de la ruta de la dirección URL o una cadena de consulta. Al pasar dicho parámetro al punto de conexión HTML, puede cambiar de forma dinámica el contenido de la página. Por ejemplo, puede cambiar la imagen de fondo en la página de inicio de sesión o de registro de Azure AD B2C en función de un parámetro que se pasa desde la aplicación web o dispositivo móvil. El parámetro puede ser cualquier [solucionador de notificaciones](claim-resolver-overview.md), como el identificador de la aplicación, el identificador de idioma o el parámetro de cadena de consulta personalizada, como `campaignId`.

### <a name="sending-query-string-parameters"></a>Envío de parámetros de cadena de consulta

Para enviar parámetros de cadena de consulta, en la [directiva del usuario de confianza](relyingparty.md), agregue un elemento `ContentDefinitionParameters` tal como se muestra a continuación.

```xml
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <UserJourneyBehaviors>
    <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        <Parameter Name="lang">{Culture:LanguageName}</Parameter>
        <Parameter Name="appId">{OIDC:ClientId}</Parameter>
    </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    ...
</RelyingParty>
```

En la definición de contenido, cambie el valor de `LoadUri` a `https://<app_name>.azurewebsites.net/home/unified`. La directiva personalizada `ContentDefinition` debería ser similar al fragmento de código siguiente:

```xml
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://<app_name>.azurewebsites.net/home/unified</LoadUri>
  ...
</ContentDefinition>
```

Cuando Azure AD B2C carga la página, realiza una llamada al punto de conexión del servidor web:

```http
https://<app_name>.azurewebsites.net/home/unified?campaignId=123&lang=fr&appId=f893d6d3-3b6d-480d-a330-1707bf80ebea
```

### <a name="dynamic-page-content-uri"></a>URI de contenido de página dinámica

Puede extraer contenido de distintos lugares, según los parámetros utilizados. En el punto de conexión habilitado para CORS, configure una estructura de carpetas para hospedar contenido. Por ejemplo, puede organizar el contenido en la estructura siguiente. Raíz *carpeta/carpeta por idioma/sus archivos html*. Por ejemplo, el URI de la página personalizada podría ser similar al siguiente:

```xml
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>https://contoso.blob.core.windows.net/{Culture:LanguageName}/myHTML/unified.html</LoadUri>
  ...
</ContentDefinition>
```

Azure AD B2C envía el código ISO de dos letras para el idioma, `fr` para francés:

```http
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los elementos de la interfaz de usuario que se pueden personalizar, consulte la [guía de referencia para la personalización de la interfaz de usuario para los flujos de usuario](customize-ui-overview.md).
