---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8901f54c4802766eb3f12830d98d29fb47337ae5
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90705221"
---
Mientras se encuentra en versión preliminar, NFS tiene las siguientes limitaciones:

- NFS 4.1 actualmente solo admite las características obligatorias de la [especificación del protocolo](https://tools.ietf.org/html/rfc5661). No se admiten características opcionales, como delegaciones y devoluciones de llamada de todo tipo, actualizaciones de bloqueos y degradados, y el cifrado y la autenticación Kerberos.
- Si la mayoría de las solicitudes se centran en metadatos, la latencia será peor en comparación con las operaciones de lectura, escritura y actualización.
- Debe crear una cuenta de almacenamiento para crear un recurso compartido de NFS.
- Solo se admiten las API de REST del plano de administración. Las API de REST del plano de datos no están disponibles, lo que significa que las herramientas como el Explorador de Storage no funcionarán con los recursos compartidos de NFS ni podrán examinar los datos de recursos compartidos de NFS en Azure Portal.
- Solo están disponibles para el nivel prémium.
- Actualmente solo están disponibles para el almacenamiento con redundancia local.

### <a name="azure-storage-features-not-yet-supported"></a>Características de Azure Storage que aún no son compatibles

Además, las siguientes características de Azure Files no están disponibles con los recursos compartidos de NFS:

- Autenticación basada en identidad
- Soporte técnico de Azure Backup
- Instantáneas
- Eliminación temporal
- Compatibilidad total con el cifrado en tránsito (para más información, vea [seguridad de NFS](../articles/storage/files/storage-files-compare-protocols.md#security))
- Azure File Sync (solo disponible para los clientes de Windows, que son incompatibles con NFS 4.1)