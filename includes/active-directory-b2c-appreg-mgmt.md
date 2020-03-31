---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 31d4b65c6cd6a3dfe3cb779022329f338436c69b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184370"
---
Para registrar una aplicación en el inquilino de Azure AD B2C, puede usar la experiencia **Aplicaciones** actual o la nueva experiencia **Registros de aplicaciones (versión preliminar)** unificada. [Más información acerca de la nueva experiencia](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Aplicaciones](#tab/applications/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Registros de aplicaciones (característica heredada)** .
1. Seleccione **Nuevo registro de aplicaciones**.
1. Escriba un nombre para la aplicación. Por ejemplo, *managementapp1*.
1. En **Tipo de aplicación**, seleccione**Aplicación web o API**.
1. En **Dirección URL de inicio de sesión**, escriba una dirección URL válida. Por ejemplo, `https://localhost`. No es necesario que el punto de conexión sea accesible, pero debe ser una dirección URL válida.
1. Seleccione **Crear**.
1. Anote el **identificador de aplicación** que aparece en la página de información general **Aplicación registrada**. Se usará este valor en un paso posterior.

#### <a name="app-registrations-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones (versión preliminar)** y luego **Nuevo registro**.
1. Escriba un **Nombre** para la aplicación. Por ejemplo, *managementapp1*.
1. Seleccione **Solo las cuentas de este directorio organizativo**.
1. En **Permisos**, desactive la casilla *Conceda permiso del administrador a los permisos openid y offline_access*.
1. Seleccione **Registrar**.
1. Registre el valor **Id. de aplicación (cliente)** que se muestra en la página de información general de la aplicación. Se usará este valor en un paso posterior.