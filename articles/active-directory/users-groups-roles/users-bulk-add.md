---
title: Creación masiva de usuarios (versión preliminar) en el portal de Azure Active Directory | Microsoft Docs
description: Adición de usuarios en bloque en el Centro de administración de Azure AD de Azure Active Directory
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/30/2019
ms.topic: article
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: a10dfffa69652ee2b75053c04b97f6492c46811e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72174309"
---
# <a name="bulk-create-users-preview-in-azure-active-directory"></a>Creación masiva de usuarios (versión preliminar) en Azure Active Directory

Azure Active Directory (Azure AD) admite operaciones de creación y eliminación de usuarios en bloque, invitar usuarios en bloque y la descarga de listas de usuarios, grupos y miembros del grupo.

## <a name="required-permissions"></a>Permisos necesarios

Para crear usuarios en bloque en el portal de administración, debe iniciar sesión como administrador global o administrador de usuarios.

## <a name="to-create-users-in-bulk"></a>Para crear usuarios en bloque

1. [Inicie sesión en la organización de Azure AD](https://aad.portal.azure.com) con una cuenta que sea la del administrador de usuarios de la organización.
1. En Azure AD, seleccione **Usuarios** > **Creación masiva**.
1. En la página **Creación masiva de usuarios**, seleccione **Descargar** para recibir un archivo de valores separados por comas (CSV) válido de propiedades de usuario y, a continuación, agregue los usuarios que desea crear.

   ![Selección de un archivo .csv local en el que se indican los usuarios que quiere agregar](./media/users-bulk-add/upload-button.png)

1. Abra el archivo CSV y agregue una línea por cada usuario que desee crear. Los únicos valores necesarios son **Nombre**, **Nombre principal del usuario**, **Contraseña inicial** y **Bloquear inicio de sesión (Sí/No)** . A continuación, guarde el archivo.

   ![El archivo CSV contiene los nombres y los identificadores de los usuarios que se crearán.](./media/users-bulk-add/add-csv-file.png)

1. En la página **Bulk create user (Preview)** (Creación masiva de usuarios [versión preliminar]), en Cargue el archivo csv, vaya al archivo. Al seleccionar el archivo y hacer clic en **Enviar**, comienza su validación.
1. Cuando finalice la validación del contenido del archivo, aparecerá el mensaje **Archivo cargado correctamente**. Si hay errores, debe corregirlos para poder enviar el trabajo.
1. Cuando el archivo supere la validación, seleccione **Enviar** para iniciar la operación masiva de Azure que importa los nuevos usuarios.
1. Cuando la operación de importación finalice, verá una notificación que indicará el estado del trabajo de la operación masiva.

Si hay errores, puede descargar y ver el archivo de resultados en la página **Resultados de la operación masiva**. El archivo contiene el motivo de cada error.

## <a name="check-status"></a>Comprobar estado

Puede ver el estado de todas las solicitudes masivas pendientes en la página de **resultados de la operación masiva (versión preliminar)** .

   ![Comprobación del estado de carga en la página de resultados de la operación masiva](./media/users-bulk-add/bulk-center.png)

A continuación, puede comprobar que los usuarios creados existen en la organización de Azure AD en Azure Portal o mediante PowerShell.

## <a name="verify-users-in-the-azure-portal"></a>Comprobación de usuarios en Azure Portal

1. [Inicie sesión en el Centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta que sea la del administrador de usuarios de la organización.
1. En el panel de navegación, seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Usuarios**.
1. En **Mostrar**, seleccione **Todos los usuarios** y compruebe que se incluyen los usuarios creados.

### <a name="verify-users-with-powershell"></a>Comprobación de usuarios con PowerShell

Ejecute el siguiente comando:

``` PowerShell
Get-AzureADUser -Filter "UserType eq 'Member'"
```

Debería ver que se han incluido los usuarios creados.

## <a name="bulk-import-service-limits"></a>Límites del servicio de importación en bloque

La ejecución de cada actividad en bloque para crear usuarios puede durar hasta una hora. Esto permite la creación en bloque de al menos 50 000 usuarios.

## <a name="next-steps"></a>Pasos siguientes

- [Eliminación masiva de usuarios](users-bulk-delete.md)
- [Descarga de la lista de usuarios](users-bulk-download.md)
- [Restauración de usuarios masiva](users-bulk-restore.md)
