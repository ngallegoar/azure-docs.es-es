---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 07/07/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 65f6c239f34775efff6a2ea2e399064a7702606a
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89663365"
---
![Documentación de Dsv3](media/vm-disk-performance/dsv3-documentation.jpg)

El rendimiento de disco máximo **no almacenado en caché** es el límite máximo de almacenamiento predeterminado que la máquina virtual puede controlar. El límite del rendimiento de almacenamiento **almacenado en caché** máximo es un límite independiente cuando se habilita el almacenamiento en caché del host. La habilitación del almacenamiento en caché del host puede realizarse al crear la máquina virtual y conectar discos. También puede ajustarlo para activar y desactivar el almacenamiento en caché del host de los discos en una máquina virtual existente:

![Almacenamiento en caché del host](media/vm-disk-performance/host-caching.jpg)

El almacenamiento en caché del host se puede ajustar para que coincida con los requisitos de carga de trabajo de cada disco. Puede establecer que el almacenamiento en caché del host sea de solo lectura para las cargas de trabajo que solo realizan operaciones de lectura y lectura/escritura para cargas de trabajo que equilibran las operaciones de lectura y escritura. Si la carga de trabajo no sigue ninguno de estos patrones, desafortunadamente no podrá usar el almacenamiento en caché del host. 

Vamos a continuar con un ejemplo con la máquina virtual Standard_D8s_v3. La excepción es que, esta vez, habilitaremos el almacenamiento en caché del host en los discos y ahora el límite de IOPS de la máquina virtual es 16 000 IOPS. Hay tres discos P30 subyacentes conectados a la máquina virtual que pueden administrar 5000 IOPS.

Configuración:
- Standard_D8s_v3 
    - IOPS en caché: 16 000
    - IOPS no almacenadas en caché: 12.800
- Disco de SO P30 
    - E/S por segundo: 5.000
    - Almacenamiento en caché del host habilitado 
- Dos discos de datos P30
    - E/S por segundo: 5.000
    - Almacenamiento en caché del host habilitado

![Ejemplo de almacenamiento en caché del host](media/vm-disk-performance/host-caching-example-without-remote.jpg)

Ahora, la aplicación que usa esta máquina virtual Standard_D8s_v3 con el almacenamiento en caché habilitado realiza una solicitud de 15 000 IOPS. Dichas solicitudes se dividen en 5000 IOPS en cada disco subyacente conectado y no se produce ningún límite de rendimiento.

## <a name="combined-uncached-and-cached-limits"></a>Límites no almacenados en caché y almacenados en caché combinados

Los límites de almacenamiento en caché de una máquina virtual son independientes de los límites no almacenados en caché. Esto significa que puede habilitar el almacenamiento en caché del host en los discos conectados a una máquina virtual y, al mismo tiempo, no habilitar el almacenamiento en caché del host en otros discos para permitir que las máquinas virtuales obtengan una E/S de almacenamiento total del límite almacenado en caché más el límite no almacenado en caché. Vamos a ver un ejemplo de esto para apuntalar el conocimiento sobre el modo en que estos límites funcionan juntos y continuaremos con la configuración de la máquina virtual Standard_D8s_v3 y los discos premium conectados.

Configuración:
- Standard_D8s_v3 
    - IOPS en caché: 16 000
    - IOPS no almacenadas en caché: 12.800
- Disco de SO P30 
    - E/S por segundo: 5.000
    - Almacenamiento en caché del host habilitado 
- Dos discos de datos P30 X 2
    - IOPS: 5.000
    - Almacenamiento en caché del host habilitado
- Dos discos de datos P30 X 2
    - IOPS: 5.000
    - Almacenamiento en caché del host deshabilitado

![Ejemplo de almacenamiento en caché del host con almacenamiento remoto](media/vm-disk-performance/host-caching-example-with-remote.jpg)

Ahora, la aplicación que se ejecuta en esta máquina virtual Standard_D8s_v3 realiza una solicitud de 25 000 IOPS. Esta solicitud se divide en 5000 IOPS en cada disco subyacente conectado, donde 3 de esos discos están usando el almacenamiento en caché del host y 2 de los discos no lo están usando. Dado que los 3 que utilizan el almacenamiento en caché del host están dentro de los límites de almacenamiento en caché de 16 000, esas solicitudes se completan correctamente y no se produce ningún límite de rendimiento de almacenamiento. Además, como los 2 discos que no utilizan el almacenamiento en caché del host están dentro de los límites sin almacenamiento en caché de 12 800, esas solicitudes también se completan correctamente y no se produce ninguna limitación.

## <a name="metrics-for-disk-performance"></a>Métricas de rendimiento de disco
Tenemos métricas en Azure que proporcionan información sobre el rendimiento de las máquinas virtuales y los discos. Estas métricas se pueden ver a través de Azure Portal o se pueden recuperar a través de una llamada de API. Las métricas se calculan en intervalos de un minuto. Las siguientes métricas están disponibles para obtener información sobre el rendimiento de la máquina virtual y la E/S de disco:
- **Profundidad de la cola de disco del SO**: el número de solicitudes de E/S pendientes actualmente que esperan a que se lean desde el SO o se escriban en este.
- **Bytes de lectura de disco de SO por segundo**: el número de bytes que se leen en un segundo desde el disco del sistema operativo.
- **Operaciones de lectura de disco de SO por segundo**: el número de operaciones de entrada que se leen en un segundo desde el disco del sistema operativo.
- **Bytes de escritura en disco del SO por segundo**: el número de bytes que se escriben en un segundo desde el disco del sistema operativo.
- **Operaciones de escritura en disco de sistema operativo por segundo**: el número de operaciones de salida que se escriben en un segundo desde el disco del sistema operativo.
- **Profundidad de la cola de disco de datos**: el número de solicitudes de E/S pendientes actualmente que esperan a que se lean desde los discos de datos o se escriban en estos.
- **Bytes de lectura de disco de datos por segundo**: el número de bytes que se leen en un segundo desde los discos de datos.
- **Operaciones de lectura de disco de datos por segundo**: el número de operaciones de entrada que se leen en un segundo desde los discos de datos.
- **Bytes de escritura en disco de datos por segundo**: el número de bytes que se escriben en un segundo desde los discos de datos.
- **Operaciones de escritura en disco de datos por segundo**: el número de operaciones de salida que se escriben en un segundo desde los discos de datos.
- **Bytes de lectura de disco por segundo**: el número de bytes totales que se leen en un segundo desde los discos conectados a una máquina virtual.
- **Operaciones de lectura de disco por segundo**: el número de operaciones de entrada que se leen en un segundo desde todos los discos conectados a una máquina virtual.
- **Bytes de escritura en disco por segundo**: el número de bytes que se escriben en un segundo desde todos los discos conectados a una máquina virtual.
- **Operaciones de escritura en disco por segundo**: el número de operaciones de salida que se escriben en un segundo desde todos los discos conectados a una máquina virtual.

## <a name="storage-io-utilization-metrics"></a>Métricas de uso de E/S de almacenamiento
Métricas que ayudan a diagnosticar el límite de E/S de disco:
- **Porcentaje de consumo de IOPS de disco de datos**: el porcentaje calculado por la IOPS de disco de datos completada sobre la IOPS del disco de datos aprovisionado. Si esta cantidad es del 100 %, se restringirá la E/S de la aplicación en ejecución respecto al límite de IOPS del disco de datos.
- **Porcentaje de consumo de ancho de banda de disco de datos**: el porcentaje calculado por el rendimiento del disco de datos completado sobre el rendimiento del disco de datos aprovisionado. Si esta cantidad es del 100 %, se restringirá la E/S de la aplicación en ejecución respecto al límite de ancho de banda del disco de datos.
- **Porcentaje de consumo de IOPS del disco del sistema operativo**: el porcentaje calculado por la IOPS del disco del sistema operativo completada sobre la IOPS del disco del SO aprovisionada. Si esta cantidad es del 100 %, se restringirá la E/S de la aplicación en ejecución respecto al límite de IOPS del disco del sistema operativo.
- **Porcentaje de consumo de ancho de banda de disco de SO**: el porcentaje calculado por el rendimiento del disco del sistema operativo completado sobre el rendimiento del disco del sistema operativo aprovisionado. Si esta cantidad es del 100 %, se restringirá la E/S de la aplicación en ejecución respecto al límite de ancho de banda del disco del sistema operativo.