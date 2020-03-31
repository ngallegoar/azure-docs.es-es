---
title: 'Administración de unidades administrativas (versión preliminar): Azure AD | Microsoft Docs'
description: Uso de unidades administrativas para una delegación más detallada de permisos en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 11/13/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c4958975a080a98fb900bf317229ba9eda795d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74028409"
---
# <a name="administrative-units-management-in-azure-active-directory-preview"></a>Administración de unidades administrativas en Azure Active Directory (versión preliminar)

En este artículo se describen las unidades administrativas en Azure Active Directory (Azure AD). Una unidad administrativa es un recurso de Azure AD que puede ser un contenedor para otros recursos de Azure AD. En esta versión preliminar, estos recursos solo pueden ser usuarios. Por ejemplo, un administrador de cuentas de usuario con ámbito de la unidad administrativa puede actualizar la información del perfil, restablecer contraseñas y asignar licencias para usuarios solo en su unidad administrativa.

Puede usar las unidades administrativas para delegar permisos administrativos en subconjuntos de usuarios y aplicar directivas a uno de estos subconjuntos. Puede usar las unidades administrativas para delegar permisos en los administradores regionales o establecer directivas de forma pormenorizada.

## <a name="deployment-scenario"></a>Escenario de implementación

Las unidades administrativas pueden resultar útiles en organizaciones con divisiones independientes. Considere el ejemplo de una gran universidad que se compone de muchas escuelas autónomas (escuela de negocios, escuela de ingeniería, etc.), cada una de las cuales cuenta con sus propios administradores de TI, que controlan el acceso, administran usuarios y establecen directivas para su escuela. Un administrador central podría crear una unidad administrativa para la escuela de negocios y rellenarla solo con el personal y los estudiantes de dicha escuela. Luego, un administrador central puede agregar al personal de TI de la escuela de negocios a un rol de ámbito que conceda permisos administrativos solo sobre los usuarios de Azure AD de la unidad administrativa de la misma escuela.

## <a name="license-requirements"></a>Requisitos de licencia

El uso de unidades administrativas requiere una licencia Azure Active Directory Premium para cada administrador de la unidad administrativa. Para obtener más información, consulte [Introducción a Azure AD Premium](../fundamentals/active-directory-get-started-premium.md).

## <a name="managing-administrative-units"></a>Administración de unidades administrativas

En esta versión preliminar, puede crear y administrar unidades administrativas solo usando los cmdlets del módulo de Azure Active Directory para Windows PowerShell, tal como se describe en [Trabajo con unidades administrativas](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Para más información sobre los requisitos de software y la instalación del módulo de Azure AD, además de información de referencia sobre los cmdlets del módulo de Azure AD para administrar las unidades administrativas, incluida sintaxis, descripciones de parámetros y ejemplos, vea [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Pasos siguientes

[Ediciones de Azure Active Directory](../fundamentals/active-directory-whatis.md)
