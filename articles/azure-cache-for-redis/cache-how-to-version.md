---
title: Establecimiento de la versión de Redis para Azure Cache for Redis (versión preliminar)
description: Aprenda cómo configurar la versión de Redis.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 09/30/2020
ms.openlocfilehash: d9f48de7ef5d9525a995af4ebbd12c5f14f40189
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93349144"
---
# <a name="set-redis-version-for-azure-cache-for-redis-preview"></a>Establecimiento de la versión de Redis para Azure Cache for Redis (versión preliminar)
En este artículo, aprenderá a configurar la versión del software de Redis que se usará con la instancia de caché. Azure Cache for Redis ofrece la versión principal más reciente de Redis y al menos una versión anterior. Actualizará estas versiones con regularidad a medida que se publique el software de Redis más reciente. Puede elegir entre las dos versiones disponibles. Tenga en cuenta que la instancia de caché se actualizará a la siguiente versión automáticamente si ya no se admite la versión que usa actualmente.

## <a name="prerequisites"></a>Requisitos previos
* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)

## <a name="create-a-cache"></a>Creación de una caché
Para crear una instancia de caché, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después seleccione **Crear un recurso**.
  
1. En la página **Nuevo** , seleccione **Base de datos** y, a continuación, seleccione **Azure Cache for Redis**.

    :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Selección de Azure Cache for Redis.":::
   
1. En la página **Basics** (Información básica), configure las opciones de la nueva instancia de caché.
   
    | Configuración      | Valor sugerido  | Descripción |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Suscripción** | Seleccione su suscripción. | La suscripción en la que se creará esta nueva instancia de Azure Cache for Redis. | 
    | **Grupos de recursos** | Seleccione un grupo de recursos o **Crear nuevo** , y escriba un nombre nuevo para el grupo de recursos. | Nombre del grupo de recursos en el que se van a crear la caché y otros recursos. Al colocar todos los recursos de la aplicación en un grupo de recursos, puede administrarlos o eliminarlos fácilmente. | 
    | **Nombre DNS** | Escriba un nombre único global. | El nombre de la memoria caché debe ser una cadena de entre 1 y 63 caracteres, y solo puede contener números, letras o guiones. El nombre debe comenzar y terminar por un número o una letra y no puede contener guiones consecutivos. El *nombre de host* de la instancia de caché será *\<DNS name>.redis.cache.windows.net*. | 
    | **Ubicación** | Seleccione una ubicación. | Seleccione una [región](https://azure.microsoft.com/regions/) cerca de otros servicios que vayan a usar la memoria caché. |
    | **Tipo de caché** | Seleccione un [nivel y tamaño de caché](https://azure.microsoft.com/pricing/details/cache/). |  El plan de tarifa determina el tamaño, el rendimiento y las características disponibles para la memoria caché. Para más información, consulte la [introducción a Azure Redis Cache](cache-overview.md). |
   
1. En la página **Advanced** (Avanzado), elija la versión de Redis que se va a usar.
   
    :::image type="content" source="media/cache-how-to-version/select-redis-version.png" alt-text="Versión de Redis.":::

1. Haga clic en **Crear**. 
   
    La caché tarda un tiempo en crearse. Puede supervisar el progreso en la página **Información general** de Azure Cache for Redis. Cuando **Estado** se muestra como **En ejecución** , la memoria caché está lista para su uso.

    > [!NOTE]
    > En este momento, no se puede cambiar la versión de Redis una vez creada la instancia de caché.
    >

## <a name="next-steps"></a>Pasos siguientes
Más información sobre las características de Azure Cache for Redis.

> [!div class="nextstepaction"]
> [Niveles de servicio de Azure Cache for Redis Premium](cache-overview.md#service-tiers)
