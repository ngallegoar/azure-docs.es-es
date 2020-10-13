---
title: Configuración del registro y del inicio de sesión con una cuenta de LinkedIn
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de LinkedIn en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/22/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 10c460775bcb63028f03d0e8d0b1f7ed1507cdb4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91259465"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de LinkedIn mediante Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Creación de una aplicación de LinkedIn

Para usar una cuenta de LinkedIn como [proveedor de identidades](authorization-code-flow.md) en Azure Active Directory B2C (Azure AD B2C), tiene que crear una aplicación en el inquilino que la represente. Si aún no tiene una cuenta de LinkedIn, puede obtenerla en [https://www.linkedin.com/](https://www.linkedin.com/).

1. Inicie sesión en el [sitio web para desarrolladores de LinkedIn](https://www.developer.linkedin.com/) con las credenciales de su cuenta de LinkedIn.
1. Seleccione **My Apps** (Mis aplicaciones) y, a continuación, haga clic en **Create Application** (Crear aplicación).
1. Escriba lo que corresponda en **App name** (Nombre de la aplicación), **LinkedIn Page** (Página de LinkedIn), **Privacy policy URL** (Dirección URL de la directiva de privacidad) y **App logo** (Logotipo de la aplicación).
1. Acepte las **condiciones de uso de API** de LinkedIn y haga clic en **Create app** (Crear aplicación).
1. Seleccione la pestaña **Autenticación**. En **Authentication Keys** (Claves de autenticación), copie los valores de **Client ID** (Id. de cliente) y **Client Secret** (Secreto de cliente). Necesitará ambos para configurar LinkedIn como proveedor de identidades de su inquilino. **secreto de cliente** es una credencial de seguridad importante.
1. Seleccione el lápiz de edición situado junta a **Authorized redirect URLs for your app** (Direcciones URL de redireccionamiento autorizadas para la aplicación) y, después, seleccione **Add redirect URL** (Agregar dirección URL de redireccionamiento). Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` y reemplace `your-tenant-name` por el nombre del inquilino. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C. Seleccione **Actualizar**.
2. De forma predeterminada, la aplicación de LinkedIn no está aprobada para los ámbitos relacionados con el inicio de sesión. Para solicitar una revisión, seleccione la pestaña **Products** (Productos) y, a continuación, seleccione **Sign In with LinkedIn** (Iniciar sesión con LinkedIn). Una vez completada la revisión, los ámbitos requeridos se agregarán a la aplicación.
   > [!NOTE]
   > Puede ver los ámbitos permitidos actualmente para su aplicación en la pestaña **Auth** de la sección **OAuth 2.0 scopes** (Ámbitos de OAuth 2.0).

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Configuración de una cuenta de LinkedIn como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **LinkedIn**.
1. Escriba un **nombre**. Por ejemplo, *LinkedIn*.
1. En **Id. de cliente**, escriba el identificador de cliente de LinkedIn que creó anteriormente.
1. En **Secreto de cliente**, escriba el secreto de cliente que ha anotado.
1. Seleccione **Guardar**.

## <a name="migration-from-v10-to-v20"></a>Migración desde la versión 1.0 a la 2.0

Recientemente, LinkedIn [actualizó sus API de la versión 1.0 a 2.0](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Como parte de la migración, Azure AD B2C solo es capaz de obtener el nombre completo del usuario de LinkedIn durante la suscripción. Si una dirección de correo electrónico es uno de los atributos que se recopilan durante el proceso de suscripción, el usuario debe escribir manualmente la dirección de correo electrónico y validarla.
