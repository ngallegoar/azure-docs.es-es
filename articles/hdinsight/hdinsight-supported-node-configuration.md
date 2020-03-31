---
title: Configuraciones de nodos admitidas de Azure HDInsight
description: Obtenga información sobre las configuraciones mínimas y recomendadas para los nodos de clúster de HDInsight.
keywords: tamaños de vm, tamaños de clúster, configuración de clúster
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: e75146266568001d8fee7be26898ac8bdfffb7fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77484793"
---
# <a name="what-are-the-default-and-recommended-node-configurations-for-azure-hdinsight"></a>¿Cuáles son las configuraciones de nodos predeterminadas y recomendadas para Azure HDInsight?

En este artículo se tratan las configuraciones de nodos predeterminadas y recomendadas para los clústeres de Azure HDInsight.

## <a name="default-and-minimum-recommended-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Configuración predeterminada y mínima recomendada de los nodos y tamaños de máquina virtual para clústeres

En las tablas siguientes se indican los tamaños de máquina virtual predeterminados y recomendados para clústeres de HDInsight.  Esta información es necesaria para entender qué tamaños de VM se deben usar al crear scripts de PowerShell o la CLI de Azure para implementar clústeres de HDInsight.

Si necesita más de 32 nodos de trabajo en un clúster, seleccione un tamaño de nodo principal con al menos 8 núcleos y 14 GB de RAM. 

Los únicos tipos de clúster que tienen discos de datos son los clústeres Kafka y HBase con la característica Escrituras aceleradas habilitada. HDInsight admite los tamaños de disco P30 y S30 en estos escenarios.

En la tabla siguiente se resumen las especificaciones de todos los tipos de máquina virtual mínimos recomendados que se usan en este documento.

| Size              | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Rendimiento máximo de almacenamiento temporal: IOPS / MBps de lectura / MBps de escritura | Discos de datos máx. / rendimiento: E/S | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D3_v2 | 4    | 14          | 200                    | 12000 / 187 / 93                                           | 16             / 16x500           | 4 / 3000                                       |
| Standard_D4_v2 | 8    | 28          | 400                    | 24000 / 375 / 187                                          | 32            / 32x500           | 8 / 6000                                       |
| Standard_D5_v2 | 16   | 56          | 800                    | 48000 / 750 / 375                                          | 64             / 64x500           | 8 / 12 000                                    |
| Standard_D12_v2   | 4         | 28          | 200            | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000 / 750 / 375                                        | 64 / 64x500                       | 8 / 12 000          |
| Standard_A1_v2  | 1         | 2           | 10             | 1000 / 20 / 10                                           | 2 / 2x500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000 / 40 / 20                                           | 4 / 4x500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000 / 80 / 40                                           | 8 / 8x500               | 4 / 1000                     |

Para más detalles sobre las especificaciones de cada tipo de máquina virtual, consulte los siguientes documentos:

* [Tamaños de máquina virtual de uso general: serie Dv2 1-5](../virtual-machines/dv2-dsv2-series.md)
* [Tamaños de máquina virtual optimizada para memoria: serie Dv2 11-15](../virtual-machines/dv2-dsv2-series-memory.md)
* [Tamaños de máquina virtual de uso general: serie Av2 1-8](../virtual-machines/av2-series.md)

### <a name="all-supported-regions-except-brazil-south-and-japan-west"></a>Todas las regiones admitidas, excepto Sur de Brasil y Japón Occidental

> [!Note]
> Para obtener el identificador de SKU para su uso en PowerShell y otros scripts, agregue `Standard_` al principio de todas las SKU de máquina virtual en las tablas siguientes. Por ejemplo, `D12_v2` se convertiría en `Standard_D12_v2`.

| Tipo de clúster | Hadoop | HBase | Interactive Query | Storm | Spark | ML Server | Kafka |
|---|---|---|---|---|---|---|---|
| Principal: tamaño de máquina virtual predeterminado | D12_v2 | D12_v2 | D13_v2 | A4_v2 | D12_v2, <br/>D13_v2* | D12_v2 | D3_v2 |
| Principal: tamaños mínimos de máquina virtual recomendados | D5_v2 | D3_v2 | D13_v2 | A4_v2 | D12_v2, <br/>D13_v2* | D12_v2 | D3_v2 |
| Trabajo: tamaño de máquina virtual predeterminado | D4_v2 | D4_v2 | D14_v2 | D3_v2 | D13_v2 | D4_v2 | 4 D12_v2 con 2 discos S30 por agente |
| Trabajo: tamaños mínimos de máquina virtual recomendados | D5_v2 | D3_v2 | D13_v2 | D3_v2 | D12_v2 | D4_v2 | D3_v2 |
| Zookeeper: tamaño de máquina virtual predeterminado |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 | A4_v2 |
| ZooKeeper: tamaños mínimos de máquina virtual recomendados |  | A4_v2 | A4_v2 | A2_v2 |  | A2_v2 | A4_v2 |
| Servicio Machine Learning: tamaño de máquina virtual predeterminado |  |  |  |  |  | D4_v2 |  |
| Servicios Machine Learning: tamaño mínimo de máquina virtual recomendado |  |  |  |  |  | D4_v2 |  |

\* = tamaños de máquina virtual para los clústeres de Enterprise Security Package (ESP) de Spark

### <a name="brazil-south-and-japan-west-only"></a>Solo Sur de Brasil y Japón Occidental

| Tipo de clúster | Hadoop | HBase | Interactive Query | Storm | Spark | Machine Learning Services |
|---|---|---|---|---|---|---|
| Principal: tamaño de máquina virtual predeterminado | D12 | D12 | D13 | A4_v2 | D12 | D12 |
| Principal: tamaños mínimos de máquina virtual recomendados | D5_v2 | D3_v2 | D13_v2 | A4_v2 | D12_v2 | D12_v2 |
| Trabajo: tamaño de máquina virtual predeterminado | D4 | D4 | D14 | D3 | D13 | D4 |
| Trabajo: tamaños mínimos de máquina virtual recomendados | D5_v2 | D3_v2 | D13_v2 | D3_v2 | D12_v2 | D4_v2 |
| Zookeeper: tamaño de máquina virtual predeterminado |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 |
| ZooKeeper: tamaños mínimos de máquina virtual recomendados |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 |
| Servicio Machine Learning: tamaños de máquina virtual predeterminados |  |  |  |  |  | D4 |
| Servicios Machine Learning: tamaños mínimos de máquina virtual recomendados |  |  |  |  |  | D4_v2 |

> [!NOTE]
> - Nota: Principal se conoce como *Nimbus* para el tipo de clúster de Storm.
> - El trabajo se conoce como *Supervisor* para el tipo de clúster de Storm.
> - El trabajo se conoce como *Región* para el tipo de clúster de HBase.

## <a name="next-steps"></a>Pasos siguientes

* [¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?](hdinsight-component-versioning.md)
