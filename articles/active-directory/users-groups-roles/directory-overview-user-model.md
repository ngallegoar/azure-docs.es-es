---
title: 'Introducción a usuarios, grupos, licencias y roles: Azure AD | Microsoft Docs'
description: La relación entre usuarios y licencias asignadas, roles de administrador y pertenencia a grupos en Azure Active Directory
keywords: ''
author: curtand
manager: daveba
ms.author: curtand
ms.reviewer: vincesm
ms.date: 11/08/2019
ms.topic: overview
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
services: active-directory
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8052bb44df6d29e17304763051fa2099009b9701
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583020"
---
# <a name="users-groups-licensing-and-roles-for-large-organizations"></a>Usuarios, grupos, licencias y roles para grandes organizaciones

Este artículo introduce al administrador de Azure AD a la relación entre las tareas principales de [administración de identidades](/azure/active-directory/fundamentals/identity-fundamentals?context=azure/active-directory/users-groups-roles/context/ugr-context) de los usuarios en cuanto a sus grupos, licencias, aplicaciones empresariales implementadas y roles de administrador. A medida que crece la organización, puede usar grupos de Azure AD y roles de administrador para:

* Asignar licencias a grupos en lugar de individualmente
* Delegar permisos para distribuir el trabajo de administración de Azure AD a roles con menos privilegios
* Asignar acceso a aplicaciones empresariales a grupos

## <a name="assign-users-to-groups"></a>Asignar usuarios a grupos

Puede usar grupos de Azure AD para asignar licencias a un gran número de usuarios o para asignar a los usuarios acceso a las aplicaciones empresariales implementadas. Puede usar grupos para asignar todos los roles de administrador en Azure AD excepto el de administrador global, o puede conceder acceso a recursos externos, como aplicaciones de SaaS o sitios de SharePoint.

Para ofrecer una mayor flexibilidad y reducir el trabajo de administrar la pertenencia a grupos, puede usar [grupos dinámicos](groups-create-rule.md) en Azure AD para expandir y contraer automáticamente la pertenencia a grupos. Necesitará una licencia de Azure AD Premium P1 para cada usuario único que sea miembro de uno o varios grupos dinámicos.

## <a name="assign-licenses-to-groups"></a>Asignación de licencias a grupos

Asignar licencias a los usuarios o quitárselas de uno en uno pueden exigir tiempo y atención. Si en su lugar [asigna licencias a grupos](/azure/active-directory/fundamentals/license-users-groups?context=azure/active-directory/users-groups-roles/context/ugr-context), puede facilitar la administración de licencias a gran escala.

En Azure AD, cuando los usuarios se unen a un grupo con licencia, se les asignan automáticamente las licencias correspondientes. Cuando los usuarios dejan el grupo, Azure AD quita sus asignaciones de licencia. Sin grupos de Azure AD, tendría que escribir un script de PowerShell o usar Graph API para agregar o quitar licencias de usuario de forma masiva para los usuarios que se unen a la organización o salen de ella.

Si no hay suficientes licencias disponibles, o se produce un problema, por ejemplo, planes de servicio que no se pueden asignar al mismo tiempo, se puede ver el estado de cualquier problema de licencia del grupo en Azure Portal.

>[!NOTE]
>La característica de administración de licencias basadas en grupos está actualmente en versión preliminar pública. Durante la versión preliminar, la característica está disponible con cualquier evaluación gratuita o plan de licencia de Azure Active Directory (Azure AD) de pago.

## <a name="delegate-administrator-roles"></a>Delegación de roles de administrador

Muchas grandes organizaciones desean opciones para que sus usuarios obtengan permisos suficientes para sus tareas de trabajo sin asignar el importante rol de administrador global a, por ejemplo, los usuarios que deben registrar las aplicaciones. Este es un ejemplo de nuevos roles de administrador de Azure AD para ayudarle a distribuir el trabajo de administración de aplicaciones de forma más pormenorizada:

 Nombre de rol | Resumen de permisos
 --------- | -------------------
 **Administrador de aplicaciones** | Puede agregar y administrar aplicaciones empresariales y registros de aplicación y configurar la aplicación de proxy. Los administradores de aplicaciones pueden ver los dispositivos y las directivas de acceso condicional, pero no administrarlas.
 **Administrador de aplicaciones en la nube** | Puede agregar y administrar aplicaciones empresariales y registros de aplicaciones empresariales. Este rol tiene todos los permisos del administrador de aplicaciones, salvo que no puede administrar la configuración de proxy de aplicación.
**Desarrollador de aplicaciones** | Puede agregar y actualizar registros de aplicaciones, pero no puede administrar aplicaciones empresariales ni configurar un proxy de aplicación.

Se está en proceso de agregar nuevos roles de administrador de Azure AD. Visite Azure Portal o consulte la [referencia de permisos del rol de administrador](directory-assign-admin-roles.md) para conocer los roles actualmente disponibles.

## <a name="assign-app-access"></a>Asignación de acceso a aplicaciones

Puede usar Azure AD para asignar al grupo acceso a las [aplicaciones empresariales que están implementadas en la organización de Azure AD](/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups?context=azure/active-directory/users-groups-roles/context/ugr-context). Si combina grupos dinámicos con la asignación de grupos a aplicaciones, puede automatizar las asignaciones de acceso a los usuarios a las aplicaciones a medida que crece la organización. Para asignar acceso a las aplicaciones empresariales, necesitará una licencia de Azure Active Directory Premium P1 o Premium P2.

Azure AD también le ofrece un control pormenorizado de los datos que fluyen entre la aplicación y los grupos a los que se asigna el acceso. En [Aplicaciones empresariales](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps), abra una aplicación y seleccione **Aprovisionamiento** para:

* Configurar el aprovisionamiento automático de las aplicaciones que lo admitan
* Proporcionar credenciales para conectarse a la API de administración del usuario de la aplicación
* Configurar las asignaciones que controlan qué atributos de usuario fluyen entre Azure AD y la aplicación cuando se aprovisionan o actualizan las cuentas de usuario
* Iniciar y detener el servicio de aprovisionamiento de Azure AD de una aplicación, borrar la caché de aprovisionamiento o reiniciar el servicio
* Ver el **informe de actividad de aprovisionamiento** que proporciona un registro de todos los usuarios y grupos creados, actualizados y quitados entre Azure AD y la aplicación, y el **informe de errores de aprovisionamiento** que ofrece más información sobre los mensajes de error

## <a name="next-steps"></a>Pasos siguientes

Si es un administrador de Azure AD principiante, conozca los conceptos básicos en [Fundamentos de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/index).

También, puede comenzar a [crear grupos](/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context), [asignar licencias](/azure/active-directory/fundamentals/license-users-groups?context=azure/active-directory/users-groups-roles/context/ugr-context), [asignar acceso a la aplicación](/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups?context=azure/active-directory/users-groups-roles/context/ugr-context) o [asignar roles de administrador](directory-assign-admin-roles.md).
