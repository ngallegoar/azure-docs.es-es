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
ms.openlocfilehash: 086ebf71e2da19a96433f32cfb1bae133e875400
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92518095"
---
![Gráfico en el que se muestran las especificaciones de Dsv3.](media/vm-disk-performance/dsv3-documentation.jpg)

- El rendimiento de disco máximo *no almacenado en caché* es el límite máximo de almacenamiento predeterminado que la máquina virtual puede controlar.
- El límite del rendimiento de almacenamiento *almacenado en caché* máximo es un límite independiente cuando se habilita el almacenamiento en caché del host.

El almacenamiento en caché de host funciona con el almacenamiento más cercano a la máquina virtual que puede escribirse o leerse rápidamente. La cantidad de almacenamiento que está disponible para la máquina virtual para el almacenamiento en caché del host se encuentra en la documentación. Por ejemplo, puede ver que la Standard_D8s_v3 incluye 200 GiB de almacenamiento en caché.

Puede habilitar el almacenamiento en caché de host al crear la máquina virtual y conectar los discos. También puede activar y desactivar el almacenamiento en caché de host de los discos en una máquina virtual existente.

![Captura de pantalla en la que se muestra el almacenamiento en caché de host.](media/vm-disk-performance/host-caching.jpg)

Puede ajustar el almacenamiento en caché de host para que coincida con los requisitos de carga de trabajo de cada disco. Puede configurar el almacenamiento en caché de host para que sea:

- **Solo lectura** : para cargas de trabajo que solo realizan operaciones de lectura
- **Lectura/escritura** : para cargas de trabajo que realizan un equilibrio de operaciones de lectura y escritura

Si la carga de trabajo no sigue ninguno de estos patrones, no se recomienda usar el almacenamiento en caché de host.

Ahora se verán un par de ejemplos de diferentes opciones de configuración de caché de host y cómo afecta al flujo de datos y al rendimiento. En este primer ejemplo, se examina lo que sucede con las solicitudes de E/S cuando la opción de almacenamiento en caché de host está establecida en **Solo lectura**.

**Configuración:**

- Standard_D8s_v3
  - IOPS en caché: 16 000
  - IOPS no almacenadas en caché: 12.800
- Disco de datos P30
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Solo lectura**

Cuando se realiza una lectura y los datos deseados están disponibles en la caché, esta devuelve los datos solicitados. No es necesario leer desde el disco. Esta lectura se cuenta para los límites en caché de la máquina virtual.

![Diagrama en el que se muestra un acierto de lectura del almacenamiento en caché de host de lectura.](media/vm-disk-performance/host-caching-read-hit.jpg)

Cuando se realiza una lectura y los datos deseados *no* están disponibles en la caché, la solicitud de lectura se retransmite al disco. Después, el disco la muestra en la caché y en la máquina virtual. Esta lectura se cuenta para el límite sin almacenamiento en caché de la máquina virtual y el límite almacenado en caché de la misma.

![Diagrama en el que se muestra un fallo de lectura del almacenamiento en caché de host de lectura.](media/vm-disk-performance/host-caching-read-miss.jpg)

Cuando se realiza una escritura, esta tiene que hacerse en la memoria caché y en el disco antes de que se considere completa. Esta escritura se cuenta para el límite sin almacenamiento en caché de la máquina virtual y el límite almacenado en caché de la misma.

![Diagrama en el que se muestra una escritura de almacenamiento en caché de host de lectura.](media/vm-disk-performance/host-caching-write.jpg)

Ahora se verá lo que sucede con las solicitudes de E/S cuando la opción de almacenamiento en caché de host está establecida en **Lectura/escritura**.

**Configuración:**

- Standard_D8s_v3
  - IOPS en caché: 16 000
  - IOPS no almacenadas en caché: 12.800
- Disco de datos P30
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Lectura/escritura**

Una lectura se controla de la misma manera que una operación de solo lectura. Las escrituras son lo único que es diferente con el almacenamiento en caché de lectura y escritura. Al escribir con el almacenamiento en caché de host establecido en **Lectura/escritura** , la escritura solo se debe realizar en la caché de host para que se considere completada. Después, la escritura se escribe de forma diferida en el disco, como un proceso en segundo plano. Esto significa que una escritura se cuenta para la E/S almacenada en caché cuando se escribe en la caché. Cuando se escribe de forma diferida en el disco, se cuenta para la E/S sin almacenamiento en caché.

![Diagrama en el que se muestra una escritura de almacenamiento en caché de host de lectura/escritura.](media/vm-disk-performance/host-caching-read-write.jpg)

Ahora continuará con la máquina virtual Standard_D8s_v3. Menos en esta ocasión, se habilitará el almacenamiento en caché de host en los discos. Además, ahora el límite de IOPS de la máquina virtual es 16 000. Hay tres discos P30 subyacentes conectados a la máquina virtual y cada uno puede administrar 5000 IOPS.

**Configuración:**

- Standard_D8s_v3
  - IOPS en caché: 16 000
  - IOPS no almacenadas en caché: 12.800
- Disco de SO P30
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Lectura/escritura**
- Dos discos de datos P30 × 2
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Lectura/escritura**

![Diagrama en el que se muestra un ejemplo de almacenamiento en caché de host.](media/vm-disk-performance/host-caching-example-without-remote.jpg)

La aplicación usa una máquina virtual Standard_D8s_v3 con el almacenamiento en caché habilitado. Realiza una solicitud de 15 000 IOPS. Las solicitudes se dividen en 5000 IOPS en cada disco subyacente conectado. No se produce ningún límite de rendimiento.

## <a name="combined-uncached-and-cached-limits"></a>Límites no almacenados en caché y almacenados en caché combinados

Los límites en caché de una máquina virtual son independientes de los límites no almacenados en caché. Esto significa que puede habilitar el almacenamiento en caché de host en los discos conectados a una máquina virtual mientras no lo habilita en otros discos. Esta configuración permite que las máquinas virtuales obtengan una E/S de almacenamiento total del límite en caché más el límite no almacenado en caché.

Ahora se verá un ejemplo para ayudarle a comprender cómo funcionan estos límites de forma conjunta. Se continuará con la configuración de una máquina virtual Standard_D8s_v3 y discos Premium conectados.

**Configuración:**

- Standard_D8s_v3
  - IOPS en caché: 16 000
  - IOPS no almacenadas en caché: 12.800
- Disco de SO P30
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Lectura/escritura**
- Dos discos de datos P30 × 2
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Lectura/escritura**
- Dos discos de datos P30 × 2
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Disabled** (Deshabilitado)

![Diagrama en el que se muestra un ejemplo de almacenamiento en caché de host con almacenamiento remoto.](media/vm-disk-performance/host-caching-example-with-remote.jpg)

En este caso, la aplicación que se ejecuta en una máquina virtual Standard_D8s_v3 realiza una solicitud de 25 000 IOPS. La solicitud se divide en 5000 IOPS en cada uno de los discos conectados. En tres discos se usa el almacenamiento en caché de host y en dos discos no.

- Como los tres discos que usan el almacenamiento en caché de host están dentro de los límites en caché de 16 000, esas solicitudes se completan correctamente. No se produce ningún límite de rendimiento de almacenamiento.
- Como los dos discos que no usan el almacenamiento en caché de host están dentro de los límites no almacenados en caché de 12 800, esas solicitudes también se completan correctamente. No se produce ningún límite.

## <a name="disk-performance-metrics"></a>Métricas de rendimiento de discos

En Azure hay métricas que proporcionan información sobre el rendimiento de las máquinas virtuales y los discos. Estas métricas se pueden ver a través de Azure Portal. También se pueden recuperar a través de una llamada API. Las métricas se calculan en intervalos de un minuto. Las métricas siguientes están disponibles para obtener conclusiones sobre la E/S de máquina virtual y disco, y también sobre el rendimiento:

- **Profundidad de la cola del disco del SO** : el número actual de solicitudes de E/S pendientes que esperan a que se lean desde el SO o se escriban en este.
- **Bytes de lectura de disco de SO por segundo** : el número de bytes que se leen en un segundo desde el disco del sistema operativo.
- **Operaciones de lectura en discos de SO por segundo** : el número de operaciones de entrada que se leen en un segundo desde el disco del sistema operativo.
- **Bytes de escritura en disco del SO por segundo** : el número de bytes que se escriben en un segundo desde el disco del sistema operativo.
- **Operaciones de escritura en disco de sistema operativo por segundo** : el número de operaciones de salida que se escriben en un segundo desde el disco del sistema operativo.
- **Profundidad de la cola de disco de datos** : el número actual de solicitudes de E/S pendientes que esperan a que se lean desde los discos de datos o se escriban en estos.
- **Bytes de lectura de discos de datos por segundo** : el número de bytes que se leen en un segundo desde los discos de datos.
- **Operaciones de lectura de disco de datos por segundo** : el número de operaciones de entrada que se leen en un segundo desde los discos de datos.
- **Bytes de escritura de discos de datos por segundo** : el número de bytes que se escriben en un segundo desde el disco de datos.
- **Operaciones de escritura de discos de datos por segundo** : el número de operaciones de salida que se escriben en un segundo desde los discos de datos.
- **Bytes de lectura de disco por segundo** : el número de bytes totales que se leen en un segundo desde los discos conectados a una máquina virtual.
- **Operaciones de lectura de disco por segundo** : el número de operaciones de entrada que se leen en un segundo desde todos los discos conectados a una máquina virtual.
- **Bytes de escritura en disco por segundo** : el número de bytes que se escriben en un segundo desde todos los discos conectados a una máquina virtual.
- **Operaciones de escritura por segundo en disco** : el número de operaciones de salida que se escriben en un segundo desde todos los discos conectados a una máquina virtual.

## <a name="storage-io-utilization-metrics"></a>Métricas de uso de E/S de almacenamiento

Métricas que ayudan a diagnosticar el límite de E/S de disco:

- **Porcentaje de consumo de IOPS de disco de datos** : el porcentaje calculado por la IOPS de disco de datos completada con respecto a la IOPS del disco de datos aprovisionado. Si esta cantidad es del 100 %, se restringe la E/S de la aplicación en ejecución respecto al límite de IOPS del disco de datos.
- **Porcentaje de ancho de banda consumido del disco de datos** : el porcentaje calculado por el rendimiento de disco de datos completado con respecto al rendimiento del disco de datos aprovisionado. Si esta cantidad es del 100 %, se restringe la E/S de la aplicación en ejecución respecto al límite de ancho de banda del disco de datos.
- **Porcentaje de consumo de IOPS de disco del sistema operativo** : el porcentaje calculado por la IOPS de disco del sistema operativo completada con respecto a la IOPS del sistema operativo aprovisionado. Si esta cantidad es del 100 %, se restringe la E/S de la aplicación en ejecución respecto al límite de IOPS del sistema operativo.
- **Porcentaje de ancho de banda consumido del sistema operativo** : el porcentaje calculado por el rendimiento del sistema operativo completado con respecto al rendimiento del sistema operativo aprovisionado. Si esta cantidad es del 100 %, se restringe la E/S de la aplicación en ejecución respecto al límite de ancho de banda del sistema operativo.

Métricas que ayudan a diagnosticar el límite de E/S de máquinas virtuales:

- **Porcentaje de consumo de IOPS en caché de la máquina virtual** : el porcentaje calculado por la IOPS total completado con respecto al límite máximo de IOPS de máquinas virtuales en caché. Si esta cantidad es del 100 %, se restringe la E/S de la aplicación en ejecución con respecto al límite de IOPS de la máquina virtual en caché.
- **Porcentaje de consumo de ancho de banda en caché de máquinas virtuales** : el porcentaje calculado por el rendimiento total de disco completado en el límite máximo del rendimiento de las máquinas virtuales almacenadas en caché. Si esta cantidad es del 100 %, se restringe la E/S de la aplicación en ejecución respecto al límite de ancho de banda del ancho de banda de la máquina virtual en caché.
- **Porcentaje de consumo de IOPS que no se encuentra almacenado en caché de máquinas virtuales** : el porcentaje calculado por la IOPS total de una máquina virtual completado en el límite máximo de IOPS de máquinas virtuales que no se encuentran almacenadas en caché. Si esta cantidad es del 100 %, se restringe la E/S de la aplicación en ejecución con respecto al límite de IOPS de la máquina virtual que no se encuentra almacenada en caché.
- **Porcentaje de consumo de ancho de banda que no está almacenado en caché de máquinas virtuales** : el porcentaje calculado por el rendimiento total de disco de una máquina virtual completado en el límite máximo del rendimiento de las máquinas virtuales aprovisionadas. Si esta cantidad es del 100 %, se restringe la E/S de la aplicación en ejecución respecto al límite de ancho de banda del ancho de banda de la máquina virtual que no está en caché.

## <a name="storage-io-utilization-metrics-example"></a>Ejemplo de métricas de uso de E/S de almacenamiento

Veamos un ejemplo de cómo utilizar estas nuevas métricas de uso de E/S de almacenamiento para depurar la ubicación de un cuello de botella en nuestro sistema. La configuración del sistema es la misma que la del ejemplo anterior, con la excepción de que el disco del sistema operativo conectado *no* se almacena en caché.

**Configuración:**

- Standard_D8s_v3
  - IOPS en caché: 16 000
  - IOPS no almacenadas en caché: 12.800
- Disco de SO P30
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Disabled** (Deshabilitado)
- Dos discos de datos P30 × 2
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Lectura/escritura**
- Dos discos de datos P30 × 2
  - IOPS: 5.000
  - Almacenamiento en caché de host: **Disabled** (Deshabilitado)
