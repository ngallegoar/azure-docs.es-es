---
title: Introducción a Reliable Collections
description: Los servicios con estado de Service Fabric proporcionan colecciones confiables que le permiten escribir aplicaciones en la nube altamente disponibles, escalables y de baja latencia.
ms.topic: conceptual
ms.date: 3/10/2020
ms.openlocfilehash: 6e0f39b994087fe44038e62e85326945589c719a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86245133"
---
# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Introducción a Reliable Collections en los servicios con estado de Azure Service Fabric

Reliable Collections le permite escribir aplicaciones en la nube altamente disponibles, escalables y de baja latencia como si estuviese escribiendo aplicaciones para un solo equipo. Las clases del espacio de nombres **Microsoft.ServiceFabric.Data.Collections** proporcionan un conjunto de colecciones que automáticamente hacen que el estado tenga una elevada disponibilidad. Los desarrolladores solo necesitan programar en las API de Reliable Collections y permiten que Reliable Collections administren el estado local y el replicado.

La diferencia clave entre Reliable Collections y otras tecnologías de alta disponibilidad (por ejemplo, Redis, Azure Table service y Azure Queue service) es que el estado se guarda localmente en la instancia del servicio mientras también se ofrece una elevada disponibilidad. Esto significa que:

* Todas las lecturas son locales, lo que provoca una latencia baja y lecturas de alto rendimiento.
* Todas las escrituras producen el número mínimo de operaciones de E/S de red, lo cual provoca una baja latencia y escrituras de alto rendimiento.

![Imagen de la evolución de colecciones](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Reliable Collections se puede considerar como la evolución natural de las clases **System.Collections** : un nuevo conjunto de colecciones que están diseñadas para la nube y aplicaciones de varios equipos sin aumentar la complejidad para los desarrolladores. De esta forma, Reliable Collections tienen los estados siguientes:

* Replicado: se replican los cambios de estado para obtener una alta disponibilidad.
* Asincrónico: las API son asincrónicas para asegurarse de que los subprocesos no se bloquean durante las E/S.
* Transaccional: las API usan la abstracción de transacciones para que pueda administrar fácilmente varias instancias de Reliable Collections dentro de un servicio.
* Persistente o volátil: los datos pueden persistir en el disco para obtener una mayor durabilidad frente a las interrupciones a gran escala (por ejemplo, un corte de alimentación del centro de datos). Algunas colecciones de confianza también admiten un modo volátil (con [Advertencias](service-fabric-reliable-services-reliable-collections-guidelines.md#volatile-reliable-collections)), donde todos los datos se mantienen en memoria, como una caché replicada en memoria.

Reliable Collections proporciona garantías de homogeneidad sólidas de fábrica con el fin de facilitar el razonamiento sobre el estado de la aplicación.
La coherencia fuerte se consigue asegurándose de que las confirmaciones de las transacciones finalizan únicamente tras registrar toda la transacción en un cuórum mayoritario de réplicas, incluida la principal.
Para conseguir una coherencia más débil, las aplicaciones pueden efectuar la confirmación al cliente o al solicitante antes de devolver la confirmación asincrónica.

Las API de Reliable Collections son una evolución de las API de colecciones simultáneas (que se encuentran en el espacio de nombres **System.Collections.Concurrent** ):

* Asincrónico: devuelve una tarea debido a que, a diferencia de las colecciones simultáneas, las operaciones se replican y se guardan.
* Ningún parámetro de salida: usa `ConditionalValue<T>` para devolver un valor y un `bool` en lugar de parámetros de salida. `ConditionalValue<T>` es similar a `Nullable<T>`, pero no requiere que T sea una estructura.
* Transacciones: utiliza un objeto de transacción para permitir que el usuario agrupe acciones en varias colecciones confiables en una transacción.

Por el momento, el espacio de nombres **Microsoft.ServiceFabric.Data.Collections** contiene tres colecciones:

* [Diccionario de confianza](/dotnet/api/microsoft.servicefabric.data.collections.ireliabledictionary-2?view=azure-dotnet#microsoft_servicefabric_data_collections_ireliabledictionary_2): representa una colección replicada, transaccional y asincrónica de pares clave/valor. De forma similar a **ConcurrentDictionary**, la clave y el valor pueden ser de cualquier tipo.
* [Cola de confianza](/dotnet/api/microsoft.servicefabric.data.collections.ireliablequeue-1?view=azure-dotnet#microsoft_servicefabric_data_collections_ireliablequeue_1): representa una cola estricta replicada, transaccional y asincrónica de tipo primero en entrar primero en salir (FIFO). De forma similar a **ConcurrentQueue**, el valor puede ser de cualquier tipo.
* [Cola simultánea de confianza](service-fabric-reliable-services-reliable-concurrent-queue.md): representa una mejor cola de ordenación replicada, transaccional y asincrónica para obtener un alto rendimiento. De forma similar a **ConcurrentQueue**, el valor puede ser de cualquier tipo.

## <a name="next-steps"></a>Pasos siguientes

* [Recomendaciones y directrices de Reliable Collections](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Trabajo con Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transacciones y bloqueos](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Administración de datos
  * [Copia de seguridad y restauración](service-fabric-reliable-services-backup-restore.md)
  * [Notificaciones](service-fabric-reliable-services-notifications.md)
  * [Serialización de colección confiable](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Serialización y actualización](service-fabric-application-upgrade-data-serialization.md)
  * [Configuración del administrador de estado confiable](service-fabric-reliable-services-configuration.md)
* Otros
  * [Guía de inicio rápido de Reliable Services](service-fabric-reliable-services-quick-start.md)
  * [Referencia para desarrolladores de colecciones confiables](/dotnet/api/microsoft.servicefabric.data.collections?view=azure-dotnet#microsoft_servicefabric_data_collections)
