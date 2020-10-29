---
title: 'Incorporación y eliminación de unidades administrativas: Azure Active Directory | Microsoft Docs'
description: Uso de unidades administrativas para restringir el ámbito de los permisos de rol en Azure Active Directory.
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
ms.openlocfilehash: 7b47ea3cc55be26521dfa6e2b3230b477f82f442
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92374132"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Administración de unidades administrativas en Azure Active Directory

Para obtener un control administrativo más específico en Azure Active Directory (Azure AD), puede asignar usuarios a un rol de Azure AD con un ámbito limitado a una o más unidades administrativas.

## <a name="get-started"></a>Introducción

1. Para ejecutar consultas a partir de las siguientes instrucciones mediante el [Probador de Graph](https://aka.ms/ge), asegúrese de cumplir con lo siguiente:

    a. En Azure Portal, vaya a Azure AD. En la lista de aplicaciones, seleccione **Probador de Graph** y **Grant admin consent to Graph Explorer** (Conceder consentimiento de administrador al Probador de Graph).

    ![Captura de pantalla que muestra un vínculo a "Conceder consentimiento del administrador"](./media/admin-units-manage/select-graph-explorer.png)


1. Use la versión preliminar de Azure AD PowerShell.

## <a name="add-an-administrative-unit"></a>Incorporación de una unidad administrativa

### <a name="use-the-azure-portal"></a>Uso de Azure Portal

1. En Azure Portal, vaya a Azure AD y, luego, en el panel izquierdo, seleccione **Unidades administrativas** .

    ![Captura de pantalla del vínculo Unidades administrativas en Azure AD](./media/admin-units-manage/nav-to-admin-units.png)

1. Seleccione **Agregar** y, a continuación, escriba el nombre de la unidad administrativa. Si quiere, agregue una descripción de la unidad administrativa.

    ![Captura de pantalla del botón Agregar y el cuadro de texto para escribir el nombre de la unidad administrativa](./media/admin-units-manage/add-new-admin-unit.png)

1. Seleccione **Agregar** para finalizar la unidad administrativa.

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

1. En Azure Portal, vaya a **Azure AD** > **Unidades administrativas** . 
1. Seleccione la unidad administrativa que quiera eliminar y, a continuación, seleccione **Eliminar** . 
1. Para confirmar que quiere eliminar la unidad administrativa, seleccione **Sí** . Se elimina la unidad administrativa.

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
