---
title: Formatos de archivo admitidos en Azure Data Factory
description: En este tema se describen los formatos de archivo y los códigos de compresión que admiten los conectores basados en archivos en Azure Data Factory.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: 6855eea5939419c9a0a867de4e0621b4d4ae02b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75439568"
---
# <a name="supported-file-formats-and-compression-codecs-in-azure-data-factory"></a>Formatos de archivo y códecs de compresión admitidos en Azure Data Factory

*Este artículo se aplica a los conectores siguientes: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [File System](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md) y [SFTP](connector-sftp.md).*

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

La [actividad de copia](copy-activity-overview.md) se puede usar para copiar archivos tal cual entre dos almacenes de datos basados en archivos, en cuyo caso los datos se copian de manera eficaz sin serialización ni deserialización. 

Además, también puede analizar o generar archivos con un formato determinado. Por ejemplo, puede hacer lo siguiente:

* Copiar datos de una base de datos de SQL Server local y escribirlos en Azure Data Lake Storage Gen2 en formato Parquet.
* Copiar archivos en formato de texto (CSV) desde el sistema de archivos local y escribirlos en Azure Blob Storage en formato Avro
* Copiar archivos comprimidos del sistema de archivos local, descomprimirlos sobre la marcha y escribirlos en Azure Data Lake Storage Gen2.
* Copiar datos en formato de texto comprimido Gzip (CSV) de Azure Blob Storage y escribirlos en Azure SQL Database
* Muchas más actividades que requieren serialización y deserialización o compresión y descompresión.

## <a name="next-steps"></a>Pasos siguientes

Consulte los otros artículos de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Rendimiento de la actividad de copia](copy-activity-performance.md)
