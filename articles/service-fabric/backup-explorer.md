---
title: Lectura y actualización local de una copia de seguridad de Reliable Collections
description: Use Explorador de Backup en Azure Service Fabric para leer y actualizar una copia de seguridad local de Reliable Collections.
author: athinanthny
ms.topic: conceptual
ms.date: 07/01/2020
ms.author: atsenthi
ms.openlocfilehash: 1da70c37c8a6ed93e7abe1b5d329e808c592e43a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86034761"
---
# <a name="read-and-update-a-reliable-collections-backup-by-using-backup-explorer"></a>Lectura y actualización de una copia de seguridad de Reliable Collections mediante Explorador de Backup

Explorador de Backup de Azure Service Fabric ayuda con la corrección de datos si los datos están dañados en Reliable Collections de Service Fabric. El estado actual de los datos puede estar dañado por cualquier error que se haya introducido en una aplicación o por entradas erróneas realizadas en un clúster activo.

Con la ayuda de Explorador de Backup, puede realizar las siguientes tareas:
-   Consultar los metadatos de la colección.
-   Ver el estado actual y sus entradas en la colección de la copia de seguridad que se carga.
-   Mostrar las transacciones realizadas desde el último punto de control.
-   Actualizar la colección agregando, actualizando o eliminando entradas en la colección.
-   Realizar una nueva copia de seguridad con el estado actualizado.

> [!NOTE]
> La solución Explorador de Backup de Azure Service Fabric solo admite actualmente la visualización y edición de Reliable Collections en la copia de seguridad.
>

## <a name="access-the-backup"></a>Acceso a la copia de seguridad

La solución Explorador de Backup de Azure Service Fabric se puede consumir de cualquiera de las siguientes maneras para ver o actualizar Reliable Collections en la copia de seguridad:
-   **Binario**: use un paquete NuGet para ver y modificar Reliable Collections.
-   **HTTP/REST**: use un servidor REST basado en HTTP para ver y modificar Reliable Collections.
-   **bkpctl**: use la interfaz de la línea de comandos (CLI) de Explorador de Backup de Azure Service Fabric para ver y modificar una copia de seguridad de Reliable Collections.

La solución Explorador de Backup tiene una biblioteca de C# para usuarios avanzados. Puede usar la biblioteca de la aplicación directamente para trabajar con Reliable Collections de manera similar a la forma en que los clientes trabajan con el administrador de estado en sus servicios con estado existentes. En el caso los usuarios Basic y en un caso de uso básico, el explorador también tiene un servidor REST independiente que expone las API para inspeccionar las copias de seguridad. La herramienta bkpctl de la CLI funciona sobre las API de REST y se basa en Python. Puede usar la herramienta de la CLI para leer y actualizar las copias de seguridad, así como para realizar nuevas copias de seguridad a través de la línea de comandos.

Para más información, consulte el repositorio [Explorador de Backup de Azure Service Fabric](https://github.com/microsoft/service-fabric-backup-explorer) de GitHub. El repositorio contiene información de origen y de versión, además de instrucciones de configuración.

También puede generar el repositorio localmente y trabajar en las copias de seguridad.
 
El paquete NuGet para Explorador de Backup (Microsoft.ServiceFabric.ReliableCollectionBackup.Parser) estará disponible en [nuget.org](https://www.nuget.org/). 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [Reliable Collections en los servicios con estado de Azure Service Fabric](service-fabric-reliable-services-reliable-collections.md).
* Revise [Procedimientos recomendados de Azure Service Fabric](service-fabric-best-practices-overview.md).
