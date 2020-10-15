---
title: Configuración de la complejidad de contraseñas mediante directivas personalizadas
titleSuffix: Azure AD B2C
description: Cómo configurar los requisitos de complejidad de las contraseñas con una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4ab196e894fc53b1243ac363f9863d5c7d4e328f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85389010"
---
# <a name="configure-password-complexity-using-custom-policies-in-azure-active-directory-b2c"></a>Configuración de la complejidad de las contraseñas con directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En Azure Active Directory B2C (Azure AD B2C), puede configurar los requisitos de complejidad de las contraseñas que proporciona un usuario al crear una cuenta. De forma predeterminada, Azure AD B2C utiliza contraseñas **seguras**. En este artículo se muestra cómo configurar la complejidad de las contraseñas en [directivas personalizadas](custom-policy-overview.md). También es posible configurar la complejidad de las contraseñas en [flujos de usuario](user-flow-password-complexity.md).

## <a name="prerequisites"></a>Prerrequisitos

Realice los pasos del artículo [Introducción a las directivas personalizadas](custom-policy-get-started.md). Debe tener una directiva personalizada activa para registrar e iniciar sesión de cuentas locales.


## <a name="add-the-elements"></a>Adición de los elementos

Para configurar la complejidad de la contraseña, reemplace los [tipos de notificaciones](claimsschema.md) de `newPassword` y `reenterPassword` por una referencia a las [validaciones de predicado](predicates.md#predicatevalidations). El elemento PredicateValidations agrupa un conjunto de predicados para formar una validación de entrada de usuario que se puede aplicar a un tipo de notificación. Abra el archivo de extensiones de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** </em>.

1. Busque el elemento [BuildingBlocks](buildingblocks.md). Si el elemento no existe, agréguelo.
1. Busque el elemento [ClaimsSchema](claimsschema.md). Si el elemento no existe, agréguelo.
1. Agregue las notificaciones `newPassword` y `reenterPassword` al elemento **ClaimsSchema**.

    ```xml
    <ClaimType Id="newPassword">
      <PredicateValidationReference Id="CustomPassword" />
    </ClaimType>
    <ClaimType Id="reenterPassword">
      <PredicateValidationReference Id="CustomPassword" />
    </ClaimType>
    ```

1. [Predicates](predicates.md) define una validación básica para comprobar el valor de un tipo de notificación y devuelve true o false. La validación se realiza mediante un elemento de método especificado y un conjunto de parámetros pertinentes para el método. Agregue los siguientes predicados al elemento **BuildingBlocks** justo después del cierre del elemento `</ClaimsSchema>`:

    ```xml
    <Predicates>
      <Predicate Id="LengthRange" Method="IsLengthRange">
        <UserHelpText>The password must be between 6 and 64 characters.</UserHelpText>
        <Parameters>
          <Parameter Id="Minimum">6</Parameter>
          <Parameter Id="Maximum">64</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Lowercase" Method="IncludesCharacters">
        <UserHelpText>a lowercase letter</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">a-z</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="IncludesCharacters">
        <UserHelpText>an uppercase letter</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">A-Z</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="IncludesCharacters">
        <UserHelpText>a digit</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">0-9</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="IncludesCharacters">
        <UserHelpText>a symbol</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">@#$%^&amp;*\-_+=[]{}|\\:',.?/`~"();!</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    ```

1. Agregue las siguientes validaciones de predicado al elemento **BuildingBlocks** justo después del cierre del elemento `</Predicates>`:

    ```xml
    <PredicateValidations>
      <PredicateValidation Id="CustomPassword">
        <PredicateGroups>
          <PredicateGroup Id="LengthGroup">
            <PredicateReferences MatchAtLeast="1">
              <PredicateReference Id="LengthRange" />
            </PredicateReferences>
          </PredicateGroup>
          <PredicateGroup Id="CharacterClasses">
            <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
            <PredicateReferences MatchAtLeast="3">
              <PredicateReference Id="Lowercase" />
              <PredicateReference Id="Uppercase" />
              <PredicateReference Id="Number" />
              <PredicateReference Id="Symbol" />
            </PredicateReferences>
          </PredicateGroup>
        </PredicateGroups>
      </PredicateValidation>
    </PredicateValidations>
    ```

1. Los siguientes perfiles técnicos son [perfiles técnicos de Active Directory](active-directory-technical-profile.md), que leen y escriben datos en Azure Active Directory. Reemplace estos perfiles técnicos en el archivo de extensión. Use `PersistedClaims` para deshabilitar la directiva de contraseñas seguras. Busque el elemento **ClaimsProviders**.  Agregue los siguientes proveedores de notificaciones tal como se indica a continuación:

    ```xml
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
          <PersistedClaims>
            <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
          </PersistedClaims>
        </TechnicalProfile>
        <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
          <PersistedClaims>
            <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
          </PersistedClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Guarde el archivo de directiva.

## <a name="test-your-policy"></a>Prueba de la directiva

### <a name="upload-the-files"></a>Carga de los archivos

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Marco de experiencia de identidad**.
5. En la página Directivas personalizadas, haga clic en **Cargar directiva**.
6. Seleccione **Sobrescribir la directiva si existe**, y busque y seleccione el archivo *TrustFrameworkExtensions.xml*.
7. Haga clic en **Cargar**.

### <a name="run-the-policy"></a>Ejecución de la directiva

1. Abra la directiva de registro o de inicio de sesión. Por ejemplo, *B2C_1A_signup_signin*.
2. En **Aplicación**, seleccione la aplicación que registró anteriormente. Para ver el token, **URL de respuesta** debe mostrar `https://jwt.ms`.
3. Haga clic en **Ejecutar ahora**.
4. Seleccione **Registrarse ahora**, escriba una dirección de correo electrónico y escriba una contraseña nueva. Se presenta una guía sobre las restricciones de las contraseñas. Termine de especificar la información de usuario y, a continuación, haga clic en **Crear**. Debería ver el contenido del token que se devolvió.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Configuración del cambio de contraseñas mediante directivas personalizadas en Azure Active Directory B2C](custom-policy-password-change.md).
- Más información sobre los elementos [Predicates](predicates.md) y [PredicateValidations](predicates.md#predicatevalidations) en la referencia de IEF.
