---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/17/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4b10955a1d3b85acbcae109836bebc03ec04c72c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76279304"
---
Azure proporciona los siguientes roles RBAC integrados para autorizar el acceso a datos de blob y cola con Azure AD y OAuth:

- [Propietario de datos de Storage Blob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): se usa para establecer la propiedad y administrar el control de acceso POSIX en Azure Data Lake Storage Gen2. Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Colaborador de datos de Storage Blob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): se usa para conceder permisos de lectura, escritura y eliminación a los recursos de almacenamiento de blobs.
- [Lector de datos de Storage Blob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader): se usa para conceder permisos de solo lectura a los recursos de almacenamiento de blobs.
- [Colaborador de datos de la cola de Storage](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): se usa para conceder permisos de lectura, escritura y eliminación a las colas de Azure.
- [Lector de datos de la cola de Storage](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): se usa para conceder permisos de solo lectura a las colas de Azure.
- [Procesador de mensajes de datos de la cola de Storage](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): se usa para conceder permisos de inspección, recuperación y eliminación a los mensajes de las colas de Azure Storage.
- [Emisor de mensajes de datos de la cola de Storage](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): se usa para conceder permisos de adición a los mensajes de las colas de Azure Storage.

> [!NOTE]
> Las asignaciones de roles RBAC pueden tardar hasta cinco minutos en propagarse.
>
> Solo los roles definidos explícitamente para el acceso a datos permiten a una entidad de seguridad acceder a los datos de blobs o colas. Los roles como **Propietario**, **Colaborador** y **Colaborador de la cuenta de almacenamiento** permiten que una entidad de seguridad administre una cuenta de almacenamiento, pero no proporcionan acceso a los datos de blobs o colas dentro de esa cuenta.
>
> El acceso a datos de blob o de cola en Azure Portal se puede autorizar mediante la cuenta de Azure AD o la clave de acceso a la cuenta de almacenamiento. Para más información, consulte [Usar Azure Portal para tener acceso a datos de blob o de cola](../articles/storage/common/storage-access-blobs-queues-portal.md).
