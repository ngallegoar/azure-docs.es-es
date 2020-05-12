---
title: Bancos de pruebas de rendimiento de Azure NetApp Files para Linux| Microsoft Docs
description: Describe los bancos de pruebas de rendimiento que Azure NetApp Files ofrece para Linux.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: b-juche
ms.openlocfilehash: b763a734866dd5fed5bf0500d4d52b9324c92a79
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82614013"
---
# <a name="azure-netapp-files-performance-benchmarks-for-linux"></a>Bancos de pruebas de rendimiento de Azure NetApp Files para Linux

En este artículo se describen los bancos de pruebas de rendimiento que Azure NetApp Files ofrece para Linux.

## <a name="linux-scale-out"></a>Escalado horizontal de Linux

En esta sección se describen los bancos de pruebas de rendimiento del rendimiento de la carga de trabajo de Linux y de IOPS de la carga de trabajo.

### <a name="linux-workload-throughput"></a>Rendimiento de la carga de trabajo de Linux  

En el siguiente gráfico se representa una carga de trabajo secuencial de 64 Kibibytes (KiB) y un conjunto de trabajo de 1 TiB. Muestra que un solo volumen de Azure NetApp Files puede administrar entre ~1 600 MiB/s de escrituras secuenciales puras y ~4 500 MiB/s de lecturas secuenciales puras.  

En el gráfico se muestran reducciones de un 10 % cada vez, desde la lectura pura hasta la escritura pura. Muestra lo que puede esperar al usar distintas proporciones de lectura/escritura (100 %:0 %, 90 %:10 %, 80 %:20 %, etc.).

![Rendimiento de la carga de trabajo de Linux](../media/azure-netapp-files/performance-benchmarks-linux-workload-throughput.png)  

### <a name="linux-workload-iops"></a>IOPS de carga de trabajo de Linux  

En el gráfico siguiente se representa una carga de trabajo aleatoria de 4 Kibibytes (KiB) y un conjunto de trabajo de 1 TiB. En el gráfico se muestra que un volumen de Azure NetApp Files puede administrar entre ~130 000 escrituras aleatorias puras y ~460 000 lecturas aleatorias puras.  

En este gráfico se muestran reducciones de un 10 % cada vez, desde la lectura pura hasta la escritura pura. Muestra lo que puede esperar al usar distintas proporciones de lectura/escritura (100 %:0 %, 90 %:10 %, 80 %:20 %, etc.).

![IOPS de carga de trabajo de Linux](../media/azure-netapp-files/performance-benchmarks-linux-workload-iops.png)  

## <a name="linux-scale-up"></a>Escalado vertical de Linux  

El kernel de Linux 5.3 habilita las redes de escalado horizontal de cliente único para `nconnect` de NFS. En los gráficos de esta sección se muestran los resultados de las pruebas de validación para la opción de montaje del lado cliente con NFSv3. La característica está disponible en SUSE (a partir de SLES12SP4) y Ubuntu (a partir de la versión 19.10). Es similar en concepto a SMB multicanal y Oracle Direct NFS.

En los gráficos se comparan las ventajas de `nconnect` con un volumen montado no conectado. En los gráficos, FIO ha generado la carga de trabajo a partir de una sola instancia de D32s_v3 en la región de Azure Oeste de EE. UU. 2.

### <a name="linux-read-throughput"></a>Rendimiento de lectura de Linux  

En los gráficos siguientes se muestran lecturas secuenciales de ~3 500 MiB/s de lecturas con `nconnect`, que aproximadamente unas 2,3 veces no son `nconnect`.

![Rendimiento de lectura de Linux](../media/azure-netapp-files/performance-benchmarks-linux-read-throughput.png)  

### <a name="linux-write-throughput"></a>Rendimiento de escritura de Linux  

En los gráficos siguientes se muestran escrituras secuenciales. Indican que `nconnect` no tiene ninguna ventaja apreciable para las escrituras secuenciales. 1 500 MiB/s es aproximadamente el límite superior del volumen de escrituras secuenciales y el límite de salida de la instancia de D32s_v3.

![Rendimiento de escritura de Linux](../media/azure-netapp-files/performance-benchmarks-linux-write-throughput.png)  

### <a name="linux-read-iops"></a>IOPS de lectura de Linux  

En los gráficos siguientes se muestran lecturas aleatorias de ~200 000 IOPS de lectura con `nconnect`, aproximadamente unas 3 veces no son `nconnect`.

![IOPS de lectura de Linux](../media/azure-netapp-files/performance-benchmarks-linux-read-iops.png)  

### <a name="linux-write-iops"></a>IOPS de escritura de Linux  

En los gráficos siguientes se muestran escrituras aleatorias de ~135 000 IOPS de escritura con `nconnect`, que aproximadamente unas 3 veces no son `nconnect`.

![IOPS de escritura de Linux](../media/azure-netapp-files/performance-benchmarks-linux-write-iops.png)  

## <a name="next-steps"></a>Pasos siguientes

- [Azure NetApp Files: cómo sacar el máximo partido del almacenamiento en la nube](https://cloud.netapp.com/hubfs/Resources/ANF%20PERFORMANCE%20TESTING%20IN%20TEMPLATE.pdf?hsCtaTracking=f2f560e9-9d13-4814-852d-cfc9bf736c6a%7C764e9d9c-9e6b-4549-97ec-af930247f22f)
