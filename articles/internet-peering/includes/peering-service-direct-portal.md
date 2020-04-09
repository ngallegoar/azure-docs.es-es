---
title: archivo de inclusión
titleSuffix: Azure
description: archivo de inclusión
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: include
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: 60752cf1b3c05ab7817083e70310ba7b40227dec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129925"
---
1. Haga clic en una conexión de emparejamiento que desee habilitar para Peering Service y, a continuación, haga clic en el botón **...**  > **Editar conexión**.
    > [!div class="mx-imgBorder"]
    > ![Edición de la conexión de emparejamiento](../media/setup-direct-modify-editconnection.png)
1. En la sección ***Use for Peering Service*** (Uso para Peering Service), haga clic en **Habilitado** y en **Guardar**.
    > [!div class="mx-imgBorder"]
    > ![Peering Service para la habilitación de la conexión de emparejamiento](../media/setup-direct-modify-editconnectionsettings-peering-service.png)
1. En la pantalla de información general, verá los detalles de la implementación. Una vez completada la implementación, haga clic en **Ir al recurso**.
    > [!div class="mx-imgBorder"]
    > ![Se completó la implementación](../media/setup-direct-modify-overview-deployment-complete.png)

1. Después, verá en Configuración **Prefijos registrados**. Haga clic en **Agregar un prefijo registrado**.
    > [!div class="mx-imgBorder"]
    > ![Prefijos y conexiones registrados](../media/setup-direct-modify-add-registered-prefix.png)
1. Para registrar un prefijo, seleccione un **Nombre** y un **Prefijo** y haga clic en **Guardar**
    > [!div class="mx-imgBorder"]
    >  ![Registrar un prefijo](../media/setup-direct-modify-register-a-prefix.png) 

1. Una vez creado el prefijo, lo verá en la lista de prefijos registrados. Haga clic en el **Nombre** del prefijo para ver más detalles.
    > [!div class="mx-imgBorder"]
    > ![Prefijos y conexiones registrados](../media/setup-direct-modify-registered-prefixes.png)
1. En la página del prefijo registrado, verá los detalles completos para incluir la **clave de prefijo** para cada prefijo. Esta clave se debe proporcionar al cliente asignado al prefijo desde su proveedor ISP. A continuación, el cliente puede registrar su prefijo dentro de su suscripción con esta clave.
    > [!div class="mx-imgBorder"]
    > ![Prefijo con clave de prefijo](../media/setup-direct-modify-registered-prefix-detail.png)