---
title: Configuración del registro y del inicio de sesión con una cuenta de WeChat
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de WeChat en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c53210939358255b20d0e976df9c4bff88580a80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184444"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de WeChat mediante Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-wechat-application"></a>Creación de una aplicación de WeChat

Para usar una cuenta de WeChat como proveedor de identidades en Azure Active Directory B2C (Azure AD B2C), debe crear una aplicación en el inquilino que la represente. Si aún no tiene una cuenta de WeChat, puede obtener información en [https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registro de una aplicación de WeChat

1. Inicie sesión en [https://open.weixin.qq.com/](https://open.weixin.qq.com/) con sus credenciales de WeChat.
1. Seleccione **管理中心**(centro de administración).
1. Siga los pasos para registrar una nueva aplicación.
1. Escriba `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **授权回调域** (Dirección URL de devolución de llamada). Por ejemplo, si el nombre de inquilino es contoso, establezca la dirección URL para que sea `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
1. Copie el valor de **APP ID** (Identificador de la aplicación) y de **APP KEY** (Clave de la aplicación). Los necesitará para agregar el proveedor de identidades a su inquilino.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Configuración de WeChat como proveedor de identidades del inquilino

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **WeChat (versión preliminar)** .
1. Escriba un **nombre**. Por ejemplo, *WeChat*.
1. En **Id. de cliente**, escriba el identificador de aplicación de WeChat que ha creado anteriormente.
1. En **Secreto de cliente**, escriba la clave de aplicación que ha anotado.
1. Seleccione **Guardar**.
