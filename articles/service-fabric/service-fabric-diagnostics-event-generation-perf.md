---
title: Supervisión de rendimiento de Azure Service Fabric
description: Obtenga información sobre los contadores de rendimiento para la supervisión y el diagnóstico de clústeres de Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 30b9b8393007033a7c2e6798cd57d9cf0128820d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75464703"
---
# <a name="performance-metrics"></a>Métricas de rendimiento

Se deben recopilar métricas para comprender el rendimiento de un clúster, así como de las aplicaciones se ejecutan en él. Para los clústeres de Service Fabric, recomendamos recopilar los siguientes contadores de rendimiento.

## <a name="nodes"></a>Nodos

Para las máquinas del clúster, considere la posibilidad de recopilar los siguientes contadores de rendimiento para comprender la carga de cada equipo y tomar las decisiones apropiadas sobre el escalado del clúster.

| Categoría de contador | Nombre del contador |
| --- | --- |
| Disco lógico | Espacio disponible en el disco lógico |
| Disco físico (por disco) | Prom. Longitud de la cola de lectura de disco |
| Disco físico (por disco) | Prom. Longitud de la cola de escritura de disco |
| Disco físico (por disco) | Prom. Segundos de disco/lecturas |
| Disco físico (por disco) | Prom. Segundos de disco/escrituras |
| Disco físico (por disco) | Lecturas de disco/s |
| Disco físico (por disco) | Bytes de lectura de disco/s |
| Disco físico (por disco) | Escrituras en disco/s |
| Disco físico (por disco) | Bytes de escritura en disco/s |
| Memoria | MB disponibles |
| Archivo de paginación | % de uso |
| Procesador (total) | % de tiempo de procesador |
| Proceso (por servicio) | % de tiempo de procesador |
| Proceso (por servicio) | Id. de proceso |
| Proceso (por servicio) | Bytes privados |
| Proceso (por servicio) | Número de subprocesos |
| Proceso (por servicio) | Bytes virtuales |
| Proceso (por servicio) | Espacio de trabajo |
| Proceso (por servicio) | Espacio de trabajo privado |
| Interfaz de red (todas las instancias) | Bytes leídos |
| Interfaz de red (todas las instancias) | Bytes enviados |
| Interfaz de red (todas las instancias) | Total de bytes |
| Interfaz de red (todas las instancias) | Longitud de la cola de salida |
| Interfaz de red (todas las instancias) | Paquetes de salida descartados |
| Interfaz de red (todas las instancias) | Paquetes recibidos descartados |
| Interfaz de red (todas las instancias) | Paquetes de salida con errores |
| Interfaz de red (todas las instancias) | Paquetes recibidos con errores |

## <a name="net-applications-and-services"></a>Aplicaciones y servicios .NET

Recopile los contadores siguientes si va a implementar servicios de .NET en el clúster. 

| Categoría de contador | Nombre del contador |
| --- | --- |
| Memoria CLR de .NET (por servicio) | Identificador del proceso |
| Memoria CLR de .NET (por servicio) | Número total de bytes confirmados |
| Memoria CLR de .NET (por servicio) | Número total de bytes reservados |
| Memoria CLR de .NET (por servicio) | Número de bytes en todos los montones |
| Memoria CLR de .NET (por servicio) | Tamaño del montón del objeto grande |
| Memoria CLR de .NET (por servicio) | Número de identificadores del GC |
| Memoria CLR de .NET (por servicio) | Número de colecciones de gen. 0 |
| Memoria CLR de .NET (por servicio) | Número de colecciones de gen. 1 |
| Memoria CLR de .NET (por servicio) | Número de colecciones de gen. 2 |
| Memoria CLR de .NET (por servicio) | % de tiempo del GC |

### <a name="service-fabrics-custom-performance-counters"></a>Contadores de rendimiento personalizados de Service Fabric

Service Fabric genera una cantidad considerable de contadores de rendimiento personalizados. Si tiene instalado el SDK, puede ver la lista completa en el equipo Windows, en la aplicación Monitor de rendimiento (Inicio > Monitor de rendimiento). 

En las aplicaciones que está implementando en el clúster, si usa Reliable Actors, agregue los contadores de las categorías `Service Fabric Actor` y `Service Fabric Actor Method` (consulte [Diagnóstico de Reliable Actors de Service Fabric](service-fabric-reliable-actors-diagnostics.md)).

Si usa Reliable Services o la comunicación remota de servicio, también tenemos las categorías de contadores `Service Fabric Service` y `Service Fabric Service Method` de las que debería recopilar los contadores de rendimiento. Consulte los temas de [supervisión con la comunicación remota de servicio](service-fabric-reliable-serviceremoting-diagnostics.md) y [contadores de rendimiento de Reliable Services](service-fabric-reliable-services-diagnostics.md#performance-counters). 

Si usa Reliable Collections, se recomienda agregar `Avg. Transaction ms/Commit` desde `Service Fabric Transactional Replicator` para recopilar la latencia promedio de confirmación por cada métrica de transacción.


## <a name="next-steps"></a>Pasos siguientes

* Aprenda más sobre la [generación de eventos en el nivel de plataforma ](service-fabric-diagnostics-event-generation-infra.md) en Service Fabric.
* Recopilar métricas de rendimiento mediante el [agente de Log Analytics](service-fabric-diagnostics-oms-agent.md)
