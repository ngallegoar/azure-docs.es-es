---
title: Restauración masiva de usuarios (versión preliminar) en el portal de Azure Active Directory | Microsoft Docs
description: Restauración de usuarios en bloque en el Centro de administración de Azure AD de Azure Active Directory
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: d392ae97a8325dd4a56acd807ebfb2b951216eae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72174244"
---
# <a name="bulk-restore-deleted-users-preview-in-azure-active-directory"></a>Restauración masiva de usuarios (versión preliminar) en Azure Active Directory

Azure Active Directory (Azure AD) admite operaciones de creación y eliminación de usuarios en bloque, invitar usuarios en bloque y la descarga de listas de usuarios, grupos y miembros del grupo.

## <a name="to-bulk-restore-users"></a>Para restaurar usuarios en masa

1. [Inicie sesión en la organización de Azure AD](https://aad.portal.azure.com) con una cuenta de administrador de usuarios de esta organización.
1. En Azure AD, seleccione **Usuarios** > **Usuarios eliminados**.
1. En la página **Usuarios eliminados**, seleccione **Restauración masiva** para cargar un archivo .csv válido de las propiedades de los usuarios que se van a restaurar.

   ![Selección del comando de restauración masiva en la página Usuarios eliminados](./media/users-bulk-restore/bulk-restore.png)

1. Abra el archivo CSV y agregue una línea por cada usuario que desee restaurar. El único valor necesario es **ObjectID**. A continuación, guarde el archivo.

   ![Selección de un archivo .csv local en el que se indican los usuarios que quiere agregar](./media/users-bulk-restore/upload-button.png)

1. En la página **Bulk restore (Preview)** (Restauración masiva [versión preliminar]), en **Cargue el archivo csv**, vaya al archivo. Al seleccionar el archivo y hacer clic en **Enviar**, comienza su validación.
1. Cuando finalice la validación del contenido del archivo, aparecerá el mensaje **Archivo cargado correctamente**. Si hay errores, debe corregirlos para poder enviar el trabajo.
1. Cuando el archivo supere la validación, seleccione **Enviar** para iniciar la operación masiva de Azure que restaura los usuarios.
1. Cuando la operación de restauración finalice, verá una notificación que indicará que la operación masiva se realizó correctamente.

Si hay errores, puede descargar y ver el archivo de resultados en la página **Resultados de la operación masiva**. El archivo contiene el motivo de cada error.

## <a name="check-status"></a>Comprobar estado

Puede ver el estado de todas las solicitudes masivas pendientes en la página de **resultados de la operación masiva (versión preliminar)** .

   ![Comprobación del estado de carga en la página de resultados de la operación masiva](./media/users-bulk-restore/bulk-center.png)

A continuación, puede comprobar si los usuarios restaurados existen en la organización de Azure AD en Azure Portal o mediante PowerShell.

## <a name="view-restored-users-in-the-azure-portal"></a>Visualización de los usuarios restaurados en Azure Portal

1. [Inicie sesión en el Centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta que sea la del administrador de usuarios de la organización.
1. En el panel de navegación, seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Usuarios**.
1. En **Mostrar**, seleccione **Todos los usuarios** y compruebe que se incluyen los usuarios restaurados.

### <a name="view-users-with-powershell"></a>Visualización de usuarios con PowerShell

Ejecute el siguiente comando:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Debería ver que se han incluido los usuarios restaurados.

## <a name="next-steps"></a>Pasos siguientes

- [Importación en bloque de usuarios](users-bulk-add.md)
- [Eliminación masiva de usuarios](users-bulk-delete.md)
- [Descarga de la lista de usuarios](users-bulk-download.md)
