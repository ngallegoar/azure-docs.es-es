---
title: archivo de inclusión
description: archivo de inclusión
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 11/05/2019
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: a737e130d616a67bab28c7c96c0372216a6707af
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "73720355"
---
### <a name="retrieve-host-name-ports-and-access-keys-from-the-azure-portal"></a>Recuperación del nombre de host, los puertos y las claves de acceso desde Azure Portal

Para conectar con una instancia de Azure Cache for Redis, los clientes de dicha caché necesitan el nombre de host, los puertos y una clave para la caché. Es posible que algunos clientes utilicen nombres ligeramente diferentes para estos elementos. Puede obtener el nombre de host, los puertos y las claves de [Azure Portal](https://portal.azure.com).

- Para obtener las claves de acceso, en el panel de navegación izquierdo de Azure Cache for Redis, seleccione **Claves de acceso**. 
  
  ![Claves de Azure Redis Cache](media/redis-cache-access-keys/redis-cache-keys.png)

- Para obtener el nombre de host y los puertos, en el panel de navegación izquierdo de Azure Cache for Redis, seleccione **Propiedades**. El nombre de host tiene el formato *\<DNS name>.redis.cache.windows.net*.

  ![Propiedades de Azure Redis Cache](media/redis-cache-access-keys/redis-cache-hostname-ports.png)

