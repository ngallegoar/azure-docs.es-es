---
title: Decisión sobre cuándo usar Azure Blobs, Azure Files o Azure Disks
description: Obtenga información sobre las distintas formas de almacenar datos y acceder a ellos en Azure para que le sea más fácil decidir qué la tecnología usar.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 4b1a42e25a6d8c7b4a3c24dffcb858ffe63dd10b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "71671043"
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-disks"></a>Decisión sobre cuándo usar Azure Blobs, Azure Files o Azure Disks

Microsoft Azure proporciona varias características en Azure Storage para almacenar los datos en la nube y tener acceso a ellos. En este artículo se describen Azure Files, Azure Blobs y Azure Disks, y está diseñado para ayudarle a elegir entre estas características.

## <a name="scenarios"></a>Escenarios

En la tabla siguiente se compara Azure Files, Azure Blobs y Azure Disks y se muestran los escenarios de ejemplo adecuados para cada uno.

| Característica | Descripción | Cuándo se usa |
|--------------|-------------|-------------|
| **Archivos de Azure** | Proporciona una interfaz SMB, bibliotecas de cliente y una [interfaz de REST](/rest/api/storageservices/file-service-rest-api) que permite el acceso desde cualquier lugar a archivos almacenados. | Cuando desee migrar mediante lift-and-shift una aplicación a la nube que ya usa las API del sistema de archivos nativo para compartir datos entre ella y otras aplicaciones que se ejecutan en Azure.<br/><br/>Cuando desee almacenar herramientas de desarrollo y depuración a las que es necesario acceder desde muchas máquinas virtuales. |
| **Azure Blobs** | Proporciona bibliotecas de cliente y una [interfaz de REST](/rest/api/storageservices/blob-service-rest-api) que permite que los datos no estructurados se almacenen y se acceda a ellos a gran escala en blobs en bloques.<br/><br/>También admite [Azure Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md) para soluciones de análisis de macrodatos empresariales. | Cuando desea que la aplicación admita escenarios de streaming y de acceso aleatorio.<br/><br/>Cuando desea poder tener acceso a datos de aplicación desde cualquier lugar.<br/><br/>Desea crear una instancia empresarial de Data Lake en Azure y realizar análisis de macrodatos. |
| **Azure Disks** | Proporciona bibliotecas de cliente y una [interfaz de REST](/rest/api/compute/manageddisks/disks/disks-rest-api) que permite que los datos se almacenen persistentemente y se acceda a ellos desde un disco duro virtual conectado. | Cuando desea migrar mediante lift-and-shift aplicaciones que usan las API del sistema de archivos nativo para leer y escribir datos en discos persistentes.<br/><br/>Cuando desea almacenar datos a los que no se necesita acceder desde fuera de la máquina virtual a la que está conectado el disco. |


## <a name="next-steps"></a>Pasos siguientes

Al tomar decisiones sobre cómo se almacenan los datos y cómo se accede a ellos, también debe tener en cuenta los costos implicados. Para más información, consulte [Precios de Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/).
  
Algunas características SMB no son aplicables a la nube. Para más información, consulte [Features not supported by the Azure File service](/rest/api/storageservices/features-not-supported-by-the-azure-file-service) (Características que no admite el servicio Azure File).
 
Para más información sobre los blobs de Azure, consulte nuestro artículo [¿Qué es Azure Blob Storage?](../blobs/storage-blobs-overview.md).

Para más información sobre Disk Storage, consulte nuestra [Introducción a Managed Disks](../../virtual-machines/windows/managed-disks-overview.md).

Para más información sobre Azure files, consulte nuestro artículo [Planeamiento de una implementación de Azure Files](../files/storage-files-planning.md).