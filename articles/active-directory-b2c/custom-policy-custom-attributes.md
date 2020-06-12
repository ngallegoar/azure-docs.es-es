---
title: Incorporación de atributos a las directivas personalizadas
titleSuffix: Azure AD B2C
description: Un tutorial sobre cómo usar las propiedades de extensión y los atributos personalizados, y cómo incluirlos en la interfaz de usuario.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 15623d622a40fa80889a8704e0b7f19d358a052c
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84295412"
---
# <a name="azure-active-directory-b2c-enable-custom-attributes-in-a-custom-profile-policy"></a>Azure Active Directory B2C: Habilitación de los atributos personalizados en una directiva de perfil personalizada

En el artículo [Adición de notificaciones y personalización de la entrada del usuario mediante directivas personalizadas](custom-policy-configure-user-input.md) aprendió a usar los [atributos de perfil de usuario](user-profile-attributes.md) integrados. En este artículo, habilitará un atributo personalizado en el directorio de Azure Active Directory B2C (Azure AD B2C). Más adelante, puede usar el nuevo atributo como una notificación personalizada en los [flujos de usuario](user-flow-overview.md) o las [directivas personalizadas](custom-policy-get-started.md) simultáneamente.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="prerequisites"></a>Requisitos previos

Siga los pasos que aparecen en el artículo [Azure Active Directory B2C: introducción a las directivas personalizadas](custom-policy-get-started.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers"></a>Uso de atributos personalizados para recopilar información sobre los clientes 

El directorio de Azure AD B2C viene con un [conjunto integrado de atributos](user-profile-attributes.md). Sin embargo, a menudo es necesario crear sus propios atributos para administrar un escenario concreto; por ejemplo, en las siguientes situaciones:

* Una aplicación orientada al cliente necesita conservar un atributo **LoyaltyId**.
* Un proveedor de identidades tiene un identificador de usuario único (**uniqueUserGUID**) que se debe conservar.
* Un recorrido del usuario personalizado debe conservar el estado del usuario, **migrationStatus**, para que otra lógica funcione.

Azure AD B2C le permite ampliar el conjunto de atributos que se almacenan en cada cuenta de usuario. También puede leer y escribir estos atributos mediante [Microsoft Graph API](manage-user-accounts-graph-api.md).

## <a name="azure-ad-b2c-extensions-app"></a>Aplicación de extensiones de Azure AD B2C

Los atributos de extensión solo se pueden registrar en un objeto de aplicación, aunque pueden contener datos para un usuario. El atributo de extensión está conectado a la aplicación denominada b2c-extensions-app. No modifique esta aplicación, ya que Azure AD B2C la usa para almacenar los datos de usuario. Puede encontrar esta aplicación en los registros de aplicaciones de Azure AD B2C.

En el contexto de este artículo, los términos *propiedad de extensión*, *atributo personalizado* y *notificación personalizada* se refieren a lo mismo. El nombre varía según el contexto, como aplicación, objeto o directiva.

## <a name="get-the-application-properties"></a>Obtención de las propiedades de la aplicación

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones** y, después, **Todas las aplicaciones**.
1. Seleccione la aplicación `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.`.
1. Copie los identificadores siguientes en el Portapapeles y guárdelos:
    * **Identificador de aplicación**. Ejemplo: `11111111-1111-1111-1111-111111111111`.
    * **Identificador de objeto**. Ejemplo: `22222222-2222-2222-2222-222222222222`.

## <a name="modify-your-custom-policy"></a>Modificación de la directiva personalizada

Para habilitar los atributos personalizados en la directiva, especifique el **Id. de aplicación** y el **Id. de objeto** de la aplicación en los metadatos de perfil técnico AAD-Common. El perfil técnico *AAD-Common* se encuentra en el perfil técnico base de [Azure Active Directory](active-directory-technical-profile.md) y ofrece compatibilidad con la administración de usuarios Azure AD. Otros perfiles técnicos de Azure AD incluyen AAD-Common para aprovechar su configuración. Reemplace el perfil técnico AAD-Common en el archivo de extensión.

1. Abra el archivo de extensiones de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.
1. Busque el elemento ClaimsProviders. Agregue un nuevo ClaimsProvider al elemento ClaimsProviders.
1. Reemplace `ApplicationObjectId` por el id. de objeto que haya registrado previamente. Reemplace el valor de `ClientId` con el id. de aplicación que haya registrado previamente en el fragmento de código siguiente.

    ```xml
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-Common">
          <Metadata>
            <!--Insert b2c-extensions-app application ID here, for example: 11111111-1111-1111-1111-111111111111-->  
            <Item Key="ClientId"></Item>
            <!--Insert b2c-extensions-app application ObjectId here, for example: 22222222-2222-2222-2222-222222222222-->
            <Item Key="ApplicationObjectId"></Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles> 
    </ClaimsProvider>
    ```

## <a name="upload-your-custom-policy"></a>Carga de la directiva personalizada

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Asegúrese de usar el directorio que contiene el inquilino de Azure AD. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino de Azure AD B2C.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Marco de experiencia de identidad**.
5. Seleccione **Cargar directiva personalizada** y cargue los dos archivos de la directiva TrustFrameworkExtensions.xml modificados.

> [!NOTE]
> La primera vez que el perfil técnico de Azure AD conserva la notificación en el directorio, comprueba si existe el atributo personalizado. Si no existe, lo crea.  

## <a name="create-a-custom-attribute-through-azure-portal"></a>Creación de un atributo personalizado mediante Azure Portal

Los mismos atributos de extensión se comparten entre directivas integradas y personalizadas. Cuando se agregan atributos personalizados a través de la experiencia del portal, estos se registran mediante la aplicación **b2c-extensions-app** que existe en cada inquilino B2C.

Puede crear estos atributos mediante la interfaz de usuario del portal antes o después de usarlos en las directivas personalizadas. Siga las instrucciones sobre cómo [definir los atributos personalizados en Azure Active Directory B2C](user-flow-custom-attributes.md). Cuando se crea un atributo **loyaltyId** en el portal, tiene que hacer referencia a él como se muestra a continuación:

|Nombre     |Se usa en |
|---------|---------|
|`extension_loyaltyId`  | Directiva personalizada|
|`extension_<b2c-extensions-app-guid>_loyaltyId`  | [Microsoft Graph API](manage-user-accounts-graph-api.md)|

En el ejemplo siguiente se muestra el uso de atributos personalizados en una definición de notificación de directiva personalizada de Azure AD B2C.

```xml
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="extension_loyaltyId">
      <DisplayName>Loyalty Identification</DisplayName>
      <DataType>string</DataType>
      <UserHelpText>Your loyalty number from your membership card</UserHelpText>
      <UserInputType>TextBox</UserInputType>
    </ClaimType>
  </ClaimsSchema>
</BuildingBlocks>
```

En el ejemplo siguiente se muestra el uso de un atributo personalizado en una directiva personalizada de Azure AD B2C en un perfil técnico y en notificaciones de entrada, salida y persistentes.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="extension_loyaltyId"  />
</InputClaims>
<PersistedClaims>
  <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
</PersistedClaims>
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
</OutputClaims>
```

## <a name="use-a-custom-attribute-in-a-policy"></a>Uso de un atributo personalizado en una directiva

Siga las instrucciones sobre cómo [agregar notificaciones y personalizar la entrada del usuario mediante directivas personalizadas](custom-policy-configure-user-input.md). En este ejemplo se usa una notificación "city" integrada. Para usar un atributo personalizado, reemplace "city" por sus propios atributos personalizados.


## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

- [Atributos de perfil de usuario de Azure AD B2C](user-profile-attributes.md)
- [Definición de atributos de extensión](user-profile-attributes.md#extension-attributes)
- [Administrar cuentas de usuario de Azure AD B2C con Microsoft Graph](manage-user-accounts-graph-api.md)
