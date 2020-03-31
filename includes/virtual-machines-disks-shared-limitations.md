---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 77fc6070010791bf96c944114929eba95842c9d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77471720"
---
En la versión preliminar, los discos administrados que tienen discos compartidos habilitados están sujetos a las siguientes limitaciones:

- Actualmente solo están disponible con los discos SSD prémium.
- Solo se admite actualmente en la región Centro-oeste de EE. UU.
- Todas las máquinas virtuales que comparten un disco deben implementarse en los mismos [grupos de selección de ubicación de proximidad](../articles/virtual-machines/windows/proximity-placement-groups.md).
- Solo se puede habilitar en discos de datos, no en discos de sistema operativo.
- Solo se pueden usar discos básicos con algunas versiones del clúster de conmutación por error de Windows Server. Para más información, consulte [Requisitos de hardware y opciones de almacenamiento de clústeres de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- El almacenamiento en caché del host de solo lectura no está disponible para los discos SSD prémium con `maxShares>1`.
- Los conjuntos de disponibilidad y los conjuntos de escalado de máquinas virtuales solo se pueden usar con el valor de `FaultDomainCount` establecido en 1.
- La compatibilidad con Azure Backup y Azure Site Recovery todavía no está disponible.

Si está interesado en probar discos compartidos, [regístrese en nuestra versión preliminar](https://aka.ms/AzureSharedDiskPreviewSignUp).
