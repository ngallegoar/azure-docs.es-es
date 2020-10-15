---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de QQ mediante Azure Active Directory B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de QQ en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d230bc8a1e9bf388e1cca4e3a3a691223146d734
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85387990"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de QQ mediante Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-qq-application"></a>Creación de una aplicación de QQ

Para usar una cuenta de QQ como proveedor de identidades en Azure Active Directory B2C (Azure AD B2C), debe crear una aplicación en el inquilino que la represente. Si aún no tiene una cuenta de QQ, puede suscribirse en [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>Registro para el programa para desarrolladores de QQ

1. Inicie sesión en el [portal para desarrolladores de QQ](http://open.qq.com) e inicie sesión con sus credenciales de la cuenta de QQ.
1. Después de iniciar sesión, vaya a [https://open.qq.com/reg](https://open.qq.com/reg) para registrarse como un programador.
1. Seleccione **个人** (Desarrollador individual).
1. Escriba la información necesaria y luego seleccione **下一步** (Paso siguiente).
1. Complete el proceso de comprobación de correo electrónico. Debe esperar unos días para recibir la aprobación después de registrarse como desarrollador.

### <a name="register-a-qq-application"></a>Registro de una aplicación QQ

1. Vaya a [https://connect.qq.com/index.html](https://connect.qq.com/index.html).
1. Seleccione **应用管理** (Administración de aplicaciones).
1. Seleccione **创建应用**(Crear aplicación) y escriba la información necesaria.
1. Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp` en **授权回调域** (Dirección URL de devolución de llamada). Por ejemplo, si `tenant_name` es Contoso, establezca la dirección URL para que sea `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
1. Seleccione **创建应用** (Crear aplicación).
1. En la página de confirmación, seleccione **应用管理** (Administración de aplicaciones) para volver a la página de administración de aplicaciones.
1. Seleccione **查看** (Ver) junto a la aplicación que acaba de crear.
1. Seleccione **修改**(Editar).
1. Copie el valor de **APP ID** (Identificador de la aplicación) y de **APP KEY** (Clave de la aplicación). Necesitará los dos valores para agregar el proveedor de identidades a su inquilino.

## <a name="configure-qq-as-an-identity-provider"></a>Configuración de QQ como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **QQ (versión preliminar)** .
1. Escriba un **nombre**. Por ejemplo, *QQ*.
1. En **Id. de cliente**, escriba el identificador de aplicación de QQ que ha creado anteriormente.
1. En **Secreto de cliente**, escriba la clave de aplicación que ha anotado.
1. Seleccione **Guardar**.
