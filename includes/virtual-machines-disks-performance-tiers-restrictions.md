---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/20/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 241a2be834d7828cdb56f59313c0c1a9c478ff77
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96016542"
---
- Esta característica es compatible actualmente solo con SSD Premium.
- Debe desasignar la máquina virtual o desconectar el disco de una máquina virtual en ejecución antes de que pueda cambiar el nivel del disco.
- Los niveles de rendimiento P60, P70 y P80 solo pueden usarlos aquellos discos cuyo tamaño supere los 4096 GiB.
- El nivel de rendimiento de un disco solo se puede cambiar a un nivel inferior una vez cada 12 horas.