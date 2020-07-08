---
title: Opciones de almacenamiento de estado en Azure Service Fabric Mesh
description: Obtenga información sobre el almacenamiento confiable del estado en las aplicaciones de Service Fabric mesh que se ejecutan en Azure Service Fabric mesh.
author: dkkapur
ms.author: dekapur
ms.date: 11/27/2018
ms.topic: conceptual
ms.openlocfilehash: 0b0bd611fa86d155bb5bf2e07808e7365e28871c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84692689"
---
# <a name="state-management-with-service-fabric"></a>Administración del estado con Service Fabric

Service Fabric admite muchas opciones distintas para el almacenamiento de estado. Para obtener información general conceptual de los patrones de administración de estado y Service Fabric, consulte [Conceptos de Service Fabric: estado](/azure/service-fabric/service-fabric-concepts-state). Estos mismos conceptos se aplican tanto si los servicios se ejecutan dentro de Service Fabric mesh como fuera. 

Con Service Fabric mesh, puede implementar una nueva aplicación fácilmente y conectarla a un almacén de datos existente hospedado en Azure. Además de usar cualquier base de datos remota, hay varias opciones para almacenar datos, dependiendo de si el servicio desea almacenamiento local o remoto. 

## <a name="volumes"></a>Volúmenes

Los contenedores suelen usar discos temporales. Sin embargo, los discos temporales son efímeros, por lo que puede obtener un nuevo disco temporal y perder la información cuando un contenedor se bloquea. También es difícil compartir información de los discos temporales con otros contenedores. Los volúmenes son directorios que se montan dentro de las instancias de contenedor que puede usar para conservar el estado. Los volúmenes proporcionan almacenamiento de archivos de propósito general y permiten leer y escribir archivos mediante las API de archivos de E/S de disco normal. El recurso de volumen describe cómo montar un directorio y qué almacenamiento de respaldo utilizar. Puede elegir Azure File Storage o un disco de volumen de Service Fabric para almacenar los datos.

![Volúmenes][image3]

### <a name="service-fabric-reliable-volume"></a>Volumen confiable de Service Fabric

El volumen confiable de Service Fabric es un controlador de volumen de Docker utilizado para montar un volumen local en un contenedor. Las escrituras y lecturas son operaciones locales y rápidas. Los datos se replican en nodos secundarios, para que ofrezcan una alta disponibilidad. La conmutación por error también es rápida. Cuando se bloquea un contenedor, conmuta a un nodo que ya tiene una copia de los datos. Para consultar un ejemplo, consulte cómo [implementar una aplicación con un volumen confiable de Service Fabric](service-fabric-mesh-howto-deploy-app-sfreliable-disk-volume.md).

### <a name="azure-files-volume"></a>Volumen de Azure Files

Un volumen de Azure Files es un controlador de volumen de Docker utilizado para montar un recurso compartido de Azure Files en un contenedor. El almacenamiento de Azure Files usa el almacenamiento de red, por lo que las lecturas y escrituras se realizan a través de la red. En comparación con el volumen confiable de Service Fabric, el almacenamiento de Azure Files ofrece menos rendimiento, pero ofrece una opción de datos más económica y totalmente confiable. Para ver un ejemplo, consulte la información sobre [cómo implementar una aplicación con un volumen de Azure Files](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre el modelo de aplicación, consulte [Recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md).

[image3]: ./media/service-fabric-mesh-storing-state/volumes.png
