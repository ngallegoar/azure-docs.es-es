---
title: Administración de claves de acceso de cuentas
titleSuffix: Azure Storage
description: Obtenga información sobre cómo ver, administrar y rotar las claves de acceso de la cuenta de almacenamiento.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/31/2020
ms.author: tamram
ms.openlocfilehash: b4e91aa59168deb18375bf86ae77f655ca3dab47
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521265"
---
# <a name="manage-storage-account-access-keys"></a>Administración de las claves de acceso de la cuenta de almacenamiento

Cuando se crea una cuenta de almacenamiento, Azure genera dos claves de acceso de cuenta de almacenamiento de 512 bits. Estas claves se pueden usar para autorizar el acceso a los datos de su cuenta de almacenamiento mediante la autorización con clave compartida.

Microsoft recomienda usar Azure Key Vault para administrar las claves de acceso, así como rotar y volver a generar las claves de forma periódica. El uso de Azure Key Vault facilita la rotación de las claves sin ocasionar interrupciones en las aplicaciones. También puede rotar las claves de forma manual.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="view-access-keys-and-connection-string"></a>Visualización de claves de acceso y cadena de conexión

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

## <a name="use-azure-key-vault-to-manage-your-access-keys"></a>Uso de Azure Key Vault para administrar las claves de acceso

Microsoft recomienda usar Azure Key Vault para administrar y rotar las claves de acceso. La aplicación puede acceder de forma segura a las claves en Key Vault, de modo que puede evitar almacenarlas en el código de la aplicación. Para obtener más información sobre el uso de Key Vault en la administración de claves, consulte los siguientes artículos:

- [Administración de claves de cuenta de almacenamiento con Azure Key Vault y Azure PowerShell](../../key-vault/key-vault-overview-storage-keys-powershell.md)
- [Administración de claves de cuenta de almacenamiento con Azure Key Vault y la CLI de Azure](../../key-vault/key-vault-ovw-storage-keys.md)

## <a name="manually-rotate-access-keys"></a>Rotación manual de las claves de acceso

Microsoft recomienda rotar las claves de acceso periódicamente para ayudar a proteger la cuenta de almacenamiento. De ser posible, use Azure Key Vault para administrar las claves de acceso. Si no está usando Key Vault, deberá rotar las claves manualmente.

Para poder rotar las claves, se asignan dos claves de acceso. De este modo, se garantiza que la aplicación mantiene el acceso a Azure Storage a lo largo del proceso.

> [!WARNING]
> La regeneración de las claves de acceso puede afectar a aplicaciones o servicios de Azure que dependen de la clave de cuenta de almacenamiento. Los clientes que usan la clave de cuenta para acceder a la cuenta de almacenamiento se deben actualizar para usar la nueva clave, lo que incluye los servicios multimedia, las aplicaciones de nube, escritorio y móviles y las aplicaciones de interfaz gráfica de usuario de Azure Storage, como el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

Para rotar las claves de la cuenta de almacenamiento, siga este proceso:

1. Actualice las cadenas de conexión en el código de aplicación para usar la clave secundaria.
2. Vuelva a generar la clave de acceso primaria para su cuenta de almacenamiento. En la hoja **Claves de acceso** de Azure Portal, haga clic en **Regenerar clave1** y luego haga clic en **Sí** para confirmar que quiere generar una nueva clave.
3. Actualice las cadenas de conexión en su código para hacer referencia a la nueva clave de acceso primaria.
4. Vuelva a generar la clave de acceso secundaria de la misma forma.

> [!NOTE]
> Microsoft recomienda usar solo una de las claves en todas las aplicaciones al mismo tiempo. Si utiliza la Clave 1 en algunos lugares y la Clave 2 en otros, no podrá rotar las claves sin que alguna aplicación pierda el acceso.

Para rotar las claves de acceso de una cuenta, el usuario debe ser un administrador de servicios o debe tener asignado un rol de RBAC que incluya **Microsoft.Storage/storageAccounts/regeneratekey/Action**. Algunos de los roles de RBAC integrados que incluyen esta acción son **Propietario**, **Colaborador** y **Rol de servicio del operador de claves de cuentas de almacenamiento**. Para obtener más información sobre el rol de administrador de servicios, consulte [Roles de administrador de suscripciones clásico de RBAC de Azure y de administrador de Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md). Para obtener información detallada sobre los roles integrados de RBAC para Azure Storage, consulte la sección **Almacenamiento** en [Roles integrados de Azure para RBAC de Azure](../../role-based-access-control/built-in-roles.md#storage).

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a las cuentas de Azure Storage](storage-account-overview.md)
- [Cree una cuenta de almacenamiento](storage-account-create.md)
