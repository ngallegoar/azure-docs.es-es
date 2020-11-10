---
title: Unidades administrativas en Azure Active Directory | Microsoft Docs
description: Utilice unidades administrativas para una delegación más detallada de permisos en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 09/22/2020
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0fb24d0cd2714969b5a888b1036f524c4c062d76
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "93027625"
---
# <a name="administrative-units-in-azure-active-directory"></a>Unidades administrativas en Azure Active Directory

En este artículo se describen las unidades administrativas en Azure Active Directory (Azure AD). Una unidad administrativa es un recurso de Azure AD que puede ser un contenedor para otros recursos de Azure AD. Una unidad administrativa solo puede contener usuarios y grupos.

Mediante el uso de unidades administrativas, puede conceder permisos de administrador que están restringidos a un departamento, a una región o a otro segmento de la organización que se defina. Puede usar las unidades administrativas para delegar permisos en los administradores regionales o establecer directivas de forma pormenorizada. Por ejemplo, un administrador de cuentas de usuario podría actualizar la información del perfil, restablecer las contraseñas y asignar las licencias para los usuarios solo en su unidad administrativa.

Por ejemplo, podría delegar en los especialistas de soporte técnico regionales el rol [Administrador del departamento de soporte técnico](permissions-reference.md#helpdesk-administrator), restringido solo a administrar a los usuarios de la región en cuestión.

## <a name="deployment-scenario"></a>Escenario de implementación

Puede resultar útil para restringir el ámbito administrativo mediante el uso de unidades administrativas en organizaciones compuestas de divisiones independientes de cualquier tipo. Considere el ejemplo de una universidad grande que se compone de muchas escuelas autónomas (Escuela de negocios, Escuela de ingeniería, etc.). Cada escuela tiene un equipo de administradores de TI que controlan el acceso, administran los usuarios y establecen directivas para su escuela. 

Un administrador central podría:

- Crear un rol con permisos administrativos solo sobre los usuarios de Azure AD de la unidad administrativa de la Escuela de negocios.
- Crear una unidad administrativa para la Escuela de negocios.
- Rellenar la unidad administrativa solo con los alumnos y el personal de la Escuela de negocios.
- Agregar el equipo de TI de la Escuela de negocios al rol, junto con su ámbito.

## <a name="license-requirements"></a>Requisitos de licencia

Para usar unidades administrativas, necesita una licencia de Azure Active Directory Premium para cada administrador de unidad administrativa y licencias gratuitas de Azure Active Directory para los miembros de las unidades administrativas. Para obtener más información, consulte [Introducción a Azure AD Premium](../fundamentals/active-directory-get-started-premium.md).

## <a name="manage-administrative-units"></a>Administración de unidades administrativas

Puede administrar las unidades administrativas mediante Azure Portal, cmdlets y scripts de PowerShell o Microsoft Graph. Para más información, consulte:

- [Crear, quitar, rellenar y agregar roles a las unidades administrativas](admin-units-manage.md): incluye procedimientos completos.
- [Trabajo con unidades administrativas](/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0&preserve-view=true): se describe cómo trabajar con unidades administrativas mediante PowerShell.
- [Soporte de Graph en unidades administrativas](/graph/api/resources/administrativeunit?view=graph-rest-1.0&preserve-view=true): proporciona documentación detallada sobre Microsoft Graph para unidades administrativas.

### <a name="plan-your-administrative-units"></a>Planeamiento de las unidades administrativas

Puede utilizar unidades administrativas para agrupar recursos de Azure AD de manera lógica. Por ejemplo, en el caso de una organización cuyo departamento de TI está disperso a nivel global, puede que tenga sentido crear unidades administrativas que definan esos límites geográficos. En otro escenario, en el que una organización multinacional tiene distintas *suborganizaciones* que son semiautónomas en las operaciones, una unidad administrativa puede representar a cada suborganización.

Los requisitos únicos de una organización guían los criterios según los cuales se crean las unidades administrativas. Las unidades administrativas son una forma habitual de definir la estructura en todos los servicios de Microsoft 365. Se recomienda que prepare las unidades administrativas teniendo en mente su uso en los servicios de Microsoft 365. Puede aprovechar al máximo las unidades administrativas cuando puede asociar recursos comunes en Microsoft 365 bajo una unidad administrativa.

Puede esperar que la creación de las unidades administrativas en la organización pase por las fases siguientes:

1. **Adopción inicial** : la organización empezará a crear unidades administrativas en función de los criterios iniciales y el número de unidades administrativas aumentará a medida que se perfeccionan los criterios.
1. **Eliminación** : una vez que los criterios estén bien definidos, se eliminarán las unidades administrativas que ya no se requieran.
1. **Estabilización** : la estructura de la organización está bien definida y el número de unidades administrativas no va a cambiar significativamente a corto plazo.

## <a name="currently-supported-scenarios"></a>Escenarios admitidos actualmente

Como administrador global o administrador de roles con privilegios, puede usar el portal de Azure AD para crear unidades administrativas, agregar usuarios como miembros de las unidades administrativas y, luego, asignar personal de TI a los roles de administrador con ámbito de unidad administrativa. Los administradores con ámbito de unidad administrativa pueden usar el centro de administración de Microsoft 365 para la administración básica de los usuarios de sus unidades administrativas.

Además, puede agregar grupos como miembros de una unidad administrativa. Un administrador de grupo con ámbito de unidad administrativa puede administrarlos mediante PowerShell, Microsoft Graph y el portal de Azure AD.

En la tabla siguiente se describe la compatibilidad actual con los escenarios de unidades administrativas:

### <a name="administrative-unit-management"></a>Administración de unidades administrativas

| Permisos |   Graph y PowerShell   | Portal de Azure AD | Centro de administración de Microsoft 365 | 
| -- | -- | -- | -- |
| Crear y eliminar unidades administrativas   |    Compatible    |   Compatible   |    No compatible | 
| Agregar y quitar miembros de las unidades administrativas de manera individual    |   Compatible    |   Compatible   |    No compatible | 
| Agregar o eliminar miembros de las unidades administrativas de forma masiva con archivos .csv   |    No compatible     |  Compatible   |    Sin planes de compatibilidad | 
| Asignar administradores con ámbito de unidad administrativa  |     Compatible    |   Compatible    |   No compatible | 
| Agregar y eliminar miembros de las unidades administrativas de manera dinámica en función de los atributos | No compatible | No compatible | No compatible 

### <a name="user-management"></a>Administración de usuarios

| Permisos |   Graph y PowerShell   | Portal de Azure AD | Centro de administración de Microsoft 365 |
| -- | -- | -- | -- |
| Administración con ámbito de unidad administrativa de las propiedades de usuario, contraseñas y licencias   |    Compatible     |  Compatible   |   Compatible |
| Bloqueo y desbloqueo con ámbito de unidad administrativa de los inicios de sesión de usuario    |   Compatible   |    Compatible   |    Compatible |
| Administración con ámbito de unidad administrativa de las credenciales de autenticación multifactor del usuario   |    Compatible   |   Compatible   |   No compatible |

### <a name="group-management"></a>Administración de grupos

| Permisos |   Graph y PowerShell   | Portal de Azure AD | Centro de administración de Microsoft 365 |
| -- | -- | -- | -- |
| Administración con ámbito de unidad administrativa de los miembros y las propiedades de un grupo     |  Compatible   |    Compatible    |  No compatible |
| Administración con ámbito de unidad administrativa de las licencias de grupo   |    Compatible  |    Compatible   |   No compatible |


Las unidades administrativas solo aplican el ámbito a los permisos de administración. No impiden que los miembros o los administradores usen sus [permisos de usuario predeterminados](../fundamentals/users-default-permissions.md) para examinar otros usuarios, grupos o recursos fuera de la unidad administrativa. En el centro de administración de Microsoft 365, se filtran los usuarios que están fuera de las unidades administrativas de un administrador con ámbito. Sin embargo, puede examinar otros usuarios en el portal de Azure AD, PowerShell y otros servicios de Microsoft.

## <a name="next-steps"></a>Pasos siguientes

- [Administración de unidades administrativas](admin-units-manage.md)
- [Adición y administración de usuarios en las unidades administrativas en Azure Active Directory](admin-units-add-manage-users.md)
- [Adición y administración de grupos en las unidades administrativas en Azure Active Directory](admin-units-add-manage-groups.md)
- [Asignación de roles con ámbito a una unidad administrativa](admin-units-assign-roles.md)
