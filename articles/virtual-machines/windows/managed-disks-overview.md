---
title: Introducción a Azure Disk Storage para máquinas virtuales Windows
description: Información general sobre los discos administrados de Azure, que controlan las cuentas de almacenamiento automáticamente cuando se usan máquinas virtuales Windows de Azure
author: roygara
ms.service: virtual-machines-windows
ms.topic: overview
ms.date: 12/02/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 7b2c2c1289a40d63b2f396ee59000c3aedb14c3d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "75460031"
---
# <a name="introduction-to-azure-managed-disks"></a>Introducción a los discos administrados de Azure

Los discos administrados de Azure son volúmenes de almacenamiento de nivel de bloque que administra Azure y que se usan con Azure Virtual Machines. Los discos administrados se pueden considerar como un disco físico en un servidor local, pero virtualizado. Con los discos administrados, lo único que tiene que hacer es especificar el tamaño y el tipo del disco y aprovisionarlo. Cuando aprovisione el disco, Azure controla el resto.

Los tipos de discos disponibles son discos ultra, unidades de estado sólido (SSD) prémium, unidades SSD estándar y unidades de disco duro estándar (HDD). Para más información acerca de cada tipo de disco individual, consulte [Selección de un tipo de disco para máquinas virtuales IaaS](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selección de un tipo de disco para máquinas virtuales de IaaS](disks-types.md)
