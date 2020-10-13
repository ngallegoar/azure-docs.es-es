---
title: Deshabilitación de la comprobación de correo electrónico durante la suscripción de clientes con una directiva personalizada
titleSuffix: Azure AD B2C
description: Obtenga información sobre cómo deshabilitar la comprobación de correo electrónico durante la suscripción de clientes mediante una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9372a9caff5aefb53bfa8adf7eb1c68d2a3b7b2e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91259624"
---
# <a name="disable-email-verification-during-customer-sign-up-using-a-custom-policy-in-azure-active-directory-b2c"></a>Deshabilitación de la comprobación de correo electrónico durante la suscripción de clientes mediante una directiva personalizada en Azure Active Directory B2C

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

## <a name="prerequisites"></a>Prerrequisitos

Realice los pasos del artículo [Introducción a las directivas personalizadas](custom-policy-get-started.md). Debe tener una directiva personalizada activa para suscribirse e iniciar sesión con cuentas sociales y locales.

## <a name="add-the-metadata-to-the-self-asserted-technical-profile"></a>Incorporación de los metadatos al perfil técnico autoafirmado

El perfil técnico **LocalAccountSignUpWithLogonEmail** es [autofirmado](self-asserted-technical-profile.md), y se invoca durante el flujo de suscripción. Para deshabilitar la comprobación de correo electrónico, establezca los metadatos `EnforceEmailVerification` en false. Invalide los perfiles técnicos LocalAccountSignUpWithLogonEmail en el archivo de extensión. 

1. Abra el archivo de extensiones de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** </em>.
1. Busque el elemento `ClaimsProviders`. Si el elemento no existe, agréguelo.
1. Agregue el siguiente proveedor de notificaciones al elemento `ClaimsProviders`:

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
      <Metadata>
        <Item Key="EnforceEmailVerification">false</Item>
      </Metadata>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="test-the-custom-policy"></a>Prueba de la directiva personalizada

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD. Para ello, seleccione el filtro **Directorio y suscripción** que se encuentra en el menú superior y elija el directorio que contiene el inquilino de Azure AD.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Marco de experiencia de identidad**.
5. Seleccione **Cargar directiva personalizada** y cargue los dos archivos de directiva modificados.
2. Seleccione la directiva de registro o inicio de sesión que cargó y haga clic en el botón **Ejecutar ahora**.
3. Debe poder suscribirse con una dirección de correo electrónico sin la validación.


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el [perfil técnico autoafirmado](self-asserted-technical-profile.md) en la referencia de IEF.
