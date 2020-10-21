---
title: 'Configuración del comportamiento de la sesión: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo configurar el comportamiento de la sesión en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 091704fabb7b50a0c83625c6ae46d9a807f01ffc
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91961041"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Configuración del comportamiento de la sesión en Azure Active Directory B2C

La administración de [sesiones SSO (inicio de sesión único)](session-overview.md) en Azure Active Directory B2C (Azure AD B2C) permite a los administradores controlar la interacción con el usuario después de que este se haya autenticado. Por ejemplo, el administrador puede controlar si se muestra la selección de proveedores de identidades, o si se tienen que volver a especificar los detalles de la cuenta. En este artículo se describe cómo establecer la configuración de SSO para Azure AD B2C.

## <a name="session-behavior-properties"></a>Propiedades de comportamiento de sesión

Puede usar las propiedades siguientes para administrar sesiones de la aplicación web:

- **Vigencia (en minutos) de la sesión de la aplicación web**: la vigencia de la cookie de sesión de Azure AD B2C almacenada en el explorador del usuario tras una autenticación correcta.
    - Valor predeterminado: 1440 minutos.
    - Mínimo (incluido) = 15 minutos.
    - Máximo (incluido) = 1440 minutos.
- **Tiempo de expiración de la sesión de la aplicación web**: el [tipo de expiración de sesión](session-overview.md#session-expiry-type), *Rolling* (acumulado) o *Absolute* (absoluto). 
- **Configuración de inicio de sesión único**: el [ámbito de la sesión](session-overview.md#session-scope) del comportamiento de inicio de sesión único (SSO) entre varias aplicaciones y flujos de usuario en el inquilino de Azure AD B2C.


## <a name="configure-the-properties"></a>Configuración de las propiedades

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino de Azure AD B2C.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Flujos de usuario**.
5. Abra el flujo de usuario que creó anteriormente.
6. Seleccione **Propiedades**.
7. Configure las opciones **Duración de la sesión de la aplicación web (minutos)** , **Tiempo de espera de la sesión de la aplicación web**, **Configuración de inicio de sesión único** y **Requerir token de identificador en solicitudes de cierre de sesión** según sea necesario.

    ![Configuración de las propiedades de comportamiento de sesión en Azure Portal](./media/session-behavior/session-behavior.png)

8. Haga clic en **Save**(Guardar).

## <a name="configure-sign-out-behavior"></a>Configuración del comportamiento de cierre de sesión

### <a name="secure-your-logout-redirect"></a>Protección de la redirección de cierre de sesión

Después del cierre de sesión, se redirige al usuario al URI especificado en el parámetro `post_logout_redirect_uri`, independientemente de las direcciones URL de respuesta que se hayan especificado para la aplicación. Sin embargo, si se pasa un valor de `id_token_hint` válido y la opción **Requerir token de identificador en solicitudes de cierre de sesión** está activada, Azure AD B2C comprueba que el valor de `post_logout_redirect_uri` coincida con uno de los URI de redirección configurados de la aplicación antes de realizar la redirección. Si no se configuró ninguna dirección URL de respuesta coincidente para la aplicación, se muestra un mensaje de error y no se redirige al usuario. Para requerir un token de identificador en las solicitudes de cierre de sesión:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Flujos de usuario**.
1. Abra el flujo de usuario que creó anteriormente.
1. Seleccione **Propiedades**.
1. Habilite **Requerir token de identificador en solicitudes de cierre de sesión**.
1. Vuelva a **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones** y, luego, seleccione su aplicación.
1. Seleccione **Autenticación**.
1. En el cuadro de texto **URL de cierre de sesión**, escriba el URI de redirección posterior al cierre de sesión y, luego, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [sesión de Azure AD B2C](session-overview.md).
