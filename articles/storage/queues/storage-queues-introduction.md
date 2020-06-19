---
title: Introducción a Azure Queues en Azure Storage
description: Introducción a Azure Queues
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/18/2020
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: dineshm
ms.openlocfilehash: 678ab8a3f3dace17671c242939c20cb5499b2e39
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/16/2020
ms.locfileid: "84805126"
---
# <a name="what-are-azure-queues"></a>¿Qué son las colas de Azure?

Azure Queue Storage es un servicio para almacenar grandes cantidades de mensajes, a los que se puede acceder desde cualquier lugar del mundo a través de llamadas autenticadas mediante HTTP o HTTPS. Un mensaje de la cola puede llegar a tener hasta 64 KB. Una cola puede contener millones de mensajes, hasta el límite de capacidad total de una cuenta de almacenamiento. Las colas se utilizan normalmente para crear un trabajo pendiente del trabajo que se va a procesar de forma asincrónica.

## <a name="queue-service-concepts"></a>Conceptos del servicio Queue

El servicio Cola contiene los siguientes componentes:

![Diagrama que muestra la relación entre una cuenta de almacenamiento, las colas y los mensajes](./media/storage-queues-introduction/queue1.png)

* **Formato de dirección URL**: Las colas son direccionables mediante el siguiente formato de dirección URL:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    La siguiente dirección URL dirige a una cola del diagrama:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Cuenta de almacenamiento**: Todo el acceso a Azure Storage se realiza a través de una cuenta de almacenamiento. Para más información sobre la capacidad de la cuenta de almacenamiento, consulte [Objetivos de escalabilidad y rendimiento para cuentas de almacenamiento estándar](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Cola**: una cola contiene un conjunto de mensajes. El nombre de la cola **debe** escribirse en minúsculas. Para más información, consulte [Asignar nombres a colas y metadatos](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Mensaje**: un mensaje, en cualquier formato, de hasta 64 KB. Antes de la versión 2017-07-29, el máximo tiempo de vida permitido es de siete días. A partir de la versión del 2017-07-29, inclusive, el tiempo de vida máximo puede ser cualquier número positivo o -1, lo que indica que el mensaje no expira. Si se omite este parámetro, el tiempo de vida predeterminado es siete días.

## <a name="next-steps"></a>Pasos siguientes

* [Cree una cuenta de almacenamiento](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Introducción a las colas con .NET](storage-dotnet-how-to-use-queues.md)
