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
ms.openlocfilehash: f5ac97812f973a20f6ee4c2dea34baaeb91203af
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92016558"
---
![Documentación de Dsv3](media/vm-disk-performance/dsv3-documentation.jpg)

El rendimiento de disco máximo **no almacenado en caché** es el límite máximo de almacenamiento predeterminado que la máquina virtual puede controlar. El límite del rendimiento de almacenamiento **almacenado en caché** máximo es un límite independiente cuando se habilita el almacenamiento en caché del host. El almacenamiento en caché de host funciona con el almacenamiento más cercano a la máquina virtual que puede escribirse o leerse rápidamente. La cantidad de almacenamiento que está disponible para la máquina virtual para el almacenamiento en caché del host se encuentra en la documentación. Por ejemplo, puede ver que la Standard_D8s_v3 incluye 200 GiB de almacenamiento en caché.

La habilitación del almacenamiento en caché del host puede realizarse al crear la máquina virtual y conectar discos. También puede ajustarlo para activar y desactivar el almacenamiento en caché del host de los discos en una máquina virtual existente.

![Almacenamiento en caché del host](media/vm-disk-performance/host-caching.jpg)

El almacenamiento en caché del host se puede ajustar para que coincida con los requisitos de carga de trabajo de cada disco. Puede establecer que el almacenamiento en caché del host sea de solo lectura para las cargas de trabajo que solo realizan operaciones de lectura y lectura/escritura para cargas de trabajo que equilibran las operaciones de lectura y escritura. Si la carga de trabajo no sigue ninguno de estos patrones, no recomendamos utilizar el almacenamiento en caché del host. 

Veamos un par de ejemplos de diferentes opciones de configuración de caché de host y cómo afecta al flujo de datos y al rendimiento. En este primer ejemplo, echaremos un vistazo a lo que sucede con las solicitudes de E/S cuando la opción de almacenamiento en caché de host está establecida en **solo lectura**.

Configuración:
- Standard_D8s_v3 
    - IOPS en caché: 16 000
    - IOPS no almacenadas en caché: 12.800
- Discos de datos P30 
    - IOPS: 5.000
    - **Almacenamiento en caché del host: Solo lectura** 

Cuando se realiza una lectura y los datos deseados están disponibles en la memoria caché, esta devuelve los datos solicitados y no es necesario leer el disco. Esta lectura se cuenta para los límites de la memoria caché de la máquina virtual.

![Lectura de lectura de almacenamiento en caché del host](media/vm-disk-performance/host-caching-read-hit.jpg)

Cuando se realiza una lectura y los datos deseados **no** están disponibles en la memoria caché, la solicitud de lectura se retransmite al disco que, a continuación, lo muestra en la memoria caché y en la máquina virtual. Esta lectura se cuenta para el límite sin almacenamiento en caché de la máquina virtual y el límite almacenado en caché de la misma.

![Fallo de lectura de almacenamiento en caché del host](media/vm-disk-performance/host-caching-read-miss.jpg)

Cuando se realiza una escritura, esta tiene que hacerse en la memoria caché y en el disco antes de que se considere completa. Esta escritura se cuenta para el límite sin almacenamiento en caché de la máquina virtual y el límite almacenado en caché de la misma.

![Escritura de lectura de almacenamiento en caché del host](media/vm-disk-performance/host-caching-write.jpg)

En el siguiente ejemplo, echaremos un vistazo a lo que sucede con las solicitudes de E/S cuando la opción de almacenamiento en caché de host está establecida en **lectura/escritura**.

Configuración:
- Standard_D8s_v3 
    - IOPS en caché: 16 000
    - IOPS no almacenadas en caché: 12.800
- Discos de datos P30 
    - IOPS: 5.000
    - **Almacenamiento en caché del host: Lectura/escritura** 

Las lecturas se administran exactamente igual que las solo lectura, mientras que las escrituras son lo único que es diferente con respecto al almacenamiento en caché de lectura/escritura. Al escribir con el almacenamiento en caché de host establecido en lectura/escritura, la escritura solo debe hacerse en la caché del host para que se considere completada. Después, la escritura se escribe de forma diferida en el disco, como un proceso en segundo plano. Esto significa que las escrituras se contarán para la E/S almacenada en caché cuando se escribe en la memoria caché; por el contrario, cuando se escribe de forma diferida en el disco, se contará hacia la E/S no almacenada en caché.

![Escritura/lectura de almacenamiento en caché del host](media/vm-disk-performance/host-caching-read-write.jpg)

Vamos a continuar con un ejemplo con la máquina virtual Standard_D8s_v3. La excepción es que, esta vez, habilitaremos el almacenamiento en caché del host en los discos y ahora el límite de IOPS de la máquina virtual es 16 000 IOPS. Hay tres discos P30 subyacentes conectados a la máquina virtual que pueden administrar 5000 IOPS.

Configuración:
- Standard_D8s_v3 
    - IOPS en caché: 16 000
    - IOPS no almacenadas en caché: 12.800
- Disco de SO P30 
    - E/S por segundo: 5.000
    - Almacenamiento en caché del host: Lectura/escritura 
- Dos discos de datos P30
    - E/S por segundo: 5.000
    - Almacenamiento en caché del host: Lectura/escritura

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
    - Almacenamiento en caché del host: Lectura/escritura
- Dos discos de datos P30 X 2
    - IOPS: 5.000
    - Almacenamiento en caché del host: Lectura/escritura
- Dos discos de datos P30 X 2
    - IOPS: 5.000
    - Almacenamiento en caché del host: Disabled

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

Métricas que ayudan a diagnosticar el límite de E/S de máquinas virtuales:
- **Porcentaje de consumo de IOPS en caché de máquinas virtuales**: el porcentaje calculado por el IOPS total completado en el límite máximo de IOPS de máquinas virtuales almacenadas en caché. Si esta cantidad es del 100 %, se restringirá la E/S de la aplicación en ejecución respecto al límite de IOPS de la máquina virtual en caché.
- **Porcentaje de consumo de ancho de banda en caché de máquinas virtuales**: el porcentaje calculado por el rendimiento total de disco completado en el límite máximo del rendimiento de las máquinas virtuales almacenadas en caché. Si esta cantidad es del 100 %, se restringirá la E/S de la aplicación en ejecución respecto al límite de ancho de banda del ancho de banda de la máquina virtual en caché.
- **Porcentaje de consumo de IOPS que no se encuentra almacenado en caché de máquinas virtuales**: el porcentaje calculado por el IOPS total de una máquina virtual completado en el límite máximo de IOPS de máquinas virtuales que no se encuentran almacenadas en caché. Si esta cantidad es del 100 %, se restringirá la E/S de la aplicación en ejecución respecto al límite de IOPS de la máquina virtual que no se encuentra almacenada en caché.
- **Porcentaje de consumo de ancho de banda que no está almacenado en caché de máquinas virtuales**: el porcentaje calculado por el rendimiento total de disco de una máquina virtual completado en el límite máximo del rendimiento de las máquinas virtuales provisionadas. Si esta cantidad es del 100 %, se restringirá la E/S de la aplicación en ejecución respecto al límite del ancho de banda de la máquina virtual que no se encuentra almacenada en caché.

## <a name="storage-io-utilization-metrics-example"></a>Ejemplo de métricas de uso de E/S de almacenamiento
Veamos un ejemplo de cómo utilizar estas nuevas métricas de uso de E/S de almacenamiento para depurar la ubicación de un cuello de botella en nuestro sistema. La configuración del sistema es exactamente lo que teníamos en el ejemplo anterior, con la excepción de que el disco del sistema operativo adjuntado **no** está almacenado en caché.

Configuración:
- Standard_D8s_v3 
    - IOPS en caché: 16 000
    - IOPS no almacenadas en caché: 12.800
- Disco de SO P30 
    - E/S por segundo: 5.000
    - Almacenamiento en caché del host: Disabled
- Dos discos de datos P30 X 2
    - IOPS: 5.000
    - Almacenamiento en caché del host: Lectura/escritura
- Dos discos de datos P30 X 2
    - IOPS: 5.000
    - Almacenamiento en caché del host: Disabled

