---
title: Rendimiento del tamaño de las máquinas virtuales de la serie HC
description: Conozca los resultado de la prueba de rendimiento de los distintos tamaños de las máquinas virtuales de la serie HC en Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/10/2020
ms.author: amverma
ms.openlocfilehash: 0d63d9770dacf6a200e8b81e8d47d9f807a8a448
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90603453"
---
# <a name="hc-series-virtual-machine-sizes"></a>Tamaños de las máquinas virtuales de la serie HC

Se han realizado varias pruebas de rendimiento en los distintos tamaños de la serie HC. Estos son algunos de los resultados de esta prueba.

| Carga de trabajo                                        | HB                    |
|-------------------------------------------------|-----------------------|
| STREAM Triad                                    | 190 GB/s (Intel MLC AVX-512)  |
| High-Performance Linpack (HPL)                  | 3520 GigaFLOPS (Rpeak), 2970 GigaFLOPS (Rmax) |
| Latencia y ancho de banda de RDMA                        | 1,05 microsegundos; 96,8 Gb/s   |
| FIO en SSD NVMe local                           | 1,3 GB/s lecturas; 900 MB/s escrituras |  
| IOR en SSD Premium 4 Azure (P30 Managed Disks, RAID0) **  | 780 MB/s lecturas, 780 MB/escrituras |

## <a name="mpi-latency"></a>Latencia de MPI

Se ejecuta la prueba de latencia de MPI del conjunto de pruebas OSU de microrrealización. Los scripts de ejemplo se encuentran en [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh)

```bash
./bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./osu_latency 
```

:::image type="content" source="./media/latency-hc.png" alt-text="Latencia de MPI en HC de Azure.":::

## <a name="mpi-bandwidth"></a>Ancho de banda de MPI

Se ejecuta la prueba de ancho de banda de MPI del conjunto de pruebas OSU de microrrealización. Los scripts de ejemplo se encuentran en [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh)

```bash
./mvapich2-2.3.install/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./mvapich2-2.3/osu_benchmarks/mpi/pt2pt/osu_bw
```

:::image type="content" source="./media/bandwidth-hc.png" alt-text="Latencia de MPI en HC de Azure.":::


## <a name="mellanox-perftest"></a>Mellanox Perftest

El [paquete Mellanox Perftest](https://community.mellanox.com/s/article/perftest-package) tiene muchas pruebas de InfiniBand, como la de latencia (ib_send_lat) y la de ancho de banda (ib_send_bw). El siguiente es un ejemplo de comando.

```console
numactl --physcpubind=[INSERT CORE #]  ib_send_lat -a
```

## <a name="next-steps"></a>Pasos siguientes

- En los [blogs de la comunidad de Azure Compute Tech](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute) encontrará los anuncios más recientes y algunos ejemplos y resultados de informática de alto rendimiento (HPC).
- Si quiere una visión general arquitectónica de la ejecución de cargas de trabajo de HPC, vea [Informática de alto rendimiento (HPC) en Azure](/azure/architecture/topics/high-performance-computing/).
