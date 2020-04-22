---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a5e0e459800e7cb57672518597f3d04a74f53118
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81008658"
---
Por ahora, los discos Ultra tienen limitaciones adicionales, como se indica a continuación:

Las únicas opciones de redundancia de infraestructura disponibles actualmente para Ultra Disks son las zonas de disponibilidad. Las máquinas virtuales que usan cualquier otra opción de redundancia no pueden conectar discos Ultra Disks.

En la tabla siguiente se describen las regiones en las que los discos Ultra Disks están disponibles, así como las opciones de disponibilidad correspondientes:

> [!NOTE]
> Algunas zonas de disponibilidad dentro de estas regiones no ofrecen discos Ultra Disks.

|Regions  |Sin redundancia de la infraestructura  |Zonas de disponibilidad  |
|---------|---------|---------|
|Oeste de EE. UU.     |Sí         |No         |
|Oeste de EE. UU. 2    |No         |Sí         |
|Este de EE. UU.     |No         |Sí         |
|Este de EE. UU. 2     |No         |Sí         |
|Sudeste de Asia     |No         |Sí         |
|Norte de Europa     |No         |Sí         |
|Oeste de Europa     |No         |Sí         |
|Sur de Reino Unido 2     |No         |Sí         |

- Solo se admiten en las siguientes series de máquinas virtuales:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- No todos los tamaños de máquina virtual están disponibles en las regiones admitidas con discos Ultra
- Solo están disponibles como discos de datos y solo admiten un tamaño de sector físico de 4000. Debido al tamaño de sector nativo de 4000 de los discos Ultra, hay algunas aplicaciones que no son compatibles con ellos. Un ejemplo sería Oracle Database, que requiere la versión 12.2, o posterior, para admitir discos Ultra.  
- Solo pueden crearse como discos vacíos.  
- Actualmente no admiten instantáneas de disco, imágenes de máquinas virtuales, conjuntos de disponibilidad, instancias de Azure Dedicated Host ni Azure Disk Encryption.
- Actualmente no admiten la integración con Azure Backup o Azure Site Recovery.
- El límite máximo actual de IOPS en máquinas virtuales de disponibilidad general es 80 000.

De forma predeterminada, los discos Ultra Disks de Azure ofrecen hasta 16 TiB por región y suscripción, pero los discos Ultra admiten una mayor capacidad por solicitud. Para solicitar un aumento del límite, póngase en contacto con Soporte técnico de Azure.