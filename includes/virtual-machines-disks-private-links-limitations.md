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
ms.openlocfilehash: 28717deadd8d842a68ab6ae6b52093f0117ad2c7
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "94630036"
---
- Solo se puede vincular una red virtual a un objeto de acceso a disco.
- La red virtual debe estar en la misma suscripción que el objeto de acceso a disco para vincularlas.
- Se pueden importar o exportar hasta 10 discos o instantáneas al mismo tiempo con el mismo objeto de acceso a disco.
- No se puede solicitar la aprobación manual para vincular una red virtual a un objeto de acceso al disco.
- Las instantáneas incrementales no se pueden exportar cuando están asociadas a un objeto de acceso al disco.

