---
title: Modelos de ejemplo
description: Muestra los orígenes de los modelos de ejemplo.
author: florianborn71
ms.author: flborn
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: 0c28d36c7934fbbac0ddd11562c8dc237b0360ee
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80678496"
---
# <a name="sample-models"></a>Modelos de ejemplo

En este artículo se enumeran algunos recursos de datos de ejemplo que se pueden usar para probar el servicio Azure Remote Rendering.

## <a name="built-in-sample-model"></a>Modelo de ejemplo integrado

Proporcionamos un modelo de ejemplo integrado que se puede cargar en cualquier momento con la dirección URL **builtin://Engine**

![Modelo de ejemplo](./media/sample-model.png "Modelo de ejemplo")

Estadísticas del modelo:

| Nombre | Value |
|-----------|:-----------|
| [Tamaño de máquina virtual requerido](../how-tos/session-rest-api.md#create-a-session) | estándar |
| Número de triángulos | 18,7 millones |
| Número de piezas móviles | 2073 |
| Número de materiales | 94 |

## <a name="third-party-data"></a>Datos de terceros

El Grupo Khronos mantiene un conjunto de modelos de ejemplo glTF para pruebas. ARR admite el formato glTF en texto ( *.gltf*) y en formato binario ( *.glb*). Se recomienda usar los modelos PBR para obtener los mejores resultados visuales:

* [Modelos de ejemplo glTF](https://github.com/KhronosGroup/glTF-Sample-Models)

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Representación de un modelo con Unity](../quickstarts/render-model.md)
* [Inicio rápido: Conversión de un modelo para su representación](../quickstarts/convert-model.md)
