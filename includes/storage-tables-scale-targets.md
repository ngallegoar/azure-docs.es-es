---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/09/2020
ms.author: tamram
ms.openlocfilehash: 27617da97ced9ac775beae71e4c25202944b2aba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78942060"
---
La tabla siguiente describe la capacidad, escalabilidad y los objetivos de rendimiento de Table Storage.

| Resource | Destino |
|----------|---------------|
| Número de tablas en una cuenta de Azure Storage | Solo limitadas por la capacidad de la cuenta de almacenamiento |
| Número de particiones en una tabla | Solo limitadas por la capacidad de la cuenta de almacenamiento |
| Número de entidades de una partición | Solo limitadas por la capacidad de la cuenta de almacenamiento |
| Tamaño máximo de una tabla individual | 500 TiB |
| Tamaño máximo de una única entidad, incluidos todos los valores de propiedad | 1 MiB |
| Número máximo de propiedades de una entidad de tabla | 255 (incluyendo las tres propiedades de sistema: **PartitionKey**, **RowKey** y **Timestamp**) |
| Tamaño máximo total de una propiedad individual en una entidad | Varía en función del tipo de propiedad. Para obtener más información, consulte los **tipos de propiedades** en la [descripción del modelo de datos del servicio Tabla](/rest/api/storageservices/understanding-the-table-service-data-model). |
| Tamaño de la **PartitionKey** | Una cadena de hasta 1 KB |
| Tamaño de la **RowKey** | Una cadena de hasta 1 KB |
| Tamaño de una transacción de un grupo de entidades | Una transacción puede incluir como máximo 100 entidades y la carga debe ser inferior a 4 MiB. Una transacción de un grupo de entidades solo puede incluir una actualización de una entidad. |
| Número máximo de directivas de acceso almacenadas por tabla | 5 |
| Tasa de solicitud total por cuenta de almacenamiento | 20 000 transacciones por segundo, lo que supone un tamaño de entidad de 1 KiB |
| Rendimiento de destino de una sola partición de tabla (entidades de 1 KiB) | Hasta 2000 entidades por segundo |