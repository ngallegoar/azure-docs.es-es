---
title: Alta disponibilidad en Azure Cache for Redis
description: Más información sobre las características y opciones de alta disponibilidad de Azure Cache for Redis
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: yegu
ms.openlocfilehash: f0bb8fd2d0b0ac271a167ad5474a55646bdafc65
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92536800"
---
# <a name="high-availability-for-azure-cache-for-redis"></a>Alta disponibilidad en Azure Cache for Redis

Azure Cache for Redis tiene alta disponibilidad integrada. El objetivo de su arquitectura de alta disponibilidad es asegurarse de que la instancia administrada de Redis funcione incluso cuando sus máquinas virtuales subyacentes se vean afectadas por interrupciones planeadas o no planeadas. Ofrece tasas de porcentaje mucho mayores que las que se logran al hospedar Redis en una sola máquina virtual.

Azure Cache for Redis implementa alta disponibilidad usando varias máquinas virtuales, llamadas *nodos* , para una caché. Estos nodos los configura de forma que la replicación de datos y la conmutación por error se realizan de forma coordinada. También organiza operaciones de mantenimiento, como la aplicación de revisión de software de Redis. Hay varias opciones de alta disponibilidad disponibles en los niveles Estándar y Premium:

| Opción | Descripción | Disponibilidad | Estándar | Premium |
| ------------------- | ------- | ------- | :------: | :---: |
| [Replicación estándar](#standard-replication)| Configuración replicada de dos nodos en un único centro de centros de recursos o zona de disponibilidad, con conmutación automática por error | 99,9 % |✔|✔|
| [Varias réplicas](#multiple-replicas) | Configuración replicada de varios nodos en una o varias zonas de disponibilidad, con conmutación automática por error | 99,95 % (con redundancia de zona) |-|✔|
| [Redundancia de zona](#zone-redundancy) | Configuración replicada de varios nodos en varias zonas de disponibilidad, con conmutación automática por error | 99,95 % (con varias réplicas) |-|✔|
| [Replicación geográfica](#geo-replication) | Instancias de caché vinculadas en dos regiones, con conmutación por error controlada por el usuario | 99,9 % (para una sola región) |-|✔|

## <a name="standard-replication"></a>Replicación estándar

De forma predeterminada, Azure Cache for Redis en el nivel Estándar o Premium se ejecuta en un par de servidores de Redis. Los dos servidores se hospedan en máquinas virtuales dedicadas. Redis de código abierto solo permite que un servidor controle las solicitudes de escritura de datos. Este servidor es el nodo *principal* , mientras que el otro es el de *réplica* . Después de aprovisionar los nodos de servidor, Azure Cache for Redis les asigna roles principales y de réplica. Normalmente, el nodo principal es responsable del mantenimiento de las solicitudes de escritura y de lectura de los clientes de Redis. En una operación de escritura, confirma una nueva clave y una actualización de claves en su memoria interna y responde inmediatamente al cliente. Además, reenvía la operación al nodo de réplica de forma asincrónica.

:::image type="content" source="media/cache-high-availability/replication.png" alt-text="Configuración de la replicación de datos":::
   
>[!NOTE]
>Normalmente, un cliente de Redis se comunica con el nodo principal en una caché en Redis para controlar todas las solicitudes de lectura y escritura. Algunos clientes de Redis se pueden configurar para que lleven a cabo operaciones de lectura desde el nodo de réplica.
>
>

Si el nodo principal de una caché en Redis no está disponible, el de réplica se promoverá para convertirse en el nodo principal automáticamente. Este proceso se denomina *conmutación por error* . El nodo de réplica esperará durante el tiempo suficiente antes de realizar la recuperación, en caso de que el principal se recupere rápidamente. Cuando se produce una conmutación por error, Azure Cache for Redis aprovisiona una nueva máquina virtual y la une a la caché como el nodo de réplica. El nodo de réplica realiza una sincronización completa de datos con el principal para que tenga otra copia de los datos de caché.

Un nodo principal puede retirarse del servicio como parte de una actividad de mantenimiento planeado, como la actualización del sistema operativo o del software de Redis. También puede dejar de funcionar debido a eventos no planeados como errores en el hardware, el software o la red subyacentes. En el artículo [Conmutación por error y aplicación de revisión de Azure Cache for Redis](cache-failover.md) se proporciona una explicación detallada sobre los tipos de conmutaciones por error de Redis. Una instancia de Azure Cache for Redis pasará por muchas conmutaciones por error durante su vigencia. La arquitectura de alta disponibilidad está diseñada para realizar estos cambios en una caché de la forma más transparente posible para sus clientes.

## <a name="multiple-replicas"></a>Varias réplicas

>[!NOTE]
>Opción disponible en versión preliminar.
>
>

Azure Cache for Redis permite nodos de réplica adicionales en el nivel Premium. Una [caché de varias réplicas](cache-how-to-multi-replicas.md) puede configurarse con hasta tres nodos de réplica. Normalmente, la existencia de más réplicas mejora la resistencia debido a los nodos adicionales que copian el principal. Incluso con más réplicas, una instancia de Azure Cache for Redis se puede ver seriamente afectada por una interrupción en las zonas de disponibilidad o el centro de datos. Puede aumentar la disponibilidad de la caché usando varias réplicas juntas con [redundancia de zona](#zone-redundancy).

## <a name="zone-redundancy"></a>Redundancia de zona

>[!NOTE]
>Opción disponible en versión preliminar.
>
>

Azure Cache for Redis admite configuraciones con redundancia de zona en el nivel Premium. Una [caché con redundancia de zona](cache-how-to-zone-redundancy.md) puede colocar sus nodos en diferentes [zonas de disponibilidad de Azure](../availability-zones/az-overview.md) dentro de la misma región. Además, acaba con el problema de que las interrupciones de las zonas de disponibilidad o del centro de datos sean el único punto de error, y aumenta la disponibilidad general de la caché.

En el diagrama siguiente se muestra la configuración de redundancia de zona:

:::image type="content" source="media/cache-high-availability/zone-redundancy.png" alt-text="Configuración de la replicación de datos":::
   
Azure Cache for Redis distribuye mediante un mecanismo round-robin los nodos de una caché con redundancia de zona por las zonas de disponibilidad que ha seleccionado. También determina qué nodo actuará como el principal inicialmente.

Una caché con redundancia de zona proporciona conmutación automática por error. Cuando el nodo principal actual no está disponible, una de las réplicas tomará el control. Es posible que la aplicación experimente un mayor tiempo de respuesta de la caché si el nuevo nodo principal se encuentra en otra zona de disponibilidad. Las zonas de disponibilidad están separadas geográficamente. Si se cambia de una zona de disponibilidad a otra, se altera la distancia física entre el lugar en el que se hospedan la aplicación y la caché. Este cambio afecta a las latencias de red bidireccionales entre la aplicación y la caché. La latencia adicional debería estar dentro de un intervalo aceptable en la mayoría de las aplicaciones. Se recomienda probar la aplicación para asegurarse de que pueda funcionar correctamente con una caché con redundancia de zona.

## <a name="geo-replication"></a>Replicación geográfica

La replicación geográfica está diseñada principalmente para la recuperación ante desastres. Ofrece la posibilidad de configurar una instancia de Azure Cache for Redis, en una región de Azure diferente, para hacer una copia de seguridad de la caché principal. En el artículo [Configuración de la replicación geográfica para Azure Cache for Redis](cache-how-to-geo-replication.md) se proporciona una explicación detallada sobre cómo funciona la replicación geográfica.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo configurar las opciones de alta disponibilidad de Azure Cache for Redis.

* [Niveles de servicio de Azure Cache for Redis Premium](cache-overview.md#service-tiers)
* [Incorporación de réplicas a Azure Cache for Redis](cache-how-to-multi-replicas.md)
* [Habilitación de la redundancia de zona para Azure Cache for Redis](cache-how-to-zone-redundancy.md)
* [Configuración de la replicación geográfica para Azure Cache for Redis](cache-how-to-geo-replication.md)