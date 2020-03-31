---
title: Incorporación de almacenamiento a una instancia de Azure HPC Cache
description: Definición de los destinos de almacenamiento para que Azure HPC Cache pueda usar el sistema NFS local o los contenedores de Azure Blob Storage para el almacenamiento de archivos a largo plazo
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: rohogue
ms.openlocfilehash: a68bf06bad995f71bedf6a5bdedcb676737a8c61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233444"
---
# <a name="add-storage-targets"></a>Incorporación de destinos de almacenamiento

Los *destinos de almacenamiento* son espacios de almacenamiento en servidores back-end para archivos a los que se accede a través de una instancia de Azure HPC Cache. Puede agregar almacenamiento NFS (como un sistema de hardware local) o almacenar datos en Azure Blob Storage.

Puede definir hasta diez destinos de almacenamiento diferentes para una caché. La caché presenta todos los destinos de almacenamiento en un espacio de nombres agregado.

Recuerde que las exportaciones de almacenamiento deben ser accesibles desde la red virtual de la caché. En el caso del almacenamiento en hardware local, es posible que tenga que configurar un servidor DNS que pueda resolver nombres de host para el acceso al almacenamiento de NFS. Obtenga más información en [Acceso DNS](hpc-cache-prereqs.md#dns-access).

Agregue destinos de almacenamiento después de crear su caché. El procedimiento es ligeramente diferente en función de si agrega almacenamiento de Azure Blob Storage o una exportación de NFS. A continuación se muestran los detalles de cada uno.

## <a name="open-the-storage-targets-page"></a>Abra la página de destinos de almacenamiento

En Azure Portal, abra la instancia de caché y haga clic en **Destinos de almacenamiento** en la barra lateral izquierda. En la página Destinos de almacenamiento se enumeran todos los destinos existentes y se proporciona un vínculo para agregar uno nuevo.

![captura de pantalla del vínculo de destinos de almacenamiento en la barra lateral, en el encabezado Configurar, que se encuentra entre los encabezados de categoría Configuración y Supervisión](media/hpc-cache-storage-targets-sidebar.png)

## <a name="add-a-new-azure-blob-storage-target"></a>Incorporación de un nuevo destino de almacenamiento de Azure Blob Storage

Un nuevo destino de almacenamiento de Azure Blob Storage necesita un contenedor de blobs vacío o un contenedor rellenado con datos con el formato de sistema de archivos en la nube de Azure HPC Cache. Obtenga más información sobre la carga previa de un contenedor de blobs en [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md).

Puede crear un contenedor desde esta página justo antes de agregarlo.

Para definir un contenedor de blobs de Azure, escriba esta información.

![captura de pantalla de la página de incorporación de destino de almacenamiento, rellenada con información para un nuevo destino de almacenamiento de Azure Blob Storage](media/hpc-cache-add-blob.png)

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.
* **Target type** (Tipo de destino): elija **Blob**.
* **Cuenta de almacenamiento**: seleccione la cuenta que quiere usar.

  Tendrá que autorizar a la instancia de caché para acceder a la cuenta de almacenamiento, tal como se describe en [Incorporación de los roles de acceso](#add-the-access-control-roles-to-your-account).

  Para más información sobre el tipo de cuenta de almacenamiento que puede usar, consulte [Requisitos de Blob Storage](hpc-cache-prereqs.md#blob-storage-requirements).

* **Contenedor de almacenamiento**: seleccione el contenedor de blobs para este destino o haga clic en **Crear**.

  ![Captura de pantalla del cuadro de diálogo para especificar el nombre y el nivel de acceso (privado) del nuevo contenedor](media/add-blob-new-container.png)

* **Virtual namespace path** (Ruta de acceso del espacio de nombres virtual): establezca la ruta de archivo orientada al cliente para este destino de almacenamiento. Lea [Configuración del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más información acerca de la característica de espacio de nombres virtual.

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

> [!NOTE]
> Si el firewall de la cuenta de almacenamiento está configurado para restringir el acceso solo a "redes seleccionadas", use la solución alternativa documentada en [Solución alternativa para la configuración del firewall de la cuenta de Blob Storage](hpc-cache-blob-firewall-fix.md).

### <a name="add-the-access-control-roles-to-your-account"></a>Incorporación de los roles de control de acceso a la cuenta

Azure HPC Cache usa el [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/index) para autorizar el acceso del servicio de caché a la cuenta de almacenamiento de los destinos de Azure Blob Storage.

El propietario de la cuenta de almacenamiento debe agregar explícitamente los roles [Colaborador de la cuenta de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor) y [Colaborador de datos de Storage Blob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) para el usuario "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache).

Puede realizar esta tarea con anterioridad, o bien hacer clic en un vínculo de la página donde se agrega un destino de almacenamiento de Azure Blob Storage. Tenga en cuenta que la configuración de roles puede tardar hasta cinco minutos en propagarse a través del entorno de Azure, por lo que debe esperar unos minutos después de agregar los roles antes de crear un destino de almacenamiento.

Pasos para agregar los roles de RBAC:

1. Abra la página **Control de acceso (IAM)** correspondiente a la cuenta de almacenamiento. El vínculo de la página **Add storage target** (Agregar destino de almacenamiento) abre automáticamente esta página para la cuenta seleccionada.

1. Haga clic en **+** en la parte superior de la página y elija **Agregar una asignación de roles**.

1. Seleccione el rol "Colaborador de la cuenta de almacenamiento" en la lista.

1. En el campo **Asignar acceso a**, deje el valor predeterminado seleccionado ("Usuario, grupo o entidad de servicio de Azure AD").  

1. En el campo **Seleccionar**, busque "hpc".  Esta cadena debe coincidir con una entidad de servicio, denominada "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache). Haga clic en ella para seleccionarla.

   > [!NOTE]
   > Si una búsqueda de "hpc" no funciona, pruebe a usar la cadena "storagecache" en su lugar. Es posible que los usuarios que se han unido a las versiones preliminares (antes de la disponibilidad general) necesiten usar el nombre anterior de la entidad de servicio.

1. Haga clic en el botón **Guardar** en la parte inferior.

1. Repita este proceso para asignar el rol "Colaborador de datos de Storage Blob".  

![captura de pantalla de la GUI de Agregar asignación de roles](media/hpc-cache-add-role.png)

## <a name="add-a-new-nfs-storage-target"></a>Incorporación de un nuevo destino de almacenamiento NFS

Un destino de almacenamiento NFS tiene más campos que el destino de almacenamiento de blobs. Estos campos especifican cómo se llega a la exportación del almacenamiento y cómo se almacenan en caché sus datos de forma eficaz. Asimismo, un destino de almacenamiento NFS le permite crear varias rutas de acceso de espacio de nombres si el host NFS dispone de más de una exportación.

![Captura de pantalla de la página para agregar destino de almacenamiento con destino NFS definido](media/hpc-cache-add-nfs-target.png)

Proporcione esta información para un destino de almacenamiento respaldado por NFS:

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.

* **Target type** (Tipo de destino): elija **NFS**.

* **Hostname** (Nombre de host): escriba la dirección IP o el nombre de dominio completo del sistema de almacenamiento NFS. Use un nombre de dominio solo si la caché tiene acceso a un servidor DNS que puede resolverlo.

* **Usage model** (Modelo de uso): elija uno de los perfiles de almacenamiento en caché de datos en función del flujo de trabajo, tal como se describe en [Selección de un modelo de uso](#choose-a-usage-model), a continuación.

### <a name="nfs-namespace-paths"></a>Rutas de acceso del espacio de nombres NFS

Un destino de almacenamiento NFS puede tener varias rutas de acceso virtuales, siempre y cuando cada ruta represente una exportación o un subdirectorio diferente sen el mismo sistema de almacenamiento.

Cree todas las rutas de acceso de un destino de almacenamiento.

Puede [agregar y editar rutas de acceso de espacio de nombres](hpc-cache-edit-storage.md) en un destino de almacenamiento en cualquier momento.

Rellene estos valores para cada ruta de acceso del espacio de nombres:

* **Virtual namespace path** (Ruta de acceso del espacio de nombres virtual): establezca la ruta de archivo orientada al cliente para este destino de almacenamiento. Lea [Configuración del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más información acerca de la característica de espacio de nombres virtual.

<!--  The virtual path should start with a slash ``/``. -->

* **NFS export path** (Ruta de acceso de exportación de NFS): escriba la ruta de acceso a la exportación de NFS.

* **Subdirectory path** (Ruta de acceso del subdirectorio): si desea montar un subdirectorio específico de la exportación, indíquelo aquí. En caso contrario, deje este campo en blanco.

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

### <a name="choose-a-usage-model"></a>Selección de un modelo de uso
<!-- referenced from GUI - update aka.ms link if you change this heading -->

Cuando cree un destino de almacenamiento que apunte a un sistema de almacenamiento NFS, deberá elegir el *modelo de uso* de ese destino. Este modelo determina cómo se almacenan los datos en caché.

Hay tres opciones:

* **Lectura de textos densos y poco frecuentes**: utilice esta opción si desea acelerar el acceso de lectura a archivos que son estáticos o que no se suelen modificar.

  Esta opción almacena en caché los archivos que los clientes leen, pero pasa las escrituras por el almacenamiento de back-end inmediatamente. Los archivos almacenados en la memoria caché nunca se comparan con los archivos del volumen de almacenamiento NFS.

  No use esta opción si existe el riesgo de que un archivo se pueda modificar directamente en el sistema de almacenamiento sin escribirlo primero en la memoria caché. Si esto ocurre, la versión almacenada en caché del archivo nunca se actualizará con los cambios del back-end y el conjunto de datos puede volverse incoherente.

* **Mayor que el 15 % de textos**: esta opción acelera el rendimiento de lectura y escritura. Al usar esta opción, todos los clientes deben acceder a los archivos mediante Azure HPC Cache en lugar de montar el almacenamiento de back-end directamente. Los archivos almacenados en caché tendrán cambios recientes que no se almacenan en el back-end.

  En este modelo de uso, los archivos de la memoria caché no se comparan con los archivos del almacenamiento de back-end. Se supone que la versión en caché del archivo es más actual. Un archivo modificado en la memoria caché solo se escribe en el sistema de almacenamiento de back-end después de que esté en la memoria caché durante una hora sin cambios adicionales.

* **Los clientes escriben en el destino NFS omitiendo la caché**: elija esta opción si algún cliente del flujo de trabajo escribe datos directamente en el sistema de almacenamiento sin escribir primero en la memoria caché. Los archivos que solicitan los clientes se almacenan en caché, pero los cambios que se realicen en esos archivos desde el cliente se devuelven al sistema de almacenamiento de back-end inmediatamente.

  Con este modelo de uso, los archivos de la memoria caché se comparan con frecuencia con las versiones de back-end para determinar si hay actualizaciones. Esta comprobación permite cambiar los archivos fuera de la memoria caché al tiempo que se preserva la coherencia de los datos.

En esta tabla se resumen las diferencias de los modelos de uso:

| Modelo de uso | Modo de almacenamiento en caché | Comprobación de back-end | Retraso máximo de reescritura |
| ---- | ---- | ---- | ---- |
| Lectura de textos densos y poco frecuentes | Lectura | Nunca | None |
| Mayor que el 15 % de textos | Lectura/escritura | Nunca | 1 hora |
| Los clientes omiten la memoria caché | Lectura | 30 segundos | None |

## <a name="next-steps"></a>Pasos siguientes

Después de crear destinos de almacenamiento, considere la posibilidad de realizar una de estas tareas:

* [Montaje de la instancia de Azure HPC Cache](hpc-cache-mount.md)
* [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md)

Si necesita actualizar cualquier configuración, puede [editar un destino de almacenamiento](hpc-cache-edit-storage.md).
