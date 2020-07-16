---
title: Búsqueda de ejemplos de Azure Service Fabric Mesh
description: Este es un índice de las aplicaciones de ejemplo de Service Fabric Mesh disponibles. El código fuente de estos ejemplos muestra cómo lograr un escenario particular mediante el modelo de recursos de Service Fabric.
ms.date: 12/03/2018
ms.topic: conceptual
ms.openlocfilehash: 64d9e4e112f26f83eedd02b034c8191c6f677618
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261281"
---
# <a name="find-service-fabric-mesh-samples"></a>Búsqueda de ejemplos de Service Fabric Mesh

En esta tabla se describen las aplicaciones de ejemplo de Service Fabric malla disponibles. El código fuente de estos ejemplos muestra cómo lograr un escenario particular mediante el modelo de recursos de Service Fabric.

Para más información sobre la implementación de plantillas directamente en Azure, consulte la [página de plantillas de ejemplo de GitHub](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/README.md).

|Plantilla de ejemplo|Descripción del escenario|Código fuente|Herramientas para desarrolladores|
|------------|--------------------|----------|----------------------|
| [Aplicación Hello World](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/helloworld) | Página web estática hospedada en un contenedor. Para Linux, usa nginx, para Windows usa IIS | [Código fuente](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) | Sin requisitos |
| [Aplicación de contador para volúmenes de archivos de Azure](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.md) | Almacenamiento del estado al montar el volumen basado en Azure Files en el contenedor. <br><br> **Nota:** Esta plantilla requiere un recurso compartido de archivos de Azure Files que ya se va a aprovisionar [Instrucciones](../storage/files/storage-how-to-create-file-share.md) | [Código fuente](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Herramientas de Visual Studio Mesh |
| [TodoListApp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/todolist) | Creación de una aplicación con un servicio de front-end y back-end que utiliza la resolución basada en DNS. Se usa como un tutorial [aquí](./service-fabric-mesh-tutorial-create-dotnetcore.md) | [Código fuente](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/todolistapp) | Herramientas de Visual Studio Mesh |
| [Aplicación de objetos visuales](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/visualobjects) | Escalado y actualización de microservicios en una aplicación. | [Código fuente](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) |  Herramientas de Visual Studio Mesh |
| [Aplicación de votación](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/voting) | Creación de una aplicación con un servicio de front-end y back-end que utiliza la resolución basada en DNS. | [Código fuente](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) | Las herramientas de Visual Studio Mesh para la versión de Windows, VS Code/CLI de dotnet se pueden utilizar para la versión de Linux. |
| [Aplicación de contador para volúmenes confiables de Service Fabric](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.sfreliablevolume.md)| Almacenamiento de estado al montar el volumen confiable basado en discos de Service Fabric dentro del contenedor.| [Código fuente](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Herramientas de Visual Studio Mesh |
