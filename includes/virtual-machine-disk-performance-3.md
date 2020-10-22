---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 10/12/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 14e74bfbcd087ccc1d8c5f2f10a8e44ed37cce84
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92016593"
---
![Menú de métricas](media/vm-disk-performance/utilization-metrics-example/fio-output.jpg)

Sin embargo, el Standard_D8s_v3 puede alcanzar un total de 28.600 IOPS, ya que el uso de las métricas permite investigar lo que está ocurriendo e identificar nuestro cuello de botella de E/S de almacenamiento. En primer lugar, busque el botón de métricas en el menú de la izquierda y haga clic en él:

![Menú de métricas](media/vm-disk-performance/utilization-metrics-example/metrics-menu.jpg)

Primero de todo, echemos un vistazo a nuestra métrica del **porcentaje de consumo de IOPS en caché de la máquina virtual**:

![Porcentaje de consumo de IOPS en caché de la máquina virtual](media/vm-disk-performance/utilization-metrics-example/vm-cached.jpg)

Esta métrica nos indica que, de los 16.000 IOPS asignados a los IOPS almacenados en el caché de la máquina virtual, se está utilizando el 61 %. Esto significa que el cuello de botella de E/S de almacenamiento no es con los discos que están almacenados en el caché, porque no está al 100 %. Ahora echemos un vistazo a nuestra métrica del **porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual**:

![Porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual](media/vm-disk-performance/utilization-metrics-example/vm-uncached.jpg)

Esta métrica está al 100 %, lo cual indica que se están utilizando todos los 12.800 IOPA asignados a los IOPS almacenados en el caché de la máquina virtual. Una manera de corregir esto es cambiar el tamaño de la máquina virtual a un tamaño mayor que pueda controlar la E/S adicional. Pero antes, echemos un vistazo al disco adjunto para ver cuántos IOPS están viendo. Primero de todo, echemos un vistazo al disco del sistema operativo examinando el **porcentaje de consumo de IOPS de disco del sistema operativo**:

![Porcentaje de consumo de IOPS de disco del sistema operativo](media/vm-disk-performance/utilization-metrics-example/os-disk.jpg)

Esta métrica nos indica que, de las 5.000 IOPS aprovisionadas para este disco del sistema operativo P30, se está utilizado alrededor del 90 %. Esto significa que no hay cuellos de botella aquí en el disco del sistema operativo. Ahora, echemos un vistazo a los discos de datos adjuntos a la máquina virtual examinando el **porcentaje de consumo de IOPS de disco de datos**:

![Porcentaje de consumo de IOPS de disco de datos](media/vm-disk-performance/utilization-metrics-example/data-disks-no-splitting.jpg)

Esta métrica nos indica que el porcentaje de consumo promedio de IOPS en todos los discos adjuntos es aproximadamente del 42 %. Este porcentaje se calcula en función de los IOPS que usan los discos y que no se atienden desde la memoria caché del host. Profundicemos en esta métrica para ver la aplicación de la **división** en estas métricas y la división por el valor del LUN:

![Porcentaje de IOPS de disco de datos consumido con divisiones](media/vm-disk-performance/utilization-metrics-example/data-disks-splitting.jpg)

Esta métrica nos indica que los discos de datos adjuntos a los LUN 3 y 2 utilizan alrededor del 85 % de los IOPS aprovisionados. Este es un diagrama del aspecto de la E/S de la arquitectura de discos y máquinas virtuales:

![Diagrama de ejemplo de métricas de E/S de almacenamiento](media/vm-disk-performance/utilization-metrics-example/metrics-diagram.jpg)
