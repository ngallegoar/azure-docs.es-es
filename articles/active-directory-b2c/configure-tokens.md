---
title: 'Configuración de tokens: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo configurar las opciones de compatibilidad y duración de los tokens en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 49a5ff61e5f7a17005561e0729a9b0fcb0f954d4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85389571"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Configuración de tokens en Azure Active Directory B2C

En este artículo, aprenderá a configurar la [duración y la compatibilidad de un token](tokens-overview.md) en Azure Active Directory B2C (Azure AD B2C).

## <a name="prerequisites"></a>Prerrequisitos

[Cree un flujo de usuario](tutorial-create-user-flows.md) para permitir a los usuarios registrarse e iniciar sesión en la aplicación.

## <a name="configure-jwt-token-lifetime"></a>Configuración de la duración de los tokens de JWT

Puede configurar la duración de los tokens en cualquier flujo de usuario.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio y suscripción** del menú superior y el directorio que contiene el inquilino de Azure AD B2C.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Flujos de usuario (directivas)** .
5. Abra el flujo de usuario que creó anteriormente.
6. Seleccione **Propiedades**.
7. En **Vigencia del token**, ajuste las propiedades siguientes para satisfacer las necesidades de su aplicación:

    ![Configuración de la propiedad de duración del token en Azure Portal](./media/configure-tokens/token-lifetime.png)

8. Haga clic en **Save**(Guardar).

## <a name="configure-jwt-token-compatibility"></a>Configuración de compatibilidad de tokens de JWT

1. Seleccione **Flujos de usuario (directivas)** .
2. Abra el flujo de usuario que creó anteriormente.
3. Seleccione **Propiedades**.
4. En **Configuración de compatibilidad del token**, ajuste las propiedades siguientes para satisfacer las necesidades de su aplicación:

    ![Configuración de la propiedad de compatibilidad del token en Azure Portal](./media/configure-tokens/token-compatibility.png)

5. Haga clic en **Save**(Guardar).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo [solicitar tokens de acceso](access-tokens.md).



