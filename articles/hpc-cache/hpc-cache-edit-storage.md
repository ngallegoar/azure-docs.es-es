---
title: Actualización de los destinos de almacenamiento en Azure HPC Cache
description: Procedimientos para editar los destinos de almacenamiento en Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 1/08/2020
ms.author: rohogue
ms.openlocfilehash: 5635bfc6ea5faea41b125037c76c0b8635e0f528
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75867002"
---
# <a name="edit-storage-targets"></a>Edición de los destinos de almacenamiento

Puede quitar o modificar un destino de almacenamiento desde la página **Destinos de almacenamiento** de la caché.

## <a name="remove-a-storage-target"></a>Eliminación de un destino de almacenamiento

Para quitar un destino de almacenamiento, selecciónelo en la lista y haga clic en el botón **Eliminar**.

Esta acción quita la asociación del destino de almacenamiento con este sistema de Azure HPC Cache, pero no cambia el sistema de almacenamiento de back-end. Por ejemplo, si ha usado un contenedor Azure Blob Storage, el contenedor y su contenido siguen existiendo después de eliminarlo de la memoria caché. Puede agregar el contenedor a otra instancia de Azure HPC Cache, volver a agregarlo a esta caché o eliminarlo con Azure Portal.

Los cambios de archivo almacenados en la caché se escriben en el sistema de almacenamiento de back-end antes de que se quite el destino de almacenamiento. Este proceso puede tardar una hora o más si hay muchos datos cambiados en la caché.

## <a name="update-storage-targets"></a>Actualización de destinos de almacenamiento

Se pueden editar los destinos de almacenamiento para modificar algunas de sus propiedades. Para cada tipo de almacenamiento se pueden modificar distintas propiedades:

* En el caso de los destinos de almacenamiento de blob, se puede cambiar la ruta de acceso del espacio de nombres.

* En el caso de los destinos de almacenamiento de NFS, se pueden cambiar estas propiedades:

  * Ruta de acceso del espacio de nombres
  * Modelo de uso
  * Exportación
  * Subdirectorio de exportación

No se puede editar el nombre, el tipo o el sistema de almacenamiento de back-end de un destino de almacenamiento (contenedor de blobs, o dirección IP o nombre de host de NFS). Si tiene que cambiar estas propiedades, elimine el destino de almacenamiento y cree un reemplazo con el valor nuevo.

Para modificar un destino de almacenamiento, haga clic en el nombre del destino de almacenamiento con el fin de abrir su página de detalles. Algunos campos de la página son editables.

![captura de pantalla de la página de edición de un destino de almacenamiento de NFS](media/hpc-cache-edit-storage-nfs.png)

## <a name="update-an-nfs-storage-target"></a>Actualización de un destino de almacenamiento de NFS

En el caso de un destino de almacenamiento de NFS, se pueden actualizar varias propiedades. (Consulte la captura de pantalla anterior para obtener una página de edición de ejemplo).

* **Modelo de uso**: el modelo de uso influye en el modo en que la memoria caché conserva los datos. Lea [Elección de un modelo de uso](hpc-cache-add-storage.md#choose-a-usage-model) para obtener más información.
* **Ruta de acceso del espacio de nombres virtual**: ruta de acceso que los clientes usan para montar este destino de almacenamiento. Lea [Planeamiento del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más detalles.
* **Ruta de acceso de exportación de NFS**: exportación del sistema de almacenamiento que se va a usar para esta ruta de acceso del espacio de nombres.
* **Ruta de acceso del subdirectorio**: subdirectorio (en la exportación) que se va a asociar a esta ruta de acceso del espacio de nombres. Deje este campo en blanco si no necesita especificar un subdirectorio.

Cada ruta de acceso del espacio de nombres necesita una combinación única de exportación y subdirectorio. Es decir, no se pueden crear dos rutas de acceso de cliente distintas al mismo directorio en el sistema de almacenamiento de back-end.

Después de realizar los cambios, haga clic en **Aceptar** para actualizar el destino de almacenamiento o en **Cancelar** para descartar los cambios.

## <a name="update-an-azure-blob-storage-target"></a>Actualización de un destino de almacenamiento de blobs de Azure

La página de detalles de un destino de almacenamiento de blob permite modificar la ruta de acceso del espacio de nombres virtual.

![captura de pantalla de la página de edición de un destino de almacenamiento de blob](media/hpc-cache-edit-storage-blob.png)

Cuando se haya terminado, haga clic en **Aceptar** para actualizar el destino de almacenamiento o en **Cancelar** para descartar los cambios.

## <a name="next-steps"></a>Pasos siguientes

* Lea [Incorporación de destinos de almacenamiento](hpc-cache-add-storage.md) para obtener más información sobre estas opciones.
* Lea [Planeamiento del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más recomendaciones sobre el uso de rutas de acceso virtuales.
