---
title: Introducción al almacenamiento en disco
description: Introducción a los discos administrados de Azure, que controlan las cuentas de almacenamiento automáticamente cuando se usan máquinas virtuales.
author: roygara
ms.service: virtual-machines
ms.topic: overview
ms.date: 04/24/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: c66324841989580712d893ada9121ae40f5161a3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82147762"
---
# <a name="introduction-to-azure-managed-disks"></a>Introducción a los discos administrados de Azure

Los discos administrados de Azure son volúmenes de almacenamiento de nivel de bloque que administra Azure y que se usan con Azure Virtual Machines. Los discos administrados se pueden considerar como un disco físico en un servidor local, pero virtualizado. Con los discos administrados, lo único que tiene que hacer es especificar el tamaño y el tipo del disco y aprovisionarlo. Cuando aprovisione el disco, Azure controla el resto.

Los tipos de discos disponibles son discos ultra, unidades de estado sólido (SSD) prémium, unidades SSD estándar y unidades de disco duro estándar (HDD). Para más información acerca de cada tipo de disco individual, consulte [Selección de un tipo de disco para máquinas virtuales IaaS](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Selección de un tipo de disco para máquinas virtuales de IaaS](disks-types.md)