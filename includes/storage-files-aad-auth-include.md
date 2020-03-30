---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/30/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b248bbb526baf355faf2564358884fd83422b037
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77565099"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) admite la autenticación basada en identidades mediante el Bloque de mensajes del servidor (SMB) con [Azure Active Directory Domain Services (Azure AD DS)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) (versión preliminar) y [Azure AD Domain Services (Azure AD DS)](../articles/active-directory-domain-services/overview.md) (disponibilidad general). En este artículo se describe el modo en que Azure Files puede aprovechar los servicios de dominio, ya sea de forma local o en Azure, para admitir el acceso basado en identidad a Azure Files a través de SMB. Esto le permite reemplazar fácilmente los servidores de archivos existentes por Azure Files y seguir usando el servicio de directorio existente, mientras se mantiene un acceso de usuario sin problemas a los recursos compartidos. 

Azure Files aplica la autorización en el acceso del usuario al recurso compartido y al nivel de directorio o archivo. La asignación de permisos de nivel de recurso compartido se puede asignar a los usuarios o grupos administrados de Azure AD mediante el modelo de [control de acceso basado en rol (RBAC)](../articles/role-based-access-control/overview.md) habitual. Con RBAC, las credenciales que se usan para el acceso a archivos deben estar disponibles o sincronizadas con Azure AD. Puede asignar roles de RBAC integrados como el lector de recursos compartidos de SMB de datos de archivos de almacenamiento a usuarios o grupos en Azure AD para conceder acceso de lectura a un recurso compartido de archivos de Azure.

En el nivel de directorio o archivo, Azure Files admite la conservación, la herencia y la aplicación de [DACL de Windows](https://docs.microsoft.com/windows/win32/secauthz/access-control-lists) igual que cualquier servidor de archivos de Windows. Si copia datos en SMB desde un recurso compartido de archivos a Azure Files, o viceversa, puede optar por mantener las DACL de Windows. Tanto si planea aplicar la autorización como si no, puede aprovechar Azure Files para hacer copias de seguridad de las ACL junto con los datos. 
