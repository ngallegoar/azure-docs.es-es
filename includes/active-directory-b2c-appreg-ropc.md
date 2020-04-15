---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: cea3245176e6c38137d68e3ad4b47477bedc78be
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529187"
---
Para registrar una aplicación en el inquilino de Azure AD B2C, puede usar la experiencia **Aplicaciones** actual o la nueva experiencia **Registros de aplicaciones (versión preliminar)** unificada. [Más información acerca de la nueva experiencia](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Aplicaciones](#tab/applications/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y **Agregar**.
1. Escriba un nombre para la aplicación. Por ejemplo, *ROPC_Auth_app*.
1. En **Cliente nativo**, seleccione **Sí**.
1. Deje los demás valores como están y seleccione **Crear**.
1. Anote el valor de **ID. DE APLICACIÓN** para usarlo en un paso posterior.

#### <a name="app-registrations-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones (versión preliminar)** y luego **Nuevo registro**.
1. Escriba un **Nombre** para la aplicación. Por ejemplo, *ROPC_Auth_app*.
1. Deje los demás valores como están y seleccione **Registrar**.
1. Anote el **Id. de aplicación (cliente)** para usarlo en un paso posterior.
1. En **Administrar**, seleccione **Autenticación**.
1. Seleccione **Probar la nueva experiencia** (si se muestra).
1. En **Tipo de cliente predeterminado**, seleccione **Sí** para tratar el cliente como un cliente público. Esta opción de configuración es necesaria para el flujo de ROPC.
1. Seleccione **Guardar**.
1. En el menú de la izquierda, seleccione **Manifiesto** para abrir el editor de manifiestos. 
1. Establezca el atributo **oauth2AllowImplicitFlow** en *true*:
    ```json
    "oauth2AllowImplicitFlow": true,
    ```
1. Seleccione **Guardar**.