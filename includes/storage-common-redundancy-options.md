---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/14/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 73c2b742ede21a4e86d717d994f8ebc4f16389c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77157232"
---
Entre las opciones de redundancia para una cuenta de almacenamiento se incluyen las siguientes:

* Almacenamiento con redundancia local (LRS): una estrategia de redundancia simple y de bajo costo. Los datos se copian de forma sincrónica tres veces dentro de la región primaria.
* Almacenamiento con redundancia de zona (ZRS): redundancia para escenarios que requieren alta disponibilidad. Los datos se copian de forma sincrónica en tres zonas de disponibilidad de Azure en la región primaria.
* Almacenamiento con redundancia geográfica (GRS): redundancia entre regiones para protegerse frente a las interrupciones regionales. Los datos se copian sincrónicamente tres veces en la región primaria y, luego, se replican de forma asincrónica en la región secundaria. Para obtener acceso de lectura a los datos de la región secundaria, habilite el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS).
* Almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar): redundancia para escenarios que requieren alta disponibilidad y máxima durabilidad. Los datos se copian sincrónicamente en tres zonas de disponibilidad de Azure en la región primaria y, luego, se copian de forma asincrónica en la región secundaria. Para obtener acceso de lectura a los datos de la región secundaria, habilite el almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS).

Para más información sobre las opciones de redundancia en Azure Storage, consulte [Redundancia de Azure Storage](../articles/storage/common/storage-redundancy.md).
