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
ms.openlocfilehash: ec0b3ef559abe1c65872d8ecf87f63e6ff3ed4b0
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2020
ms.locfileid: "86223914"
---
# <a name="quickstart-delete-an-application-from-your-azure-active-directory-azure-ad-tenant"></a>Inicio rápido: Eliminación de una aplicación del inquilino de Azure Active Directory (Azure AD)

Este inicio rápido utiliza Azure Portal para eliminar una aplicación que se agregó al inquilino de Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Requisitos previos

Para eliminar una aplicación del inquilino de Azure AD, necesita:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Uno de los siguientes roles: Administrador global, Administrador de aplicaciones en la nube, Administrador de aplicaciones o Propietario de la entidad de servicio.
- Opcional: Haber finalizado el inicio rápido de [Visualización de las aplicaciones](view-applications-portal.md).
- Opcional: Haber finalizado el inicio rápido de [Incorporación de una aplicación](add-application-portal.md).
- Opcional: Haber finalizado el inicio rápido de [Configuración de una aplicación](add-application-portal-configure.md).
- Opcional: Haber finalizado el inicio rápido de [Configuración del inicio de sesión único](add-application-portal-setup-sso.md).

>[!IMPORTANT]
>Para probar los pasos de este inicio rápido, use un entorno que no sea de producción.

## <a name="delete-an-application-from-your-azure-ad-tenant"></a>Eliminación de una aplicación del inquilino de Azure AD

Para eliminar una aplicación del inquilino de Azure AD:

1. En el portal de Azure AD, seleccione **Aplicaciones empresariales**. Busque y seleccione la aplicación que desea eliminar. En este caso, se eliminará la aplicación **GitHub_test** que agregamos en el inicio rápido anterior.
1. En la sección **Administrar** en el panel izquierdo, seleccione **Propiedades**.
1. Seleccione **Eliminar** y, a continuación, seleccione **Sí** para confirmar que quiere eliminar la aplicación de su inquilino de Azure AD.


## <a name="next-steps"></a>Pasos siguientes

- [Procedimientos recomendados de administración de aplicaciones](application-management-fundamentals.md)
- [Escenarios comunes de administración de aplicaciones](common-scenarios.md)
- [Visibilidad y control de la administración de aplicaciones](cloud-app-security.md)