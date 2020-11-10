---
title: 'Solución de problemas de las unidades administrativas y Preguntas frecuentes: Azure Active Directory | Microsoft Docs'
description: Investigue las unidades administrativas para conceder permisos con ámbito restringido en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 09/22/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56bb9ab927c3383c49106a3a7b263ff8e137db9c
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "93026520"
---
# <a name="azure-ad-administrative-units-troubleshooting-and-faq"></a>Unidades administrativas de Azure AD: Solución de problemas y preguntas frecuentes

Para un control administrativo más pormenorizado en Azure Active Directory (Azure AD), puede asignar a los usuarios un rol de Azure AD con un ámbito limitado a una o varias unidades administrativas. Para obtener scripts de PowerShell de ejemplo para tareas comunes, consulte [Uso de unidades administrativas para trabajar](/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0&preserve-view=true).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P: ¿Por qué no puedo crear una unidad administrativa?**

**R:** Solo un *administrador global* o un *administrador de rol con privilegios* puede crear una unidad administrativa en Azure AD. Compruebe que el usuario que intenta crear la unidad administrativa tenga asignado el rol de *administrador global* o de *administrador de rol con privilegios*.

**P: He agregado un grupo a una unidad administrativa. ¿Por qué todavía no aparecen los miembros del grupo?**

**R:** Al agregar un grupo a una unidad administrativa, no todos los miembros del grupo se agregan a esta. Los usuarios deben asignarse directamente a la unidad administrativa.

**P: Acabo de agregar (o quitar) un miembro de la unidad administrativa. ¿Por qué no se muestra (o se sigue mostrando) en la interfaz de usuario?**

**R:** A veces, la adición o eliminación de uno o varios miembros de una unidad administrativa puede tardar unos minutos antes de que se refleje en el panel **Unidades administrativas**. También puede ir directamente a las propiedades del recurso asociado y ver si se ha completado la acción. Para más información sobre los usuarios y grupos de las unidades administrativas, consulte [Visualización de una lista de unidades administrativas para un usuario](admin-units-add-manage-users.md) y [Visualización de una lista de unidades administrativas para un grupo](admin-units-add-manage-groups.md).

**P: Soy administrador de contraseñas delegado de una unidad administrativa. ¿Por qué no puedo restablecer la contraseña de un usuario específico?**

**R:** Como administrador de una unidad administrativa, solo puede restablecer las contraseñas de los usuarios asignados a la unidad administrativa. Asegúrese de que el usuario para el que se produjo el error de restablecimiento de contraseña pertenece a la unidad administrativa a la que se le asignó. Si el usuario pertenece a la misma unidad administrativa y todavía no puede restablecer su contraseña, compruebe los roles que tiene asignados. 

Para evitar una elevación de privilegios, un administrador con ámbito de unidad administrativa no puede restablecer la contraseña de un usuario asignado a un rol con un ámbito de toda la organización.

**P: ¿Por qué son necesarias las unidades administrativas? ¿No podrían usarse grupos de seguridad para definir un ámbito?**

**R:** Los grupos de seguridad tienen un propósito y modo de autorización existentes. Un *administrador de usuarios* , por ejemplo, puede administrar la pertenencia a todos los grupos de seguridad de la organización de Azure AD. El rol podría usar grupos para administrar el acceso a aplicaciones como Salesforce. Un *administrador de usuarios* no debe poder administrar el modelo de delegación, que sucedería si los grupos de seguridad se ampliaran para admitir escenarios de "agrupación de recursos". 

Las unidades administrativas, como las unidades organizativas en Windows Server Active Directory, están diseñadas para proporcionar una manera de administrar con ámbito una amplia gama de objetos de directorio. Los grupos de seguridad pueden ser miembros de los ámbitos de recursos. El uso de grupos de seguridad para definir el conjunto de grupos de seguridad que un administrador puede administrar podría resultar confuso.

**P: ¿Por qué se agrega un grupo a una unidad administrativa?**

**R:** Al agregar un grupo a una unidad administrativa, el grupo se coloca en el ámbito de administración de cualquier *administrador de usuarios* cuyo ámbito también sea esa unidad administrativa. Los administradores de usuarios de la unidad administrativa pueden administrar el nombre y la pertenencia del grupo. No se concede al *administrador de usuarios* ningún permiso para administrar los usuarios del grupo (por ejemplo, restablecer sus contraseñas). Para conceder al *administrador de usuario* la capacidad de administrar usuarios, estos deben ser miembros directos de la unidad administrativa.

**P: ¿Un recurso (usuario o grupo) puede ser miembro de más de una unidad administrativa?**

**R:** Sí, un recurso puede ser miembro de más de una unidad administrativa. Los administradores con ámbito de la unidad administrativa o toda la organización que tienen permisos sobre el recurso pueden administrarlo.

**P: ¿Las unidades administrativas están disponibles en las organizaciones B2C?**

**R:** No, las unidades administrativas no están disponibles para las organizaciones B2C.

**P: ¿Se admiten las unidades administrativas anidadas?**

**R:** No se admiten las unidades administrativas anidadas.

**P: ¿Las unidades administrativas son compatibles con PowerShell y Graph API?**

**R:** Sí. Puede encontrar la compatibilidad con las unidades administrativas en la [documentación de cmdlets de PowerShell](/powershell/module/Azuread/?view=azureadps-2.0&preserve-view=true) y los [scripts de ejemplo](/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0&preserve-view=true).

La compatibilidad con el [tipo de recurso administrativeUnit](/graph/api/resources/administrativeunit?view=graph-rest-1.0&preserve-view=true) puede encontrarla en Microsoft Graph.

## <a name="next-steps"></a>Pasos siguientes

- [Restricción del ámbito de los roles mediante las unidades administrativas](administrative-units.md)
- [Administración de unidades administrativas](admin-units-manage.md)
