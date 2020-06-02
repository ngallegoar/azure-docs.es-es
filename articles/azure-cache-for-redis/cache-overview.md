---
title: ¿Qué es Azure Cache for Redis?
description: Información sobre Azure Cache for Redis para permitir cache-aside, el almacenamiento en caché de contenido, el almacenamiento en caché de la sesión de usuario, el trabajo y la cola de mensajes, así como las transacciones distribuidas.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 05/12/2020
ms.openlocfilehash: 09a7a28716e437bab71c7386bd332712a4d192dd
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83196372"
---
# <a name="azure-cache-for-redis"></a>Azure Cache for Redis

Azure Cache for Redis proporciona un almacén de datos en memoria basado en el software de código abierto [Redis](https://redis.io/). Cuando se usa como memoria caché, Redis mejora el rendimiento y la escalabilidad de aquellos sistemas que dependen en gran medida de almacenes de datos de back-end. La mejora del rendimiento se logra mediante la copia de los datos a los que se accede con mayor frecuencia en un almacenamiento rápido ubicado cerca de la aplicación. Con Azure Cache for Redis, este almacenamiento rápido se coloca en la memoria en lugar de que una base de datos lo cargue desde el disco.

Azure Cache for Redis se puede usar como una caché de datos distribuidos, un almacén de sesión y un agente de mensajes. El rendimiento de las aplicaciones mejora porque se aprovecha de la baja latencia y alto rendimiento del motor Redis.

Azure Cache for Redis ofrece acceso a una caché en Redis segura y dedicada. Azure Redis Cache lo administra Microsoft, está hospedado en Azure y cualquier aplicación de dentro o fuera de Azure puede acceder a él.

## <a name="using-azure-cache-for-redis"></a>Uso de Azure Redis Cache

Azure Cache for Redis mejora el rendimiento de las aplicaciones, ya que admite patrones de arquitectura de aplicaciones comunes. Estos son algunos de los más comunes:

| Patrón      | Descripción                                        |
| ------------ | -------------------------------------------------- |
| [Cache-Aside](cache-web-app-cache-aside-leaderboard.md) | Las bases de datos suelen ser demasiado grandes para cargarlas directamente en una caché. Es habitual usar el patrón [cache-aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) para cargar datos en la memoria caché solo cuando es necesario. Cuando el sistema realiza cambios en los datos, también puede actualizar la caché, que se distribuye luego a otros clientes. Además, el sistema puede establecer una fecha de expiración en los datos o usar una directiva de expulsión para desencadenar las actualizaciones de los datos en la memoria caché.|
| [Almacenamiento en caché de contenido](cache-aspnet-output-cache-provider.md) | Muchas páginas web se generan a partir de plantillas que usan contenido estático como encabezados, pies de página y banners. Estos elementos estáticos no deberían cambiar a menudo. El uso de una caché en memoria proporciona acceso rápido a contenido estático en comparación con los almacenes de datos de back-end. Este patrón reduce el tiempo de procesamiento y la carga del servidor, lo que permite que los servidores web tengan mayor capacidad de respuesta. Puede permitirle reducir el número de servidores necesarios para administrar las cargas. Azure Cache for Redis proporciona el proveedor de caché de salida de Redis, que admite este patrón con ASP.NET.|
| [Almacenamiento en caché de sesión de usuario](cache-aspnet-session-state-provider.md) | Este patrón se utiliza normalmente con carros de la compra y otros datos de historial del usuario que una aplicación web puede desear asociar con las cookies del usuario. El almacenamiento de demasiados datos en una cookie puede tener un impacto negativo en el rendimiento, ya que aumenta su tamaño y no hay que olvidar que se pasa y se valida con cada solicitud. Una solución habitual usa la cookie como clave cuando se consultan datos en una base de datos. El uso de una memoria caché en memoria, como Azure Redis Cache, para asociar información a un usuario es mucho más rápido que su interacción con una base de datos relacional completa. |
| Almacenamiento en cola de trabajos y mensajes | Las aplicaciones agregan a menudo tareas a una cola cuando las operaciones asociadas a la solicitud tardan tiempo en ejecutarse. Las operaciones con ejecuciones más largas se ponen en cola para procesarse en secuencia, a menudo por parte de otro servidor.  Este método de aplazar trabajo se denomina puesta en cola de tareas. Azure Cache for Redis proporciona una cola distribuida que habilita este patrón en la aplicación.|
| Distributed transactions | A veces, las aplicaciones requieren una serie de comandos en un almacén de datos de back-end para ejecutarse como una única operación atómica. El resultado de todos los comandos debe ser satisfactorio, o todos deben revertirse al estado inicial. Azure Cache for Redis admite la ejecución de un lote de comandos como [transacción](https://redis.io/topics/transactions) única. |

## <a name="azure-cache-for-redis-offerings"></a>Ofertas de Azure Redis Cache

Azure Redis Cache está disponible en los niveles siguientes:

| Nivel | Descripción |
|---|---|
Básica | Caché de un solo nodo. Este nivel admite varios tamaños de memoria (250 MB-53 GB) y es ideal para el desarrollo o la realización de pruebas, así como para cargas de trabajo no críticas. El nivel Básico no tiene ningún Acuerdo de Nivel de Servicio (SLA) |
| Estándar | Memoria caché replicada en una configuración de dos nodos, principal y secundario, administrada por Azure y con un Acuerdo de Nivel de Servicio de alta disponibilidad (99,9 %). |
| Premium | El nivel Premium es el nivel específico para la empresa. Las memorias caché de nivel Premium admiten más características y tienen un rendimiento mayor con latencias más bajas. En el nivel Premium, las memorias caché se implementan en un hardware más potente, lo que proporciona un mejor rendimiento en comparación con los niveles Estándar o Básico. Esta ventaja significa que el rendimiento de una caché del mismo tamaño será mayor en el nivel Premium, en comparación con el nivel Estándar. |

> [!TIP]
> Para más información acerca del tamaño, la transferencia y el ancho de banda de las memorias caché de nivel Premium, consulte el artículo [Azure Redis Cache FAQ](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use) (Preguntas más frecuentes de Azure Redis Cache).
>

Una vez creada, la memoria caché se puede escalar a un nivel superior. Sin embargo, no se admite su reducción vertical. Para instrucciones detalladas acerca del escalado, consulte [How to Scale Azure Cache for Redis](cache-how-to-scale.md) (Escalado de Azure Redis Cache) y [How to automate a scaling operation](cache-how-to-scale.md#how-to-automate-a-scaling-operation) (Automatización de una operación de escalado).

### <a name="feature-comparison"></a>Comparación de características

En la página [Precios de Azure Cache for Redis](https://azure.microsoft.com/pricing/details/cache/) encontrará una comparación detallada de cada nivel. La tabla siguiente le ayuda a describir algunas de las características que admite cada nivel:

| Descripción de la característica | Premium | Estándar | Básica |
| ------------------- | :-----: | :------: | :---: |
| [Acuerdo de Nivel de Servicio (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Persistencia de datos de Redis](cache-how-to-premium-persistence.md) |✔|-|-|
| [Clúster en Redis](cache-how-to-premium-clustering.md) |✔|-|-|
| [Seguridad a través de las reglas de firewall](cache-configure.md#firewall) |✔|✔|✔|
| Cifrado en tránsito |✔|✔|✔|
| [Aislamiento y seguridad mejorados con VNet](cache-how-to-premium-vnet.md) |✔|-|-|
| [Import/Export](cache-how-to-import-export-data.md) |✔|-|-|
| [Actualizaciones programadas](cache-administration.md#schedule-updates) |✔|✔|✔|
| [Replicación geográfica](cache-how-to-geo-replication.md) |✔|-|-|
| [Reboot](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido de una aplicación web de ASP.NET](cache-web-app-howto.md) Cree una aplicación web de ASP.NET simple que use Azure Redis Cache.
* [Inicio rápido de .NET](cache-dotnet-how-to-use-azure-redis-cache.md) Cree una aplicación .NET que use Azure Redis Cache.
* [Inicio rápido de .NET Core](cache-dotnet-core-quickstart.md) Cree una aplicación .NET Core que use Azure Redis Cache.
* [Inicio rápido de Node.js](cache-nodejs-get-started.md) Cree una aplicación Node.js simple que use Azure Redis Cache.
* [Inicio rápido de Java](cache-java-get-started.md) Cree una aplicación Java simple que use Azure Redis Cache.
* [Inicio rápido de Python](cache-python-get-started.md) Cree una aplicación Python que use Azure Redis Cache.
