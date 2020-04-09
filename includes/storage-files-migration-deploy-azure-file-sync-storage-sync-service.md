---
title: Implementación de un servicio de sincronización de almacenamiento
description: Implementación del recurso de nube de Azure File Sync. Un servicio de sincronización de almacenamiento. Un bloque de texto común, compartido entre los documentos de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: fe74bae95784dcb63a80ee7e280c02de2eadeb5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124710"
---
En este paso, necesita las credenciales de la suscripción a Azure.

El recurso principal para configurar Azure File Sync se denomina "servicio de sincronización de almacenamiento".
Se recomienda implementar solo uno para todos los servidores de la empresa que sincronizan el mismo conjunto de archivos ahora o en el futuro. Solo debe crear varios servicios de sincronización de almacenamiento si tiene distintos conjuntos de servidores que nunca deben intercambiar datos. (Por ejemplo: sincronizar el mismo recurso compartido de archivos de Azure). De lo contrario, el procedimiento recomendado es contar con un único servicio de sincronización de almacenamiento.

Elija una región de Azure para el servicio de sincronización de almacenamiento que esté cerca de la ubicación de su oficina. Todos los demás recursos de nube se deben implementar en la misma región.
Para simplificar el proceso de administración, cree un nuevo grupo de recursos en la suscripción para hospedar los recursos de sincronización y almacenamiento.

En el siguiente artículo se describe cómo implementar un servicio de sincronización de almacenamiento. Solo siga esta parte del documento. Tendrá vínculos a otros apartados de este documento en pasos posteriores.

[Obtenga información sobre la implementación del servicio de sincronización de almacenamiento.](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service)
