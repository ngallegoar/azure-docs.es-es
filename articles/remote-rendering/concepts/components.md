---
title: Componentes
description: Definición de componentes en el ámbito de Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: a488e2499b92b290ad2b55120c3c70a18d45d426
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613939"
---
# <a name="components"></a>Componentes

Azure Remote Rendering usa el patrón del [sistema de componentes de entidades](https://en.wikipedia.org/wiki/Entity_component_system). Mientras que las [entidades](entities.md) representan la posición y la composición jerárquica de los objetos, los componentes son responsables de implementar el comportamiento.

Los tipos de componentes que se usan con más frecuencia son los [:::no-loc text="mesh components":::](meshes.md), que agregan mallas a la canalización de representación. Del mismo modo, los [componentes de iluminación](../overview/features/lights.md) se usan para agregar iluminación, y los [planos de corte](../overview/features/cut-planes.md) se emplean para cortar mallas abiertas.

Todos estos componentes usan la transformación (posición, rotación y escala) de la entidad a la que están asociados, como su punto de referencia.

## <a name="working-with-components"></a>Trabajo con componentes

Puede agregar, quitar y manipular fácilmente los componentes mediante programación:

```cs
// create a point light component
AzureSession session = GetCurrentlyConnectedSession();
PointLightComponent lightComponent = session.Actions.CreateComponent(ObjectType.PointLightComponent, ownerEntity) as PointLightComponent;

lightComponent.Color = new Color4Ub(255, 150, 20, 255);
lightComponent.Intensity = 11;

// ...

// destroy the component
lightComponent.Destroy();
lightComponent = null;
```

```cpp
// create a point light component
ApiHandle<AzureSession> session = GetCurrentlyConnectedSession();

ApiHandle<PointLightComponent> lightComponent = session->Actions()->CreateComponent(ObjectType::PointLightComponent, ownerEntity)->as<PointLightComponent>();

// ...

// destroy the component
lightComponent->Destroy();
lightComponent = nullptr;
```

Un componente se asocia a una entidad en el momento de su creación. Después, no se puede pasar a otra entidad. Los componentes se eliminan explícitamente con `Component.Destroy()` o de forma automática cuando se destruye la entidad propietaria del componente.

Solo se puede agregar una instancia de cada tipo de componente a una entidad a la vez.

## <a name="unity-specific"></a>Específicos de Unity

La integración de Unity tiene funciones de extensión adicionales para interactuar con los componentes. Consulte [Componentes y objetos de juegos de Unity](../how-tos/unity/objects-components.md).

## <a name="api-documentation"></a>Documentación de la API

* [ComponentBase de C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.componentbase)
* [RemoteManager.CreateComponent() de C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.createcomponent)
* [Entity.FindComponentOfType() de C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.entity.findcomponentoftype)
* [ComponentBase C++](https://docs.microsoft.com/cpp/api/remote-rendering/componentbase)
* [RemoteManager::CreateComponent() de C++](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#createcomponent)
* [Entity::FindComponentOfType() de C++](https://docs.microsoft.com/cpp/api/remote-rendering/entity#findcomponentoftype)

## <a name="next-steps"></a>Pasos siguientes

* [Límites de objetos](object-bounds.md)
* [Mallas](meshes.md)
