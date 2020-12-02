---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/05/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b8a72640ebe8daffedb196456df7d40bc41b7b1b
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95559470"
---
Azure Storage ofrece varios tipos de cuentas de almacenamiento. Cada tipo admite diferentes características y tiene su propio modelo de precios. Tenga en cuenta estas diferencias antes de crear una cuenta de almacenamiento para determinar el tipo de cuenta más adecuada para sus aplicaciones. Estos son los tipos de cuentas de almacenamiento:

- **Cuentas de uso general v2**: tipo de cuenta de almacenamiento básico para blobs, archivos, colas y tablas. Se recomienda para la mayoría de los escenarios con Azure Storage.
- **Cuentas de uso general v1**: tipo de cuenta heredada para blobs, archivos, colas y tablas. Cuando sea posible, utilice en su lugar las cuentas de uso general v2.
- **Cuentas BlockBlobStorage**: cuentas de almacenamiento con características de rendimiento prémium para blobs en bloques y en anexos. Se recomiendan para escenarios con altas tasas de transacciones, que utilizan objetos más pequeños o que requieren una latencia de almacenamiento constantemente baja.
- **Cuentas FileStorage**: cuentas de almacenamiento solo para archivos con características de rendimiento superiores. Se recomienda para empresas y aplicaciones de escalado de alto rendimiento.
- **Cuentas BlobStorage**: cuentas de almacenamiento de solo blobs heredadas. Cuando sea posible, utilice en su lugar las cuentas de uso general v2.

En la siguiente tabla se describen los tipos de cuentas de almacenamiento y sus capacidades:

| Tipo de cuenta de almacenamiento | Servicios admitidos                       | Niveles de rendimiento admitidos      | Niveles de acceso admitidos         | Opciones de replicación               | Modelo de implementación<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Cifrado<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Uso general v2   | Blob, archivo, cola, tabla, disco y Data Lake Gen2<div role="complementary" aria-labelledby="data-lake-gen2"><sup>6</sup></div>      | Estándar, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Frecuente, esporádico, archivo<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS, GZRS (versión preliminar), RA-GZRS (versión preliminar)<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Resource Manager             | Cifrados              |
| Uso general v1   | Blob, archivo, cola, tabla y disco       | Estándar, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | N/D                            | LRS, GRS, RA-GRS                  | Resource Manager, clásico    | Cifrados              |
| BlockBlobStorage   | Blob (solo blobs en bloques y blobs en anexos) | Premium                       | N/D                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Cifrados              |
| FileStorage   | Solo archivo | Premium                       | N/D                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Cifrados              |
| BlobStorage         | Blob (solo blobs en bloques y blobs en anexos) | Estándar                      | Frecuente, esporádico, archivo<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Resource Manager             | Cifrados              |

<div id="deployment-model"><sup>1</sup>Se recomienda usar el modelo de implementación de Azure Resource Manager. Las cuentas de almacenamiento que usan el modelo de implementación clásico aún se pueden crear en algunas ubicaciones; asimismo, se seguirán admitiendo las cuentas clásicas ya existentes. Para obtener más información, consulte <a href="/azure/azure-resource-manager/resource-manager-deployment-model">Implementación de Azure Resource Manager frente a la implementación clásica: Conozca los modelos de implementación y el estado de los recursos</a>.</div><br/>

<div id="encryption"><sup>2</sup>Todas las cuentas de almacenamiento se encriptan mediante Storage Service Encryption (SSE) para los datos en reposo. Para más información, consulte <a href="/azure/storage/common/storage-service-encryption">Cifrado del servicio Azure Storage para datos en reposo</a>.</div><br/>

<div id="archive"><sup>3</sup> El almacenamiento de archivo y el almacenamiento por niveles de blob solo admiten blobs en bloques. El nivel de archivo está disponible solo en el nivel de un blob individual y no en el nivel de cuenta de almacenamiento. Para obtener más información, consulte <a href="/azure/storage/blobs/storage-blob-storage-tiers">Azure Blob Storage: Niveles de almacenamiento de acceso frecuente, de acceso esporádico y de acceso de archivo</a>.</div><br/>

<div id="zone-redundant-storage"><sup>4</sup>El almacenamiento con redundancia de zona (ZRS) y el almacenamiento con redundancia de zona geográfica (GZRS/RA-GZRS) (versión preliminar) solo están disponibles para las cuentas estándar de uso general V2, BlockBlobStorage y FileStorage en determinadas regiones. Para más información sobre las opciones de redundancia de Azure Storage, consulte <a href="/azure/storage/common/storage-redundancy">Redundancia de Azure Storage</a>.</div><br/>

<div id="premium-performance"><sup>5</sup>El rendimiento prémium para las cuentas v2 y v1 de uso general está disponible solo para blobs en páginas y en disco. El rendimiento Premium para blobs en bloque o en anexos solo está disponible en las cuentas de BlockBlobStorage. El rendimiento Premium para archivos solo está disponible en las cuentas de FileStorage.</div><br/>

<div id="data-lake-gen2"><sup>6</sup>Azure Data Lake Storage Gen2 es un conjunto de funcionalidades dedicadas al análisis de macrodatos basado en Azure Blob Storage. Data Lake Storage Gen2 solo se admite en las cuentas de almacenamiento de uso general V2 con un espacio de nombres jerárquico habilitado. Para más información sobre Data Lake Storage Gen2, consulte <a href="/azure/storage/blobs/data-lake-storage-introduction">Introducción a Azure Data Lake Storage Gen2</a>.</div>