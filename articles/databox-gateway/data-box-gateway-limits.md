---
title: Límites de Azure Data Box Gateway | Microsoft Docs
description: Se describen los límites del sistema y los tamaños recomendados de Microsoft Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 10/20/2020
ms.author: alkohli
ms.openlocfilehash: 15b01f92fe0d39d099c10c7c086790a4dbb91379
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96581199"
---
# <a name="azure-data-box-gateway-limits"></a>Límites de Azure Data Box Gateway

Tenga en cuenta estos límites cuando implemente y use su solución de Microsoft Azure Data Box Gateway.

## <a name="data-box-gateway-service-limits"></a>Límites de servicio de Data Box Gateway

[!INCLUDE [data-box-gateway-service-limits](../../includes/data-box-gateway-service-limits.md)]

## <a name="data-box-gateway-device-limits"></a>Límites de dispositivos de Data Box Gateway

En la tabla siguiente se describen los límites del dispositivo de Data Box Gateway.

| Descripción | Value |
|---|---|
|No. de archivos por dispositivo |100 millones <br> Por cada 25 millones de archivos que se agregan (con el límite máximo en 100 millones), debe agregar 2 TB de espacio en disco, 8 GB de RAM y 4 núcleos de CPU. |
|No. de recursos compartidos por dispositivo |24 |
|No. de recursos compartidos por contenedor de Azure Storage |1 |
|Tamaño máximo de archivo escrito en un recurso compartido|Para un dispositivo virtual de 2 TB, el tamaño máximo de archivo es de 500 GB. <br> El tamaño máximo de archivo aumenta con el tamaño del disco de datos en la relación anterior hasta que alcanza un máximo de 5 TB. |

## <a name="azure-storage-limits"></a>Límites de almacenamiento de Azure

[!INCLUDE [data-box-gateway-storage-limits](../../includes/data-box-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Advertencias al cargar los datos

[!INCLUDE [data-box-gateway-storage-data-upload-caveats](../../includes/data-box-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Límites de tamaño objeto y de tamaño de cuenta de almacenamiento en Azure

[!INCLUDE [data-box-gateway-storage-acct-limits](../../includes/data-box-gateway-storage-acct-limits.md)]

## <a name="azure-object-size-limits"></a>Límites de tamaño de objeto de Azure

[!INCLUDE [data-box-gateway-storage-object-limits](../../includes/data-box-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Preparación para implementar Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
