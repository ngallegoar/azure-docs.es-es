---
title: Habilitación de los discos compartidos para Azure Managed Disks
description: Configuración de un disco administrado de Azure con discos compartidos, con el fin de poder compartirlo entre varias máquinas virtuales
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 07/16/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: e36c539cc1143490aeb4862c928589db5c502656
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87080237"
---
# <a name="enable-shared-disk"></a>Habilitación del disco compartido

En este artículo se describe cómo habilitar la característica de discos compartidos en Azure Managed Disks. Los discos compartidos de Azure son una nueva característica de Azure Managed Disks que permite conectar un disco administrado a varias máquinas virtuales (VM) al mismo tiempo. Si adjunta un disco administrado en varias VM, podrá implementar nuevas aplicaciones en clúster o migrar las existentes a Azure. 

Si busca información conceptual sobre los discos administrados que tienen habilitados discos compartidos, consulte [Discos compartidos de Azure](disks-shared.md).
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]