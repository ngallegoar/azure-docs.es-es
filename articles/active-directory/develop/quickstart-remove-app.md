---
title: 'Procedimiento: Eliminación de una aplicación registrada en la plataforma de identidad de Microsoft | Azure'
titleSuffix: Microsoft identity platform
description: En este procedimiento, aprenderá a registrar una aplicación mediante la plataforma de identidad de Microsoft.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 11/15/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: marsma, aragra, lenalepa, sureshja
ms.openlocfilehash: edfed8ca361d456e622096e23079d5887f96ab30
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96452995"
---
# <a name="how-to-remove-an-application-registered-with-the-microsoft-identity-platform"></a>Eliminación de una aplicación registrada con la plataforma de identidad de Microsoft.

Es posible que los desarrolladores empresariales y proveedores de software como servicio (SaaS) que han registrado aplicaciones con la plataforma de identidad de Microsoft deban quitar el registro de una aplicación.

En las secciones siguientes, aprenderá a:

* Eliminación de una aplicación creada por el usuario o por la organización
* Eliminación de una aplicación creada por otra organización

## <a name="prerequisites"></a>Prerrequisitos

* Una [aplicación registrada en el inquilino de Azure AD](quickstart-register-app.md)

## <a name="remove-an-application-authored-by-you-or-your-organization"></a>Eliminación de una aplicación creada por el usuario o por la organización

Las aplicaciones que el usuario o la empresa han registrado están representadas tanto por un objeto de aplicación como por un objeto de entidad de servicio en el inquilino. Para obtener más información, vea [Objetos de aplicación y objetos de entidad de servicio](./app-objects-and-service-principals.md).

Para eliminar una aplicación, es necesario que aparezca como propietario de la aplicación o que tenga privilegios de administrador.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
1. En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory** y, después, seleccione **Registros de aplicaciones**. Busque y seleccione la aplicación que desea configurar. Cuando haya seleccionado la aplicación, verá la página **Introducción** de la aplicación.
1. En la página **Introducción**, seleccione **Eliminar**.
1. Seleccione **Sí** para confirmar que quiere eliminar la aplicación.

## <a name="remove-an-application-authored-by-another-organization"></a>Eliminación de una aplicación creada por otra organización

Si está viendo **Registros de aplicaciones** en el contexto de un inquilino, un subconjunto de las aplicaciones que aparecen en la pestaña **Todas las aplicaciones** son de otro inquilino y se registraron en su inquilino durante el proceso de consentimiento. Más específicamente, se representan solo mediante un objeto de entidad de servicio en su inquilino, sin un objeto aplicación correspondiente. Para más información sobre las diferencias entre objetos de aplicación y objetos de entidad de servicio, consulte [Objetos de aplicación y de entidad de servicio en Azure AD](./app-objects-and-service-principals.md).

Para quitar el acceso de una aplicación a su directorio (después de concederle consentimiento), el administrador de la compañía debe eliminar su entidad de servicio. El administrador debe tener acceso de administrador global y puede quitar la aplicación mediante Azure Portal o usar los [cmdlets de PowerShell de Azure AD](/previous-versions/azure/jj151815(v=azure.100)) para eliminarlo.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los [objetos de aplicación y de entidad de servicio](app-objects-and-service-principals.md) en la plataforma de identidad de Microsoft.
