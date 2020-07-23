---
title: Seguridad de disco administrado
description: Aprenda sobre la seguridad de disco para discos de Azure y para máquinas virtuales de Azure
author: albecker1
ms.author: albecker
ms.date: 06/02/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 4408b7d442ce3f4fbb1ea61031295375ae036493
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86494554"
---
# <a name="managed-disk-bursting"></a>Seguridad de disco administrado
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Seguridad en el nivel de máquina virtual
La compatibilidad con la seguridad de nivel de máquina virtual está habilitada en todas las regiones de la nube pública en estos tamaños admitidos: 
- [Serie Lsv2](../lsv2-series.md)

La seguridad está habilitada de forma predeterminada para las máquinas virtuales que la admiten.

## <a name="disk-level-bursting"></a>Seguridad en el nivel de disco
La seguridad también está disponible en las unidades [SSD Premium](disks-types.md#premium-ssd) para tamaños de disco P20 e inferiores en todas las regiones de la nube pública de Azure, la nube de administración pública y la nube de China. La ampliación del disco está habilitada de forma predeterminada en las implementaciones nuevas y actuales de los tamaños del disco que la admiten. 

[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]
