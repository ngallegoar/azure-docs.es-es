---
title: Eventos de Azure Service Fabric
description: Obtenga información acerca de los eventos de Service Fabric que le proporcionarán ayuda para supervisar el clúster de Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 638b650e485ad3e83bd6021639a7e55b540d9cdc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451724"
---
# <a name="service-fabric-events"></a>Eventos de Service Fabric 

La plataforma Service Fabric escribe varios eventos estructurados para actividades operativas clave que se producen dentro del clúster. Estas van desde las actualizaciones del clúster hasta las decisiones de ubicación de réplicas. Cada evento que Service Fabric expone se asigna a una de las siguientes entidades en el clúster:
* Clúster
* Application
* Servicio
* Partition
* Réplica 
* Contenedor

Para ver una lista completa de los eventos que expone la plataforma, consulte: [eventos de la lista de Service Fabric](service-fabric-diagnostics-event-generation-operational.md).

Estos son algunos ejemplos de escenarios en los que puede ver los eventos para el clúster. 
* Eventos de ciclo de vida del nodo: a medida que los nodos ascienden, descienden, se reducen o escalan horizontalmente, se activan o desactivan, los eventos que se expondrán le mostrarán qué ha ocurrido, y le ayudarán a identificar si hay algún problema con la propia máquina o si existe una API que se llamó a través de SF para modificar el estado de un nodo.
* Actualización del clúster: cuando el clúster se actualiza (la versión o la configuración de SF cambiará), puede comprobar cómo se inicia la actualización, avanza por cada uno de los dominios de actualización y se completa (o revierte). 
* Actualizaciones de aplicaciones: igual que las actualizaciones de clúster, hay un conjunto completo de eventos a medida que avanza la actualización. Estos eventos pueden ser útiles para entender el momento en que se programa una actualización, el estado actual de la misma y la secuencia general de eventos. Esto es útil para ver qué actualizaciones se han realizado correctamente o si se ha desencadenado una reversión.
* Implementar o eliminar una aplicación o servicio: hay eventos para cada aplicación, servicio y contenedor que se vaya a crear o eliminar. Resultan útiles para reducir o escalar horizontalmente, por ejemplo, para aumentar el número de réplicas.
* Movimientos de partición (reconfiguración): cada vez que se lleva a cabo una reconfiguración (un cambio en el conjunto de réplicas) en una partición con estado, se registra un evento. Esto es útil si desea saber cada cuánto cambia o se conmuta por error el conjunto de réplicas de la partición, o si quiere realizar un seguimiento para saber en cualquier momento qué nodo estaba ejecutando la réplica principal.
* Eventos de caos: al usar el servicio [Chaos](service-fabric-controlled-chaos.md) de Service Fabric, verá los eventos cada vez que se inicie o detenga el servicio, o cuando inserte un error en el sistema.
* Eventos de mantenimiento: Service Fabric expone eventos de mantenimiento cada vez que se crea un informe de mantenimiento de advertencia o error, si una entidad vuelve a un estado correcto o si un informe de mantenimiento expira. Estos eventos son muy útiles para realizar el seguimiento de las estadísticas de mantenimiento históricas de una entidad. 

## <a name="how-to-access-events"></a>Cómo obtener acceso a eventos

Hay varias maneras mediante las cuales se puede obtener acceso a eventos de Service Fabric:
* Los eventos se registran a través de canales estándar, como registros de eventos de ETW/Windows, y se pueden visualizar con cualquier herramienta de supervisión compatible, como los registros de Azure Monitor. De forma predeterminada, los clústeres creados en el portal tienen activados los diagnósticos y tienen el agente de diagnósticos de Microsoft Azure que envía los eventos a Azure Table Storage, pero aun así deberá integrarlos con el recurso de análisis de registros. Obtenga más información sobre la configuración del [agente de Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) para modificar la configuración de diagnósticos del clúster de modo que seleccione más registros o contadores de rendimiento y sobre la [integración de los registros de Azure Monitor](service-fabric-diagnostics-event-analysis-oms.md).
* Las API de REST del servicio EventStore que le permiten consultar el clúster directamente o a través de la biblioteca cliente de Service Fabric. Consulte [Query EventStore APIs for cluster events](service-fabric-diagnostics-eventstore-query.md) (Consultar las API del servicio EventStore para obtener eventos de clúster).

## <a name="next-steps"></a>Pasos siguientes
* Obtener más información acerca de cómo supervisar el clúster: [Supervisión del clúster y la plataforma](service-fabric-diagnostics-event-generation-infra.md).
* Obtener más información sobre el servicio EventStore: - [EventStore service overview](service-fabric-diagnostics-eventstore.md) (Información general sobre el servicio EventStore).
