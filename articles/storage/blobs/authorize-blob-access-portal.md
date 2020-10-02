---
title: Elección de la forma de autorizar el acceso a los datos de blob en Azure Portal
titleSuffix: Azure Storage
description: Cuando se accede a los datos de blob desde Azure Portal, este realiza ciertas solicitudes a Azure Storage en segundo plano. Estas solicitudes a Azure Storage se pueden autenticar y autorizar utilizando bien la cuenta de Azure AD, bien la clave de acceso a la cuenta de almacenamiento.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/08/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: blobs
ms.custom: contperfq1
ms.openlocfilehash: 4a9f243ce32dee9948cd2f2410b5c7f1382bf7c8
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90088857"
---
# <a name="choose-how-to-authorize-access-to-blob-data-in-the-azure-portal"></a>Elección de la forma de autorizar el acceso a los datos de blob en Azure Portal

Cuando se accede a los datos de blob desde [Azure Portal](https://portal.azure.com), este realiza ciertas solicitudes a Azure Storage en segundo plano. Una solicitud a Azure Storage se puede autorizar mediante la cuenta de Azure AD o la clave de acceso a la cuenta de almacenamiento. El portal indica qué método está usando, y le permite alternar entre ambos si tiene los permisos adecuados.  

También puede especificar cómo autorizar una operación de carga de blobs individual en Azure Portal. De forma predeterminada, el portal usa el método que ya está utilizando para autorizar una operación de carga de blobs, pero tiene la opción de cambiar esta configuración al cargar un blob.

## <a name="permissions-needed-to-access-blob-data"></a>Permisos necesarios para acceder a datos de blob

Necesitará permisos específicos según cómo quiera autorizar el acceso a los datos de blob en Azure Portal. En la mayoría de los casos, estos permisos se proporcionan a través del control de acceso basado en roles (RBAC). Para más información acerca de RBAC, consulte [¿Qué es el control de acceso basado en rol (RBAC) de Azure?](../../role-based-access-control/overview.md)

### <a name="use-the-account-access-key"></a>Uso de la clave de acceso de la cuenta

Para acceder a los datos de blob con la clave de acceso a la cuenta, debe tener asignado un rol de Azure que incluya la acción de RBAC **Microsoft.Storage/storageAccounts/listkeys/action**. Este rol de Azure puede ser un rol integrado o personalizado. Los roles integrados que admiten **Microsoft.Storage/storageAccounts/listkeys/action** son estos:

- El rol [Propietario](../../role-based-access-control/built-in-roles.md#owner) de Azure Resource Manager
- El rol [Colaborador](../../role-based-access-control/built-in-roles.md#contributor) de Azure Resource Manager
- El rol [Colaborador de la cuenta de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

Al intentar acceder a los datos de blob en Azure Portal, este comprueba primero si tiene asignado un rol con **Microsoft.Storage/storageAccounts/listkeys/action**. Si se le ha asignado un rol con esta acción, Azure Portal usa la clave de cuenta para tener acceso a los datos de blob. Si no tiene un rol asignado con esta acción, Portal intenta obtener acceso a los datos mediante su cuenta de Azure AD.

> [!NOTE]
> Los roles clásicos de administrador de suscripciones Administrador del servicio y Coadministrador equivalen al rol [Propietario](../../role-based-access-control/built-in-roles.md#owner) de Azure Resource Manager. El rol **Propietario** engloba todas las acciones (incluida **Microsoft.Storage/storageAccounts/listkeys/action**), por lo que un usuario con uno de estos roles administrativos también puede acceder a datos de blob con la clave de cuenta. Para obtener más información, consulte [Roles de administrador de suscripciones clásico, de Azure y de administrador de Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="use-your-azure-ad-account"></a>Uso de la cuenta de Azure AD

Para acceder a datos de blob desde Azure Portal con la cuenta de Azure AD, se deben cumplir estas dos premisas:

- Tiene asignado como mínimo el rol [Lector](../../role-based-access-control/built-in-roles.md#reader) de Azure Resource Manager, con el ámbito establecido en el nivel de la cuenta de almacenamiento o en un nivel superior. El rol **Lector** concede los permisos más restringidos, pero otro rol de Azure Resource Manager que conceda acceso a los recursos de administración de la cuenta de almacenamiento también es aceptable.
- Tiene asignado un rol (ya sea integrado o personalizado) que proporciona acceso a los datos de blob.

La asignación del rol **Lector** o de otro rol de Azure Resource Manager es necesaria para que el usuario pueda ver los recursos de administración de la cuenta de almacenamiento en Azure Portal y navegar por ellos. Los roles de Azure que conceden acceso a los datos de blob no se lo conceden a los recursos de administración de la cuenta de almacenamiento. Para acceder a datos de blob en Azure Portal, el usuario necesita permisos para navegar por los recursos de la cuenta de almacenamiento. Para obtener más información sobre este requisito, vea [Asignar el rol Lector para acceder a Portal](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

Estos son los roles integrados que admiten el acceso a los datos de blob:

- [Propietario de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): para el control de acceso POSIX de Azure Data Lake Storage Gen2.
- [Colaborador de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): permisos de lectura, escritura y eliminación de blobs.
- [Lector de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): permisos de solo de lectura de blobs.

Los roles personalizados pueden admitir diferentes combinaciones de los mismos permisos que proporcionan los roles integrados. Para obtener más información sobre cómo crear roles RBAC de Azure personalizados, consulte el artículo sobre [roles personalizados de Azure](../../role-based-access-control/custom-roles.md) y la [descripción de las definiciones de roles de recursos de Azure](../../role-based-access-control/role-definitions.md).

> [!IMPORTANT]
> La versión preliminar de Explorador de Storage en Azure Portal no admite el uso de credenciales de Azure AD para ver y modificar datos de blob. Explorador de Storage en Azure Portal usa siempre las claves de cuenta para acceder a los datos. Para usar Explorador de Storage en Azure Portal, debe tener asignado un rol que incluya **Microsoft.Storage/storageAccounts/listkeys/action**.

## <a name="navigate-to-blobs-in-the-azure-portal"></a>Desplazamiento a blobs en Azure Portal

Para ver datos de blob en Azure Portal, vaya a la sección **Información general** de la cuenta de almacenamiento y haga clic en los vínculos **Blobs**. También puede ir a la sección**Blob service** del menú.

:::image type="content" source="media/anonymous-read-access-configure/blob-public-access-portal.png" alt-text="Captura de pantalla que muestra cómo ir a los datos de blob en Azure Portal":::

## <a name="determine-the-current-authentication-method"></a>Determinar el método de autenticación actual

Al ir a un contenedor, Azure Portal indica si lo que se usa actualmente para autenticarse es la clave de acceso a la cuenta o la cuenta de Azure AD.

### <a name="authenticate-with-the-account-access-key"></a>Autenticación con la clave de acceso de la cuenta

Si se autentica mediante la clave de acceso a la cuenta, verá **Clave de acceso** especificado como método de autenticación en Portal:

:::image type="content" source="media/authorize-blob-access-portal/auth-method-access-key.png" alt-text="Captura de pantalla que muestra cómo ir a los datos de blob en Azure Portal":::

Para cambiar y usar la cuenta de Azure AD, haga clic en el vínculo que aparece resaltado en la imagen. Si posee los permisos adecuados a través de los roles de Azure que tiene asignados, podrá continuar. Pero, si no los tiene, verá un mensaje de error como el siguiente:

:::image type="content" source="media/authorize-blob-access-portal/auth-error-azure-ad.png" alt-text="Captura de pantalla que muestra cómo ir a los datos de blob en Azure Portal":::

Cabe mencionar que la lista no contendrá ningún blob si su cuenta de Azure AD no tiene permisos para verlos. Haga clic en el vínculo **Cambiar a la clave de acceso** para usar la clave de acceso para intentar autenticarse de nuevo.

### <a name="authenticate-with-your-azure-ad-account"></a>Autenticación con la cuenta de Azure AD

Si se autentica utilizando la cuenta de Azure AD, verá **Cuenta de usuario de Azure AD** especificado como método de autenticación en Portal:

:::image type="content" source="media/authorize-blob-access-portal/auth-method-azure-ad.png" alt-text="Captura de pantalla que muestra cómo ir a los datos de blob en Azure Portal":::

Para cambiar y usar la clave de acceso a la cuenta, haga clic en el vínculo que aparece resaltado en la imagen. Si tiene acceso a la clave de cuenta, podrá continuar. Pero, si no puede acceder a ella, verá un mensaje de error como el siguiente:

:::image type="content" source="media/authorize-blob-access-portal/auth-error-access-key.png" alt-text="Captura de pantalla que muestra cómo ir a los datos de blob en Azure Portal":::

Cabe mencionar que la lista no contendrá ningún blob si carece de acceso a las claves de cuenta. Haga clic en el vínculo **Cambiar a la cuenta de usuario de Azure AD** para usar la cuenta de Azure AD para intentar autenticarse de nuevo.

## <a name="specify-how-to-authorize-a-blob-upload-operation"></a>Especificación de cómo autorizar una operación de carga de blobs

Al cargar un blob desde Azure Portal, puede especificar si desea autenticar y autorizar esa operación con la clave de acceso a la cuenta o con sus credenciales de Azure AD. De forma predeterminada, el portal usa el método de autenticación actual, como se muestra en [Determinar el método de autenticación actual](#determine-the-current-authentication-method).

Para especificar cómo autorizar una operación de carga de blobs, siga estos pasos:

1. En Azure Portal, vaya al contenedor en el que desea cargar un blob.
1. Seleccione el botón **Cargar**.
1. Expanda la sección **Avanzado** para mostrar las propiedades avanzadas del blob.
1. En el campo **Tipo de autenticación**, indique si desea autorizar la operación de carga mediante su cuenta de Azure AD o con la clave de acceso a la cuenta, tal como se muestra en la siguiente imagen:

    :::image type="content" source="media/authorize-blob-access-portal/auth-blob-upload.png" alt-text="Captura de pantalla que muestra cómo ir a los datos de blob en Azure Portal":::

## <a name="next-steps"></a>Pasos siguientes

- [Autenticar el acceso a blobs y colas de Azure con Azure Active Directory](../common/storage-auth-aad.md)
- [Conceder acceso a contenedores y colas de Azure con RBAC en Azure Portal](../common/storage-auth-aad-rbac-portal.md)
- [Conceder acceso a datos de blob y de cola de Azure con RBAC mediante la CLI de Azure](../common/storage-auth-aad-rbac-cli.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC mediante PowerShell](../common/storage-auth-aad-rbac-powershell.md)
