---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/15/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 72d87142f9b9c1f7bcb2b02281851bd1e29bc9c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78262337"
---
Actualmente, Azure Managed Disks ofrece cuatro tipos de discos. Cada uno estos tipos está pensado para escenarios de clientes específicos.

## <a name="disk-comparison"></a>Comparación de discos

En la tabla siguiente se proporciona una comparación entre los discos Ultra, las unidades de estado sólido (SSD) prémium, los discos SSD estándar y las unidades de disco duro (HDD) estándar para Managed Disks, a fin de ayudarle a decidir qué opción debe usar.

|   | Disco Ultra   | SSD Premium   | SSD estándar   | HDD estándar   |
|---------|---------|---------|---------|---------|
|Tipo de disco   |SSD   |SSD   |SSD   |HDD   |
|Escenario   |Cargas de trabajo de uso intensivo de E/S, como [SAP HANA](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md), bases de datos de nivel superior (por ejemplo, SQL y Oracle) y otras cargas de trabajo con muchas transacciones.   |Cargas de trabajo confidenciales de producción y rendimiento   |Servidores web, aplicaciones empresariales poco utilizadas y desarrollo y pruebas   |Copia de seguridad, no crítico, acceso poco frecuente   |
|Tamaño máximo del disco   |65 536 gibibyte (GiB)    |32 767 GiB    |32 767 GiB   |32 767 GiB   |
|Rendimiento máx.   |2000 MiB/s    |900 MiB/s   |750 MiB/s   |500 MiB/s   |
|IOPS máx.   |160 000    |20.000   |6,000   |2\.000   |

## <a name="ultra-disk"></a>Disco Ultra

Los discos Ultra de Azure ofrecen un alto rendimiento, un número elevado de IOPS y un almacenamiento en disco coherente y de baja latencia para máquinas virtuales IaaS de Azure. Algunas ventajas adicionales de los discos Ultra incluyen la capacidad de cambiar dinámicamente el rendimiento del disco junto con sus cargas de trabajo sin tener que reiniciar las máquinas virtuales. Además, los discos Ultra son adecuados para cargas de trabajo con grandes cantidades de datos, como SAP HANA, bases de datos de nivel superior y cargas de trabajo que admitan muchas transacciones. Los discos Ultra solo se pueden utilizar como discos de datos. Por ello, recomendamos usar los discos SSD Premium como discos de sistema operativo.

### <a name="performance"></a>Rendimiento

Cuando aprovisione un disco ultra, puede configurar de forma independiente la capacidad y el rendimiento del disco. Los discos Ultra vienen en varios tamaños fijos que van desde los 4 GiB hasta los 64 TiB y cuentan con un modelo de configuración de rendimiento flexible que le permite configurar las unidades IOPS y el rendimiento de forma independiente.

Estas son algunas funcionalidades clave de los discos Ultra:

- Capacidad de disco: intervalos de capacidad de discos Ultra desde 4 GiB hasta 64 TiB.
- IOPS de disco: los dispositivos Ultra admiten límites de IOPS de 300 IOPS/GiB y hasta un máximo de 160 K IOPS por disco. Para recuperar la tasa de unidades IOPS que aprovisionó, asegúrese de que la cantidad de IOPS de disco seleccionadas sea menor que el límite de IOPS de la máquina virtual. El valor mínimo garantizado de IOPS por disco es 2 IOPS/GiB, con una base de referencia general mínima de 100 IOPS. Por ejemplo, si tuviera un disco Ultra de 4 GiB, tendrá un mínimo de 100 IOPS, en lugar de ocho IOPS.
- Rendimiento del disco: con los discos Ultra, el límite de rendimiento de un solo disco es de 256 KiB/s por cada IOPS aprovisionada, y hasta 2000 MBps como máximo por disco (donde MBps = 10^6 bytes por segundo). El rendimiento mínimo garantizado por disco es 4 KiB/s por cada IOPS aprovisionada, con una base de referencia total como mínima de 1 MBps.
- Los discos Ultra admiten el ajuste de los atributos de rendimiento del disco (IOPS y rendimiento) en tiempo de ejecución sin necesidad de desasociar el disco de la máquina virtual. Cuando se ha enviado una operación de cambio de tamaño del rendimiento del disco en un disco, este cambio puede tardar hasta una hora en surtir efecto. Hay un límite de cuatro operaciones de cambio de tamaño de rendimiento en un período de 24 horas. Se puede producir un error en la operación de ajuste de tamaño del rendimiento debido a la falta de capacidad de ancho de banda de rendimiento.

### <a name="disk-size"></a>Tamaño del disco

|Tamaño de disco (GiB)  |Capacidad de IOPS  |Capacidad de rendimiento (MBps)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9600         |2\.000         |
|64     |19 200         |2\.000         |
|128     |38 400         |2\.000         |
|256     |76 800         |2\.000         |
|512     |80 000         |2\.000         |
|1024 - 65 536 (los tamaños de este intervalo aumentan en incrementos de 1 TiB)     |160 000         |2\.000         |

### <a name="ga-scope-and-limitations"></a>Ámbito y limitaciones de la disponibilidad general

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]
