---
title: Asignación de usuarios a las aplicaciones | Microsoft Docs
description: Descripción de cómo se asignan los usuarios a una aplicación del inquilino
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 516bffa7057f8fee3b8e38d46f3b2da905880044
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2020
ms.locfileid: "88639943"
---
# <a name="how-to-assign-users-to-applications"></a>Asignación de usuarios a las aplicaciones

Este artículo le ayudará a comprender cómo se asignan los usuarios a una aplicación del inquilino.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>¿Cómo se asignan los usuarios a una aplicación de Azure AD?

Para que un usuario tenga acceso a una aplicación, se le debe primero asignar a ella de alguna forma. Un administrador, un delegado de la empresa o, en ocasiones, el usuario mismo puede realizar esta asignación. A continuación se describen las maneras en las que se pueden asignar los usuarios a las aplicaciones:

1.  Un administrador [asigna un usuario](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) a la aplicación directamente

2.  Un administrador [asigna un grupo](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal), del que el usuario forma parte, a la aplicación, incluidos:

    * Un grupo que se ha sincronizado de forma local

    * Un grupo de seguridad estático creado en la nube

    * Un [grupo de seguridad dinámico](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) creado en la nube

    * Un grupo de Office 365 creado en la nube

    * El grupo [Todos los usuarios](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups)

3.  Un administrador habilita [Acceso de autoservicio a las aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) para permitir que un usuario agregue una aplicación mediante la característica **Agregar aplicación** de la página [Aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **sin aprobación de la empresa**.

4.  Un administrador habilita [Acceso de autoservicio a las aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) para permitir que un usuario agregue una aplicación mediante la característica **Agregar aplicación** de la página [Aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) pero solo **con la aprobación previa de un conjunto seleccionado de aprobadores de la empresa**.

5.  Un administrador habilita [Administración de grupos de autoservicio](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) para permitir que un usuario se una a un grupo al que se ha asignado una aplicación **sin la aprobación de la empresa**

6.  Un administrador habilita [Administración de grupos de autoservicio](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) para permitir que un usuario se una a un grupo al que se ha asignado una aplicación pero solo **con la aprobación anterior de un conjunto seleccionado de aprobadores de la empresa**

7.  Un administrador asigna una licencia a un usuario directamente para una aplicación original de Microsoft, como [Microsoft Office 365](https://products.office.com/)

8.  Un administrador asigna una licencia a un grupo del que el usuario forma parte para una aplicación original de Microsoft, como [Microsoft Office 365](https://products.office.com/)

9.  Un [administrador da su consentimiento a una aplicación](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) para que la usen todos los usuarios y, a continuación, un usuario inicia sesión en ella

10. Un usuario [da su consentimiento a una aplicación](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) por sí mismo iniciando sesión en ella directamente

## <a name="next-steps"></a>Pasos siguientes
[Administración de aplicaciones con Azure Active Directory](what-is-application-management.md)
