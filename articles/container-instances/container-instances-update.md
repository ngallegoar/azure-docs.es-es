---
title: Actualización de un grupo de contenedores
description: Aprenda a actualizar contenedores en ejecución en los grupos de contenedores de Azure Container Instances.
ms.topic: article
ms.date: 09/03/2019
ms.openlocfilehash: f57ebcf050b5563b45f10af57c1721338df88ff9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533298"
---
# <a name="update-containers-in-azure-container-instances"></a>Actualización de contenedores en Azure Container Instances

Durante el funcionamiento normal de las instancias de contenedor, puede que sea necesario actualizar los contenedores en ejecución de un [grupo de contenedores](container-instances-container-groups.md). Por ejemplo, podría querer actualizar la versión de la imagen, cambiar un nombre DNS, actualizar las variables de entorno o el estado de un contenedor cuya aplicación se ha bloqueado.

> [!NOTE]
> No se pueden actualizar los grupos de contenedores finalizados o eliminados. Una vez que un grupo de contenedores ha finalizado (se encuentra en un estado correcto o con errores) o se ha eliminado, el grupo se debe implementar como nuevo.

## <a name="update-a-container-group"></a>Actualización de un grupo de contenedores

Para actualizar los contenedores de un grupo de contenedores en ejecución, vuelva a implementar un grupo existente con al menos una propiedad modificada. Al actualizar un grupo de contenedores, todos los contenedores en ejecución en el grupo se reinician en contexto, por lo general en el mismo host de contenedor subyacente.

Para reimplementar un grupo de contenedores existente, emita el comando create (o use Azure Portal) y especifique el nombre de un grupo existente. Modifique al menos una propiedad válida del grupo cuando emita el comando CREATE para desencadenar la reimplementación y deje sin modificar las propiedades restantes (o siga usando los valores predeterminados). No todas las propiedades del grupo de contenedores son válidas para una reimplementación. Para ver una lista de propiedades no admitidas, consulte [Propiedades que requieren la eliminación del contenedor](#properties-that-require-container-delete).

En el siguiente ejemplo de la CLI de Azure se actualiza un grupo de contenedores con una nueva etiqueta de nombre DNS. Dado la propiedad de etiqueta del nombre DNS del grupo es una de las que se pueden actualizar, se vuelve a implementar el grupo de contenedores y se reinician sus contenedores.

Implementación inicial con la etiqueta de nombre DNS *myapplication-staging*:

```azurecli-interactive
# Create container group
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication-staging
```

Actualice el grupo de contenedores con una nueva etiqueta de nombre DNS, *myapplication*, y deje las propiedades restantes sin cambios:

```azurecli-interactive
# Update DNS name label (restarts container), leave other properties unchanged
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication
```

## <a name="update-benefits"></a>Ventajas de la actualización

La principal ventaja de actualizar un grupo de contenedores existente es una implementación más rápida. Al volver a implementar un grupo de contenedores existente, sus capas de imagen de contenedor se extraen de las almacenados en caché por la implementación anterior. En lugar de extraer todas las capas de imagen actualizadas del Registro como se hace con las nuevas implementaciones, solo se extraen las capas modificadas (si existen).

Las aplicaciones basadas en imágenes de contenedor más grandes, como Windows Server Core, pueden experimentar una considerable mejora en la velocidad de implementación al actualizarlas, en lugar de eliminarlas y volver a implementarlas.

## <a name="limitations"></a>Limitaciones

No todas las propiedades de un grupo de contenedores admiten actualizaciones. Para cambiar algunas propiedades de un grupo de contenedores, primero debe eliminar y volver a implementar el grupo. Para más información, consulte [Propiedades que requieren la eliminación del contenedor](#properties-that-require-container-delete).

Todos los contenedores de un grupo de contenedores se reinician al actualizar el grupo de contenedores. No se puede realizar una actualización o reinicio local de un contenedor específico en un grupo de varios contenedores.

La dirección IP de un contenedor no cambia normalmente entre actualizaciones, pero no se garantiza que permanezca igual. Mientras el grupo de contenedores esté implementado en el mismo host subyacente, conservará su dirección IP. Aunque es poco habitual, y Azure Container Instances hace todo lo posible para volver a realizar la implementación en el mismo host, algunos eventos internos de Azure pueden provocar la reimplementación en otro host. Para mitigar este problema, use siempre una etiqueta de nombre DNS para las instancias de contenedor.

No se pueden actualizar los grupos de contenedores finalizados o eliminados. Una vez que se ha detenido un grupo de contenedores (se encuentra en estado *Terminado* estado) o se ha eliminado, el grupo se implementa como nuevo.

## <a name="properties-that-require-container-delete"></a>Propiedades que requieren la eliminación del contenedor

Como se mencionó anteriormente, no todas las propiedades del grupo de contenedores se pueden actualizar. Por ejemplo, para cambiar los puertos o reiniciar la directiva de un contenedor, primero debe eliminar el grupo de contenedores y luego volver a crearlo.

Estas propiedades requieren la eliminación del grupo de contenedores antes de volver a implementarlo:

* Tipo de SO
* CPU
* Memoria
* Directiva de reinicio
* Puertos

Al eliminar un grupo de contenedores y volver a crearlo, no se reimplementa sino que se crea uno nuevo. Todas las capas de imagen se extraen actualizadas del Registro, y no de las almacenadas en caché por una implementación anterior. La dirección IP del contenedor también podría cambiar debido a que se implementa en un host subyacente diferente.

## <a name="next-steps"></a>Pasos siguientes

Se ha mencionado varias veces en este artículo el **grupo de contenedores**. Cada contenedor de Azure Container Instances se implementa en un grupo de contenedores y los grupos de contenedores pueden contener más de un contenedor.

[Grupos de contenedores en Azure Container Instances](container-instances-container-groups.md)

[Implementación de grupos de varios contenedores](container-instances-multi-container-group.md)

[Detener o iniciar contenedores manualmente en Azure Container Instances](container-instances-stop-start.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[azure-cli-install]: /cli/azure/install-azure-cli
