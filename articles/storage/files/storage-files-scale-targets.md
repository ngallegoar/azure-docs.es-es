---
title: Objetivos de escalabilidad y rendimiento de Azure Files
description: Obtenga información sobre los objetivos de escalabilidad y rendimiento de Azure Files, como la capacidad, la tasa de solicitudes, y los límites de ancho de banda entrante y saliente.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 46c46faf8f7ee52978ae5542ab7ebd72a41b8357
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536457"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Objetivos de escalabilidad y rendimiento de Azure Files

[Azure Files](storage-files-introduction.md) ofrece recursos compartidos de archivos en la nube totalmente administrados, a los que se puede acceder mediante el protocolo SMB estándar. En este artículo se explican los objetivos de escalabilidad y rendimiento de Azure Files y Azure File Sync.

Los objetivos de escalabilidad y rendimiento que aparecen aquí son exigentes, pero pueden verse afectados por otras variables de la implementación. Por ejemplo, el rendimiento de un archivo puede verse limitado también por el ancho de banda de red disponible, no solo los servidores que hospedan el servicio Azure Files. Se recomienda probar el patrón de uso para determinar si la escalabilidad y el rendimiento de Azure Files cumplen sus requisitos. También nos comprometemos a aumentar estos límites con el tiempo. No dude en enviarnos comentarios, ya sea a través del cuadro de comentarios de debajo o de [UserVoice de Azure Files](https://feedback.azure.com/forums/217298-storage/category/180670-files), sobre los límites que quiere que aumentemos.

## <a name="azure-storage-account-scale-targets"></a>Objetivos de escalabilidad de la cuenta de almacenamiento Azure

El recurso primario de un recurso compartido de archivos de Azure es una cuenta de Azure Storage. Una cuenta de almacenamiento representa un grupo de almacenamiento de Azure que pueden utilizar varios servicios de almacenamiento, como Azure Files, para almacenar datos. Otros servicios que almacenan datos en cuentas de almacenamiento son Azure Blob Storage, Azure Queue Storage y Azure Table Storage. Los objetivos siguientes se aplican a todos los servicios de almacenamiento que guardan datos en una cuenta de almacenamiento:

[!INCLUDE [azure-storage-account-limits-standard](../../../includes/azure-storage-account-limits-standard.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> La utilización de la cuenta de almacenamiento de uso general desde otros servicios de almacenamiento afecta a los recursos compartidos de archivos de Azure de su cuenta de almacenamiento. Por ejemplo, si alcanza la capacidad máxima de la cuenta de almacenamiento con Azure Blob Storage, no podrá crear archivos en el recurso compartido de archivos de Azure, aunque este no llegue al tamaño máximo del recurso compartido.

## <a name="azure-files-scale-targets"></a>Objetivos de escalabilidad de Azure Files

Existen tres categorías de limitaciones a tener en cuenta para Azure Files: cuentas de almacenamiento, recursos compartidos y archivos.

Por ejemplo: con los recursos compartidos de archivos Premium, un solo recurso compartido puede lograr 100 000 IOPS y un único archivo puede escalar verticalmente hasta 5000 IOPS. Por lo tanto, si tiene tres archivos en un recurso compartido, el número máximo de IOPS que puede obtener de ese recurso compartido es de 15 000.

### <a name="standard-storage-account-limits"></a>Límites de la cuenta de almacenamiento estándar

Consulte la sección [Objetivos de escalabilidad de la cuenta de almacenamiento Azure](#azure-storage-account-scale-targets) para conocer estos límites.

### <a name="premium-filestorage-account-limits"></a>Límites de la cuenta FileStorage Premium

[!INCLUDE [azure-storage-limits-filestorage](../../../includes/azure-storage-limits-filestorage.md)]

> [!IMPORTANT]
> Los límites de la cuenta de almacenamiento se aplican a todos los recursos compartidos. El escalado vertical hasta el número máximo de cuentas FileStorage solo se puede lograr si hay un solo recurso compartido por cuenta FileStorage.

### <a name="file-share-and-file-scale-targets"></a>Objetivos de escalabilidad de archivos y recursos compartido de archivos

> [!NOTE]
> Los recursos compartidos de archivos estándar de más de 5 TiB presentan algunas limitaciones. Para obtener una lista de limitaciones e instrucciones para habilitar tamaños más grandes de recursos compartidos de archivos, consulte la sección [Habilitación de recursos compartidos de archivos más grandes en recursos compartidos de archivos estándar](storage-files-planning.md#enable-standard-file-shares-to-span-up-to-100-tib) de la guía de planeación.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Objetivos de escalabilidad de Azure File Sync

Azure File Sync se ha diseñado con el objetivo de un uso sin límites, pero el uso sin límites no siempre es posible. La tabla siguiente indica los límites de las pruebas de Microsoft y también indica qué objetivos son límites estrictos:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Métricas de rendimiento de Azure File Sync

Dado que el agente de Azure File Sync se ejecuta en una máquina con Windows Server que se conecta a los recursos compartidos de archivos de Azure, el rendimiento de sincronización efectivo depende de una serie de factores de su infraestructura: Windows Server y la configuración del disco subyacente, el ancho de banda de la red entre el servidor y Azure Storage, el tamaño del archivo, el tamaño total del conjunto de datos y la actividad en el conjunto de datos. Dado que Azure File Sync funciona en el nivel de archivos, las características de rendimiento de una solución basada en Azure File Sync se mide mejor en el número de objetos (archivos y directorios) que se procesan por segundo.

En el caso de Azure File Sync, el rendimiento es fundamental en dos fases:

1. **Aprovisionamiento inicial que se realiza una sola vez**: para optimizar el rendimiento del aprovisionamiento inicial, consulte [Incorporación con Azure File Sync](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync), donde obtendrá los detalles de una implementación óptima.
2. **Sincronización en curso**: después de que los datos se inicializan en los recursos compartidos de archivos de Azure, Azure File Sync mantiene varios puntos de conexión sincronizados.

Para ayudarle a planear la implementación de cada una de las fases, a continuación encontrará los resultados observados durante las pruebas internas en un sistema con una configuración

| Configuración del sistema |  |
|-|-|
| CPU | 64 núcleos virtuales con una memoria caché L3 de 64 MiB |
| Memoria | 128 GB |
| Disco | Discos SAS con RAID 10 con caché con respaldo de batería |
| Red | Red de 1 Gbps |
| Carga de trabajo | Servidor de archivos de uso general|

| Aprovisionamiento inicial que se realiza una sola vez  |  |
|-|-|
| Número de objetos | 25 millones de objetos |
| Tamaño del conjunto de datos| ~4,7 TiB |
| Tamaño de archivo medio | ~200 KiB (archivo más grande: 100 GiB) |
| Rendimiento de carga | 20 objetos por segundo por grupo de sincronización |
| Rendimiento de descarga de espacio de nombres* | 400 objetos por segundo |

\* Cuando se crea un nuevo punto de conexión de servidor, el agente de Azure File Sync no descarga nada del contenido del archivo. En primer lugar sincroniza el espacio de nombres completo y, después, desencadena la recuperación en segundo plano para descargar los archivos, ya sea en su totalidad o, si está habilitada la organización en niveles en la nube, la directiva de niveles en la nube establecida en el punto de conexión del servidor.

| Sincronización en curso  |   |
|-|--|
| Número de objetos sincronizados| 125 000 objetos (renovación ~ 1 %) |
| Tamaño del conjunto de datos| 50 GiB |
| Tamaño de archivo medio | ~500 KiB |
| Rendimiento de carga | 20 objetos por segundo por grupo de sincronización |
| Rendimiento de descarga completa* | 60 objetos por segundo |

*Si están habilitados los niveles de la nube, es probable que observe un rendimiento mejor, ya que solo se descargan algunos de los datos de los archivos. Azure File Sync solo descarga los datos de los archivos almacenados en la memoria caché cuando cambian en cualquiera de los puntos de conexión. En el caso de los archivos en niveles o recién creados, el agente no descarga los datos de los archivos, solo sincroniza el espacio de nombres en todos los puntos de conexión del servidor. El agente también admite descargas parciales de archivos en capas cuando el usuario accede a ellos. 

> [!Note]  
> Los números anteriores no indican el rendimiento que experimentará. El rendimiento real dependerá de varios factores, como se ha indicado al principio de esta sección.

Como guía general para la implementación, debería tener varios factores en cuenta:

- El rendimiento de los objetos se escala aproximadamente en proporción al número de grupos de sincronización del servidor. La división de los datos en varios grupos de sincronización en un servidor mejora el rendimiento, que también está limitado por el servidor y la red.
- El rendimiento de los objetos es inversamente proporcional al rendimiento de MiB por segundo. En archivos pequeños, el rendimiento será mayor en cuanto al número de objetos procesados por segundo, pero el rendimiento en MiB por segundo será inferior. Por el contrario, en archivos grandes, se procesarán menos objetos por segundo, pero el rendimiento en MiB por segundo será superior. El rendimiento en MiB por segundo está limitado por los destinos del escalado de Azure Files.

## <a name="see-also"></a>Consulte también

- [Planeamiento de una implementación de Azure Files](storage-files-planning.md)
- [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
