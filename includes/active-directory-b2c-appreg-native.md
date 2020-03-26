---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: d3a323a28d92e2a5834b65316d61c7d53a42aacf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78183792"
---
Para registrar una aplicación en el inquilino de Azure AD B2C, puede usar la experiencia **Aplicaciones** actual o la nueva experiencia **Registros de aplicaciones (versión preliminar)** unificada. [Más información acerca de la nueva experiencia](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Aplicaciones](#tab/applications/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y **Agregar**.
1. Escriba un nombre para la aplicación. Por ejemplo, *nativeapp1*.
1. En **Cliente nativo**, seleccione **Sí**.
1. Escriba un **URI de redirección personalizado** con un esquema único. Por ejemplo, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. Hay dos aspectos importantes que se deben tener en cuenta al elegir un URI de redirección:
    * **Único**: el esquema del identificador URI de redirección debe ser único para todas las aplicaciones. En el ejemplo `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`, `com.onmicrosoft.contosob2c.exampleapp` es el esquema. Se debe seguir este patrón. Si dos aplicaciones comparten el mismo esquema, el usuario tiene opción de elegir una aplicación. Si el usuario elige incorrectamente, se produce un error en el inicio de sesión.
    * **Completo**: el URI de redirección debe tener un esquema y una ruta de acceso. La ruta de acceso debe contener al menos una barra diagonal después del dominio. Por ejemplo, `//oauth/` sirve y `//oauth` produce un error. No incluya caracteres especiales en el URI, por ejemplo, caracteres de subrayado.
1. Seleccione **Crear**.

#### <a name="app-registrations-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones (versión preliminar)** y luego **Nuevo registro**.
1. Escriba un **Nombre** para la aplicación. Por ejemplo, *nativeapp1*.
1. En **Tipos de cuenta compatibles**, seleccione **Cuentas de cualquier directorio de la organización o cualquier proveedor de identidades**.
1. En **URI de redirección**, use la lista desplegable para seleccionar **Cliente público o nativo (móvil y escritorio)** .
1. Escriba un URI de redirección con un esquema único. Por ejemplo, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. Hay dos aspectos importantes que se deben tener en cuenta al elegir un URI de redirección:
    * **Único**: el esquema del identificador URI de redirección debe ser único para todas las aplicaciones. En el ejemplo `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`, `com.onmicrosoft.contosob2c.exampleapp` es el esquema. Se debe seguir este patrón. Si dos aplicaciones comparten el mismo esquema, el usuario tiene opción de elegir una aplicación. Si el usuario elige incorrectamente, se produce un error en el inicio de sesión.
    * **Completo**: el URI de redirección debe tener un esquema y una ruta de acceso. La ruta de acceso debe contener al menos una barra diagonal después del dominio. Por ejemplo, `//oauth/` sirve y `//oauth` produce un error. No incluya caracteres especiales en el URI, por ejemplo, caracteres de subrayado.
1. En **Permisos**, active la casilla *Conceda consentimiento del administrador a los permisos openid y offline_access*.
1. Seleccione **Registrar**.