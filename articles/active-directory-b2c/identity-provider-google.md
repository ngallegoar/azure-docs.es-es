---
title: Configuración del registro y del inicio de sesión con una cuenta de Google
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Google en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 48955caddb64069f897078f5e47066d9f11d119b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188147"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Google mediante Azure Active Directory B2C

## <a name="create-a-google-application"></a>Creación de una aplicación de Google

Para usar una cuenta de Google como [proveedor de identidades](authorization-code-flow.md) en Azure Active Directory B2C (Azure AD B2C), tiene que crear una aplicación en su consola de desarrolladores de Google. Si aún no tiene una cuenta de Google, puede obtenerla en [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Inicie sesión en [Google Developers Console](https://console.developers.google.com/) e inicie sesión con las credenciales de su cuenta de Google.
1. En la esquina superior izquierda de la página, seleccione la lista de proyectos y, luego, **Nuevo proyecto**.
1. Escriba un nombre en **Project Name** (Nombre de proyecto) y seleccione **Create** (Crear).
1. Asegúrese de que esté usando el nuevo proyecto; para ello, seleccione la lista desplegable de proyectos en la parte superior izquierda de la pantalla, luego, el proyecto por su nombre y, por último, **Open** (Abrir).
1. Seleccione **OAuth consent screen** (Pantalla de consentimiento de OAuth) en el menú de la izquierda, seleccione **External** (Externo) y, a continuación, **Create** (Crear).
Escriba el **nombre** de la aplicación. Escriba *b2clogin.com* en la sección **Authorized domains** (Dominios autorizados) y seleccione **Save** (Guardar).
1. Seleccione **Credentials** (Credenciales) en el menú izquierdo y, a continuación, seleccione **Create credentials** > **Oauth client ID** (Crear credenciales, Id. de cliente de Oauth).
1. En **Application type** (Tipo de aplicación), seleccione **Web application** (Aplicación web).
1. Escriba un **Name** (nombre) para la aplicación, escriba `https://your-tenant-name.b2clogin.com` en **Authorized JavaScript origins** (Orígenes de JavaScript autorizados) y `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **Authorized redirect URIs** (URI de redirección autorizados). Reemplace `your-tenant-name` por el nombre del inquilino. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C.
1. Haga clic en **Crear**.
1. Copie los valores de **Client ID** y **Client Secret**. Necesitará ambos para configurar Google como proveedor de identidades de su inquilino. **Secreto del cliente** es una credencial de seguridad importante.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Configuración de una cuenta de Google como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **Google**.
1. Escriba un **nombre**. Por ejemplo, *Google*.
1. En **Id. de cliente**, escriba el identificador de cliente de Google que creó anteriormente.
1. En **Secreto de cliente**, escriba el secreto de cliente que ha anotado.
1. Seleccione **Guardar**.
