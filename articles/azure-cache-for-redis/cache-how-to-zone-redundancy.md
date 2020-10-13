---
title: Habilitación de la redundancia de zona para Azure Cache for Redis (versión preliminar)
description: Obtenga información sobre cómo configurar la redundancia de zona para el nivel Premium de las instancias de Azure Cache for Redis.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 33c346fa2e4572799ad6341bd5115cdd6e3b9ec9
ms.sourcegitcommit: f796e1b7b46eb9a9b5c104348a673ad41422ea97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91569977"
---
# <a name="enable-zone-redundancy-for-azure-cache-for-redis-preview"></a>Habilitación de la redundancia de zona para Azure Cache for Redis (versión preliminar)
En este artículo, aprenderá a configurar una instancia de Azure Cache con redundancia de zona mediante Azure Portal.

Los niveles Estándar y Premium de Azure Cache for Redis hospedan cada caché en dos máquinas virtuales dedicadas para proporcionar redundancia integrada. Aunque estas máquinas virtuales se encuentran en distintos [dominios de error y actualización de Azure](/azure/virtual-machines/windows/manage-availability) y tienen alta disponibilidad, son susceptibles a tener errores en el nivel del centro de datos. Azure Cache for Redis también admite la redundancia de zona en el nivel Premium. Una instancia de caché con redundancia de zona se ejecuta en máquinas virtuales distribuidas en [varias zonas de disponibilidad](/azure/virtual-machines/windows/manage-availability#use-availability-zones-to-protect-from-datacenter-level-failures). Proporciona mayor resistencia y disponibilidad.

> [!IMPORTANT]
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Condiciones de uso complementarias de las versiones preliminares de Microsoft Azure.](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 
> 

## <a name="prerequisites"></a>Requisitos previos
* Una suscripción a Azure:  [cree una cuenta gratuita](https://azure.microsoft.com/free/).

> [!NOTE]
> Esta característica se encuentra actualmente en versión preliminar: [póngase en contacto con nosotros](mailto:azurecache@microsoft.com) si le interesa.
>

## <a name="create-a-cache"></a>Creación de una caché
Para crear una instancia de caché, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después seleccione **Crear un recurso**.
  
1. En la página **Nuevo**, seleccione **Base de datos** y, a continuación, seleccione **Azure Cache for Redis**.

    :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Selección de Azure Cache for Redis.":::
   
1. En la página **Basics** (Información básica), configure las opciones de la nueva instancia de caché.
   
    | Configuración      | Valor sugerido  | Descripción |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Suscripción** | Seleccione su suscripción. | La suscripción en la que se creará esta nueva instancia de Azure Cache for Redis. | 
    | **Grupos de recursos** | Seleccione un grupo de recursos o **Crear nuevo**, y escriba un nombre nuevo para el grupo de recursos. | Nombre del grupo de recursos en el que se van a crear la caché y otros recursos. Al colocar todos los recursos de la aplicación en un grupo de recursos, puede administrarlos o eliminarlos fácilmente. | 
    | **Nombre DNS** | Escriba un nombre único global. | El nombre de la memoria caché debe ser una cadena de entre 1 y 63 caracteres, y solo puede contener números, letras o guiones. El nombre debe comenzar y terminar por un número o una letra y no puede contener guiones consecutivos. El *nombre de host* de la instancia de caché será *\<DNS name>.redis.cache.windows.net*. | 
    | **Ubicación** | Seleccione una ubicación. | Seleccione una [región](https://azure.microsoft.com/regions/) cerca de otros servicios que vayan a usar la memoria caché. |
    | **Tipo de caché** | Seleccione un plan de tarifa [Premium](https://azure.microsoft.com/pricing/details/cache/). |  El plan de tarifa determina el tamaño, el rendimiento y las características disponibles para la memoria caché. Para más información, consulte la [introducción a Azure Redis Cache](cache-overview.md). |
   
1. En la página **Advanced** (Avanzadas), elija **Recuento de réplicas**.
   
    :::image type="content" source="media/cache-how-to-multi-replicas/create-multi-replicas.png" alt-text="Selección de Azure Cache for Redis.":::

1. Seleccione **Zonas de disponibilidad**. 
   
    :::image type="content" source="media/cache-how-to-zone-redundancy/create-zones.png" alt-text="Selección de Azure Cache for Redis.":::

1. Deje las demás opciones con los valores predeterminados. 

    > [!NOTE]
    > Actualmente, la compatibilidad con la redundancia de zona solo funciona con instancias de caché sin clúster e instancias de caché replicadas sin redundancia geográfica. Además, no admite vínculos privados, escalado, persistencia de datos ni importación o exportación.
    >

1. Haga clic en **Crear**. 
   
    La caché tarda un tiempo en crearse. Puede supervisar el progreso en la página **Información general** de Azure Cache for Redis. Cuando **Estado** se muestra como **En ejecución**, la memoria caché está lista para su uso.
   
    > [!NOTE]
    > Las zonas de disponibilidad no se pueden cambiar una vez creada la instancia de caché.
    >

## <a name="next-steps"></a>Pasos siguientes
Más información sobre las características de Azure Cache for Redis.

> [!div class="nextstepaction"]
> [Niveles de servicio de Azure Cache for Redis Premium](cache-overview.md#service-tiers)
