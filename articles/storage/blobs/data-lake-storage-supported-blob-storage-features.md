---
title: Características de Blob Storage disponibles en Azure Data Lake Storage Gen2 | Microsoft Docs
description: Más información acerca de las características de Blob Storage que puede usar con Azure Data Lake Storage Gen2
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: b270ce3cd15dbd1e8dd53bd60376a87d6e08f75c
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637210"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Características de Blob Storage disponibles en Azure Data Lake Storage Gen2

Las características de Blob Storage, como el [registro de diagnóstico](../common/storage-analytics-logging.md), los [niveles de acceso](storage-blob-storage-tiers.md) y las  [directivas de administración del ciclo de vida de Blob Storage](storage-lifecycle-management-concepts.md), ya funcionan con las cuentas que tienen un espacio de nombres jerárquico. Por tanto, puede habilitar los espacios de nombres jerárquicos en las cuentas de Blob Storage sin perder el acceso a estas características.

En esta tabla se enumeran las características de Blob Storage que puede usar con Azure Data Lake Storage Gen2. Los elementos que aparecen en estas tablas van a cambiar con el tiempo a medida que se siga ampliando la compatibilidad.

## <a name="supported-blob-storage-features"></a>Características de Blob Storage compatibles

> [!NOTE]
> El nivel de compatibilidad solo se refiere al modo en que se admite la característica con Data Lake Storage Gen2.

|Característica de Blob Storage |Nivel de compatibilidad |Artículos relacionados |
|---------------|-------------------|---|
|Nivel de acceso frecuente|Disponibilidad general|[Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](storage-blob-storage-tiers.md)|
|Nivel de acceso esporádico|Disponibilidad general|[Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](storage-blob-storage-tiers.md)|
|Eventos|Disponibilidad general|[Reacción a eventos de Blob Storage](storage-blob-event-overview.md)|
|Métricas (clásica)|Disponibilidad general|[Métricas de Azure Storage Analytics (clásicas)](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Métricas en Azure Monitor|Disponibilidad general|[Métricas de Azure Storage en Azure Monitor](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Comandos de PowerShell de Blob Storage|Disponibilidad general|[Inicio rápido: Carga, descarga y enumeración de blobs mediante PowerShell](storage-quickstart-blobs-powershell.md)|
|Comandos de la CLI de Azure de Blob Storage|Disponibilidad general|[Inicio rápido: Creación, descarga y enumeración de blobs mediante la CLI de Azure](storage-quickstart-blobs-cli.md)|
|API de Blob Storage|Disponibilidad general|[Inicio rápido: Biblioteca cliente de Azure Blob Storage v12 para .NET](storage-quickstart-blobs-dotnet.md)<br>[Inicio rápido: Administración de blobs con el SDK de Java v12](storage-quickstart-blobs-java.md)<br>[Inicio rápido: Administración de blobs con el SDK de Python v12](storage-quickstart-blobs-python.md)<br>[Inicio rápido: Administración de blobs con el SDK de JavaScript v12 en Node.js](storage-quickstart-blobs-nodejs.md)|
|Nivel de acceso de archivo|Vista previa|[Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](storage-blob-storage-tiers.md)|
|Directivas de administración del ciclo de vida|Vista previa|[Administración del ciclo de vida de Azure Blob Storage](storage-lifecycle-management-concepts.md)|
|Registros de diagnóstico|Disponibilidad general|[Registro de Azure Storage Analytics](../common/storage-analytics-logging.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Fuente de cambios|Todavía no se admite|[Compatibilidad con la fuente de cambios en Azure Blob Storage](storage-blob-change-feed.md)|
|Conmutación por error de cuenta|Todavía no se admite|[Recuperación ante desastres y conmutación por error de la cuenta](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|ACL del contenedor de blobs|Todavía no se admite|[Establecer lista de control de acceso de contenedor](https://docs.microsoft.com/rest/api/storageservices/set-container-acl)|
|Dominios personalizados|Todavía no se admite|[Asignación de un dominio personalizado a un punto de conexión de Azure Blob Storage](storage-custom-domain-name.md)|
|Almacenamiento inmutable|Todavía no se admite|[Almacenamiento inmutable de los datos críticos para la empresa en Azure Blob Storage](storage-blob-immutable-storage.md)|
|Instantáneas|Todavía no se admite|[Creación y administración de una instantánea de blob en .NET](storage-blob-snapshots.md)|
|Eliminación temporal|Todavía no se admite|[Eliminación temporal de blobs de Azure Storage](storage-blob-soft-delete.md)|
|Sitios web estáticos|Todavía no se admite|[Hospedaje de sitios web estáticos en Azure Storage](storage-blob-static-website.md)|
|Registro en Azure Monitor|Todavía no se admite|No disponible todavía|
|Blobs en bloques Premium|Todavía no se admite|[Creación de una cuenta BlockBlobStorage](storage-blob-create-account-block-blob.md)|

## <a name="see-also"></a>Consulte también

- [Problemas conocidos con Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Servicios de Azure que admiten Azure Data Lake Storage Gen2](data-lake-storage-supported-azure-services.md)
- [Plataformas de código abierto que admiten Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md)
- [Acceso multiprotocolo en Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)