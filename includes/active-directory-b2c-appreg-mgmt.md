---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 11728c62202b15f0052017609ac384e8ef259211
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84298943"
---
#### <a name="app-registrations"></a>[Registros de aplicaciones](#tab/app-reg-ga/)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones** y luego **Nuevo registro**.
1. Escriba un **Nombre** para la aplicación. Por ejemplo, *managementapp1*.
1. Seleccione **Solo las cuentas de este directorio organizativo**.
1. En **Permisos**, desactive la casilla *Conceda permiso del administrador a los permisos openid y offline_access*.
1. Seleccione **Registrar**.
1. Registre el valor **Id. de aplicación (cliente)** que se muestra en la página de información general de la aplicación. Se usará este valor en un paso posterior.