---
title: Configuración del registro y del inicio de sesión con una cuenta de Twitter
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Twitter en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 62a283efb93987d3c4a6564c9b25d2031c269559
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051464"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Twitter mediante Azure Active Directory B2C

## <a name="create-an-application"></a>Crear una aplicación

Para usar Twitter como proveedor de identidades en Azure AD B2C, tiene que crear una aplicación de Twitter. Si aún no tiene una cuenta de Twitter, puede suscribirse en [https://twitter.com/signup](https://twitter.com/signup).

1. Inicie sesión en el sitio web para [desarrolladores de Twitter](https://developer.twitter.com/en/apps) con las credenciales de la cuenta de Twitter.
1. Seleccione **Create an app** (Crear una aplicación).
1. Escriba un **nombre de aplicación** y una **descripción de aplicación**.
1. En **URL del sitio web**, escriba `https://your-tenant.b2clogin.com`. Reemplace `your-tenant` por el nombre del inquilino. Por ejemplo, `https://contosob2c.b2clogin.com`.
1. Para la **URL de devolución de llamada**, escriba `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-user-flow-Id/oauth1/authresp`. Sustituya `your-tenant` por el nombre del inquilino y `your-user-flow-Id` por el identificador del flujo de usuario. Por ejemplo, `b2c_1A_signup_signin_twitter`. Cuando especifique el nombre de inquilino y el identificador de flujo de usuario, escriba todas las letras en minúscula, aunque se hayan definido con letras en mayúscula en Azure AD B2C.
1. En la parte inferior de la página, lea y acepte los términos y seleccione **Crear**.
1. En la página **Detalles de la aplicación**, seleccione **Editar > Editar detalles**, active la casilla **Enable Sign in with Twitter** (Habilitar inicio de sesión con Twitter) y luego seleccione **Guardar**.
1. Seleccione **Claves y tokens** y registre los valores de **clave de API de consumidor** y **clave secreta de API de consumidor** que se van a usar más adelante.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configuración de Twitter como proveedor de identidades del inquilino

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **Twitter**.
1. Escriba un **nombre**. Por ejemplo, *Twitter*.
1. En **Id. de cliente**, escriba la clave de API de consumidor de Twitter que ha creado anteriormente.
1. En **Secreto de cliente**, escriba la clave secreta de API de consumidor que ha anotado.
1. Seleccione **Guardar**.
