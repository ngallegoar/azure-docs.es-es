---
title: Configuración del registro y del inicio de sesión con una cuenta de Weibo
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Weibo en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 08aa7e4af6dc5d5e5bff470bc4c5d023e25b3014
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85387888"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Weibo mediante Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-weibo-application"></a>Creación de una aplicación de Weibo

Para usar una cuenta de Weibo como proveedor de identidades en Azure Active Directory B2C (Azure AD B2C), debe crear una aplicación en el inquilino que la represente. Si aún no tiene una cuenta de Weibo, puede suscribirse en [https://weibo.com/signup/signup.php?lang=en-us](https://weibo.com/signup/signup.php?lang=en-us).

1. Inicie sesión en el [portal para desarrolladores de Weibo](https://open.weibo.com/) con sus credenciales de cuenta de Weibo.
1. Después de iniciar sesión, seleccione el nombre para mostrar en la esquina superior derecha.
1. En el menú desplegable, seleccione **编辑开发者信息** (Editar información de desarrollador).
1. Escriba la información necesaria y luego seleccione **提交** (Enviar).
1. Complete el proceso de comprobación de correo electrónico.
1. Vaya a la [página de comprobación de identidad](https://open.weibo.com/developers/identity/edit).
1. Escriba la información necesaria y luego seleccione **提交** (Enviar).

### <a name="register-a-weibo-application"></a>Registro de una aplicación de Weibo

1. Vaya a la [página de registro de la aplicación de Weibo](https://open.weibo.com/apps/new).
1. Escriba la información de aplicación necesaria.
1. Seleccione **创建** (Crear).
1. Copie los valores de **Clave de la aplicación** y **Secreto de la aplicación**. Necesitará ambos para agregar el proveedor de identidades a su inquilino.
1. Cargue las fotos y escriba la información necesarias.
1. Seleccione **保存以上信息** (Guardar).
1. Seleccione **高级信息** (Información avanzada).
1. Seleccione **编辑** (Editar) junto al campo de OAuth2.0 **授权设置** (Dirección URL de redireccionamiento).
1. Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en OAuth2.0 **授权设置** (Dirección URL de redireccionamiento). Por ejemplo, si el nombre de inquilino es contoso, establezca la dirección URL para que sea `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
1. Seleccione **提交** (Enviar).

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Configuración de una cuenta de Weibo como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **Weibo (versión preliminar)** .
1. Escriba un **nombre**. Por ejemplo, *Weibo*.
1. En **Id. de cliente**, escriba la clave de aplicación de Weibo que ha creado anteriormente.
1. En **Secreto de cliente**, escriba el secreto de aplicación que ha anotado.
1. Seleccione **Guardar**.
