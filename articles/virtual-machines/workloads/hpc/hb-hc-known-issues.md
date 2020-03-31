---
title: 'Problemas conocidos con las máquinas virtuales de las series HB y HC: Azure Virtual Machines | Microsoft Docs'
description: Obtenga información sobre problemas conocidos con los tamaños de VM de la serie HB en Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 8d4b57fb2fee3849e102868c86fe3cab465fc70d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67707795"
---
# <a name="known-issues-with-hb-series-and-hc-series-vms"></a>Problemas conocidos de las máquinas virtuales de las series HB y HC

Este artículo proporciona los problemas y soluciones más comunes al usar máquinas virtuales de las series HB y HC.

## <a name="dram-on-hb-series"></a>DRAM de la serie HB

Las máquinas virtuales de serie HB solo pueden exponer 228 GB de RAM para las máquinas virtuales invitadas en este momento. Esto es debido a una limitación conocida del hipervisor de Azure para evitar que las páginas se asignen a la DRAM local de CCX AMD (dominios NUMA) reservada para la máquina virtual invitada.

## <a name="accelerated-networking"></a>Redes aceleradas

Azure Accelerated Networking no está habilitado en este momento, pero lo estará a medida que progresemos en el período de la versión preliminar. Se notificará a los clientes cuando se admita esta característica.

## <a name="qp0-access-restriction"></a>Restricción de acceso qp0

Para evitar el acceso de hardware de bajo nivel que pueda provocar vulnerabilidades de seguridad, el par de cola 0 no es accesible para las VM invitadas. Esto solo debería afectar a las acciones normalmente asociadas con la administración de la NIC ConnectX-5 y a la ejecución de algunos diagnósticos de InfiniBand, como ibdiagnet, pero no a las aplicaciones de usuario final.

## <a name="ud-transport"></a>Transporte UD

En el lanzamiento, las series HB y HC no admiten el transporte conectado dinámicamente (DCT). La compatibilidad con DCT se implementarán con el tiempo. Se admiten los transportes de datagramas no confiables (UD) y de conexión (RC) de confianza.

## <a name="gss-proxy"></a>Proxy GSS

Proxy GSS tiene un problema conocido en CentOS/RHEL 7.5 que se puede producir como una reducción en la capacidad de respuesta y en el rendimiento cuando se usa con NFS. Para mitigarlo, puede llevar a cabo:

```console
sed -i 's/GSS_USE_PROXY="yes"/GSS_USE_PROXY="no"/g' /etc/sysconfig/nfs
```

## <a name="cache-cleaning"></a>Una limpieza de la memoria caché

En los sistemas HPC, a menudo resulta útil limpiar la memoria cuando finaliza un trabajo antes de que al usuario siguiente se le asigne el mismo nodo. Después de ejecutar aplicaciones en Linux, es posible que la memoria disponible se reduzca mientras aumenta la memoria del búfer, a pesar de que no se está ejecutando ninguna aplicación.

![Captura de pantalla de símbolo del sistema](./media/known-issues/cache-cleaning-1.png)

Con `numactl -H` se mostrará con qué NUMAnodes se almacenan en memoria en el búfer (posiblemente, con todos). En Linux, los usuarios pueden limpiar las memorias caché de tres formas para devolver la memoria en búfer o en caché al estado "libre". Debe ser la raíz o tener permisos de sudo.

```console
echo 1 > /proc/sys/vm/drop_caches [frees page-cache]
echo 2 > /proc/sys/vm/drop_caches [frees slab objects e.g. dentries, inodes]
echo 3 > /proc/sys/vm/drop_caches [cleans page-cache and slab objects]
```

![Captura de pantalla de símbolo del sistema](./media/known-issues/cache-cleaning-2.png)

## <a name="kernel-warnings"></a>Advertencias de kernel

Es posible que vea los siguientes mensajes de advertencia de kernel al iniciar una máquina virtual de la serie HB en Linux.

```console
[  0.004000] WARNING: CPU: 4 PID: 0 at arch/x86/kernel/smpboot.c:376 topology_sane.isra.3+0x80/0x90
[  0.004000] sched: CPU #4's llc-sibling CPU #0 is not on the same node! [node: 1 != 0]. Ignoring dependency.
[  0.004000] Modules linked in:
[  0.004000] CPU: 4 PID: 0 Comm: swapper/4 Not tainted 3.10.0-957.el7.x86_64 #1
[  0.004000] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007 05/18/2018
[  0.004000] Call Trace:
[  0.004000] [<ffffffffb8361dc1>] dump_stack+0x19/0x1b
[  0.004000] [<ffffffffb7c97648>] __warn+0xd8/0x100
[  0.004000] [<ffffffffb7c976cf>] warn_slowpath_fmt+0x5f/0x80
[  0.004000] [<ffffffffb7c02b34>] ? calibrate_delay+0x3e4/0x8b0
[  0.004000] [<ffffffffb7c574c0>] topology_sane.isra.3+0x80/0x90
[  0.004000] [<ffffffffb7c57782>] set_cpu_sibling_map+0x172/0x5b0
[  0.004000] [<ffffffffb7c57ce1>] start_secondary+0x121/0x270
[  0.004000] [<ffffffffb7c000d5>] start_cpu+0x5/0x14
[  0.004000] ---[ end trace 73fc0e0825d4ca1f ]---
```

Puede pasarlos por alto. Esto es debido a una limitación conocida del hipervisor de Azure que se solucionará con el tiempo.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la [informática de alto rendimiento](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) en Azure.
