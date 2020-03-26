---
title: 'Inicio rápido: Creación de colas de Azure Storage en el portal'
description: Use Azure Portal para crear una cola. A continuación, use Azure Portal para agregar un mensaje, ver las propiedades del mensaje y quitar el mensaje de la cola.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/06/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.reviewer: cbrooks
ms.openlocfilehash: 2876d2a680aa41372557a9f04c1d6a5eed56b8ae
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "74269246"
---
# <a name="quickstart-create-a-queue-and-add-a-message-with-the-azure-portal"></a>Inicio rápido: Creación de una cola y agregación de un mensaje con Azure Portal

En este inicio rápido, obtendrá información sobre cómo usar [Azure Portal](https://portal.azure.com/) para crear una cola en Azure Storage, agregar mensajes y quitarlos de la cola.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

## <a name="create-a-queue"></a>Creación de una cola

Para crear una cola en Azure Portal, siga estos pasos:

1. Vaya a la nueva cuenta de almacenamiento en Azure Portal.
2. En el menú izquierdo de la cuenta de almacenamiento, desplácese a la sección **Queue Service** (Servicio Cola) y, después, seleccione **Queues** (Colas).
3. Seleccione el botón **+ Cola**.
4. Escriba un nombre para la nueva cola. El nombre de la cola debe escribirse en minúsculas, comenzar por una letra o un número, y solo puede incluir letras, números y el carácter de guión (-).
6. Seleccione **Aceptar** para crear la cola.

    ![Captura de pantalla que muestra cómo crear una cola en Azure Portal](media/storage-quickstart-queues-portal/create-queue.png)

## <a name="add-a-message"></a>Agregar un mensaje

Después, agregue un mensaje a la cola nueva. Un mensaje puede ser de hasta 64 KB de tamaño.

1. Seleccione la nueva cola en la lista de colas de la cuenta de almacenamiento.
1. Seleccione el botón **+ Agregar mensaje** para agregar un mensaje a la cola. Escriba un mensaje en el campo **Texto del mensaje**. 
1. Especifique cuándo expira el mensaje. El tiempo máximo que un mensaje puede permanecer en la cola es de 7 días.
1. Indique si desea codificar el mensaje como Base64. Se recomienda la codificación de datos binarios.
1. Seleccione el botón **Aceptar** para agregar el mensaje.

    ![Captura de pantalla que muestra cómo agregar un mensaje a una cola](media/storage-quickstart-queues-portal/add-message.png)

## <a name="view-message-properties"></a>Visualización de las propiedades de los mensajes

Después de agregar un mensaje, Azure Portal muestra una lista de todos los mensajes de la cola. Puede ver el identificador, el contenido, el tiempo de inserción y la hora de expiración del mensaje. También puede ver el número de veces que este mensaje se ha quitado de la cola.

![Captura de pantalla que muestra las propiedades del mensaje](media/storage-quickstart-queues-portal/view-message-properties.png)

## <a name="dequeue-a-message"></a>Quitar de la cola un mensaje

Puede quitar un mensaje de la parte delantera de la cola desde Azure Portal. Cuando se quita de la cola un mensaje, se elimina. 

Al eliminar de la cola siempre se quita el último mensaje en la cola. 

![Captura de pantalla que muestra cómo quitar un mensaje de la cola desde el portal](media/storage-quickstart-queues-portal/dequeue-message.png)

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, aprendió a crear una cola, agregar un mensaje, ver las propiedades de mensaje y quitar de la cola un mensaje en Azure Portal.

> [!div class="nextstepaction"]
> [¿Qué son las colas de Azure?](storage-queues-introduction.md)
