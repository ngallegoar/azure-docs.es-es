---
title: 'Informática de alto rendimiento en máquinas virtuales de la serie H: Azure Virtual Machines'
description: Obtenga información sobre las características y funcionalidades de las máquinas virtuales de serie H optimizadas para HPC.
author: vermagit
ms.author: amverma
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 07/02/2019
ms.openlocfilehash: b3d5d003db89a11a013c3236a3afbe03ffe68557
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76271015"
---
# <a name="high-performance-computing-on-h-series-vms"></a>Informática de alto rendimiento en máquinas virtuales de serie H

La informática de alto rendimiento (HPC) en máquinas virtuales de serie HC y HB permite el rendimiento de HPC más optimizado de todas las máquinas virtuales de Azure. Las máquinas virtuales optimizadas para HPC se utilizan para resolver algunos de los problemas matemáticos más difíciles, tales como dinámica de fluidos, simulaciones de petróleo y gas, y modelos meteorológicos.

En este artículo se describen algunas de las principales características de las máquinas virtuales de las series HB y HC, por qué estas máquinas virtuales funcionan bien en escenarios de HPC y cómo empezar a trabajar.

## <a name="features-and-capabilities"></a>Características y funcionalidades

Las máquinas virtuales de las series HB y HC están diseñadas para proporcionar el mejor rendimiento de HPC, escalabilidad de la interfaz de paso de mensajes (MPI) y rentabilidad para cargas de trabajo de HPC.

### <a name="message-passing-interface"></a>Interfaz de paso de mensajes

Las series HB y HC admiten casi todos los tipos y versiones de MPI. Algunos de los tipos de MPI más comunes que se admiten son: OpenMPI, MVAPICH2, Platform MPI, Intel MPI y todos los verbos de acceso directo a memoria remota (RDMA). Para más información, consulte [Configuración de la interfaz de paso de mensajes para HPC](setup-mpi.md).

### <a name="rdma-and-infiniband"></a>RDMA e InfiniBand

La interfaz RDMA es estándar en las máquinas virtuales de las series HC y HB. Las instancias compatibles con RDMA se comunican a través de una red InfiniBand y funcionan a velocidades de datos mejoradas (EDR) para las máquinas virtuales de las series HB y HC. Las instancias compatibles con RDMA pueden mejorar la escalabilidad y el rendimiento de algunas aplicaciones de MPI.

La configuración InfiniBand que admite las máquinas virtuales de las series HB y HC está compuesta por árboles voluminosos que no bloquean con un diseño de poco diámetro para un rendimiento de RDMA coherente.

Consulte [Habilitación de InfiniBand](enable-infiniband.md) para más información sobre la configuración de InfiniBand en las máquinas virtuales de las series HB o HC.

## <a name="get-started"></a>Introducción

En primer lugar, decida qué máquina virtual de serie H va a usar. Para más información sobre las máquinas virtuales optimizadas para HPC, consulte [Introducción a la serie HB](hb-series-overview.md) e [Introducción a la serie HC](hc-series-overview.md). Para conocer las especificaciones, consulte [Tamaños de máquina virtual de informática de alto rendimiento](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc).

Cuando haya seleccionado y creado una máquina virtual para la aplicación, deberá configurarla mediante la activación de InfiniBand. Para más información sobre cómo habilitar InfiniBand tanto Windows como en máquinas virtuales Linux, consulte [Habilitación de InfiniBand](enable-infiniband.md).

Un componente esencial de las cargas de trabajo de HPC es MPI. Las series HB y HC admiten casi todos los tipos y versiones de MPI. Para más información, consulte [Configuración de la interfaz de paso de mensajes para HPC](setup-mpi.md).

Cuando haya elegido las series de las máquinas virtuales y haya configurado Infiniband y MPI, estará preparado para comenzar a crear las cargas de trabajo de HPC.

## <a name="next-steps"></a>Pasos siguientes

- Revise la [introducción a la serie HB](hb-series-overview.md) y la [introducción a la serie HC](hc-series-overview.md) para obtener información sobre las principales diferencias y las especificaciones.

- Para una vista de nivel superior de la arquitectura de la ejecución de cargas de trabajo de HPC, consulte [Informática de alto rendimiento (HPC) en Azure](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/).
