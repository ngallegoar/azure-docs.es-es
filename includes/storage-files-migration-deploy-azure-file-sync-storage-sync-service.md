---
title: Implementación de un servicio de sincronización de almacenamiento
description: Implementación del recurso en la nube de Azure File Sync, un servicio de sincronización de almacenamiento. Un bloque de texto común, compartido entre los documentos de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: bd4af178d6e70d910d4643fc30c29fc8eb85303c
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043125"
---
En este paso, necesita las credenciales de la suscripción a Azure.

El recurso principal para configurar Azure File Sync se denomina *servicio de sincronización de almacenamiento*. Se recomienda implementar solo uno para todos los servidores que sincronizan el mismo conjunto de archivos ahora o en el futuro. Solo debe crear varios servicios de sincronización de almacenamiento si tiene distintos conjuntos de servidores que nunca deben intercambiar datos. Por ejemplo, podría tener servidores que nunca deben sincronizar el mismo recurso compartido de archivos de Azure. De lo contrario, el procedimiento recomendado es contar con un único servicio de sincronización de almacenamiento.

Elija una región de Azure para el servicio de sincronización de almacenamiento que esté cerca de su ubicación. Todos los demás recursos de nube se deben implementar en la misma región. Para simplificar el proceso de administración, cree un nuevo grupo de recursos en la suscripción para hospedar los recursos de sincronización y almacenamiento.

Para más información, consulte la [sección sobre la implementación del servicio de sincronización de almacenamiento](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service) en el artículo sobre la implementación de Azure File Sync. Siga solo esta parte del artículo. En pasos posteriores, tendrá vínculos a otras secciones del artículo.