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
ms.openlocfilehash: be014a257a68a266a5946bf5822b094c5a968f6e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86535597"
---
- Solo se puede vincular una red virtual a un objeto de acceso a disco.
- La red virtual debe estar en la misma suscripción que el objeto de acceso a disco para vincularlas.
- Se pueden importar o exportar hasta 10 discos o instantáneas al mismo tiempo con el mismo objeto de acceso a disco.
- No se puede solicitar la aprobación manual para vincular una red virtual a un objeto de acceso al disco.
- La funcionalidad diferencial no se admite para las instantáneas incrementales que están asociadas a un objeto de acceso a disco.