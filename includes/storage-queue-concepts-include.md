---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.custom: seo-python-october2019
ms.openlocfilehash: 23b93f507ef6abe19a0202b28afa31d28490b2b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80151328"
---
## <a name="what-is-queue-storage"></a>¿Qué es Queue Storage?

El almacenamiento en cola de Azure es un servicio para almacenar grandes cantidades de mensajes a los que puede obtenerse acceso desde cualquier lugar del mundo a través de llamadas autenticadas con HTTP o HTTPS. Un único mensaje en cola puede tener un tamaño de hasta 64 KB y una cola puede contener millones de mensajes, hasta el límite de capacidad total de una cuenta de almacenamiento. Queue Storage se usa normalmente para crear un trabajo pendiente del trabajo que se va a procesar de forma asincrónica.

## <a name="queue-service-concepts"></a>Conceptos del servicio Queue

Azure Queue service contiene los siguientes componentes:

![Componentes de Azure Queue service](./media/storage-queue-concepts-include/azure-queue-service-components.png)

* **Formato de dirección URL**: Las colas son direccionables mediante el formato de dirección URL siguiente: http://`<storage account>`.queue.core.windows.net/`<queue>`.
  
    La siguiente dirección URL dirige a una cola del diagrama:  
  
    `http://myaccount.queue.core.windows.net/incoming-orders`

* **Cuenta de almacenamiento**: Todo el acceso a Azure Storage se realiza a través de una cuenta de almacenamiento. Para más información sobre las cuentas de almacenamiento, consulte [Introducción a las cuentas de Storage](../articles/storage/common/storage-account-overview.md).
* **Cola**: una cola contiene un conjunto de mensajes. Todos los mensajes deben encontrarse en una cola. Tenga en cuenta que el nombre de la cola debe ir en minúsculas. Para más información, consulte [Asignar nombres a colas y metadatos](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **Mensaje**: un mensaje, en cualquier formato, de hasta 64 KB. El tiempo máximo que un mensaje puede permanecer en la cola es de 7 días. A partir de la versión del 2017-07-29, inclusive, el tiempo de vida máximo puede ser cualquier número positivo o -1, lo que indica que el mensaje no expira. Si se omite este parámetro, el tiempo de vida predeterminado es siete días.

