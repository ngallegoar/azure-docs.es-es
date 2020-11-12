---
title: 'Incorporación y eliminación de unidades administrativas: Azure Active Directory | Microsoft Docs'
description: Uso de unidades administrativas para restringir el ámbito de los permisos de rol en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: roles
ms.workload: identity
ms.date: 11/04/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: bbe76d21d948525421790f574830da7b3a163216
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93395400"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Administración de unidades administrativas en Azure Active Directory

Para un control administrativo más pormenorizado en Azure Active Directory (Azure AD), puede asignar a los usuarios un rol de Azure AD con un ámbito limitado a una o varias unidades administrativas.

## <a name="get-started"></a>Introducción

1. Para ejecutar consultas a partir de las siguientes instrucciones mediante el [Probador de Graph](https://aka.ms/ge), asegúrese de cumplir con lo siguiente:

    a. En Azure Portal, vaya a Azure AD. 
    
    b. En la lista de aplicaciones, seleccione **Probador de Graph**.
    
    c. En el panel **Permisos** , seleccione **Conceder consentimiento de administrador para Probador de Graph**.

    ![Captura de pantalla en la que se muestra el vínculo "Conceder consentimiento de administrador para Probador de Graph".](./media/admin-units-manage/select-graph-explorer.png)


1. Use la versión preliminar de Azure AD PowerShell.

## <a name="add-an-administrative-unit"></a>Incorporación de una unidad administrativa

Puede agregar una unidad administrativa mediante Azure Portal o PowerShell.

### <a name="use-the-azure-portal"></a>Uso de Azure Portal

1. En Azure Portal, vaya a Azure AD. A continuación, en el panel de la izquierda, seleccione **Unidades administrativas**.

    ![Captura de pantalla en la que se muestra el vínculo "Unidades administrativas" en Azure AD.](./media/admin-units-manage/nav-to-admin-units.png)

1. Seleccione el botón **Agregar** en la parte superior del panel y, a continuación, en el cuadro **Nombre** , escriba el nombre de la unidad administrativa. Si quiere, agregue una descripción de la unidad administrativa.

    ![Captura de pantalla en la que se muestra el botón Agregar y el cuadro Nombre para escribir el nombre de la unidad administrativa.](./media/admin-units-manage/add-new-admin-unit.png)

1. Seleccione el botón azul **Agregar** para finalizar la unidad administrativa.

### <a name="use-powershell"></a>Uso de PowerShell

Instale Azure AD PowerShell (versión preliminar) antes de intentar ejecutar los siguientes comandos:

```powershell
Connect-AzureAD
New-AzureADMSAdministrativeUnit -Description "West Coast region" -DisplayName "West Coast"
```

Puede modificar los valores entre comillas, según sea necesario.

### <a name="use-microsoft-graph"></a>Uso de Microsoft Graph

```http
Http Request
POST /administrativeUnits
Request body
{
  "displayName": "North America Operations",
  "description": "North America Operations administration"
}
```

## <a name="remove-an-administrative-unit"></a>Eliminación de una unidad administrativa

En Azure AD, puede quitar una unidad administrativa que ya no necesite como unidad de ámbito para los roles administrativos.

### <a name="use-the-azure-portal"></a>Uso de Azure Portal

1. En Azure Portal, vaya a **Azure AD** y, luego, seleccione **Unidades administrativas**. 
1. Seleccione la unidad administrativa que quiera eliminar y, a continuación, seleccione **Eliminar**. 
1. Para confirmar que quiere eliminar la unidad administrativa, seleccione **Sí**. Se elimina la unidad administrativa.

![Captura de pantalla del botón Eliminar de la unidad administrativa y de la ventana de confirmación](./media/admin-units-manage/select-admin-unit-to-delete.png)

### <a name="use-powershell"></a>Uso de PowerShell

```powershell
$delau = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'DeleteMe Admin Unit'"
Remove-AzureADMSAdministrativeUnit -ObjectId $delau.ObjectId
```

Puede modificar los valores entre comillas, según sea necesario para el entorno específico.

### <a name="use-the-graph-api"></a>Uso de Graph API

```http
HTTP request
DELETE /administrativeUnits/{Admin id}
Request body
{}
```

## <a name="next-steps"></a>Pasos siguientes

* [Administración de los usuarios en una unidad administrativa](admin-units-add-manage-users.md)
* [Administración de los grupos en una unidad administrativa](admin-units-add-manage-groups.md)
