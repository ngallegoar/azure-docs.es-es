---
title: Habilitación de InifinBand con SR-IOV (Microsoft Azure Virtual Machines) | Microsoft Docs
description: Aprenda a habilitar InfiniBand con SR-IOV.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 10/17/2019
ms.author: amverma
ms.openlocfilehash: de61403b62f80bea7872d5ab3561567ae2109590
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86500075"
---
# <a name="enable-infiniband-with-sr-iov"></a>Habilitación de InfiniBand con SR-IOV

Las series de VM de Azure NC, ND y H están respaldadas por una red InfiniBand dedicada. Todos los tamaños compatibles con RDMA son capaces de aprovechar esa red mediante Intel MPI. Algunas series de VM han ampliado la compatibilidad con todas las implementaciones de MPI y los verbos de RDMA a través de SR-IOV. Las VM compatibles con RDMA incluyen VM [optimizadas para GPU](../../sizes-gpu.md) y de [proceso de alto rendimiento (HPC)](../../sizes-hpc.md).

## <a name="choose-your-installation-path"></a>Elección de la ruta de acceso de instalación

Para empezar, la opción más sencilla es usar una imagen de plataforma preconfigurada para InfiniBand, si está disponible:

- **VM IaaS de HPC**: Para empezar a trabajar con VM IaaS para HPC, la solución más sencilla es usar la [imagen de SO de VM CentOS-HPC 7.6](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557), que ya está configurada con InfiniBand. Puesto que esta imagen ya está configurada con InfiniBand, no tiene que configurarla manualmente. Para conocer las versiones compatibles de Windows, consulte [Instancias compatibles con RDMA de Windows](../../sizes-hpc.md#rdma-capable-instances).

- **VM IaaS de GPU**: Actualmente no hay imágenes de plataforma preconfiguradas para VM optimizadas para GPU, excepto por la [imagen de SO de VM de CentOS-HPC 7.6](https://techcommunity.microsoft.com/t5/Azure-Compute/CentOS-HPC-VM-Image-for-SR-IOV-enabled-Azure-HPC-VMs/ba-p/665557). Para configurar una imagen personalizada con InfiniBand, consulte [Instalación manual de OFED de Mellanox](#manually-install-mellanox-ofed).

Si usa la imagen de VM personalizada o una VM [optimizada para GPU](../../sizes-gpu.md), debe configurarla con InfiniBand; para ello, agregue la extensión de VM InfiniBandDriverLinux o InfiniBandDriverWindows a la implementación. Aprenda a usar estas extensiones de VM con [Linux](../../sizes-hpc.md#rdma-capable-instances) y [Windows](../../sizes-hpc.md#rdma-capable-instances).

## <a name="manually-install-mellanox-ofed"></a>Instalación manual de OFED de Mellanox

Para configurar manualmente InfiniBand con SR-IOV, siga estos pasos. En el ejemplo de estos pasos se muestra la sintaxis de RHEL/CentOS, pero los pasos son generales y se pueden usar en cualquier sistema operativo compatible, como Ubuntu (16.04, 18.04, 19.04) y SLES (12 SP4 y 15). Los controladores incluidos también funcionan, pero los controladores OpenFabrics de Mellanox proporcionan más características.

Para obtener más información sobre las distribuciones admitidas para el controlador de Mellanox, consulte los [controladores OpenFabrics de Mellanox](https://www.mellanox.com/page/products_dyn?product_family=26) más recientes. Para obtener más información sobre el controlador OpenFabrics de Mellanox, consulte la [guía de usuario de Mellanox](https://docs.mellanox.com/category/mlnxofedib).

Vea el ejemplo siguiente para saber cómo configurar InfiniBand en Linux:

```bash
# Modify the variable to desired Mellanox OFED version
MOFED_VERSION=#4.7-1.0.0.1
# Modify the variable to desired OS
MOFED_OS=#rhel7.6
pushd /tmp
curl -fSsL https://www.mellanox.com/downloads/ofed/MLNX_OFED-${MOFED_VERSION}/MLNX_OFED_LINUX-${MOFED_VERSION}-${MOFED_OS}-x86_64.tgz | tar -zxpf -
cd MLNX_OFED_LINUX-*
sudo ./mlnxofedinstall
popd
```

Para Windows, descargue e instale los [controladores OFED de Mellanox para Windows](https://www.mellanox.com/page/products_dyn?product_family=32&menu_section=34).

## <a name="enable-ip-over-infiniband"></a>Habilitación de IP sobre InfiniBand

Use los siguientes comandos para habilitar IP sobre InfiniBand.

```bash
sudo sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
sudo systemctl restart waagent
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de [HPC](/azure/architecture/topics/high-performance-computing/) en Azure.
