---
title: Archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 293f0f459e1f1e464fdec16b76eaf08336c92e93
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91377053"
---
- Solo se puede vincular una red virtual a un objeto de acceso a disco.
- La red virtual debe estar en la misma suscripción que el objeto de acceso a disco para vincularlas.
- Se pueden importar o exportar hasta 10 discos o instantáneas al mismo tiempo con el mismo objeto de acceso a disco.
- No se puede solicitar la aprobación manual para vincular una red virtual a un objeto de acceso al disco.
- Las instantáneas incrementales no se pueden exportar cuando están asociadas a un objeto de acceso al disco.
- AzCopy no se puede usar para descargar un VHD de un disco o instantánea protegidos mediante vínculos privados a una cuenta de almacenamiento. Sin embargo, se puede usar para descargar VHD en máquinas virtuales.
