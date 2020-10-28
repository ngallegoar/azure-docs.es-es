---
title: 'Configuración de tokens: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo configurar las opciones de compatibilidad y duración de los tokens en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 67bc9d6b35d4841999721a00592a6bbe23bff10f
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92340230"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Configuración de tokens en Azure Active Directory B2C

En este artículo, aprenderá a configurar la [duración y la compatibilidad de un token](tokens-overview.md) en Azure Active Directory B2C (Azure AD B2C).

## <a name="prerequisites"></a>Prerrequisitos

[Cree un flujo de usuario](tutorial-create-user-flows.md) para permitir a los usuarios registrarse e iniciar sesión en la aplicación.

## <a name="configure-jwt-token-lifetime"></a>Configuración de la duración de los tokens de JWT

Puede configurar la duración de los tokens en cualquier flujo de usuario.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio y suscripción** del menú superior y el directorio que contiene el inquilino de Azure AD B2C.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C** .
4. Seleccione **Flujos de usuario (directivas)** .
5. Abra el flujo de usuario que creó anteriormente.
6. Seleccione **Propiedades** .
7. En **Vigencia del token** , ajuste las propiedades siguientes para satisfacer las necesidades de su aplicación:

    ![Configuración de la propiedad de duración del token en Azure Portal](./media/configure-tokens/token-lifetime.png)

8. Haga clic en **Save** (Guardar).

> [!NOTE]
> Las aplicaciones de página única que usan el flujo de código de autorización con PKCE siempre tienen una duración de token de actualización de 24 horas. [Más información sobre las consecuencias de seguridad de los tokens de actualización en el explorador](../active-directory/develop/reference-third-party-cookies-spas.md#security-implications-of-refresh-tokens-in-the-browser).

## <a name="configure-jwt-token-compatibility"></a>Configuración de compatibilidad de tokens de JWT

1. Seleccione **Flujos de usuario (directivas)** .
2. Abra el flujo de usuario que creó anteriormente.
3. Seleccione **Propiedades** .
4. En **Configuración de compatibilidad del token** , ajuste las propiedades siguientes para satisfacer las necesidades de su aplicación:

    ![Configuración de la propiedad de compatibilidad del token en Azure Portal](./media/configure-tokens/token-compatibility.png)

5. Haga clic en **Save** (Guardar).

## <a name="provide-optional-claims-to-your-app"></a>Proporcionar notificaciones opcionales a la aplicación

Las notificaciones de aplicación son valores que se devuelven a la aplicación. Actualice el flujo de usuario para que contenga las notificaciones que necesite.

1. Seleccione **Flujos de usuario (directivas)** .
1. Abra el flujo de usuario que creó anteriormente.
1. Seleccione **Notificaciones de aplicación** .
1. Elija las notificaciones y los atributos que quiera enviar de nuevo a la aplicación.
1. Haga clic en **Save** (Guardar).


## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo [solicitar tokens de acceso](access-tokens.md).



