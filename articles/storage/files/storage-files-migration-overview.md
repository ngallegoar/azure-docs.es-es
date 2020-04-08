---
title: Migración a recursos compartidos de archivos de Azure
description: Obtenga información sobre las migraciones a los recursos compartidos de archivos de Azure y busque la guía de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 3/18/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 903ce52120fce7c23c6a3754498b81fc6fc2430f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247323"
---
# <a name="migrate-to-azure-file-shares"></a>Migración a recursos compartidos de archivos de Azure

En este artículo se tratan los aspectos básicos de una migración a los recursos compartidos de archivos de Azure.

Además de los aspectos básicos de la migración, este artículo contiene una lista de guías de migración individualizadas y existentes. Estas guías de migración le ayudan a mover los archivos a recursos compartidos de archivos de Azure y se organizan por la ubicación en la que residen actualmente los datos y el modelo de implementación (solo en la nube o híbrido) al que tiene previsto pasar.

## <a name="migration-basics"></a>Aspectos básicos de la migración

Hay varios tipos diferentes de almacenamiento en la nube que están disponibles en Azure. Un aspecto fundamental de una migración de archivos a Azure es determinar qué opción de almacenamiento de Azure es la adecuada para los datos.

Los recursos compartidos de archivos de Azure son excelentes para los datos de archivos de uso general. En realidad, cualquier cosa para la que use un recurso compartido de SMB o NFS local. Con [Azure File Sync](storage-sync-files-planning.md), opcionalmente puede almacenar en caché el contenido de varios recursos compartidos de archivos de Azure en varios servidores de Windows locales.

Si tiene una aplicación que se está ejecutando en un servidor local, el almacenamiento de archivos en recursos compartidos de archivos de Azure puede ser adecuado para usted, en función de la aplicación. Puede mejorar la aplicación para que se ejecute en Azure y utilice recursos compartidos de archivos de Azure como almacenamiento compartido. También puede considerar los [discos de Azure](../../virtual-machines/windows/managed-disks-overview.md) para este escenario. En el caso de las aplicaciones nativas de la nube y que no dependen de SMB o el acceso local a la máquina a los datos o acceso compartido, el almacenamiento de objetos, como [blobs de Azure](../blobs/storage-blobs-overview.md), suele ser la mejor opción.

La clave de cualquier migración es capturar toda la fidelidad aplicable a los archivos al migrar los archivos desde su ubicación de almacenamiento actual a Azure. Una ayuda para elegir el almacenamiento de Azure adecuado es también el aspecto de la fidelidad compatible con la opción de almacenamiento de Azure y que es necesario en el escenario. Normalmente, los datos de archivo de uso general dependen de los metadatos de archivo. Los datos de aplicación no lo hacen. Hay dos componentes básicos en un archivo:

- **Flujo de datos** El flujo de datos de un archivo almacena el contenido del archivo.
- **Metadatos del archivo**: Los metadatos del archivo tienen varios subcomponentes:
   * Atributos de archivo: De solo lectura, por ejemplo.
   * Permisos de archivo: Se conocen como *permisos NTFS* o *ACL de archivos y carpetas*.
   * Marcas de tiempo: En particular, las marcas de tiempo *create-* y *last modified-* .
   * Flujo de datos alternativo: Un espacio para almacenar grandes cantidades de propiedades no estándar.

Por lo tanto, en una migración, se puede definir la fidelidad de los archivos como la capacidad de almacenar toda la información de archivo aplicable en el origen, la capacidad de transferirlos con la herramienta de migración y la capacidad de almacenarlos en el almacenamiento de destino de la migración.

Para asegurarse de que la migración se realiza de la manera más fluida posible, identifique [la mejor herramienta de copia para sus necesidades](#migration-toolbox) y haga coincidir un destino de almacenamiento con el origen.

Teniendo en cuenta la información anterior, queda claro cuál es el almacenamiento de destino para los archivos de uso general en Azure: [Recursos compartidos de archivos de Azure](storage-files-introduction.md) En comparación con el almacenamiento de objetos en blobs de Azure, los metadatos de archivo se pueden almacenar de forma nativa en los archivos de un recurso compartido de archivos de Azure.

Los recursos compartidos de archivos de Azure también conservan la jerarquía de archivos y carpetas. Además:
* Los permisos NTFS se pueden almacenar en archivos y carpetas, ya que son locales.
* Los usuarios de AD (o usuarios de Azure AD DS) pueden acceder de forma nativa a un recurso compartido de archivos de Azure. 
    Usan su identidad actual y obtienen acceso en función de los permisos de recurso compartido, así como de las ACL de archivos y carpetas. Un comportamiento que no es diferente de cuando los usuarios se conectan a un recurso compartido de archivos local.
*  El flujo de datos alternativo es el aspecto principal de la fidelidad de archivos que actualmente no se puede almacenar en un archivo en un recurso compartido de archivos de Azure.
   Se conserva localmente cuando Azure File Sync está implicado.

* [Más información sobre la autenticación de AD para los recursos compartidos de Azure](storage-files-identity-auth-active-directory-enable.md).
* [Más información sobre la autenticación de Azure Active Directory Domain Services (Azure DS) para recursos compartido de archivos de Azure](storage-files-identity-auth-active-directory-domain-service-enable.md) para Azure Files.

## <a name="migration-guides"></a>Guías de migración

En la tabla siguiente se enumeran las guías de migración detalladas.

Desplácese por ella de la siguiente manera:
1. Busque la fila del sistema de origen en el que están almacenados los archivos.
2. Decida si tiene como destino una implementación híbrida en la que usa Azure File Sync para almacenar en caché el contenido de uno o más recursos compartidos de archivos de Azure de forma local, o si desea usar recursos compartidos de archivos de Azure directamente en la nube. Seleccione la columna de destino que refleje su decisión.
3. Dentro de la intersección de origen y destino, una celda de la tabla muestra los escenarios de migración disponibles. Seleccione uno de ellos para vincularlo directamente a la guía de migración detallada.

Un escenario sin un vínculo no tiene todavía una guía de migración publicada. Compruebe de vez en cuando en esta tabla si hay actualizaciones. Las nuevas guías se publicarán en cuanto estén disponibles.

| Source | Destino: </br>Implementación híbrida | Destino: </br>Implementación solo en la nube |
|:---|:--|:--|
| | Combinación de herramientas:| Combinación de herramientas: |
| Windows Server 2012 R2 y más reciente | <ul><li>[Azure File Sync](storage-sync-files-deployment-guide.md)</li><li>[Azure File Sync y DataBox](storage-sync-offline-data-transfer.md)</li><li>Servicio de migración de almacenamiento y Azure File Sync</li></ul> | <ul><li>Azure File Sync</li><li>Azure File Sync y DataBox</li><li>Servicio de migración de almacenamiento y Azure File Sync</li><li>RoboCopy</li></ul> |
| Windows Server 2012 y posteriores | <ul><li>Azure File Sync y DataBox</li><li>Servicio de migración de almacenamiento y Azure File Sync</li></ul> | <ul><li>Servicio de migración de almacenamiento y Azure File Sync</li><li>RoboCopy</li></ul> |
| Almacenamiento conectado a la red (NAS) | <ul><li>[Azure File Sync y RoboCopy](storage-files-migration-nas-hybrid.md)</li></ul> | <ul><li>RoboCopy</li></ul> |
| Linux/Samba | <ul><li>[RoboCopy y Azure File Sync](storage-files-migration-linux-hybrid.md)</li></ul> | <ul><li>RoboCopy</li></ul> |
| StorSimple 8100/8600 | <ul><li>[Azure File Sync y aplicación virtual 8020](storage-files-migration-storsimple-8000.md)</li></ul> | |
| StorSimple 1200 | <ul><li>[Azure File Sync](storage-files-migration-storsimple-1200.md)</li></ul> | |
| | | |

## <a name="migration-toolbox"></a>Cuadro de herramientas de migración

### <a name="file-copy-tools"></a>Herramientas de copia de archivos

Hay varias herramientas de copia de archivos disponibles de Microsoft y que no son de Microsoft. Para seleccionar la herramienta de copia de archivos correcta para el escenario de migración, debe tener en cuenta tres preguntas fundamentales:

* ¿La herramienta de copia admite la ubicación de origen y de destino para una copia de archivo determinada? 
    * ¿Admite la ruta de acceso de red o los protocolos disponibles (por ejemplo, REST/SMB/NFS) a las ubicaciones de almacenamiento de origen y de destino, y desde ellas?
* ¿La herramienta de copia conserva la fidelidad de archivo necesaria que es compatible con la ubicación de origen y destino? En algunos casos, el almacenamiento de destino no admite la misma fidelidad que el origen. Ya ha tomado la decisión de que el almacenamiento de destino es suficiente para sus necesidades; por lo tanto, la herramienta de copia solo debe coincidir con las funcionalidades de fidelidad de archivo de destino.
* ¿La herramienta de copia tiene características que se adaptan a la estrategia de migración? 
    * Por ejemplo, considere la posibilidad de tener opciones que le permitan minimizar el tiempo de inactividad. Una buena pregunta es: ¿Puedo ejecutar esta copia varias veces en la misma ubicación y por usuarios con acceso activo? Si es así, puede reducir significativamente la cantidad de tiempo de inactividad. Compárelo con una situación en la que solo puede iniciar la copia cuando el origen deja de cambiar para garantizar una copia completa.

En la tabla siguiente se clasifican las herramientas de Microsoft y su idoneidad actual para los recursos compartidos de archivos de Azure:

| Recomendado | Herramienta | Admite los recursos compartidos de archivos de Azure. | Conserva la fidelidad de los archivos. |
| :-: | :-- | :---- | :---- |
|![Sí, recomendado.](media/storage-files-migration-overview/circle-green-checkmark.png)| RoboCopy | Compatible. Los recursos compartidos de archivos de Azure se pueden montar como unidades de red. | Fidelidad completa* |
|![Sí, recomendado.](media/storage-files-migration-overview/circle-green-checkmark.png)| Azure File Sync | Integrado de forma nativa en recursos compartidos de archivos de Azure. | Fidelidad completa* |
|![Sí, recomendado.](media/storage-files-migration-overview/circle-green-checkmark.png)| Servicio de migración de almacenamiento (SMS) | Indirectamente compatible. Los recursos compartidos de archivos de Azure se pueden montar como unidades de red en un servidor de destino SMS. | Fidelidad completa* |
|![No es totalmente recomendable.](media/storage-files-migration-overview/triangle-yellow-exclamation.png)| Azure DataBox | Compatible. | No copia los metadatos. [Se puede utilizar junto con Azure File Sync](storage-sync-offline-data-transfer.md). |
|![No se recomienda.](media/storage-files-migration-overview/circle-red-x.png)| AzCopy | Compatible. | No copia los metadatos. |
|![No se recomienda.](media/storage-files-migration-overview/circle-red-x.png)| Explorador de Azure Storage | Compatible. | No copia los metadatos. |
|![No se recomienda.](media/storage-files-migration-overview/circle-red-x.png)| Azure Data Factory | Compatible. | No copia los metadatos. |
|||||

*\* Fidelidad total: cumple o supera las funcionalidades de recurso compartido de archivos de Azure.*

### <a name="migration-helper-tools"></a>Herramientas del asistente para la migración

En esta sección se enumeran las herramientas que ayudan a planear y ejecutar migraciones.

* **RoboCopy, de Microsoft Corporation**

    Una de las herramientas de copia más adecuadas para las migraciones de archivos se incluye como parte de Microsoft Windows. Debido a las numerosas opciones de esta herramienta, la [documentación principal de RoboCopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) es una fuente útil.

* **TreeSize, de JAM Software GmbH**

    Azure File Sync se escala principalmente con el número de elementos (archivos y carpetas) y menos con la cantidad total de TiB. La herramienta se puede usar para determinar el número de archivos y carpetas en los volúmenes de Windows Server. Además, se puede usar para crear una perspectiva antes de una [implementación de Azure File Sync](storage-sync-files-deployment-guide.md), pero también después, cuando se activa la organización en niveles en la nube y se desea ver no solo el número de elementos, sino también en qué directorios se usa la memoria caché del servidor.
    Esta herramienta (versión de prueba 4.4.1) es compatible con los archivos en niveles de la nube. No se producirá la coincidencia de los archivos en niveles durante su funcionamiento normal.


## <a name="next-steps"></a>Pasos siguientes

1. Cree un plan para la implementación de los recursos compartidos de archivos de Azure (solo en la nube o híbridos) que desees.
2. Revise la lista de guías de migración disponibles para encontrar la guía detallada que coincide con el origen y la implementación de los recursos compartidos de archivos de Azure.

Hay más información disponible sobre las tecnologías de Azure Files mencionadas en este artículo:

* [¿Qué es Azure Files](storage-files-introduction.md)
* [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
* [Información general de nube por niveles ](storage-sync-cloud-tiering.md)
