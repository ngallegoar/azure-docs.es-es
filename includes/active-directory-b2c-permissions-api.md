---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/04/2020
ms.author: mimart
ms.openlocfilehash: c1628894205754c1b8f1fc954796009afbdc8ed4
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95994334"
---
#### <a name="app-registrations"></a>[Registros de aplicaciones](#tab/app-reg-ga/) 

1. Seleccione **Registros de aplicaciones** y, después, seleccione la aplicación web que debe tener acceso a la API. Por ejemplo, *webapp1*.
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

#### <a name="applications-legacy"></a>[Aplicaciones (heredado)](#tab/applications-legacy/)

1. Seleccione **Aplicaciones (heredado)** y, después, seleccione la aplicación web que debe tener acceso a la API. Por ejemplo, *webapp1*.
1. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
1. En la lista desplegable **Seleccionar API** , seleccione la API a la que se debe conceder acceso a la aplicación web. Por ejemplo, *webapi1*.
1. En la lista desplegable **Seleccionar ámbitos**, seleccione los ámbitos que ha definido anteriormente. Por ejemplo, *demo.read* y *demo.write*.
1. Seleccione **Aceptar**.
