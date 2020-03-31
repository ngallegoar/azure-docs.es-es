---
title: Usar Azure Portal para tener acceso a datos de blob o de cola
titleSuffix: Azure Storage
description: Cuando se accede a datos de blob o de cola mediante Azure Portal, Portal realiza solicitudes a Azure Storage en segundo plano. Estas solicitudes a Azure Storage se pueden autenticar y autorizar utilizando bien la cuenta de Azure AD, bien la clave de acceso a la cuenta de almacenamiento.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 01/10/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 602be49ef0c60274f1cd016c4f8e870cf033ec7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75866892"
---
# <a name="use-the-azure-portal-to-access-blob-or-queue-data"></a>Usar Azure Portal para tener acceso a datos de blob o de cola

Cuando se accede a datos de blob o de cola mediante [Azure Portal](https://portal.azure.com), Portal realiza solicitudes a Azure Storage en segundo plano. Una solicitud a Azure Storage se puede autorizar mediante la cuenta de Azure AD o la clave de acceso a la cuenta de almacenamiento. El portal indica qué método está usando, y le permite alternar entre ambos si tiene los permisos adecuados.  

## <a name="permissions-needed-to-access-blob-or-queue-data"></a>Permisos necesarios para acceder a datos de blob o de cola

Necesitará permisos específicos según cómo quiera autorizar el acceso a los datos de blob o de cola en Azure Portal. En la mayoría de los casos, estos permisos se proporcionan a través del control de acceso basado en roles (RBAC). Para obtener más información sobre RBAC, vea [¿Qué es el control de acceso basado en rol (RBAC)?](../../role-based-access-control/overview.md)

### <a name="account-access-key"></a>Clave de acceso a la cuenta

Para acceder a datos de blob y de cola con la clave de acceso a la cuenta, debe tener asignado un rol RBAC que incluya la acción de RBAC **Microsoft.Storage/storageAccounts/listkeys/action**. Este rol RBAC puede ser un rol integrado o personalizado. Los roles integrados que admiten **Microsoft.Storage/storageAccounts/listkeys/action** son estos:

- El rol [Propietario](../../role-based-access-control/built-in-roles.md#owner) de Azure Resource Manager
- El rol [Colaborador](../../role-based-access-control/built-in-roles.md#contributor) de Azure Resource Manager
- El rol [Colaborador de la cuenta de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

Al intentar tener acceso a los datos de blob o de cola en Azure Portal, Portal comprueba primero si tiene asignado un rol con **Microsoft.Storage/storageAccounts/listkeys/action**. Si tiene un rol asignado con esta acción, Portal usa la clave de cuenta para tener acceso a los datos de blob y de cola. Si no tiene un rol asignado con esta acción, Portal intenta obtener acceso a los datos mediante su cuenta de Azure AD.

> [!NOTE]
> Los roles clásicos de administrador de suscripciones Administrador del servicio y Coadministrador equivalen al rol [Propietario](../../role-based-access-control/built-in-roles.md#owner) de Azure Resource Manager. El rol **Propietario** engloba todas las acciones (incluida **Microsoft.Storage/storageAccounts/listkeys/action**), por lo que un usuario con uno de estos roles administrativos también puede tener acceso a datos de blob y de cola con la clave de cuenta. Para obtener más información, vea [Roles de administrador de suscripciones clásicas](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="azure-ad-account"></a>Cuenta de Azure AD

Para tener acceso a datos de blob o de cola desde Azure Portal con la cuenta de Azure AD, se deben cumplir estas dos premisas:

- Tiene asignado como mínimo el rol [Lector](../../role-based-access-control/built-in-roles.md#reader) de Azure Resource Manager, con el ámbito establecido en el nivel de la cuenta de almacenamiento o en un nivel superior. El rol **Lector** concede los permisos más restringidos, pero otro rol de Azure Resource Manager que conceda acceso a los recursos de administración de la cuenta de almacenamiento también es aceptable.
- Tiene asignado un rol (ya sea integrado o personalizado) que proporciona acceso a los datos de blob o de cola.

La asignación del rol **Lector** o de otro rol de Azure Resource Manager es necesaria para que el usuario pueda ver los recursos de administración de la cuenta de almacenamiento en Azure Portal y navegar por ellos. Los roles RBAC que conceden acceso a los datos de blob o de cola no dan acceso a los recursos de administración de la cuenta de almacenamiento. Para obtener acceso a datos de blob o de cola en Portal, el usuario necesita permisos para navegar por los recursos de la cuenta de almacenamiento. Para obtener más información sobre este requisito, vea [Asignar el rol Lector para acceder a Portal](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

Estos son los roles integrados que admiten el acceso a los datos de blob o de cola:

- [Propietario de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): para el control de acceso POSIX de Azure Data Lake Storage Gen2.
- [Colaborador de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): permisos de lectura, escritura y eliminación de blobs.
- [Lector de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): permisos de solo de lectura de blobs.
- [Colaborador de datos de la cola de Storage](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): permisos de lectura, escritura y eliminación de colas.
- [Lector de datos de la cola de Storage](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader): permisos de solo lectura de colas.
    
Los roles personalizados pueden admitir diferentes combinaciones de los mismos permisos que proporcionan los roles integrados. Para obtener más información sobre cómo crear roles RBAC personalizados, vea [Roles personalizados de recursos de Azure](../../role-based-access-control/custom-roles.md) y [Descripción de las definiciones de roles de recursos de Azure](../../role-based-access-control/role-definitions.md).

No se pueden obtener listas de colas con un rol de administrador de suscripciones clásico. Para obtener una lista de colas, el usuario debe tener asignado los roles **Lector**, **Lector de datos de la cola de Storage Blob** o **Colaborador de datos de la cola de Storage Blob** de Azure Resource Manager.

> [!IMPORTANT]
> La versión preliminar de Explorador de Storage en Azure Portal no admite el uso de credenciales de Azure AD para ver y modificar datos de blobs o colas. Explorador de Storage en Azure Portal usa siempre las claves de cuenta para acceder a los datos. Para usar Explorador de Storage en Azure Portal, debe tener asignado un rol que incluya **Microsoft.Storage/storageAccounts/listkeys/action**.

## <a name="navigate-to-blobs-or-queues-in-the-portal"></a>Navegar a blobs o colas en Portal

Para ver datos de blob o de cola en Portal, vaya a la sección **Introducción** de la cuenta de almacenamiento y haga clic en los vínculos **Blobs** o **Colas**. También puede navegar a las secciones **Blob service** y **Queue service** en el menú. 

![Navegar a datos de blob o de cola en Azure Portal](media/storage-access-blobs-queues-portal/blob-queue-access.png)

## <a name="determine-the-current-authentication-method"></a>Determinar el método de autenticación actual

Al navegar a un contenedor o a una cola, Azure Portal indica si lo que se está utilizando actualmente para autenticarse es la clave de acceso a la cuenta o la cuenta de Azure AD.

Los ejemplos de esta sección reflejan el acceso a un contenedor y a sus blobs, pero Portal mostraría el mismo mensaje si se accediera a una cola y a sus mensajes o si se obtuviera una lista de colas.

### <a name="account-access-key"></a>Clave de acceso a la cuenta

Si se autentica mediante la clave de acceso a la cuenta, verá **Clave de acceso** especificado como método de autenticación en Portal:

![Acceso actual a los datos del contenedor con la clave de cuenta](media/storage-access-blobs-queues-portal/auth-method-access-key.png)

Para cambiar y usar la cuenta de Azure AD, haga clic en el vínculo que aparece resaltado en la imagen. Si posee los permisos adecuados a través de los roles RBAC que tiene asignados, podrá continuar. Pero, si no los tiene, verá un mensaje de error como el siguiente:

![Error que aparece si la cuenta de Azure AD no admite el acceso](media/storage-access-blobs-queues-portal/auth-error-azure-ad.png)

Cabe mencionar que la lista no contendrá ningún blob si su cuenta de Azure AD no tiene permisos para verlos. Haga clic en el vínculo **Cambiar a la clave de acceso** para usar la clave de acceso para intentar autenticarse de nuevo.

### <a name="azure-ad-account"></a>Cuenta de Azure AD

Si se autentica utilizando la cuenta de Azure AD, verá **Cuenta de usuario de Azure AD** especificado como método de autenticación en Portal:

![Acceso actual a los datos del contenedor con una cuenta de Azure AD](media/storage-access-blobs-queues-portal/auth-method-azure-ad.png)

Para cambiar y usar la clave de acceso a la cuenta, haga clic en el vínculo que aparece resaltado en la imagen. Si tiene acceso a la clave de cuenta, podrá continuar. Pero, si no puede acceder a ella, verá un mensaje de error como el siguiente:

![Error que aparece si no tiene acceso a la clave de cuenta](media/storage-access-blobs-queues-portal/auth-error-access-key.png)

Cabe mencionar que la lista no contendrá ningún blob si carece de acceso a las claves de cuenta. Haga clic en el vínculo **Cambiar a la cuenta de usuario de Azure AD** para usar la cuenta de Azure AD para intentar autenticarse de nuevo.

## <a name="next-steps"></a>Pasos siguientes

- [Autenticar el acceso a blobs y colas de Azure con Azure Active Directory](storage-auth-aad.md)
- [Conceder acceso a contenedores y colas de Azure con RBAC en Azure Portal](storage-auth-aad-rbac-portal.md)
- [Conceder acceso a datos de blob y de cola de Azure con RBAC mediante la CLI de Azure](storage-auth-aad-rbac-cli.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC mediante PowerShell](storage-auth-aad-rbac-powershell.md)
