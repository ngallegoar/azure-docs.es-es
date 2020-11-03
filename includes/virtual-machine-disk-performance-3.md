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
ms.openlocfilehash: 8eff9da82fdfa5749fd1c2bc04652d5c8ce8dfd2
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92518092"
---
![Captura de pantalla de la salida de FIO que muestra r=22.8k resaltado.](media/vm-disk-performance/utilization-metrics-example/fio-output.jpg)

Standard_D8s_v3 puede alcanzar un total de 28 600 IOPS. Con las métricas, vamos a investigar lo que está ocurriendo e identificar nuestro cuello de botella de E/S de almacenamiento. En el panel izquierdo, seleccione **Métricas** :

![Captura de pantalla que muestra la opción Métricas resaltada en el panel izquierdo.](media/vm-disk-performance/utilization-metrics-example/metrics-menu.jpg)

Primero de todo, echemos un vistazo a nuestra métrica del **porcentaje de consumo de IOPS en caché de la máquina virtual** :

![Captura de pantalla que muestra el porcentaje consumido de IOPS en caché de la VM.](media/vm-disk-performance/utilization-metrics-example/vm-cached.jpg)

Esta métrica nos indica que se está usando el 61 % de los 16 000 IOPS asignados a los IOPS almacenados en la memoria caché de la máquina virtual. Este porcentaje significa que el cuello de botella de E/S de almacenamiento no se produce con los discos que están almacenados en la memoria caché, porque no está al 100 %. Ahora echemos un vistazo a nuestra métrica del **porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual** :

![Captura de pantalla que muestra el porcentaje consumido de IOPS no almacenados en la memoria caché de la VM.](media/vm-disk-performance/utilization-metrics-example/vm-uncached.jpg)

Esta métrica está al 100 %. Esto indica que se están utilizando los 12 800 IOPS asignados a los IOPS no almacenados en caché de la máquina virtual. Una manera de corregir esto es cambiar el tamaño de la máquina virtual a un tamaño mayor que pueda administrar la E/S adicional. Pero antes de hacerlo, echemos un vistazo al disco conectado para averiguar cuántos IOPS están viendo. Comprobemos el disco del sistema operativo examinando el **porcentaje de consumo de IOPS de disco del sistema operativo** :

![Captura de pantalla que muestra el porcentaje consumido de IOPS del disco del sistema operativo.](media/vm-disk-performance/utilization-metrics-example/os-disk.jpg)

Esta métrica nos indica que se está usando aproximadamente el 90 % de los 5000 IOPS aprovisionados para este disco del sistema operativo P30. Este porcentaje significa que no hay cuellos de botella en el disco del sistema operativo. Ahora, echemos un vistazo a los discos de datos adjuntos a la máquina virtual examinando el **porcentaje de consumo de IOPS del disco de datos** :

![Captura de pantalla que muestra el porcentaje consumido de IOPS del disco de datos.](media/vm-disk-performance/utilization-metrics-example/data-disks-no-splitting.jpg)

Esta métrica nos indica que el porcentaje de consumo promedio de IOPS en todos los discos adjuntos es aproximadamente del 42 %. Este porcentaje se calcula en función de los IOPS que usan los discos y que no se atienden desde la memoria caché del host. Profundicemos en esta métrica aplicando la *división* en estas métricas y la división por el valor del LUN:

![Captura de pantalla que muestra el porcentaje consumido de los IOPS del disco de datos con división.](media/vm-disk-performance/utilization-metrics-example/data-disks-splitting.jpg)

Esta métrica nos indica que los discos de datos adjuntos a los LUN 3 y 2 utilizan alrededor del 85 % de los IOPS aprovisionados. Este es un diagrama del aspecto de la E/S de la arquitectura de discos y máquinas virtuales:

![Diagrama del ejemplo de métricas de E/S de almacenamiento.](media/vm-disk-performance/utilization-metrics-example/metrics-diagram.jpg)
