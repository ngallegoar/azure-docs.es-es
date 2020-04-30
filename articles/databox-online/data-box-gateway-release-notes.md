---
title: Notas de la versión de disponibilidad general de Azure Data Box Gateway | Microsoft Docs
description: Se describen los problemas críticos y las soluciones para la versión de disponibilidad general de Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/26/2019
ms.author: alkohli
ms.openlocfilehash: 1a8a9840cc6e1f3627c5fbd30e0b7432db0f16e4
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82561054"
---
# <a name="azure-data-box-edgeazure-data-box-gateway-general-availability-release-notes"></a>Notas de la versión de disponibilidad general de Azure Data Box Edge/Azure Data Box Gateway

## <a name="overview"></a>Información general

En las notas de la versión siguientes se identifican los problemas críticos pendientes y los problemas resueltos de la versión de disponibilidad general para Azure Data Box Edge y Azure Data Box Gateway. 

Las notas de la versión se actualizan continuamente y se van agregando a medida que se descubren problemas críticos que requieren una solución alternativa. Antes de implementar la instancia de Data Box Edge o Data Box Gateway, revise detenidamente la información que encontrará en las notas de la versión.

Esta versión de disponibilidad general se corresponde con las versiones de software:

- **Data Box Gateway 1903 (1.5.814.447)**
- **Data Box Edge 1903 (1.5.814.447)**


## <a name="whats-new"></a>Novedades

- **Nuevas imágenes de disco virtual**: hay nuevos VHDX y VMDK disponibles mediante Azure Portal. Descargue estas imágenes para aprovisionar, configurar e implementar nuevos dispositivos de disponibilidad general de Data Box Gateway. Los dispositivos de Data Box Gateway creados en las versiones preliminares anteriores se pueden actualizar a esta versión. Para más información, consulte [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md).
- **Compatibilidad con NFS**: la compatibilidad con NFS está actualmente en versión preliminar y está disponible para los clientes de las versiones 3.0 y 4.1 que tienen acceso a los dispositivos de Data Box Edge y Data Box Gateway.
- **Resistencia al almacenamiento**: su dispositivo Data Box Edge puede resistir el error de un disco de datos con la característica de resistencia de Storage. Esta funcionalidad actualmente está en su versión preliminar. Puede habilitar la resistencia al almacenamiento seleccionando la opción **Resistente** en la **configuración de Storage** en la interfaz de usuario web local.


## <a name="known-issues-in-ga-release"></a>Problemas conocidos en la versión de disponibilidad general

En la tabla siguiente se proporciona un resumen de los problemas conocidos de la versión en ejecución de Data Box Gateway.

| No. | Característica | Problema | Soluciones alternativas o comentarios |
| --- | --- | --- | --- |
| **1.** |Tipos de archivo | No se admiten los siguientes tipos de archivos: archivos de caracteres, archivos de bloqueo, sockets, canalizaciones y vínculos simbólicos.  |Al copiar estos archivos, se crean archivos de longitud 0 en el recurso compartido NFS. Estos archivos permanecen en estado de error y también se notifican en *error.xml*. <br> Como resultado de los vínculos simbólicos a directorios, estos nunca se marcan como sin conexión. Por este motivo, no puede ver la cruz gris que indica que los directorios están disponibles sin conexión y que todo el contenido asociado se cargó por completo en Azure. |
| **2.** |Eliminación | Debido a un error en esta versión, si se elimina un recurso compartido NFS, es posible que el recurso compartido no se pueda eliminar. El estado del recurso compartido será *Deleting* (Eliminando).  |Esto solo se produce cuando el recurso compartido usa un nombre de archivo no admitido. |
| **3.** |Copiar | La copia de datos produce un error con el mensaje Error:  no se pudo completar la operación solicitada por una limitación del sistema de archivos.  |No se admite la secuencia de datos alternativa (ADS) asociada con un tamaño de archivo mayor que 128 KB.   |


## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
- [Preparación para la implementación de Azure Data Box Edge](azure-stack-edge-deploy-prep.md)
