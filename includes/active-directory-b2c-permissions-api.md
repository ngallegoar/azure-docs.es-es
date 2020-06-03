---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/04/2020
ms.author: mimart
ms.openlocfilehash: e37419b5d568b0f60e4c6a0c8fed26eb418f501e
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84297500"
---
#### <a name="app-registrations-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Seleccione **Registros de aplicaciones (versión preliminar)** y, después, seleccione la aplicación web que debe tener acceso a la API. Por ejemplo, *webapp1*.
1. En **Administrar**, seleccione **Permisos de API**.
1. En **Permisos configurados**, seleccione **Agregar un permiso**.
1. Seleccione la pestaña **Mis API**.
1. Seleccione la API a la que la aplicación web debe tener acceso. Por ejemplo, *webapi1*.
1. En **Permiso**, expanda **demo** y, a continuación, seleccione los ámbitos que definió anteriormente. Por ejemplo, *demo.read* y *demo.write*.
1. Seleccione **Agregar permisos**.
1. Seleccione **Conceder consentimiento de administrador para (el nombre de inquilino)** .
1. Si se le pide que seleccione una cuenta, seleccione la cuenta de administrador que tiene iniciada actualmente la sesión o bien inicie sesión con una cuenta en el inquilino de Azure AD B2C que tenga asignado al menos el rol *Administrador de aplicaciones en la nube*.
1. Seleccione **Sí**.
1. Seleccione **Actualizar** y, a continuación, compruebe que aparece "Concedido para..." en **Estado** para ambos ámbitos.

#### <a name="applications"></a>[Aplicaciones](#tab/applications/)

1. Seleccione **Aplicaciones** y, después, seleccione la aplicación web que debe tener acceso a la API. Por ejemplo, *webapp1*.
1. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
1. En la lista desplegable **Seleccionar API** , seleccione la API a la que se debe conceder acceso a la aplicación web. Por ejemplo, *webapi1*.
1. En la lista desplegable **Seleccionar ámbitos**, seleccione los ámbitos que ha definido anteriormente. Por ejemplo, *demo.read* y *demo.write*.
1. Seleccione **Aceptar**.
