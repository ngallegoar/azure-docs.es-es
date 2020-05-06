---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 39a013f5f4b587137366147ade77f0be1b353c4c
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204465"
---
- Las instantáneas incrementales no se pueden mover entre suscripciones.
- En este momento, solo se pueden generar URI de SAS de hasta cinco instantáneas de una determinada familia de instantáneas en un momento dado.
- No se puede crear una instantánea incremental para un disco determinado fuera de la suscripción de ese disco.
- Se pueden crear hasta siete instantáneas incrementales por disco cada cinco minutos.
- Se pueden crear un total de 200 instantáneas incrementales para un único disco.
- No se pueden obtener los cambios entre las instantáneas tomadas antes y después del cambio en el tamaño del disco primario en un límite de 4 TB. Es preciso volver a descargar la copia completa de la instantánea creada después del cambio de tamaño. Posteriormente, se pueden obtener los cambios entre las instantáneas creadas después del cambio de tamaño en el límite de 4 TB. 
