---
title: Enumeración de actores en Azure Service Fabric
description: Obtenga información sobre la enumeración de Reliable Actors y sus metadatos en una aplicación de Azure Service Fabric mediante ejemplos.
author: vturecek
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 1516c9005a7c4dd0adcb279e9954e5f882c575c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645606"
---
# <a name="enumerate-service-fabric-reliable-actors"></a>Enumerar Reliable Actors de Service Fabric
El servicio Reliable Actors permite que un cliente enumere los metadatos sobre los actores que el servicio hospeda. Debido a que el servicio de actor es un servicio con estado particionado, la enumeración se realiza por partición. Dado que cada partición puede contener muchos actores, la enumeración se devuelve como un conjunto de resultados paginados. Las páginas entran en bucle hasta que se leen todas. El siguiente ejemplo muestra cómo crear una lista de todos los actores activos en una partición de un servicio de actor:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```



## <a name="next-steps"></a>Pasos siguientes
* [Administración de estados de los actores](service-fabric-reliable-actors-state-management.md)
* [Ciclo de vida de un actor y recolección de elementos no utilizados](service-fabric-reliable-actors-lifecycle.md)
* [Documentación de referencia de la API de actores](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Código de ejemplo de .NET](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Código de ejemplo de Java](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
