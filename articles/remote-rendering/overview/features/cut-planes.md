---
title: Corte de planos
description: Explica qué son los planos de corte y cómo usarlos.
author: jakrams
ms.author: jakras
ms.date: 02/06/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: 7d8905fbdcfc03f2683698cca57ab6c066e77863
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92205938"
---
# <a name="cut-planes"></a>Corte de planos

Un *plano de corte* es una característica visual que recorta los píxeles de un lado de un plano virtual y revela el interior de las [mallas](../../concepts/meshes.md).
En la imagen siguiente se muestra el efecto. La izquierda muestra la malla original, mientras que la derecha puede buscar dentro de la malla:

![Plano de corte](./media/cutplane-1.png)

## <a name="cutplanecomponent"></a>CutPlaneComponent

Para agregar un plano de corte a la escena, cree un *CutPlaneComponent*. La ubicación y la orientación del plano viene determinada por la [entidad](../../concepts/entities.md) del propietario del componente.

```cs
void CreateCutPlane(AzureSession session, Entity ownerEntity)
{
    CutPlaneComponent cutPlane = (CutPlaneComponent)session.Actions.CreateComponent(ObjectType.CutPlaneComponent, ownerEntity);
    cutPlane.Normal = Axis.X; // normal points along the positive x-axis of the owner object's orientation
    cutPlane.FadeColor = new Color4Ub(255, 0, 0, 128); // fade to 50% red
    cutPlane.FadeLength = 0.05f; // gradient width: 5cm
}
```

```cpp
void CreateCutPlane(ApiHandle<AzureSession> session, ApiHandle<Entity> ownerEntity)
{
    ApiHandle<CutPlaneComponent> cutPlane = session->Actions()->CreateComponent(ObjectType::CutPlaneComponent, ownerEntity)->as<CutPlaneComponent>();;
    cutPlane->SetNormal(Axis::X); // normal points along the positive x-axis of the owner object's orientation
    Color4Ub fadeColor;
    fadeColor.channels = { 255, 0, 0, 128 }; // fade to 50% red
    cutPlane->SetFadeColor(fadeColor);
    cutPlane->SetFadeLength(0.05f); // gradient width: 5cm
}
```

### <a name="cutplanecomponent-properties"></a>Propiedades de CutPlaneComponent

Las siguientes propiedades se exponen en un componente de plano de corte:

* `Enabled`: Puede desactivar temporalmente los planos de corte deshabilitando el componente. Los planos de corte deshabilitados no incurren en sobrecarga de representación y tampoco se cuentan para el límite de plano de corte global.

* `Normal`: especifica qué dirección (+X,-X,+Y,-Y,+Z,-Z) se usa como normal del plano. Esta dirección es relativa a la orientación de la entidad propietaria. Mueva y gire la entidad propietaria para obtener la ubicación exacta.

* `FadeColor` y `FadeLength`:

  Si el valor alfa de *FadeColor* es distinto de cero, los píxeles cercanos al plano de corte se atenuarán hacia la parte RGB de FadeColor. La fuerza del canal alfa determina si se desvanece completamente hacia el color de fundido o solo parcialmente. *FadeLength* define con qué distancia se llevará a cabo esta transición.

* `ObjectFilterMask`: Una máscara de bits de filtro que determina qué geometría se ve afectada por el plano de corte. Consulte el párrafo siguiente para obtener información detallada.

### <a name="selective-cut-planes"></a>Planos de corte selectivos

Es posible configurar planos de corte individuales para que solo afecten a una geometría específica. En la imagen siguiente se muestra cómo puede ser esta configuración en la práctica:

![Planos de corte selectivos](./media/selective-cut-planes.png)

El filtrado funciona a través de la **comparación lógica de máscaras de bits** entre una máscara de bits en el lado del plano de corte y una segunda máscara de bits que se establece en la geometría. Si el resultado de una operación `AND` lógica entre las máscaras no es cero, el plano de corte afectará a la geometría.

* La máscara de bits del componente de plano de corte se establece a través de su propiedad `ObjectFilterMask`.
* La máscara de bits en una subjerarquía de geometría se establece mediante [HierarchicalStateOverrideComponent](override-hierarchical-state.md#features).

Ejemplos:

| Máscara de filtro de plano de corte | Máscara de filtro de geometría  | Resultado del operador `AND` lógico | ¿Afecta el plano de corte a la geometría?  |
|--------------------|-------------------|-------------------|:----------------------------:|
| (0000 0001) == 1   | (0000 0001) == 1  | (0000 0001) == 1  | Sí |
| (1111 0000) == 240 | (0001 0001) == 17 | (0001 0000) == 16 | Sí |
| (0000 0001) == 1   | (0000 0010) == 2  | (0000 0000) == 0  | No |
| (0000 0011) == 3   | (0000 1000) == 8  | (0000 0000) == 0  | No |

>[!TIP]
> Establecer `ObjectFilterMask` de un plano de corte en 0 significa que no afectará a ninguna geometría porque el resultado del operador `AND` lógico nunca puede ser no NULL. El sistema de representación no considerará esos planos en primer lugar, por lo que se trata de un método ligero para deshabilitar los planos de corte individuales. Estos planos de corte tampoco cuentan para el límite de 8 planos activos.

## <a name="limitations"></a>Limitaciones

* Azure Remote Rendering admite un **máximo de ocho planos de corte activos**. Puede crear más componentes de plano de corte, pero, si intenta habilitar más simultáneamente, se omitirá la activación. Deshabilite primero otros planos si quiere cambiar qué componentes deben afectar a la escena.
* Los planos de corte son meramente una característica visual, no afectan al resultado de las [consultas espaciales](spatial-queries.md). Si quiere realizar un trazado de rayos en una malla de corte abierto, puede ajustar el punto inicial del rayo para que esté en el plano de corte. De este modo, el rayo solo puede llegar a los elementos visibles.

## <a name="performance-considerations"></a>Consideraciones de rendimiento

Cada plano de corte activo incurre en un pequeño costo durante la representación. Deshabilite o elimine los planos de corte cuando no sean necesarios.

## <a name="api-documentation"></a>Documentación de la API

* [Clase CutPlaneComponent de C#](/dotnet/api/microsoft.azure.remoterendering.cutplanecomponent)
* [Clase CutPlaneComponent de C++](/cpp/api/remote-rendering/cutplanecomponent)

## <a name="next-steps"></a>Pasos siguientes

* [Representación de un solo lado](single-sided-rendering.md)
* [Consultas espaciales](spatial-queries.md)