---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7b41105bafedb8eeaffe5f266f5dd824957c57e0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91441203"
---
Por ahora, los discos Ultra tienen limitaciones adicionales, como se indica a continuación:

Las únicas opciones de redundancia de infraestructura disponibles actualmente para Ultra Disks son las zonas de disponibilidad. Las máquinas virtuales que usan cualquier otra opción de redundancia no pueden conectar discos Ultra Disks.

En la tabla siguiente se describen las regiones en las que los discos Ultra Disks están disponibles, así como las opciones de disponibilidad correspondientes:

> [!NOTE]
> Si una región de la lista siguiente no tiene ninguna zona de disponibilidad compatible con Disco Ultra, las VM de esa región deben implementarse sin ninguna opción de redundancia de infraestructura para poder conectar un Disco Ultra.

|Regions  |Opciones de redundancia  |
|---------|---------|
|Sur de Brasil     |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|India central     |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|Este de Asia     |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|Centro-oeste de Alemania     |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|Centro de Corea del Sur     |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|Centro-sur de EE. UU.    |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|US Gov: Arizona     |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|US Gov - Virginia     |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|Oeste de EE. UU.     |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)        |
|Centro de Australia    |Solo máquinas virtuales individuales (no se admiten conjuntos de disponibilidad ni conjuntos de escalado de máquinas virtuales)|
|Este de Australia     |Tres zonas de disponibilidad         |
|Sudeste Asiático    |Tres zonas de disponibilidad        |
|Centro de Canadá*     |Tres zonas de disponibilidad          |
|Centro de EE. UU.     |Tres zonas de disponibilidad          |
|Este de EE. UU.     |Tres zonas de disponibilidad          |
|Este de EE. UU. 2     |Tres zonas de disponibilidad         |
|Centro de Francia    |Dos zonas de disponibilidad        |
|Japón Oriental    |Tres zonas de disponibilidad        |
|Norte de Europa    |Tres zonas de disponibilidad        |
|Sur de Reino Unido    |Tres zonas de disponibilidad        |
|Oeste de Europa    | Tres zonas de disponibilidad|
|Oeste de EE. UU. 2    |Tres zonas de disponibilidad|

\* Póngase en contacto con el soporte técnico de Azure para obtener acceso a Availability Zones para esta región.

- Solo se admiten en las siguientes series de máquinas virtuales:
    - [ESv3](../articles/virtual-machines/ev3-esv3-series.md#esv3-series)
    - [Easv4](../articles/virtual-machines/eav4-easv4-series.md#easv4-series)
    - [Edsv4](../articles/virtual-machines/edv4-edsv4-series.md#edsv4-series)
    - [Esv4](../articles/virtual-machines/ev4-esv4-series.md#esv4-series)
    - [DSv3](../articles/virtual-machines/dv3-dsv3-series.md#dsv3-series)
    - [Dasv4](../articles/virtual-machines/dav4-dasv4-series.md#dasv4-series)
    - [Ddsv4](../articles/virtual-machines/ddv4-ddsv4-series.md#ddsv4-series)
    - [Dsv4](../articles/virtual-machines/dv4-dsv4-series.md#dsv4-series)
    - [FSv2](../articles/virtual-machines/fsv2-series.md)
    - [LSv2](../articles/virtual-machines/lsv2-series.md)
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- No todos los tamaños de máquina virtual están disponibles en todas las regiones admitidas con discos Ultra.
- Solo están disponibles como discos de datos. 
- Compatibilidad de forma predeterminada con el tamaño de sector físico de 4 k. El tamaño de sector de 512E se puede encontrar como una oferta disponible con carácter general, pero debe [registrarse en ella](https://aka.ms/ultradisk512e). La mayoría de las aplicaciones son compatibles con los tamaños de sector de 4 k, pero algunas requieren tamaños de sector de 512 bytes. Un ejemplo sería Oracle Database, que requiere la versión 12.2 o posterior para admitir los discos nativos de 4 k. En las versiones anteriores de Oracle DB, se requiere un tamaño de sector de 512 bytes.
- Solo pueden crearse como discos vacíos.
- Actualmente, no se admiten instantáneas de disco, imágenes de máquinas virtuales, conjuntos de disponibilidad, instancias de Azure Dedicated Host ni Azure Disk Encryption.
- Actualmente, no se admite la integración con Azure Backup o Azure Site Recovery.
- Solo se admiten lecturas y escrituras sin almacenamiento en caché.
- El límite máximo actual de IOPS en máquinas virtuales de disponibilidad general es 80 000.

De forma predeterminada, los discos Ultra Disks de Azure ofrecen hasta 16 TiB por región y suscripción, pero los discos Ultra admiten una mayor capacidad por solicitud. Para solicitar un aumento del límite, póngase en contacto con Soporte técnico de Azure.
