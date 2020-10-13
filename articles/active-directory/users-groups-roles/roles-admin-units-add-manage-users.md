---
title: 'Adición, eliminación y enumeración de usuarios en una unidad administrativa: Azure Active Directory | Microsoft Docs'
description: Administración de usuarios y sus permisos de rol en una unidad administrativa en Azure Active Directory
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
ms.openlocfilehash: 0d29f4ef5806eb8ed9385696dea78f4ae0992b93
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91818138"
---
# <a name="add-and-manage-users-in-an-administrative-unit-in-azure-active-directory"></a>Adición y administración de usuarios en las unidades administrativas en Azure Active Directory

En Azure Active Directory (Azure AD), puede agregar usuarios a una unidad administrativa para un ámbito administrativo de control más específico.

Consulte [Introducción](roles-admin-units-manage.md#get-started) para conocer los pasos previos al uso de PowerShell y Microsoft Graph para la administración de unidades administrativas.

## <a name="add-users-to-an-au"></a>Adición de usuarios a una unidad administrativa

### <a name="azure-portal"></a>Portal de Azure

Puede asignar usuarios a unidades administrativas individualmente o en una operación masiva.

- Asignación individual desde un perfil de usuario

   1. Inicie sesión en el [Centro de administración de Azure AD](https://portal.azure.com) con permisos de Administrador de roles con privilegios.
   1. Seleccione **Usuarios** y elija el usuario que se va a asignar a una unidad administrativa para abrir el perfil de usuario.
   1. Seleccione **Unidades administrativas**. El usuario se puede asignar a una o varias unidades administrativas. Para ello, seleccione **Asignar a la unidad administrativa** y elija las unidades administrativas a las que se va a asignar el usuario.

       ![seleccione Agregar y, a continuación, escriba un nombre para la unidad administrativa](./media/roles-admin-units-add-manage-users/assign-users-individually.png)

- Asignación individual desde una unidad administrativa

   1. Inicie sesión en el [Centro de administración de Azure AD](https://portal.azure.com) con permisos de Administrador de roles con privilegios.
   1. Seleccione **Unidades administrativas** y, a continuación, seleccione la unidad administrativa a la que se van a asignar los usuarios.
   1. Seleccione **Todos los usuarios** y, a continuación, seleccione **Agregar miembro** y seleccione uno o varios usuarios para asignarlos a la unidad administrativa desde el panel **Agregar miembro** .

        ![seleccione una unidad administrativa y, a continuación, seleccione Agregar miembro](./media/roles-admin-units-add-manage-users/assign-to-admin-unit.png)

- Asignación masiva

   1. Inicie sesión en el [Centro de administración de Azure AD](https://portal.azure.com) con permisos de Administrador de roles con privilegios.
   1. Seleccione **Unidades administrativas**.
   1. Seleccione la unidad administrativa a la que se van a agregar los usuarios.
   1. Abra **Todos los usuarios** > **Agregar miembros desde archivo .csv**. Después, puede descargar la plantilla de valores separados por coma (CSV) y editar el archivo. El formato es sencillo y exige que se agregue un nombre principal de usuario único en cada línea. Una vez que el archivo esté listo, guárdelo en una ubicación adecuada y, a continuación, cárguelo como parte de este paso.

    ![asignación masiva de usuarios a una unidad administrativa](./media/roles-admin-units-add-manage-users/bulk-assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

```powershell
$administrativeunitObj = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
$UserObj = Get-AzureADUser -Filter "UserPrincipalName eq 'billjohn@fabidentity.onmicrosoft.com'"
Add-AzureADMSAdministrativeUnitMember -Id $administrativeunitObj.ObjectId -RefObjectId $UserObj.ObjectId
```

En el ejemplo anterior, el cmdlet Add-AzureADAdministrativeUnitMember se usa para agregar el usuario a la unidad administrativa. Se usan como argumentos tanto el id. de objeto de la unidad administrativa como el id. de objeto del usuario que se va a agregar. La sección resaltada se puede cambiar según sea necesario para el entorno específico.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
POST /administrativeUnits/{Admin Unit id}/members/$ref
Request body
{
  "@odata.id":"https://graph.microsoft.com/v1.0/users/{id}"
}
```

Ejemplo:

```http
{
  "@odata.id":"https://graph.microsoft.com/v1.0/users/johndoe@fabidentity.com"
}
```

## <a name="list-administrative-units-for-a-user"></a>Enumeración de las unidades administrativas de un usuario

### <a name="azure-portal"></a>Portal de Azure

En Azure Portal puede abrir el perfil de un usuario mediante los pasos siguientes:

1. Abra **Azure AD** > **Usuarios**.

1. Seleccione el usuario para abrir su perfil.

1. Seleccione **Unidades administrativas** para ver la lista de unidades administrativas a las que se ha asignado el usuario.

   ![Enumeración de las unidades administrativas de un usuario](./media/roles-admin-units-add-manage-users/list-user-admin-units.png)

### <a name="powershell"></a>PowerShell

```powershell
Get-AzureADMSAdministrativeUnit | where { Get-AzureADMSAdministrativeUnitMember -Id $_.ObjectId | where {$_.RefObjectId -eq $userObjId} }
```
Nota: De forma predeterminada, Get-AzureADAdministrativeUnitMember solo devuelve 100 miembros; puede agregar "-All $true" para recuperar más miembros.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
https://graph.microsoft.com/v1.0/users/{id}/memberOf/$/Microsoft.Graph.AdministrativeUnit
```

## <a name="remove-a-single-user-from-an-au"></a>Eliminación un solo usuario de una unidad administrativa

### <a name="azure-portal"></a>Portal de Azure

Hay dos maneras de quitar un usuario de una unidad administrativa. En Azure Portal puede abrir el perfil de un usuario si va a **Azure AD** > **Usuarios**. Seleccione el usuario para abrir su perfil. Seleccione la unidad administrativa de la que quiere que se quite el usuario y seleccione **Quitar de la unidad administrativa**.

![Eliminación de un usuario de una unidad administrativa desde el perfil de usuario](./media/roles-admin-units-add-manage-users/user-remove-admin-units.png)

También puede quitar un usuario en **Azure AD** > **Unidades administrativas** al seleccionar la unidad administrativa de la que quiere que se quiten los usuarios. Seleccione el usuario y seleccione **Quitar miembro**.
  
![Eliminación de un usuario en el nivel de unidad administrativa](./media/roles-admin-units-add-manage-users/admin-units-remove-user.png)

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureADMSAdministrativeUnitMember -Id $auId -MemberId $memberUserObjId
```

### <a name="microsoft-graph"></a>Microsoft Graph

   https://graph.microsoft.com/v1.0/directory/administrativeUnits/{adminunit-id}/members/{user-id}/ $ref

## <a name="bulk-remove-more-than-one-user"></a>Eliminación masiva de más de un usuario

Puede ir a Azure AD > Unidades administrativas y seleccione la unidad administrativa de la que quiere que se quiten los usuarios. Haga clic en Bulk remove member (Eliminar miembros en masa). Descargue la plantilla CSV para proporcionar la lista de usuarios que se van a quitar.

Edite la plantilla CSV descargada con las entradas de usuario correspondientes. No quite las dos primeras filas de la plantilla. Agregue un UPN de usuario en cada fila.

![Edición del archivo CSV para la eliminación masiva de usuarios de las unidades administrativas](./media/roles-admin-units-add-manage-users/bulk-user-entries.png)

Una vez que haya guardado las entradas en el archivo, cárguelo y seleccione **Enviar**.

![Envío del archivo de carga masiva](./media/roles-admin-units-add-manage-users/bulk-user-remove.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de un rol a una unidad administrativa](roles-admin-units-assign-roles.md)
- [Adición de grupos a una unidad administrativa](roles-admin-units-add-manage-groups.md)
