---
title: Seguridad de disco administrado
description: Aprenda sobre la seguridad de disco para discos de Azure y para máquinas virtuales de Azure
author: albecker1
ms.author: albecker
ms.date: 04/27/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: baabad550f5e6b0ae39936fc182e6d9789d189d8
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650945"
---
# <a name="disk-bursting"></a>Seguridad de disco
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Seguridad en el nivel de máquina virtual
La compatibilidad con la seguridad de nivel de máquina virtual está habilitada en todas las regiones de la nube pública en estos tamaños admitidos: 
- [Serie Lsv2](../lsv2-series.md)

La seguridad está habilitada de forma predeterminada para las máquinas virtuales que la admiten.

## <a name="disk-level-bursting"></a>Seguridad en el nivel de disco
La seguridad también está disponible en las unidades [SSD Premium](disks-types.md#premium-ssd) para tamaños de disco P20 e inferiores en todas las regiones de la nube pública de Azure, la nube de administración pública y la nube de China. La ampliación del disco está habilitada de forma predeterminada en las nuevas implementaciones de los tamaños del disco que la admiten. Los tamaños del disco existentes, si admiten la ampliación del disco, pueden habilitar la seguridad a través de cualquiera de los métodos siguientes: 
- **Reinicio de la máquina virtual** 
- **Desasociación y reasociación del disco**


[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]
