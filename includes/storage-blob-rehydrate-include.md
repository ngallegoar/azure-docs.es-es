---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/09/2019
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: 81ffc87ce97a936e693c59bca6caf721cb8599cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75780254"
---
Para leer datos de un almacenamiento de archivo, primero debe cambiar el nivel del blob a acceso frecuente o esporádico. Este proceso se conoce como rehidratación y puede tardar horas en completarse. Recomendamos tamaños de blob grandes para un rendimiento óptimo de la rehidratación. La rehidratación de varios blobs pequeños a la vez puede suponer tiempo adicional. Actualmente hay dos prioridades de rehidratación: Alta (versión preliminar) y Estándar. Pueden establecerse mediante la propiedad opcional *x-ms-rehydrate-priority* en una operación [Establecer el nivel del blob](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) o [Copiar blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob).

* **Prioridad estándar**: la solicitud de rehidratación se procesará en el orden en que se recibió y puede tardar hasta 15 horas.
* **Prioridad alta (versión preliminar)** : la solicitud de rehidratación tendrá prioridad con respecto a las solicitudes estándar y podría finalizar en menos de una hora. La prioridad alta puede tardar más de una hora, según el tamaño de blob y la demanda actual. Se garantiza que las solicitudes de prioridad alta tendrán prioridad con respecto a las solicitudes de prioridad estándar.

> [!NOTE]
> La prioridad estándar es la opción de rehidratación predeterminada para el archivo. La prioridad alta es una opción más rápida que cuesta más que la rehidratación de prioridad estándar y que suele reservarse para su uso en situaciones de restauración de datos de emergencia.

Una vez que se inicia una solicitud de rehidratación, no se puede cancelar. Durante la rehidratación, puede comprobar la propiedad de blob *Archive Status* para confirmar si el nivel ha cambiado. El estado indica "rehydrate-pending-to-hot" (rehidratación pendiente para acceso frecuente) o "rehydrate-pending-to-cool" (rehidratación pendiente para acceso esporádico), según el nivel de destino. Al finalizar, se quita la propiedad de blob archive status y la propiedad de blob *Access Tier* refleja el nuevo nivel de acceso frecuente o esporádico.
