---
title: 'Inicio rápido: Eliminación de una aplicación del inquilino de Azure Active Directory (Azure AD)'
description: Este inicio rápido utiliza Azure Portal para eliminar una aplicación del inquilino de Azure Active Directory (Azure AD).
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02d12538fce5ccc3905ea1170fc1a8324309004f
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85956005"
---
# <a name="quickstart-delete-an-application-from-your-azure-active-directory-azure-ad-tenant"></a>Inicio rápido: Eliminación de una aplicación del inquilino de Azure Active Directory (Azure AD)

Este inicio rápido usa Azure Portal para eliminar una aplicación que se ha agregado a su inquilino de Azure AD.

## <a name="prerequisites"></a>Requisitos previos

Para eliminar una aplicación del inquilino de Azure AD, necesita:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Uno de los siguientes roles: Administrador global, Administrador de aplicaciones en la nube, Administrador de aplicaciones o Propietario de la entidad de servicio.
- (Opcional: finalización de [Visualización de sus aplicaciones](view-applications-portal.md)).
- (Opcional: finalización de [Adición de una aplicación](add-application-portal.md)).
- (Opcional: finalización de [Configuración de una aplicación](add-application-portal-configure.md)).
- (Opcional: finalización de [Configuración del inicio de sesión único](add-application-portal-setup-sso.md)).

>[!IMPORTANT]
>Para probar los pasos de este inicio rápido, se recomienda usar un entorno que no sea de producción.

## <a name="delete-an-application-from-your-azure-ad-tenant"></a>Eliminación de una aplicación del inquilino de Azure AD

Para eliminar una aplicación del inquilino de Azure AD:

1. En el portal de Azure AD, seleccione **Aplicaciones empresariales** y, a continuación, busque y seleccione la aplicación que desea eliminar. En este caso, se eliminará la aplicación GitHub_test que agregamos en el inicio rápido anterior.
2. En la sección Administrar de la navegación, seleccione **Propiedades**.
3. Seleccione Eliminar y, a continuación, seleccione Sí para confirmar que quiere eliminar la aplicación de su inquilino de Azure AD.


## <a name="next-steps"></a>Pasos siguientes

- [Procedimientos recomendados de administración de aplicaciones](application-management-fundamentals.md)
- [Escenarios comunes de administración de aplicaciones](common-scenarios.md)
- [Visibilidad y control de la administración de aplicaciones](cloud-app-security.md)