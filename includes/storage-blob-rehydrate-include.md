---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/08/2020
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: a369eb7000fb8622a69f4205ffcc232ae9c9d242
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95545941"
---
Para leer datos de un almacenamiento de archivo, primero debe cambiar el nivel del blob a acceso frecuente o esporádico. Este proceso se conoce como rehidratación y puede tardar horas en completarse. Recomendamos tamaños de blob grandes para un rendimiento óptimo de la rehidratación. La rehidratación de varios blobs pequeños a la vez puede suponer tiempo adicional. Actualmente hay dos prioridades de rehidratación: Alta y Estándar, que pueden establecerse mediante la propiedad opcional *x-ms-rehydrate-priority* en una operación [Establecer el nivel del blob](/rest/api/storageservices/set-blob-tier) o [Copiar blob](/rest/api/storageservices/copy-blob).

* **Prioridad estándar**: la solicitud de rehidratación se procesará en el orden en que se recibió y puede tardar hasta 15 horas.
* **Prioridad alta**: la solicitud de rehidratación tendrá prioridad con respecto a las solicitudes estándar y podría finalizar en menos de una hora para objetos con un tamaño inferior a 10 GB. 

> [!NOTE]
> La prioridad estándar es la opción de rehidratación predeterminada para el archivo. La prioridad alta es una opción más rápida que cuesta más que la rehidratación de prioridad estándar y que suele reservarse para su uso en situaciones de restauración de datos de emergencia.
>
> La prioridad alta puede tardar más de una hora, según el tamaño de blob y la demanda actual. Se garantiza que las solicitudes de prioridad alta tendrán prioridad con respecto a las solicitudes de prioridad estándar.

Una vez que se inicia una solicitud de rehidratación, no se puede cancelar. Durante el proceso de rehidratación, la propiedad del blob *x-ms-access-tier* se seguirá mostrando como un archivo hasta que la rehidratación se haya completado en un nivel en línea. Para confirmar el estado y progreso de la rehidratación, se puede llamar a [Get Blob Properties](/rest/api/storageservices/get-blob-properties) para comprobar las propiedades del blob *x-ms-archive-status* y *x-ms-rehydrate-priority*. El estado del archivo puede indicar "rehydrate-pending-to-hot" (rehidratación pendiente para acceso frecuente) o "rehydrate-pending-to-cool" (rehidratación pendiente para acceso esporádico), en función el nivel de destino de la rehidratación. La prioridad de rehidratación indicará la velocidad de "Alta" o "Estándar". Al finalizar, el estado de archivo y las propiedades de la prioridad de rehidratación, y la propiedad de blob del nivel de acceso se actualizará para reflejar el nivel de acceso frecuente o esporádico seleccionado.