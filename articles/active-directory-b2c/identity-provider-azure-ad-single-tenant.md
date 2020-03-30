---
title: Configuración del inicio de sesión para una organización de Azure AD
titleSuffix: Azure AD B2C
description: Configuración del inicio de sesión para una determinada organización de Azure Active Directory en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 35fc4e1d64fa7df392fa878db14c0464da7dccf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188314"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Configuración del inicio de sesión para una determinada organización de Azure Active Directory en Azure Active Directory B2C

Para usar una cuenta de Azure Active Directory (Azure AD) como [proveedor de identidades](authorization-code-flow.md) en Azure AD B2C, tiene que crear una aplicación que la represente. En este artículo se muestra cómo habilitar el inicio de sesión para los usuarios de una organización de Azure AD específica mediante un flujo de usuario en Azure AD B2C.

## <a name="create-an-azure-ad-app"></a>Creación de una aplicación de Azure AD

Si desea habilitar el inicio de sesión para los usuarios de una organización específica de Azure AD, es preciso registrar una aplicación en el inquilino de Azure AD de la organización, que no es el mismo que el inquilino de Azure AD B2C.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD. Seleccione el filtro **Directorio y suscripción** del menú superior y el directorio que contiene el inquilino de Azure AD. No es el mismo que el inquilino de Azure AD B2C.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Nuevo registro**.
5. Escriba un nombre para la aplicación. Por ejemplo, `Azure AD B2C App`.
6. Acepte la selección de **Solo las cuentas de este directorio organizativo** para esta aplicación.
7. Para la **URI de redirección**, acepte el valor de **Web** y escriba la siguiente dirección URL en minúscula, donde `your-B2C-tenant-name` se reemplaza por el nombre del inquilino de Azure AD B2C. Por ejemplo, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Todas las direcciones URL ahora deberían estar utilizando [b2clogin.com](b2clogin.md).

8. Haga clic en **Registrar**. Copie el **Id. de aplicación (cliente)** , ya que lo usará más adelante.
9. Seleccione **Certificados y secretos** y **Nuevo secreto de cliente**.
10. Escriba un nombre para el secreto de cliente. Por ejemplo, `Azure AD B2C App Secret`.
11. Seleccione el período de expiración. Para esta aplicación, acepte la selección de **En 1 año**.
12. Seleccione **Agregar** y copie el valor del nuevo secreto de cliente que se muestra para usarlo más tarde.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Configuración de Azure AD como proveedor de identidades

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el filtro **Directorio y suscripción** del menú superior y el directorio que contiene el inquilino de Azure AD B2C.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y luego **Nuevo proveedor de OpenID Connect**.
1. Escriba un **nombre**. Por ejemplo, escriba *Contoso Azure AD*.
1. En **URL de metadatos**, escriba la dirección URL siguiente y sustituya `your-AD-tenant-domain` por el nombre de dominio del inquilino de Azure AD:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Por ejemplo, `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

    **No** use el punto de conexión de metadatos de Azure AD v2.0, por ejemplo `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`. Si lo hace, se producirá un error similar a `AADB2C: A claim with id 'UserId' was not found, which is required by ClaimsTransformation 'CreateAlternativeSecurityId' with id 'CreateAlternativeSecurityId' in policy 'B2C_1_SignUpOrIn' of tenant 'contoso.onmicrosoft.com'` al intentar iniciar sesión.

1. En **Id. de cliente**, escriba el identificador de aplicación que ha anotado anteriormente.
1. En **Secreto de cliente**, escriba el secreto de cliente que ha anotado anteriormente.
1. Deje los valores predeterminados de **Ámbito**, **Tipo de respuesta** y **Modo de respuesta**.
1. (Opcional) Escriba un valor para **Domain_hint**. Por ejemplo, *ContosoAD*. Este es el valor que se usa al hacer referencia a este proveedor de identidades mediante *domain_hint* en la solicitud.
1. En **Asignación de notificaciones del proveedor de identidades**, escriba los siguientes valores de asignación de notificaciones:

    * **Id. de usuario**: *oid*
    * **Nombre para mostrar**: *name*
    * **Nombre propio**: *given_name*
    * **Apellido**: *family_name*
    * **Correo electrónico**: *unique_name*

1. Seleccione **Guardar**.
