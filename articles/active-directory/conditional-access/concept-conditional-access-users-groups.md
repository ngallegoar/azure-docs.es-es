---
title: 'Usuarios y grupos de una directiva de acceso condicional: Azure Active Directory'
description: Quiénes son los usuarios y los grupos de una directiva de acceso condicional de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36898e75680771a9cb084fa142bb635ddbf51c70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77192049"
---
# <a name="conditional-access-users-and-groups"></a>Acceso condicional: Usuarios y grupos

Una directiva de acceso condicional debe incluir una asignación de usuario como una de las señales del proceso de toma de decisiones. Se pueden incluir o excluir usuarios de las directivas de acceso condicional. 

![Usuario como señal en las decisiones tomadas por acceso condicional](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups.png)

## <a name="include-users"></a>Incluir usuarios

Esta lista de usuarios incluye normalmente a todos los usuarios a los que se dirige una organización en una directiva de acceso condicional. 

Las siguientes opciones están disponibles para su inclusión al crear una directiva de acceso condicional.

- None
   - Ningún usuario seleccionado
- Todos los usuarios
   - Todos los usuarios que existen en el directorio, incluidos los invitados B2B.
- Seleccionar Usuarios y grupos
   - Todos los usuarios externos e invitados (versión preliminar)
      - Esta selección incluye todos los invitados B2B y usuarios externos, incluido cualquier usuario con el atributo `user type` establecido en `guest`. Esta selección también se aplica a cualquier usuario externo que haya iniciado sesión desde una organización diferente, como un proveedor de soluciones en la nube (CSP). 
   - Roles de directorio (versión preliminar)
      - Permite a los administradores seleccionar roles de directorio específicos de Azure AD que se usan para determinar la asignación. Por ejemplo, las organizaciones pueden crear una directiva más restrictiva en los usuarios a los que se ha asignado el rol de administrador global.
   - Usuarios y grupos
      - Permite dirigirse a conjuntos específicos de usuarios. Por ejemplo, las organizaciones pueden seleccionar un grupo que contenga todos los miembros del departamento de recursos humanos cuando tenga una aplicación de Recursos Humanos seleccionada como aplicación en la nube. Un grupo puede ser cualquier tipo de grupo en Azure AD, como grupos de distribución y de seguridad dinámicos o asignados.

## <a name="exclude-users"></a>Excluir usuarios

Las exclusiones se usan normalmente para cuentas de acceso de emergencia. En los siguientes artículos encontrará más información sobre las cuentas de acceso de emergencia y por qué son importantes: 

* [Administración de cuentas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md)
* [Crear una estrategia de administración de control de acceso resistente con Azure Active Directory](../authentication/concept-resilient-controls.md)

Las siguientes opciones están disponibles para su exclusión al crear una directiva de acceso condicional.

- Todos los usuarios externos e invitados (versión preliminar)
   - Esta selección incluye todos los invitados B2B y usuarios externos, incluido cualquier usuario con el atributo `user type` establecido en `guest`. Esta selección también se aplica a cualquier usuario externo que haya iniciado sesión desde una organización diferente, como un proveedor de soluciones en la nube (CSP). 
- Roles de directorio (versión preliminar)
   - Permite a los administradores seleccionar roles de directorio específicos de Azure AD que se usan para determinar la asignación. Por ejemplo, las organizaciones pueden crear una directiva más restrictiva en los usuarios a los que se ha asignado el rol de administrador global.
- Usuarios y grupos
   - Permite dirigirse a conjuntos específicos de usuarios. Por ejemplo, las organizaciones pueden seleccionar un grupo que contenga todos los miembros del departamento de recursos humanos cuando tenga una aplicación de Recursos Humanos seleccionada como aplicación en la nube. Un grupo puede ser cualquier tipo de grupo en Azure AD, como grupos de distribución y de seguridad dinámicos o asignados.

## <a name="next-steps"></a>Pasos siguientes

- [Acceso condicional: Aplicaciones o acciones en la nube](concept-conditional-access-cloud-apps.md)

- [Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)
