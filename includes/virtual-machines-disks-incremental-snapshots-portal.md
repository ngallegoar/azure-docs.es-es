---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/02/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: de0cf260207747f4acb02a377819a13de8b8ba22
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96002532"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

## <a name="regional-availability"></a>Disponibilidad regional
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

### <a name="restrictions"></a>Restricciones

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="portal"></a>Portal


1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y navegue hasta el disco del que desea crear una instantánea.
1. En el disco, seleccione **Crear una instantánea**.

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-snapshot-button-incremental.png" alt-text="Captura de pantalla. Hoja del disco con la opción **+ Crear instantánea** resaltada, que es la que debe seleccionar.":::

1. Seleccione el grupo de recursos que desea usar y escriba un nombre.
1. Seleccione **Incremental** y elija **Revisar y crear**.

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/incremental-snapshot-create-snapshot-blade.png" alt-text="Captura de pantalla. Cree una hoja de instantánea, rellene el nombre y seleccione Incremental. A continuación, cree la instantánea.":::

1. Seleccione **Crear**

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-incremental-snapshot-validation.png" alt-text="Captura de pantalla. Página de validación de la instantánea. Confirme sus selecciones y, a continuación, cree la instantánea.":::

## <a name="next-steps"></a>Pasos siguientes

Si quiere ver un código de ejemplo que muestre la funcionalidad diferencial de las instantáneas incrementales con .NET, consulte [Copia de las copias de seguridad de Azure Managed Disks en otra región con una funcionalidad diferencial de instantáneas incrementales](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots).
