---
title: Ámbitos de cifrado para Blob Storage (versión preliminar)
description: Los ámbitos de cifrado ofrecen la posibilidad de administrar el cifrado en el nivel del contenedor o de un blob individual. Se pueden usar ámbitos de cifrado para crear límites seguros entre los datos que residen en la misma cuenta de almacenamiento, pero que pertenecen a clientes distintos.
services: storage
author: tamram
ms.service: storage
ms.date: 09/22/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 881a7fc915ab986577599b85f8412fa8107f7902
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96017396"
---
# <a name="encryption-scopes-for-blob-storage-preview"></a>Ámbitos de cifrado para Blob Storage (versión preliminar)

Los ámbitos de cifrado ofrecen la posibilidad de administrar el cifrado en el nivel del contenedor o de un blob individual. Se pueden usar ámbitos de cifrado para crear límites seguros entre los datos que residen en la misma cuenta de almacenamiento, pero que pertenecen a clientes distintos.

De forma predeterminada, una cuenta de almacenamiento está cifrada con una clave cuyo ámbito es toda la cuenta de almacenamiento. Con un ámbito de cifrado, puede especificar que uno o varios contenedores estén cifrados con una clave cuyo ámbito sea exclusivo para esos contenedores.

Puede optar por usar claves administradas por Microsoft o claves administradas por el cliente almacenadas en Azure Key Vault para proteger y controlar el acceso a la clave que cifra sus datos. Distintos ámbitos de cifrado de una misma cuenta de almacenamiento pueden usar claves administradas por Microsoft o por el cliente.

Después de crear un ámbito de cifrado, puede especificar ese ámbito de cifrado en una solicitud para crear un contenedor o un blob. Para obtener más información acerca de cómo crear un ámbito de cifrado, consulte [Creación y administración de ámbitos de cifrado (versión preliminar)](encryption-scope-manage.md).

> [!NOTE]
> Los ámbitos de cifrado no se admiten con las cuentas de almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) ni con las cuentas de almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS) durante la versión preliminar.

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

> [!IMPORTANT]
> La versión preliminar de los ámbitos de cifrado está pensada para usos distintos del de producción. En este momento no hay contratos de nivel de servicio de producción disponibles.
>
> Para evitar costos inesperados, asegúrese de deshabilitar los ámbitos de cifrado que no necesite actualmente.

## <a name="create-a-container-or-blob-with-an-encryption-scope"></a>Creación de un contenedor o blob con un ámbito de cifrado

Los blobs que se crean en un ámbito de cifrado se cifran con la clave especificada para ese ámbito. Puede especificar un ámbito de cifrado para un blob individual al crear el blob, o bien puede especificar un ámbito de cifrado predeterminado al crear un contenedor. Al especificar un ámbito de cifrado predeterminado en el nivel de un contenedor, todos los blobs de dicho contenedor se cifran con la clave asociada al ámbito predeterminado.

Al crear un blob en un contenedor que tiene un ámbito de cifrado predeterminado, puede especificar un ámbito de cifrado que invalide el ámbito de cifrado predeterminado si el contenedor está configurado para permitir invalidaciones del ámbito de cifrado predeterminado. Para evitar invalidaciones del ámbito de cifrado predeterminado, configure el contenedor para denegar las invalidaciones para un blob individual.

Las operaciones de lectura en un blob que pertenece a un ámbito de cifrado se producen de forma transparente, siempre y cuando el ámbito de cifrado no esté deshabilitado.

## <a name="disable-an-encryption-scope"></a>Deshabilitación de un ámbito de cifrado

Al deshabilitar un ámbito de cifrado, las operaciones de lectura o escritura posteriores realizadas con el ámbito de cifrado producirán un error con el código de error HTTP 403 (prohibido). Si vuelve a habilitar el ámbito de cifrado, las operaciones de lectura y escritura continuarán con normalidad.

Cuando se deshabilita un ámbito de cifrado, ya no se le facturará. Deshabilite los ámbitos de cifrado que no sean necesarios para evitar cargos innecesarios.

Si el ámbito de cifrado está protegido con claves administradas por el cliente, también puede eliminar la clave asociada en el almacén de claves para deshabilitar el ámbito de cifrado. Tenga en cuenta que las claves administradas por el cliente están protegidas por la protección de eliminación y purga temporal en el almacén de claves, y que una clave eliminada está sujeta al comportamiento definido por esas propiedades. Para más información, consulte uno de los siguientes temas en la documentación de Azure Key Vault:

- [Uso de la eliminación temporal con PowerShell](../../key-vault/general/key-vault-recovery.md)
- [Uso de la eliminación temporal con la CLI](../../key-vault/general/key-vault-recovery.md).

> [!NOTE]
> No es posible eliminar un ámbito de cifrado.

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](../common/storage-service-encryption.md)
- [Creación y administración de ámbitos de cifrado (versión preliminar)](encryption-scope-manage.md)
- [Claves administradas por el cliente para el cifrado de Azure Storage](../common/customer-managed-keys-overview.md)
- [¿Qué es Azure Key Vault?](../../key-vault/general/overview.md)