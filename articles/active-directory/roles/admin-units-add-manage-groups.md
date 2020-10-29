---
title: 'Adición, eliminación y enumeración de grupos en una unidad administrativa: Azure Active Directory | Microsoft Docs'
description: Administración de grupos y sus permisos de rol en una unidad administrativa en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 10/07/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec566fecb0dd14a4e56a2a3b9a59745bfe549faf
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92374144"
---
# <a name="add-and-manage-groups-in-administrative-units-in-azure-active-directory"></a>Adición y administración de grupos en las unidades administrativas en Azure Active Directory

En Azure Active Directory (Azure AD), puede agregar grupos a una unidad administrativa para un ámbito administrativo de control más específico.

Consulte [Introducción](admin-units-manage.md#get-started) para conocer los pasos previos al uso de PowerShell y Microsoft Graph para la administración de unidades administrativas.

## <a name="add-groups-to-an-au"></a>Adición de grupos a una unidad administrativa

### <a name="azure-portal"></a>Portal de Azure

Solo puede asignar grupos individualmente a una unidad administrativa. No hay ninguna opción para asignar grupos de forma masiva a una unidad administrativa. En el portal, puede asignar un grupo a una unidad administrativa de dos formas:

1. En la página **Azure AD > Grupos**

    Abra la página Información general de grupos en Azure AD y seleccione el grupo que debe asignarse a la unidad administrativa. En el lado izquierdo, seleccione **Unidades administrativas** para ver las unidades administrativas a las que está asignado el grupo. En la parte superior encontrará la opción Asignar a la unidad administrativa y, al hacer clic en ella, se mostrará un panel en el lado derecho para elegir la unidad administrativa.

    ![asignar un grupo de forma individual a una unidad administrativa](./media/admin-units-add-manage-groups/assign-to-group-1.png)

1. En la página **Azure AD > Unidades administrativas > Todos los grupos**

    Abra la hoja Todos los grupos en Azure AD > Unidades administrativas. Si ya hay grupos asignados a la unidad administrativa, se mostrarán en el lado derecho. Seleccione **Agregar** en la parte superior y se abrirá un panel derecho que muestra los grupos disponibles en su organización de Azure AD. Seleccione uno o más grupos para asignarlos a las unidades administrativas.

    ![seleccione una unidad administrativa y, a continuación, seleccione Agregar miembro](./media/admin-units-add-manage-groups/assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

```powershell
$administrative unitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
$GroupObj = Get-AzureADGroup -Filter "displayname eq 'TestGroup'"
Add-AzureADMSAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId -RefObjectId $GroupObj.ObjectId
```

En este ejemplo, el cmdlet Add-AzureADMSAdministrativeUnitMember se usa para agregar el grupo a la unidad administrativa. El id. de objeto de la unidad administrativa y el id. de objeto del grupo que se va a agregar se usan como argumento. La sección resaltada se puede cambiar según sea necesario para el entorno específico.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
POST /administrativeUnits/{Admin Unit id}/members/$ref

Request body
{
"@odata.id":"https://graph.microsoft.com/v1.0/groups/{id}"
}
```

Ejemplo:

```http
{
"@odata.id":"https://graph.microsoft.com/v1.0/groups/ 871d21ab-6b4e-4d56-b257-ba27827628f3"
}
```

## <a name="list-groups-in-an-au"></a>Lista de grupos en una unidad administrativa

### <a name="azure-portal"></a>Portal de Azure

Vaya a **Azure AD > Unidades administrativas** en el portal. Seleccione la unidad administrativa cuyos usuarios quiere enumerar. De forma predeterminada, la opción **Todos los usuarios** ya está seleccionada en el panel izquierdo. Seleccione **Todos los grupos** y, a la derecha, se mostrará la lista de los grupos que son miembros de la unidad administrativa seleccionada.

![Enumeración de los grupos en una unidad administrativa](./media/admin-units-add-manage-groups/list-groups-in-admin-units.png)

### <a name="powershell"></a>PowerShell

```powershell
$administrative unitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
Get-AzureADMSAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId
```

Esto le ayudará a obtener a todos los miembros de la unidad administrativa. Si quiere mostrar todos los grupos que son miembros de la unidad administrativa, puede usar el siguiente fragmento de código:

```http
foreach ($member in (Get-AzureADMSAdministrativeUnitMember -ObjectId $administrative unitObj.ObjectId)) 
{
if($member.ObjectType -eq "Group")
{
Get-AzureADGroup -ObjectId $member.ObjectId
}
}
```

### <a name="microsoft-graph"></a>Microsoft Graph

```http
HTTP request
GET /directory/administrativeUnits/{Admin id}/members/$/microsoft.graph.group
Request body
{}
```

## <a name="list-aus-for-a-group"></a>Enumeración de las unidades administrativas de un grupo

### <a name="azure-portal"></a>Portal de Azure

En el portal de Azure AD, puede abrir los detalles de un grupo al abrir **Grupos** . Seleccione un grupo para abrir su perfil. Seleccione **Unidades administrativas** para ver todas las unidades administrativas en las que el grupo es un miembro.

![Enumeración de las unidades administrativas de un grupo](./media/admin-units-add-manage-groups/list-group-au.png)

### <a name="powershell"></a>PowerShell

```powershell
Get-AzureADMSAdministrativeUnit | where { Get-AzureADMSAdministrativeUnitMember -ObjectId $_.ObjectId | where {$_.ObjectId -eq $groupObjId} }
```

### <a name="microsoft-graph"></a>Microsoft Graph

```http
https://graph.microsoft.com/v1.0/groups/<group-id>/memberOf/$/Microsoft.Graph.AdministrativeUnit
```

## <a name="remove-a-group-from-an-au"></a>Eliminación de un grupo de una unidad administrativa

### <a name="azure-portal"></a>Portal de Azure

Hay dos maneras de quitar un grupo de una unidad administrativa en Azure Portal.

- Quitar de la información general de un grupo

  1. Abra **Azure AD** > **Grupos** y abra el perfil para el grupo que quiere quitar de una unidad administrativa.
  1. Seleccione **Unidades administrativas** en el panel izquierdo para ver todas las unidades administrativas en las que el grupo es un miembro. Seleccione la unidad administrativa de la que quiere quitar el grupo y, a continuación, seleccione **Quitar de la unidad administrativa** .

    ![Eliminación de un grupo de una unidad administrativa](./media/admin-units-add-manage-groups/group-au-remove.png)

- Quitar de una unidad administrativa

  1. Abra **Azure AD** > **Unidades administrativas** y seleccione la unidad administrativa de la que el grupo es miembro.
  1. Seleccione **Grupos** en el panel izquierdo para mostrar los grupos miembros.
  1. Seleccione el grupo que se va a quitar de la unidad administrativa y, a continuación, seleccione **Quitar grupos** .

    ![Enumeración de los grupos en una unidad administrativa](./media/admin-units-add-manage-groups/list-groups-in-admin-units.png)

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureADMSAdministrativeUnitMember -ObjectId $auId -MemberId $memberGroupObjId
```

### <a name="microsoft-graph"></a>Microsoft Graph

```http
https://graph.microsoft.com/v1.0/directory/AdministrativeUnits/<adminunit-id>/members/<group-id>/$ref
```

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de un rol a una unidad administrativa](admin-units-assign-roles.md)
- [Administración de los usuarios en una unidad administrativa](admin-units-add-manage-users.md)
