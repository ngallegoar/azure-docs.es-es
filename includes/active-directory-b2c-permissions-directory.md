---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: bf8b9370abc58902d9c751505f633dee852210a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184369"
---
#### <a name="applications"></a>[Aplicaciones](#tab/applications/)

1. En la página de información general **Aplicación registrada**, seleccione **Configuración**.
1. En **Acceso de API**, seleccione **Permisos necesarios**.
1. Seleccione **Microsoft Graph**.
1. En **Permisos de la aplicación**, active la casilla del permiso que quiere conceder a la aplicación de administración. Por ejemplo:
    * **Leer todos los datos de registro de auditoría**: seleccione este permiso para leer los registros de auditoría del directorio.
    * **Leer y escribir datos de directorio**: seleccione este permiso para escenarios de migración o administración de usuarios.
    * **Leer y escribir en las directivas del marco de confianza de su organización**: seleccione este permiso para escenarios de integración continua o entrega continua (CI/CD). Por ejemplo, la implementación de directivas personalizadas con Azure Pipelines.
1. Seleccione **Guardar**.
1. Haga clic en **Conceder permisos** y, a continuación, haga clic en **Sí**. Los permisos pueden tardar unos minutos en propagarse por completo.

#### <a name="app-registrations-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. En **Administrar**, seleccione **Permisos de API**.
1. En **Permisos configurados**, seleccione **Agregar un permiso**.
1. Seleccione la pestaña **API de Microsoft** y, a continuación, seleccione **Microsoft Graph**.
1. Seleccione **Permisos de aplicación**.
1. Expanda el grupo de permisos adecuado y active la casilla del permiso que quiera conceder a la aplicación de administración. Por ejemplo:
    * **AuditLog** > **AuditLog.Read.All**: para leer los registros de auditoría del directorio.
    * **Directory** > **Directory.ReadWrite.All**: para escenarios de migración o administración de usuarios.
    * **Policy** > **Policy.ReadWrite.TrustFramework**: para escenarios de integración continua o entrega continua (CI/CD). Por ejemplo, la implementación de directivas personalizadas con Azure Pipelines.
1. Seleccione **Agregar permisos**. Como se indicó, espere unos minutos antes de continuar con el paso siguiente.
1. Seleccione **Conceder consentimiento de administrador para (el nombre de inquilino)** .
1. Seleccione la cuenta de administrador que tiene actualmente la sesión iniciada o inicie sesión con una cuenta en el inquilino de Azure AD B2C que tenga asignado al menos el rol *Administrador de aplicaciones en la nube*.
1. Seleccione **Aceptar**.
1. Seleccione **Actualizar** y, a continuación, compruebe que aparece "Concedido para..." bajo **Estado**. Los permisos pueden tardar unos minutos en propagarse.
