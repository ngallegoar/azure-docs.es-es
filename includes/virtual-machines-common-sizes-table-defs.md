---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 63c53a9b95e27486d7d6944c28f8fb085b1bc6ca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74279206"
---
<!-- Not used for Ls-series -->

## <a name="size-table-definitions"></a>Definiciones de tabla de tamaño

- La capacidad de almacenamiento se muestra en unidades de GiB o 1024^3 bytes. Cuando compare discos que se miden en GB (1000^3 bytes) con discos que se miden en GiB (1024^3), recuerde que los números que representan la capacidad en GiB pueden parecer más pequeños. Por ejemplo, 1023 GiB = 1098,4 GB.
- Se midió el rendimiento de disco en operaciones de entrada/salida por segundo (E/S por segundo) y MBps, donde Mbps = 10^6 bytes/s.
- Los discos de datos pueden funcionar en modo en caché o en modo no en caché. En el caso de la operación de disco de datos en caché, el modo de caché del host está establecido en **ReadOnly** o **ReadWrite**.  En el caso de la operación de disco de datos no en caché, el modo de caché del host está definido en **None**.
- Si desea obtener el mejor rendimiento para las VM, debe limitar el número de discos de datos a dos discos por vCPU.
- El **ancho de banda de red esperado** es el ancho de banda agregado máximo asignado por tipo de máquina virtual en todas las NIC y para todos los destinos. Para más información, consulte [Ancho de banda de red de las máquinas virtuales](../articles/virtual-network/virtual-machine-network-throughput.md).

  No se garantizan los límites superiores. Los límites ofrecen una guía para seleccionar el tipo de máquina virtual adecuado para la aplicación prevista. El rendimiento de red real dependerá de diversos factores (como, por ejemplo, la congestión de la red, las cargas de la aplicación y la configuración de red). Para más información sobre cómo optimizar el rendimiento de red, consulte [Optimización del rendimiento de red en las máquinas virtuales de Azure](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Para lograr el rendimiento de red esperado en Linux o Windows, puede que deba seleccionar una versión específica u optimizar la máquina virtual. Para obtener más información, vea [Pruebas de ancho de banda y rendimiento (NTTTCP)](../articles/virtual-network/virtual-network-bandwidth-testing.md).



