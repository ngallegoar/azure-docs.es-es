---
title: Límites de Azure Stack Edge Pro | Microsoft Docs
description: Obtenga información sobre los límites y tamaños recomendados a medida que implementa y opera Azure Stack Edge Pro, incluidos los límites de servicio, dispositivos y almacenamiento.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 10/12/2020
ms.author: alkohli
ms.openlocfilehash: f1bb5662b48765c08ec58d1f7fa9d341de4937bc
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91992754"
---
# <a name="azure-stack-edge-pro-limits"></a>Límites de Azure Stack Edge Pro

Tenga en cuenta estos límites cuando implemente y use su solución de Microsoft Azure Stack Edge Pro. 

## <a name="azure-stack-edge-service-limits"></a>Límites del servicio de Azure Stack Edge

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="azure-stack-edge-device-limits"></a>Límites del dispositivo Azure Stack Edge

En la siguiente tabla se describen los límites de los dispositivos Azure Stack Edge Pro. 

En la tabla siguiente, se describen los límites de los dispositivos de Azure Stack Edge.

| Descripción | Value |
|---|---|
|No. de archivos por dispositivo |100 millones |
|No. de recursos compartidos por contenedor |1 |
|N.º máximo de puntos de conexión de uso compartido y puntos de conexión REST por dispositivo| 24 |
|N.º máximo de cuentas de almacenamiento con niveles por dispositivo| 24|
|Tamaño máximo de archivo escrito en un recurso compartido| 5 TB |
|Número máximo de grupos de recursos por dispositivo| 800 |

## <a name="azure-storage-limits"></a>Límites de almacenamiento de Azure

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Advertencias al cargar los datos

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Límites de tamaño objeto y de tamaño de cuenta de almacenamiento en Azure

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]


## <a name="azure-object-size-limits"></a>Límites de tamaño de objeto de Azure

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Stack Edge Pro](azure-stack-edge-deploy-prep.md)
