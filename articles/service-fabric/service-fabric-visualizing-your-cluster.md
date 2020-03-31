---
title: Visualización del clúster mediante Azure Service Fabric Explorer
description: Service Fabric Explorer es una aplicación para inspeccionar y administrar nodos y aplicaciones en la nube en un clúster de Microsoft Azure Service Fabric.
author: mikkelhegn
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: mikhegn
ms.openlocfilehash: 80e3d990b6e8026c57ffff0048d0447a95529564
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229228"
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualización del clúster mediante el Explorador de Service Fabric

Service Fabric Explorer (SFX) es una herramienta de código abierto para inspeccionar y administrar clústeres de Azure Service Fabric. Service Fabric Explorer es una aplicación de escritorio para Windows, macOS y Linux.

## <a name="service-fabric-explorer-download"></a>Descarga de Service Fabric Explorer

Use los siguientes vínculos para descargar Service Fabric Explorer como una aplicación de escritorio:

- Windows
  - https://aka.ms/sfx-windows

- Linux
  - https://aka.ms/sfx-linux-x86
  - https://aka.ms/sfx-linux-x64

- macOS
  - https://aka.ms/sfx-macos

> [!NOTE]
> La versión de escritorio de Service Fabric Explorer puede tener más o menos características que la compatibilidad con clúster. Para garantizar la compatibilidad con todas las características, puede revertir a la versión de Service Fabric Explorer implementada en el clúster.
>
>

### <a name="running-service-fabric-explorer-from-the-cluster"></a>Ejecución de Service Fabric Explorer desde el clúster

Service Fabric Explorer también se hospeda en el punto de conexión de administración de HTTP de un clúster de Service Fabric. Para iniciar SFX en un explorador web, vaya al punto de conexión HTTP del clúster desde cualquier explorador, por ejemplo, https:\///clusterFQDN:19080.

Para la instalación de las estaciones de trabajo del desarrollador, puede iniciar Service Fabric Explorer en el clúster local; para ello, navegue a https://localhost:19080/Explorer. Examine este artículo para [preparar el entorno de desarrollo](service-fabric-get-started.md).

> [!NOTE]
> Si el clúster está protegido por un certificado autofirmado recibirá un mensaje de error del explorador web que le indica que "este sitio no es seguro". Puede simplemente continuar en un explorador web más moderno ignorando la advertencia. En un entorno de producción se debe proteger el clúster mediante el nombre común y un certificado emitido por una entidad de certificación. 
>
>

## <a name="connect-to-a-service-fabric-cluster"></a>Conexión a un clúster de Service Fabric
Para conectarse a un clúster de Service Fabric, necesita el punto de conexión de administración de clústeres (FQDN/IP) y el puerto del punto de conexión de administración HTTP (de forma predeterminada,19080). Por ejemplo, https\://mysfcluster.westus.cloudapp.azure.com:19080. Use la casilla "Connect to localhost" (Conectarse a localhost) para conectarse a un clúster local en la estación de trabajo.

### <a name="connect-to-a-secure-cluster"></a>Conexión a un clúster seguro
Puede controlar el acceso de cliente a su clúster de Service Fabric con certificados o mediante Azure Active Directory (AAD).

Si intenta conectarse a un clúster seguro, se le pedirá que presente un certificado de cliente o que inicie sesión mediante AAD, según la configuración del clúster.

## <a name="understand-the-service-fabric-explorer-layout"></a>Información sobre el diseño del Explorador de Service Fabric
Puede desplazarse por el Explorador de Service Fabric desde el árbol situado a la izquierda. En la raíz del árbol, el panel del clúster proporciona información general del clúster, incluido un resumen de la aplicación y del estado del nodo.

![Panel de clúster del Explorador de Service Fabric][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Visualización del diseño del clúster
Los nodos de un clúster de Service Fabric se colocan en una cuadrícula bidimensional de dominios de error y dominios de actualización. Esta colocación garantiza que las aplicaciones permanecen disponibles en presencia de errores de hardware y actualizaciones de aplicaciones. Puede ver cómo se dispone el clúster actual mediante el mapa del clúster.

![Mapa de clúster del Explorador de Service Fabric][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Visualización de aplicaciones y servicios
El clúster contiene dos subárboles: uno para las aplicaciones y otro para los nodos.

Puede usar la vista de aplicaciones para navegar por la jerarquía lógica de Service Fabric: aplicaciones, servicios, particiones y réplicas.

En el ejemplo siguiente, la aplicación **MyApp** se compone de dos servicios: **MyStatefulService** y **WebService**. Como **MyStatefulService** es un servicio con estado, incluye una partición con una réplica principal y dos réplicas secundarias. Por el contrario, el servicio WebSvcService no tiene estado y contiene una única instancia.

![Vista de aplicación del explorador de Service Fabric][sfx-application-tree]

En cada nivel del árbol, el panel principal muestra la información pertinente sobre el elemento. Por ejemplo, puede ver el estado de mantenimiento y la versión para un servicio determinado.

![Panel Essentials del explorador de Service Fabric][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Visualización de los nodos del clúster
En la vista de nodos se muestra el diseño físico del clúster. Para un nodo determinado, puede comprobar qué aplicaciones tienen el código implementado en ese nodo. Más específicamente, puede ver qué réplicas están en ejecución.

## <a name="actions"></a>Acciones
El explorador de Service Fabric ofrece una forma rápida de invocar acciones en nodos, aplicaciones y servicios dentro del clúster.

Por ejemplo, para eliminar una instancia de la aplicación, elija la aplicación en el árbol de la izquierda y, luego, elija **Acciones** > **Eliminar aplicación**.

![Eliminación de una aplicación en el explorador de Service Fabric][sfx-delete-application]

> [!TIP]
> Puede realizar las mismas acciones haciendo clic en los puntos suspensivos situados junto a cada elemento.
>
> Todas las acciones que pueden realizarse a través del Explorador de Service Fabric también pueden realizarse a través de PowerShell o una API de REST, para habilitar la automatización.
>
>

También puede utilizar el Service Fabric Explorer a fin de crear instancias de aplicaciones para un tipo de aplicación y versión. Elija el tipo de aplicación en la vista de árbol y, luego, haga clic en el vínculo **Create app instance** (Crear instancia de aplicación) situado junto a la versión que desee en el panel derecho.

![Creación de una instancia de aplicación en Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Service Fabric Explorer no admite parámetros cuando se crean instancias de aplicación. Las instancias de aplicación usan valores de parámetros predeterminados.
>
>

## <a name="event-store"></a>EventStore
EventStore es una característica que ofrece la plataforma que proporciona eventos de la plataforma Service Fabric y que está disponible en Service Fabric Explorer y en la API de REST. Puede ver una vista de instantánea de lo que está ocurriendo en el clúster para cada entidad (por ejemplo, nodo, servicio, aplicación y consulta) según la hora del evento. Obtenga más información sobre EventStore en [Información general de EventStore](service-fabric-diagnostics-eventstore.md).   

![EventStore][sfx-eventstore]

>[!NOTE]
>A partir de la versión 6.4 de Service Fabric, EventStore no está habilitado de forma predeterminada y se debe habilitar en la plantilla de Resource Manager

>[!NOTE]
>A partir de la versión 6.4 de Service Fabric, las API de EventStore solo se encuentran disponibles para los clústeres Windows que se ejecutan en Azure. Se está trabajando para portar esta funcionalidad a Linux, así como nuestros clústeres independientes.

## <a name="image-store-viewer"></a>Visor del almacén de imágenes
El visor del almacén de imágenes es una característica que se ofrece si se usa el almacén de imágenes nativo que permite ver los contenidos actuales del almacén de imágenes y obtener información de archivos y carpetas. Además, también permite la eliminación de archivos y carpetas.

![Mapa de clúster del Explorador de Service Fabric][sfx-imagestore]

## <a name="backup-and-restore"></a>Copias de seguridad y restauración
Service Fabric Explorer ofrece la posibilidad de interactuar con [Copia de seguridad y restauración](./service-fabric-reliable-services-backup-restore.md). Para ver las características de copia de seguridad y restauración en SFX, debe estar habilitado el modo avanzado .

![Habilitación del modo avanzado][0]
 
Son posibles las siguientes operaciones:

* Crear, editar y eliminar una directiva de copia de seguridad.
* Habilitar y deshabilitar la copia de seguridad para una aplicación, un servicio o una partición.
* Suspender y reanudar la copia de seguridad para una aplicación, un servicio o una partición.
* Desencadenar y hacer seguimiento de la copia de seguridad de una partición.
* Desencadenar y hacer seguimiento de la restauración de una partición.

Para más información sobre el servicio de copia de seguridad y restauración, consulte la [referencia de la API de REST](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore).
## <a name="next-steps"></a>Pasos siguientes
* [Administración de aplicaciones de Service Fabric en Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Implementación de aplicaciones de Service Fabric con PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/sfx-cluster-dashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/sfx-cluster-map.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/sfx-application-tree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/sfx-service-essentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/sfx-delete-application.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/sfx-create-app-instance.png
[sfx-eventstore]: ./media/service-fabric-diagnostics-eventstore/eventstore.png
[sfx-imagestore]: ./media/service-fabric-visualizing-your-cluster/sfx-image-store.png
[0]: ./media/service-fabric-backuprestoreservice/advanced-mode.png