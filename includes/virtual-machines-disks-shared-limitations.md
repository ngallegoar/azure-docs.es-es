---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: ce964ac197fbff64bbb7cc36e8c2bf762f93663f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84337364"
---
Durante la versión preliminar, los discos compartidos solo se pueden habilitar para un subconjunto de tipos de disco. Actualmente, solo los discos Ultra Disks y SSD Premium pueden habilitar discos compartidos. Cada disco administrado que tiene discos compartidos habilitados está sujeto a las siguientes limitaciones, organizadas según el tipo de disco:

### <a name="ultra-disks"></a>Discos Ultra

Los discos Ultra Disks tienen su propia lista independiente de limitaciones, que no están relacionadas con los discos compartidos. Para conocer las limitaciones de Ultra Disks, vea [Uso de Azure Ultra Disks](../articles/virtual-machines/linux/disks-enable-ultra-ssd.md).

Al compartir discos Ultra Disks, estos tienen las siguientes limitaciones adicionales:

- Actualmente, se limita a la compatibilidad con Azure Resource Manager o SDK.
- Solo se pueden usar discos básicos con algunas versiones del clúster de conmutación por error de Windows Server. Para más información, consulte [Requisitos de hardware y opciones de almacenamiento de clústeres de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).

### <a name="premium-ssds"></a>SSD Premium

- Solo se admite actualmente en la región Centro-oeste de EE. UU.
- Todas las máquinas virtuales que comparten un disco deben implementarse en los mismos [grupos de selección de ubicación de proximidad](../articles/virtual-machines/windows/proximity-placement-groups.md).
- Solo se puede habilitar en discos de datos, no en discos de sistema operativo.
- Solo se pueden usar discos básicos con algunas versiones del clúster de conmutación por error de Windows Server. Para más información, consulte [Requisitos de hardware y opciones de almacenamiento de clústeres de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- El almacenamiento en caché del host de solo lectura no está disponible para los discos SSD prémium con `maxShares>1`.
- Los conjuntos de disponibilidad y los conjuntos de escalado de máquinas virtuales solo se pueden usar con el valor de `FaultDomainCount` establecido en 1.
- La compatibilidad con Azure Backup y Azure Site Recovery todavía no está disponible.

Si está interesado en probar discos compartidos, [regístrese en nuestra versión preliminar](https://aka.ms/AzureSharedDiskPreviewSignUp).
