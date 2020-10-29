---
title: 'Asignación y enumeración de roles con ámbito de unidad administrativa: Azure Active Directory | Microsoft Docs'
description: Uso de unidades administrativas para restringir el ámbito de las asignaciones de roles en Azure Active Directory
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
ms.openlocfilehash: 66a4810b3a84cac55a49744025b6ac71c3f1c0a7
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92374138"
---
# <a name="assign-scoped-roles-to-an-administrative-unit"></a>Asignación de roles con ámbito a una unidad administrativa

En Azure Active Directory (Azure AD), puede asignar usuarios a un rol de Azure AD con un ámbito limitado a una o más unidades administrativas para lograr un control administrativo más específico.

Consulte [Introducción](admin-units-manage.md#get-started) para conocer los pasos previos al uso de PowerShell y Microsoft Graph para la administración de unidades administrativas.

## <a name="roles-available"></a>Roles disponibles

Role  |  Descripción
----- |  -----------
Administrador de autenticación  |  Tiene acceso para ver, configurar y restablecer la información de los métodos de autenticación de cualquier usuario que no sea administrador solo en la unidad administrativa asignada.
Administrador de grupos  |  Puede administrar todos los aspectos de los grupos y las configuraciones de grupos, como las directivas de nomenclatura y expiración solo en la unidad administrativa asignada.
Administrador del departamento de soporte técnico  |  Puede restablecer contraseñas de usuarios que no son administradores y de administradores del departamento de soporte técnico solo en la unidad administrativa asignada.
Administrador de licencias  |  Puede asignar, quitar y actualizar las asignaciones de licencia solo dentro de la unidad administrativa.
Administrador de contraseñas  |  Puede restablecer contraseñas de usuarios que no son administradores y de administradores de contraseña solo en la unidad administrativa asignada.
Administrador de usuarios  |  Puede administrar todos los aspectos de usuarios y grupos, incluido el restablecimiento de contraseñas para administradores limitados solo en la unidad administrativa asignada.

## <a name="security-principals-that-can-be-assigned-to-a-scoped-role"></a>Entidades de seguridad que se pueden asignar a un rol con ámbito

Las siguientes entidades de seguridad pueden asignarse a un rol con un ámbito de unidad administrativa:

* Usuarios
* Grupos de nube asignables a roles (versión preliminar)
* Nombre de entidad de seguridad de servicio (SPN)

## <a name="assign-a-scoped-role"></a>Asignación de un rol con ámbito

### <a name="azure-portal"></a>Azure Portal

Vaya a **Azure AD > Unidades administrativas** en el portal. Seleccione la unidad administrativa en la que quiere asignarle el rol a un usuario. En el panel izquierdo, seleccione Roles y administradores para enumerar todos los roles disponibles.

![Seleccione la unidad administrativa a la que cambiará el ámbito de rol.](./media/admin-units-assign-roles/select-role-to-scope.png)

Seleccione el rol que se va a asignar y, a continuación, seleccione **Agregar asignaciones** . Se abrirá un panel a la derecha en el que podrá seleccionar uno o varios usuarios para asignarlos al rol.

![Seleccione el rol al que se agregará el ámbito y, a continuación, seleccione Agregar asignaciones.](./media/admin-units-assign-roles/select-add-assignment.png)

> [!Note]
>
> Para asignar un rol en una unidad administrativa mediante PIM, siga los pasos que se describen [aquí](../privileged-identity-management/pim-how-to-add-role-to-user.md?tabs=new#assign-a-role-with-restricted-scope).

### <a name="powershell"></a>PowerShell

```powershell
$AdminUser = Get-AzureADUser -ObjectId "Use the user's UPN, who would be an admin on this unit"
$Role = Get-AzureADDirectoryRole | Where-Object -Property DisplayName -EQ -Value "User Account Administrator"
$administrativeUnit = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'The display name of the unit'"
$RoleMember = New-Object -TypeName Microsoft.Open.AzureAD.Model.RoleMemberInfo
$RoleMember.ObjectId = $AdminUser.ObjectId
Add-AzureADMSScopedRoleMembership -ObjectId $administrativeUnit.ObjectId -RoleObjectId $Role.ObjectId -RoleMemberInfo $RoleMember
```

La sección resaltada se puede cambiar según sea necesario para el entorno específico.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
POST /directory/administrativeUnits/{id}/scopedRoleMembers
    
Request body
{
  "roleId": "roleId-value",
  "roleMemberInfo": {
    "id": "id-value"
  }
}
```

## <a name="list-the-scoped-admins-on-an-au"></a>Enumeración de los administradores con ámbito en una unidad administrativa

### <a name="azure-portal"></a>Azure Portal

Todas las asignaciones de roles realizadas con un ámbito de unidad administrativa se pueden ver en la sección [Unidades administrativas de Azure AD](https://ms.portal.azure.com/?microsoft_aad_iam_adminunitprivatepreview=true&microsoft_aad_iam_rbacv2=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AdminUnit). Vaya a **Azure AD > Unidades administrativas** en el portal. Seleccione la unidad de administración de las asignaciones de roles que quiere enumerar. Seleccione **Roles y administradores** y abra un rol para ver las asignaciones en la unidad de administración.

### <a name="powershell"></a>PowerShell

```powershell
$administrativeUnit = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'The display name of the unit'"
Get-AzureADMSScopedRoleMembership -ObjectId $administrativeUnit.ObjectId | fl *
```

La sección resaltada se puede cambiar según sea necesario para el entorno específico.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
GET /directory/administrativeUnits/{id}/scopedRoleMembers
Request body
{}
```

## <a name="next-steps"></a>Pasos siguientes

- [Uso de grupos en la nube para administrar asignaciones de roles](groups-concept.md)
- [Solución de problemas de roles asignados a grupos en la nube](groups-faq-troubleshooting.md)
