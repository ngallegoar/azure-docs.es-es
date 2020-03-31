---
title: Creación de una instancia de Azure HPC Cache
description: Creación de una instancia de Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 11/11/2019
ms.author: rohogue
ms.openlocfilehash: c6090d19ce530829b79dca69636c2123e2519961
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129566"
---
# <a name="create-an-azure-hpc-cache"></a>Creación de una instancia de Azure HPC Cache

Uso de Azure Portal para crear una memoria caché.

![captura de pantalla de información general de caché en Azure Portal, con el botón crear en la parte inferior](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Definición de los detalles básicos

![captura de pantalla de la página de detalles del proyecto en Azure Portal](media/hpc-cache-create-basics.png)

En **Detalles del proyecto**, seleccione la suscripción y el grupo de recursos que hospedará la caché. Asegúrese de que la suscripción se encuentre en la lista de [acceso](hpc-cache-prereqs.md#azure-subscription).

En **Detalles del servicio**, establezca el nombre de la memoria caché y estos otros atributos:

* Ubicación: seleccione alguna de las [regiones admitidas](hpc-cache-overview.md#region-availability).
* Red virtual: puede optar por crear una nueva red virtual o seleccionar una existente.
* Subred: elija o cree una subred con al menos 64 direcciones IP (/24) que solo se usarán para la instancia de Azure HPC Cache.

## <a name="set-cache-capacity"></a>Establecimiento de la capacidad de la memoria caché
<!-- referenced from GUI - update aka.ms link if you change this header text -->

En la página **Caché**, debe establecer la capacidad de la memoria caché. Estos valores determinan la cantidad de datos que puede contener la memoria caché y la rapidez con la que puede atender las solicitudes de cliente.

La capacidad también afecta al costo de la caché.

Para elegir la capacidad, establezca estos dos valores:

* La velocidad de transferencia de datos máxima para la memoria caché (rendimiento), en GB/segundo
* La cantidad de almacenamiento asignado a los datos en caché, en TB

Elija uno de los valores de rendimiento disponibles y los tamaños de almacenamiento en caché.

Tenga en cuenta que la velocidad de transferencia de datos real depende de la carga de trabajo, las velocidades de red y el tipo de destinos de almacenamiento. Los valores que elija establecen el rendimiento máximo para todo el sistema de caché, pero una parte se usa para tareas de sobrecarga. Por ejemplo, si un cliente solicita un archivo que aún no está almacenado en la memoria caché, o si el archivo está marcado como obsoleto, la memoria caché utiliza parte de su capacidad de proceso para recuperarlo del almacenamiento back-end.

Azure HPC Cache administra qué archivos se almacenan en caché y se cargan previamente para maximizar las tasas de aciertos de caché. El contenido de la caché se evalúa continuamente y los archivos se mueven al almacenamiento a largo plazo cuando se accede a ellos con menos frecuencia. Elija un tamaño de almacenamiento en caché que pueda contener el conjunto activo de archivos de trabajo con espacio adicional para los metadatos y otras sobrecargas.

![captura de pantalla de la página de tamaño de caché](media/hpc-cache-create-capacity.png)

## <a name="add-resource-tags-optional"></a>Incorporación de etiquetas de recursos (opcional)

La página **Etiquetas** permiten agregar [etiquetas de recursos](https://go.microsoft.com/fwlink/?linkid=873112) a Azure HPC Cache.

## <a name="finish-creating-the-cache"></a>Terminar de crear la memoria caché

Después de configurar la nueva memoria caché, haga clic en la pestaña **Revisar y crear**. El portal valida las selecciones y le permite revisar las opciones. Si todo es correcto, haga clic en **Crear**.

La creación de la memoria caché tarda unos 10 minutos. Puede realizar el seguimiento del progreso en las notificaciones de Azure Portal.

![Captura de pantalla de las páginas de "implementación en curso" y "notificaciones" de creación de la memoria caché en el portal](media/hpc-cache-deploy-status.png)

Cuando finaliza la creación, aparece una notificación con un vínculo a la nueva instancia de Azure HPC Cache y la memoria caché aparece en la lista **Recursos** de la suscripción.
<!-- double check on notification -->

![captura de pantalla de la instancia de Azure HPC Cache en Azure Portal](media/hpc-cache-new-overview.png)

## <a name="next-steps"></a>Pasos siguientes

Una vez que la memoria caché aparezca en la lista **Recursos**, defina los destinos de almacenamiento para dar a la memoria caché acceso a los orígenes de datos.

* [Incorporación de destinos de almacenamiento](hpc-cache-add-storage.md)
