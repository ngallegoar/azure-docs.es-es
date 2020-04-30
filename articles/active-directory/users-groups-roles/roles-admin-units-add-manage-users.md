---
title: 'Adición, eliminación y enumeración de usuarios en una unidad administrativa (versión preliminar): Azure Active Directory | Microsoft Docs'
description: Administración de usuarios y sus permisos de rol en una unidad administrativa en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c2c5c083115440e1e4da203f39f2b32734458c3
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684971"
---
# <a name="add-and-manage-users-in-an-administrative-unit-in-azure-active-directory"></a>Adición y administración de usuarios en las unidades administrativas en Azure Active Directory

En Azure Active Directory (Azure AD), puede agregar usuarios a una unidad administrativa para un ámbito administrativo de control más específico.

Consulte [Introducción](roles-admin-units-manage.md#get-started) para conocer los pasos previos al uso de PowerShell y Microsoft Graph para la administración de unidades administrativas.

## <a name="add-users-to-an-au"></a>Adición de usuarios a una unidad administrativa

### <a name="azure-portal"></a>Portal de Azure

Hay dos maneras en las que puede asignar usuarios a las unidades administrativas.

1. Asignación individual

    1. Puede ir a Azure AD en el portal y seleccionar Usuarios; después, seleccione el usuario que se va a asignar a una unidad administrativa. A continuación, puede seleccionar Unidades administrativas en el panel izquierdo. El usuario se puede asignar a una o varias unidades administrativas al hacer clic en Asignar a la unidad administrativa y seleccionar las unidades administrativas en las que se va a asignar el usuario.

       ![seleccione Agregar y, a continuación, escriba un nombre para la unidad administrativa](./media/roles-admin-units-add-manage-users/assign-users-individually.png)

    1. Puede ir a Azure AD en el portal y seleccionar Unidades administrativas en el panel izquierdo; a continuación, seleccione la unidad administrativa a la que se asignarán los usuarios. Seleccione Todos los usuarios en el panel izquierdo y, a continuación, seleccione Agregar miembro. Después, puede continuar y seleccionar uno o varios usuarios para asignarlos a la unidad administrativa en el panel derecho.

        ![seleccione una unidad administrativa y, a continuación, seleccione Agregar miembro](./media/roles-admin-units-add-manage-users/assign-to-admin-unit.png)

1. Asignación masiva

    Vaya a Azure AD en el portal y seleccione Unidades administrativas. Seleccione la unidad administrativa a la que se van a agregar los usuarios. Para continuar, haga clic en Todos los usuarios > Agregar miembros desde archivo .csv. Después, puede descargar la plantilla CSV y editar el archivo. El formato es sencillo y necesita agregar un UPN único en cada línea. Una vez que el archivo esté listo, guárdelo en una ubicación adecuada y, a continuación, cárguelo en el paso 3 tal como se resalta en la instantánea.

    ![asignación masiva de usuarios a una unidad administrativa](./media/roles-admin-units-add-manage-users/bulk-assign-to-admin-unit.png)

### <a name="powershell"></a>PowerShell

    $administrativeunitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    $UserObj = Get-AzureADUser -Filter "UserPrincipalName eq 'billjohn@fabidentity.onmicrosoft.com'"
    Add-AzureADAdministrativeUnitMember -ObjectId $administrativeunitObj.ObjectId -RefObjectId $UserObj.ObjectId

En el ejemplo anterior, el cmdlet Add-AzureADAdministrativeUnitMember se usa para agregar el usuario a la unidad administrativa. Se usan como argumentos tanto el id. de objeto de la unidad administrativa como el id. de objeto del usuario que se va a agregar. La sección resaltada se puede cambiar según sea necesario para el entorno específico.

### <a name="microsoft-graph"></a>Microsoft Graph

    Http request
    POST /administrativeUnits/{Admin Unit id}/members/$ref
    Request body
    {
      "@odata.id":"https://graph.microsoft.com/beta/users/{id}"
    }

Ejemplo:

    {
      "@odata.id":"https://graph.microsoft.com/beta/users/johndoe@fabidentity.com"
    }

## <a name="list-administrative-units-for-a-user"></a>Enumeración de las unidades administrativas de un usuario

### <a name="azure-portal"></a>Portal de Azure

En Azure Portal puede abrir el perfil de un usuario si va a Azure AD > Usuarios. Haga clic en el usuario para abrir su perfil.

![Apertura de un perfil de usuario en Azure Active Directory](./media/roles-admin-units-add-manage-users/user-profile-admin-units.png)

Seleccione **Unidades administrativas** en el panel izquierdo para ver la lista de unidades administrativas en las que el usuario se ha asignado.

![Enumeración de las unidades administrativas de un usuario](./media/roles-admin-units-add-manage-users/list-user-admin-units.png)

### <a name="powershell"></a>PowerShell

    Get-AzureADAdministrativeUnit | where { Get-AzureADAdministrativeUnitMember -ObjectId $_.ObjectId | where {$_.ObjectId -eq $userObjId} }

### <a name="microsoft-graph"></a>Microsoft Graph

    https://graph.microsoft.com/beta/users//memberOf/$/Microsoft.Graph.AdministrativeUnit

## <a name="remove-a-single-user-from-an-au"></a>Eliminación un solo usuario de una unidad administrativa

### <a name="azure-portal"></a>Portal de Azure

Hay dos maneras de quitar un usuario de una unidad administrativa. En Azure Portal puede abrir el perfil de un usuario si va a **Azure AD** > **Usuarios**. Seleccione el usuario para abrir su perfil. Seleccione la unidad administrativa de la que quiere que se quite el usuario y seleccione **Quitar de la unidad administrativa**.

![Eliminación de un usuario de una unidad administrativa desde el perfil de usuario](./media/roles-admin-units-add-manage-users/user-remove-admin-units.png)

También puede quitar un usuario en **Azure AD** > **Unidades administrativas** al seleccionar la unidad administrativa de la que quiere que se quiten los usuarios. Seleccione el usuario y seleccione **Quitar miembro**.
  
![Eliminación de un usuario en el nivel de unidad administrativa](./media/roles-admin-units-add-manage-users/admin-units-remove-user.png)

### <a name="powershell"></a>PowerShell

    Remove-AzureADAdministrativeUnitMember -ObjectId $auId -MemberId $memberUserObjId

### <a name="microsoft-graph"></a>Microsoft Graph

   https://graph.microsoft.com/beta/administrativeUnits/<adminunit-id>/members/<user-id>/ $ref

## <a name="bulk-remove-more-than-one-user"></a>Eliminación masiva de más de un usuario

Puede ir a Azure AD > Unidades administrativas y seleccione la unidad administrativa de la que quiere que se quiten los usuarios. Haga clic en Bulk remove member (Eliminar miembros en masa). Descargue la plantilla CSV para proporcionar la lista de usuarios que se van a quitar.

Edite la plantilla CSV descargada con las entradas de usuario correspondientes. No quite las dos primeras filas de la plantilla. Agregue un UPN de usuario en cada fila.

![Edición del archivo CSV para la eliminación masiva de usuarios de las unidades administrativas](./media/roles-admin-units-add-manage-users/bulk-user-entries.png)

Una vez que haya guardado las entradas en el archivo, cárguelo y seleccione **Enviar**.

![Envío del archivo de carga masiva](./media/roles-admin-units-add-manage-users/bulk-user-remove.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de un rol a una unidad administrativa](roles-admin-units-assign-roles.md)
- [Adición de grupos a una unidad administrativa](roles-admin-units-add-manage-groups.md)
