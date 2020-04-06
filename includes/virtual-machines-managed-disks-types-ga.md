---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 6740ea320f2d950386da12eb44726e2c826b60a4
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2020
ms.locfileid: "80386094"
---
## <a name="premium-ssd"></a>SSD Premium

Los discos SSD Premium de Azure ofrecen soporte de disco de alto rendimiento y latencia baja para máquinas virtuales (VM) con cargas de trabajo con uso intensivo de entrada/salida (E/S). Para aprovechar la ventaja de la velocidad y el rendimiento de discos de Premium Storage, puede migrar los discos de VM existentes a SSD Premium. Los discos SSD Premium son adecuados para aplicaciones de producción críticas. Los discos SSD Premium solo se pueden usar con series de máquinas virtuales que sean compatibles con el almacenamiento premium.

Para más información sobre los tipos individuales de máquinas virtuales y los tamaños de Azure para Windows, incluidos los tamaños que son compatibles con el almacenamiento premium, consulte [Tamaños de máquina virtual Windows](../articles/virtual-machines/windows/sizes.md). Para más información sobre los tipos individuales de máquinas virtuales y los tamaños de Azure para Linux, incluidos los tamaños que son compatibles con el almacenamiento premium, consulte [Tamaños de máquina virtual Linux](../articles/virtual-machines/linux/sizes.md). En cualquiera de estos artículos, debe comprobar cada artículo de tamaño de VM individual para determinar si es compatible con almacenamiento Premium.

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Cuando se aprovisiona un disco de Premium Storage, a diferencia de Standard Storage, se garantizan la capacidad, las E/S por segundo y el rendimiento del mismo. Por ejemplo, si crea un disco P50, Azure aprovisiona una capacidad de almacenamiento de 4095 GB, 7500 E/S por segundo y un rendimiento de 250 MB/s para él. La aplicación puede usar toda la capacidad y el rendimiento o parte de ellos. Los discos SSD Premium están diseñados para proporcionar bajas latencias inferiores a 10 milisegundos y un IOPS y rendimiento que se describen en la tabla anterior como del 99,9 % del tiempo.

## <a name="bursting"></a>Creación de ráfagas

Los tamaños de SSD Premium más pequeños que P30 ahora ofrecen una ráfaga de disco y pueden aumentar los IOPS por disco en hasta 3500 y el ancho de banda en hasta 170 Mbps. La creación de ráfagas está automatizada y funciona de acuerdo con un sistema de crédito. Los créditos se acumulan automáticamente en un depósito de ráfagas cuando el tráfico del disco está por debajo del objetivo de rendimiento aprovisionado y los créditos se consumen automáticamente cuando el tráfico supera el destino, hasta el límite máximo de ráfagas. El límite máximo de ráfagas define el límite superior de IOPS y Bandwidth del disco, incluso si tiene créditos de ráfagas para consumir. La ráfaga de disco proporciona una tolerancia mejorada a cambios imprevisibles en los patrones de E/S. Puede aprovecharla mejor en el arranque del disco del sistema operativo y las aplicaciones con tráfico de picos.    

La compatibilidad con ráfagas de discos se habilitará en las nuevas implementaciones de los tamaños de disco aplicables de forma predeterminada, sin necesidad de que intervenga el usuario. En el caso de los discos existentes de tamaños aplicables, puede habilitar la ráfaga con cualquiera de estas dos opciones: desasociar y volver a adjuntar el disco, o detener y reiniciar la VM conectada. Todos los tamaños de disco aplicables de la ráfaga comenzarán con un cubo de crédito de ráfaga completo cuando el disco esté conectado a una máquina virtual que admita una duración máxima en el límite máximo de ráfaga de 30 minutos. Para obtener más información sobre el aumento del trabajo de ráfagas en discos de Azure, consulte [Creación de ráfagas SSD Premium](../articles/virtual-machines/linux/disk-bursting.md). 

### <a name="transactions"></a>Transacciones

Para los discos SSD Premium, cada operación de E/S menor o igual a 256 KiB de rendimiento se considera una sola operación de E/S. Las operaciones de E/S mayores de 256 KiB de rendimiento se consideran varias operaciones de E/S con un tamaño de 256 KiB.

## <a name="standard-ssd"></a>SSD estándar

Los discos SSD estándar de Azure son una opción de almacenamiento rentable, optimizada para cargas de trabajo que necesitan un rendimiento constante en niveles inferiores de IOPS. Los discos SSD estándar ofrecen una buena experiencia de nivel de entrada para aquellos que desean pasarse a la nube, en especial si tiene problemas con la variación de las cargas de trabajo que se ejecutan en las soluciones de disco duro locales. En comparación con los discos HDD estándar, los discos SSD estándar ofrecen mayor disponibilidad, coherencia, confiabilidad y latencia. Los discos SSD estándar son convenientes para servidores web, servidores de aplicaciones con IOPS bajas, aplicaciones empresariales de poco uso y cargas de trabajo de desarrollo/pruebas. Al igual que los discos HDD estándar, los SSD están disponibles en todas las máquinas virtuales de Azure.

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

Los discos SSD estándar están diseñados para proporcionar latencias de menos de 10 milisegundos y un nivel de IOPS y de rendimiento hasta los límites descritos en la tabla anterior durante el 99 % del tiempo. Las IOPS y el rendimiento reales pueden variar a veces, según los patrones de tráfico. Los discos SSD estándar proporcionarán un rendimiento más coherente que los discos HDD, con una latencia menor.

### <a name="transactions"></a>Transacciones

Para los discos SSD estándar, cada operación de E/S menor o igual a 256 KiB de rendimiento se considera una sola operación de E/S. Las operaciones de E/S mayores de 256 KiB de rendimiento se consideran varias operaciones de E/S con un tamaño de 256 KiB. Estas transacciones tienen un impacto en la facturación.

## <a name="standard-hdd"></a>HDD estándar

Los discos HDD estándar de Azure ofrecen compatibilidad de discos confiable y de bajo coste para las máquinas virtuales que ejecutan cargas de trabajo que no tienen en cuenta la latencia. Con Standard Storage, los datos se almacenan en unidades de disco duro (HDD). La latencia, las IOPS y el rendimiento de los discos HDD estándar pueden variar más en comparación con los discos basados en SSD. Los discos HDD estándar están diseñados para ofrecer latencias de escritura inferiores a 10 ms y latencias de lectura inferiores a 20 ms para la mayoría de las operaciones de E/S, aunque el rendimiento real puede variar según el tamaño de E/S y el patrón de carga de trabajo. Cuando se trabaja con máquinas virtuales, se pueden usar discos HDD estándar para escenarios de desarrollo/pruebas y para cargas de trabajo menos críticas. Los discos HDD estándar están disponibles en todas las regiones de Azure y se pueden utilizar con todas las máquinas virtuales de Azure.

### <a name="disk-size"></a>Tamaño del disco
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

### <a name="transactions"></a>Transacciones

En el caso de los discos HDD estándar, cada operación de E/S se considera como una única transacción, independientemente del tamaño de esta. Estas transacciones tienen un impacto en la facturación.

## <a name="billing"></a>Facturación

Al usar Managed Disks, se aplican las siguientes consideraciones de facturación:

- Tipo de disco
- Tamaño del disco administrado
- Instantáneas
- Transferencias de datos de salida
- Número de transacciones

**Tamaño del disco administrado**: los discos administrados se facturan por el tamaño aprovisionado. Azure asigna el tamaño aprovisionado (redondeado al alza) a la oferta de tamaño de disco más cercana. Para obtener detalles sobre los tamaños de disco ofrecidos, consulte las tablas anteriores. Cada disco se asigna a una oferta de tamaño de disco aprovisionado compatible y se factura según corresponda. Por ejemplo, si ha aprovisionado un disco SSD estándar de 200 GiB, se asigna a la oferta de tamaño de disco E15 (256 GiB). La facturación de cualquier disco aprovisionado se prorratea cada hora con el precio mensual de la oferta de Premium Storage. Por ejemplo, si ha aprovisionado un disco E10 y lo ha eliminado después de 20 horas, se factura la oferta E10 prorrateada a 20 horas. Esto es así independientemente de la cantidad de datos que se escriba en el disco.

**Instantáneas**: Las instantáneas se facturan según el tamaño utilizado. Por ejemplo, si crea una instantánea de un disco administrado con capacidad aprovisionada de 64 GiB y el tamaño de datos usado real es de 10 GiB, solo se le cobra por el tamaño de datos usado de 10 GiB.
