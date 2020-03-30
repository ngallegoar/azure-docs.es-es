---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/06/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 9333bb36971fc28a23a443e50d191abeef05b758
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78208257"
---
Para ver y copiar la cadena de conexión o las claves de acceso de la cuenta de almacenamiento desde Azure Portal:

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Busque su cuenta de almacenamiento.
3. En **Configuración**, seleccione **Claves de acceso**. Aparecen las claves de acceso de la cuenta, así como la cadena de conexión completa de cada clave.
4. Busque el valor de **Clave** en **key1** y haga clic en el botón **Copiar** para copiar la clave de la cuenta.
5. Como alternativa, puede copiar la cadena de conexión completa. Busque el valor de **Cadena de conexión** en **key1**y haga clic en el botón **Copiar** para copiar la cadena de conexión.

    ![Captura de pantalla que muestra cómo ver las claves de acceso en Azure Portal](media/storage-view-keys-include/portal-connection-string.png)

Puede utilizar cualquiera de las dos claves para acceder a Azure Storage, pero en general es recomendable usar la primera clave y reservar el uso de la segunda clave para cuando se roten las claves.
