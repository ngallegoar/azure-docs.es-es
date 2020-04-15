---
title: Mallas
description: Definición de mallas en el ámbito de Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: b800272a538243d39a89e4eed64bc5cbc2d53ad8
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679378"
---
# <a name="meshes"></a>Mallas

## <a name="mesh-resource"></a>Recurso de malla

Las mallas son un [recurso compartido](../concepts/lifetime.md) inmutable, que solo se puede crear a través de la [conversión de datos](../how-tos/conversion/model-conversion.md). Las mallas contienen una o varias *submallas*. Cada submalla hace referencia a un [material](materials.md) con el que se debe representar de forma predeterminada. Para colocar una malla en un espacio 3D, agregue un componente [MeshComponent](#meshcomponent) a una [entidad](entities.md).

### <a name="mesh-resource-properties"></a>Propiedades del recurso de malla

Las propiedades de clase `Mesh` son:

* **Materiales:** una matriz de materiales. Cada material se usa por medio de una submalla diferente. Varias entradas de la matriz pueden hacer referencia al mismo [material](materials.md). Estos datos no pueden modificarse en tiempo de ejecución.

* **Límites:** un rectángulo de selección alineado con el eje (AABB) del espacio local de los vértices de malla.

## <a name="meshcomponent"></a>MeshComponent

La clase `MeshComponent` se usa para colocar una instancia de un recurso de malla. Cada MeshComponent hace referencia a una sola malla. Puede invalidar los materiales que se usan para representar cada submalla.

### <a name="meshcomponent-properties"></a>Propiedades MeshComponent

* **Malla:** el recurso de malla utilizado por este componente.

* **Materiales:** la matriz de materiales especificados en el propio componente de malla. La matriz siempre tendrá la misma longitud que la matriz de *Materiales* en el recurso de malla. Los materiales que no se van a invalidar a partir del valor predeterminado de la malla se establecen en *NULL* en esta matriz.

* **UsedMaterials:** la matriz de materiales usados realmente para cada submalla. Serán idénticos a los datos de la matriz de *Materiales*, para valores que no son NULL. En caso contrario, contiene el valor de la matriz de *Materiales* en la instancia de malla.

## <a name="next-steps"></a>Pasos siguientes

* [Materiales](materials.md)
