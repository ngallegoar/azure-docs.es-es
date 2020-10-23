---
title: Almacén de eventos de Azure Service Fabric
description: Más información acerca de EventStore de Azure Service Fabric, una forma de entender y supervisar el estado de un clúster o de cargas de trabajo en cualquier momento.
author: srrengar
ms.topic: conceptual
ms.date: 6/6/2019
ms.author: srrengar
ms.openlocfilehash: 0b6258e160794eaf7d0c05775f8fd3b796e3ba47
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91354826"
---
# <a name="eventstore-overview"></a>Introducción a EventStore

>[!NOTE]
>A partir de la versión 6.4 de Service Fabric, las API de EventStore solo se encuentran disponibles para los clústeres Windows que se ejecutan en Azure. Se está trabajando para portar esta funcionalidad a Linux, así como nuestros clústeres independientes.

## <a name="overview"></a>Información general

El servicio EventStore, que se introdujo en la versión 6.2, es una opción de supervisión de Service Fabric. EventStore proporciona una manera de comprender el estado del clúster o las cargas de trabajo en un momento dado.
EventStore es un servicio de Service Fabric con estado que mantiene los eventos del clúster. El evento se expone a través de las API, de REST y de Service Fabric Explorer. EventStore consulta el clúster directamente para obtener datos de diagnóstico sobre cualquier entidad del clúster y debe usarse como ayuda para lo siguiente:

* Diagnosticar problemas en el desarrollo, prueba o allí donde use una canalización de supervisión
* Confirmar que el clúster procesa correctamente las acciones de administración que realiza
* Obtener una "instantánea" de cómo Service Fabric está interactuando con una entidad determinada

![Captura de pantalla que muestra la pestaña EVENTOS con los diversos eventos del panel Nodos, incluido un evento NodeDown.](media/service-fabric-diagnostics-eventstore/eventstore.png)

Para ver una lista completa de los eventos disponibles en EventStore, consulte [Eventos de Service Fabric](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>A partir de la versión 6.4 de Service Fabric, Las API y la experiencia de usuario de EventStore están disponibles con carácter general para los clústeres de Windows de Azure. Se está trabajando para portar esta funcionalidad a Linux, así como nuestros clústeres independientes.

Se puede consultar el servicio EventStore sobre los eventos que están disponibles para cada entidad y tipo de entidad del clúster. Esto significa que puede consultar los eventos en los siguientes niveles:
* Clúster: eventos específicos del clúster (por ejemplo, la actualización del clúster)
* Nodos: eventos de todos los niveles de nodo
* Nodo: eventos específicos de un nodo que se identifica por `nodeName`
* Aplicaciones: eventos de todos los niveles de aplicación
* Aplicación: eventos específicos de una aplicación que se identifica por `applicationId`
* Servicios: eventos de todos los servicios de los clústeres
* Servicio: eventos de un servicio específico que se identifica por `serviceId`
* Particiones: eventos de todas las particiones
* Partición: eventos de una partición específica que se identifica por `partitionId`
* Réplicas de partición: eventos de todas las réplicas o instancias dentro de una partición específica que se identifica por `partitionId`
* Réplica de partición: eventos de una réplica o instancia específica que se identifica por `replicaId` y `partitionId`

Para más información sobre la API, consulte [Referencia de EventStore API](/rest/api/servicefabric/sfclient-index-eventsstore).

El servicio EventStore también tiene la capacidad de correlacionar los eventos del clúster. El examen de los eventos que se escribieron al mismo tiempo desde distintas entidades y que pueden haberse afectado entre sí permite al servicio EventStore vincular estos eventos para identificar las causas de las actividades del clúster. Por ejemplo, si una de las aplicaciones pasa a un estado incorrecto sin realizar ningún cambio inducido, EventStore también examinará otros eventos expuestos por la plataforma y podría correlacionarlos con un evento `Error` o `Warning`. Esto ayuda a detectar los errores y analizar las causas raíz más rápidamente.

## <a name="enable-eventstore-on-your-cluster"></a>Habilitación de EventStore en el clúster

### <a name="local-cluster"></a>Clúster local

En el archivo [fabricSettings.json del clúster](service-fabric-cluster-fabric-settings.md), agregue EventStoreService como una característica complementaria y realice una actualización del clúster.

```json
    "addOnFeatures": [
        "EventStoreService"
    ],
```

### <a name="azure-cluster-version-65"></a>Versión 6.5+ del clúster de Azure
Si el clúster de Azure se actualiza a la versión 6.5 o versiones posterior, EventStore se habilitará automáticamente en el clúster. Para no participar, tendrá que actualizar la plantilla del clúster con lo siguiente:

* Usar una versión de API de `2019-03-01` o posterior 
* Agregue el código siguiente a la sección de propiedades en el clúster
  ```json  
    "fabricSettings": [
      …
    ],
    "eventStoreServiceEnabled": false
  ```

### <a name="azure-cluster-version-64"></a>Versión 6.4 del clúster de Azure

Si usa la versión 6.4, puede editar la plantilla de Azure Resource Manager para activar el servicio EventStore. Para ello tiene que realizar una [actualización de configuración del clúster](service-fabric-cluster-config-upgrade-azure.md) y agregar el siguiente código, puede usar PlacementConstraints para colocar las réplicas del servicio EventStore en un NodeType específico, por ejemplo, un NodeType dedicado para los servicios del sistema. La sección `upgradeDescription` configura la actualización de la configuración para desencadenar un reinicio en los nodos. Puede quitar la sección en otra actualización.

```json
    "fabricSettings": [
          …
          …
          …,
         {
            "name": "EventStoreService",
            "parameters": [
              {
                "name": "TargetReplicaSetSize",
                "value": "3"
              },
              {
                "name": "MinReplicaSetSize",
                "value": "1"
              },
              {
                "name": "PlacementConstraints",
                "value": "(NodeType==<node_type_name_here>)"
              }
            ]
          }
        ],
        "upgradeDescription": {
          "forceRestart": true,
          "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807",
          "healthCheckWaitDuration": "00:01:00",
          "healthCheckStableDuration": "00:01:00",
          "healthCheckRetryTimeout": "00:5:00",
          "upgradeTimeout": "1:00:00",
          "upgradeDomainTimeout": "00:10:00",
          "healthPolicy": {
            "maxPercentUnhealthyNodes": 100,
            "maxPercentUnhealthyApplications": 100
          },
          "deltaHealthPolicy": {
            "maxPercentDeltaUnhealthyNodes": 0,
            "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0,
            "maxPercentDeltaUnhealthyApplications": 0
          }
        }
```


## <a name="next-steps"></a>Pasos siguientes
* Introducción a la API de EventStore: [artículo sobre el uso de las API de EventStore en los clústeres de Azure Service Fabric](service-fabric-diagnostics-eventstore-query.md)
* Más información sobre la lista de eventos que ofrece EventStore: [artículo sobre los eventos de Service Fabric](service-fabric-diagnostics-event-generation-operational.md)
* Información general de supervisión y diagnóstico de Service Fabric: [Supervisión y diagnóstico para Azure Service Fabric](service-fabric-diagnostics-overview.md)
* Lista completa de las llamadas de API: [referencia de la API de REST de EventStore](/rest/api/servicefabric/sfclient-index-eventsstore)
* Más información sobre cómo supervisar el clúster: [Supervisión del clúster y la plataforma](service-fabric-diagnostics-event-generation-infra.md).
