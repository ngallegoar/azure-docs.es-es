---
title: Invalidación de estado jerárquico
description: Explica el concepto de componentes de invalidación de estado jerárquico.
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: 851a87885ac765c829e8c2be9fd1205e22906ca9
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445161"
---
# <a name="hierarchical-state-override"></a>Invalidación de estado jerárquico

En muchos casos, es necesario cambiar de forma dinámica el aspecto de partes de un [modelo](../../concepts/models.md), por ejemplo, ocultar subgráficos o cambiar partes a una representación transparente. Cambiar los materiales de cada parte implicada no es práctico, ya que requiere recorrer en iteración todo el gráfico de la escena y administrar la clonación y asignación de materiales en cada nodo.

Para completar este caso de uso con la menor sobrecarga posible, use `HierarchicalStateOverrideComponent`. Este componente implementa las actualizaciones de estado jerárquico en ramas arbitrarias del gráfico de escena. Esto significa que un estado se puede definir en cualquier nivel del gráfico de escena y se filtra en la jerarquía hasta que se reemplaza por un estado nuevo o se aplica a un objeto hoja.

Como ejemplo, considere el modelo de un automóvil y que quiere cambiar el vehículo completo para que sea transparente, excepto la parte del motor interno. Este caso de uso solo implica dos instancias del componente:

* El primer componente se asigna al nodo raíz del modelo y activa la representación transparente para todo el automóvil.
* El segundo componente se asigna al nodo raíz del motor y reemplaza el estado de nuevo al desactivar explícitamente el modo transparente.

## <a name="features"></a>Características

El conjunto fijo de estados que se pueden invalidar es:

* **`Hidden`** : Las mallas respectivas del gráfico de escena se ocultan o se muestran.
* **`Tint color`** : un objeto representado se puede teñir con su intensidad y color de tono individual. En la imagen siguiente se muestra el tinte de color del borde de una rueda.
  
  ![Color de tinte usado para convertir un objeto en verde](./media/color-tint.png)

* **`See-through`** : la geometría se representa de modo semitransparente; por ejemplo, para mostrar las partes internas de un objeto. En la imagen siguiente se muestra el coche completo que se representa en el modo transparente, excepto la mordaza de freno roja:

  ![Modo transparente usado para convertir en transparentes los objetos seleccionados](./media/see-through.png)

  > [!IMPORTANT]
  > El efecto transparente solo funciona cuando se usa el [modo de representación](../../concepts/rendering-modes.md) *TileBasedComposition*.

* **`Shell`** : La geometría se representa como un shell transparente y desaturado. Este modo permite atenuar las partes no importantes de una escena mientras mantiene una sensación de forma y posicionamiento relativo. Para cambiar la apariencia de la representación del shell, use el estado [ShellRenderingSettings](shell-effect.md). Vea la imagen siguiente del modelo de automóvil representado completamente en el shell, excepto los amortiguadores azules:

  ![Modo de shell usado para atenuar objetos específicos](./media/shell.png)

  > [!IMPORTANT]
  > El efecto de shell solo funciona cuando se usa el [modo de representación](../../concepts/rendering-modes.md) *TileBasedComposition*.

* **`Selected`** : la geometría se representa con un [contorno de selección](outlines.md).

  ![Opción de contorno usada para resaltar un elemento seleccionado](./media/selection-outline.png)

* **`DisableCollision`** : la geometría está exenta de [consultas espaciales](spatial-queries.md). La marca **`Hidden`** no afecta a la marca del estado de las colisiones, por lo que estas dos marcas se suelen establecer juntas.

* **`UseCutPlaneFilterMask`** : use una máscara de bits de filtro individual para controlar la selección del plano de corte. Esta marca determina si se debe usar la máscara de filtro individual o si se hereda de su elemento primario. La máscara de bits de filtro se establece mediante la propiedad `CutPlaneFilterMask`. Para obtener información detallada sobre cómo funciona el filtrado, vea el párrafo [Planos de corte selectivos](cut-planes.md#selective-cut-planes). En el ejemplo siguiente solo están cortados el neumático y la llanta, mientras que el resto de la escena permanece intacta.
![Planos de corte selectivos](./media/selective-cut-planes-hierarchical-override.png)


> [!TIP]
> Como alternativa a desactivar las consultas espaciales y de visibilidad de un subgráfico completo, se puede alternar el estado `enabled` de un objeto de juego. Si una jerarquía está deshabilitada, esto tiene preferencia sobre cualquier `HierarchicalStateOverrideComponent`.

## <a name="hierarchical-overrides"></a>Invalidaciones jerárquicas

`HierarchicalStateOverrideComponent` se puede adjuntar en varios niveles de una jerarquía de objetos. Puesto que solo puede haber un componente de cada tipo en una entidad, cada objeto `HierarchicalStateOverrideComponent` administra los estados oculto, transparente, selección, tono de color y colisión.

Por lo tanto, cada estado se puede establecer como:

* `ForceOn`: el estado está habilitado para todas las mallas de este nodo y niveles inferiores.
* `ForceOff`: el estado está deshabilitado para todas las mallas de este nodo y niveles inferiores.
* `InheritFromParent`: el estado no se ve afectado por este componente de invalidación.

Puede cambiar los estados directamente o mediante la función `SetState`:

```cs
HierarchicalStateOverrideComponent component = ...;

// set one state directly
component.HiddenState = HierarchicalEnableState.ForceOn;

// set a state with the SetState function
component.SetState(HierarchicalStates.SeeThrough, HierarchicalEnableState.InheritFromParent);

// set multiple states at once with the SetState function
component.SetState(HierarchicalStates.Hidden | HierarchicalStates.DisableCollision, HierarchicalEnableState.ForceOff);
```

```cpp
ApiHandle<HierarchicalStateOverrideComponent> component = ...;

// set one state directly
component->SetHiddenState(HierarchicalEnableState::ForceOn);

// or: set a state with the SetState function
component->SetState(HierarchicalStates::SeeThrough, HierarchicalEnableState::InheritFromParent);

// set multiple states at once with the SetState function
component->SetState(
    (HierarchicalStates)((int32_t)HierarchicalStates::Hidden | (int32_t)HierarchicalStates::DisableCollision), HierarchicalEnableState::ForceOff);

```

### <a name="tint-color"></a>Color de tono

La invalidación de `tint color` es ligeramente especial, ya que hay tanto un estado activado/desactivado/heredado como una propiedad de color de tono. La parte alfa del color de tono define la intensidad del efecto de tono: Si se establece en 0,0, no hay ningún color de tono visible y, si se establece en 1,0, el objeto se representará con un color de tono puro. En el caso de los valores intermedios, el color final se combinará con el color del tono. El color del tono se puede cambiar en cada fotograma para lograr una animación de color.

## <a name="performance-considerations"></a>Consideraciones de rendimiento

Una instancia de `HierarchicalStateOverrideComponent` no agrega mucha sobrecarga en tiempo de ejecución. Sin embargo, siempre es recomendable mantener un número bajo de componentes activos. Por ejemplo, al implementar un sistema de selección que resalta el objeto seleccionado, se recomienda eliminar el componente al quitar el resaltado. El mantenimiento de los componentes en torno a características neutras puede sumar rápidamente.

La representación transparente coloca más carga de trabajo en las GPU del servidor que la representación estándar. Si las partes grandes del gráfico de escena cambian a *transparente*, con muchas capas de geometría visibles, puede convertirse en un cuello de botella de rendimiento. Todo esto se puede aplicar a los objetos con [contornos de selección](../../overview/features/outlines.md#performance) y para la [representación del shell](../../overview/features/shell-effect.md#performance) . 

## <a name="api-documentation"></a>Documentación de la API

* [Clase HierarchicalStateOverrideComponent de C#](/dotnet/api/microsoft.azure.remoterendering.hierarchicalstateoverridecomponent)
* [Clase HierarchicalStateOverrideComponent de C++](/cpp/api/remote-rendering/hierarchicalstateoverridecomponent)

## <a name="next-steps"></a>Pasos siguientes

* [Contornos](../../overview/features/outlines.md)
* [Modos de representación](../../concepts/rendering-modes.md)
* [Consultas espaciales](../../overview/features/spatial-queries.md)