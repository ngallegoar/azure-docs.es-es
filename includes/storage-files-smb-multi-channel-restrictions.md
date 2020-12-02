---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/16/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e2a1eb8524c9d9a4b0ae603da3c05fbb20900111
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95995506"
---
SMB multicanal para los recursos compartido de archivos de Azure tiene actualmente las siguientes restricciones:
- Solo se puede usar con cuentas de FileStorage con redundancia local.
- Solo se admite para clientes de Windows. 
- El número máximo de caracteres es cuatro.
- No se admite SMB directo.
- En el caso de las cuentas de Storage con [autenticación basada en identidad](../articles/storage/files/storage-files-active-directory-overview.md) Active Directory Domain Services (AD DS) local o Azure AD DS habilitada para Azure Files, los clientes SMB no podrían usar el Explorador de archivos de Windows para configurar permisos NTFS en directorios y archivos.
    - Use la herramienta [icacls](/windows-server/administration/windows-commands/icacls) de Windows o el comando [Set-ACL](/powershell/module/microsoft.powershell.security/set-acl?view=powershell-7) para configurar los permisos.

