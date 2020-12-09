---
title: Límites de Azure Stack Edge Mini R | Microsoft Docs
description: Describe los límites del sistema y los tamaños recomendados de Azure Stack Edge Mini R.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: alkohli
ms.openlocfilehash: 6f01d62d1d11f5ff90661482ffd2db112657eee5
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465896"
---
# <a name="azure-stack-edge-mini-r-limits"></a>Límites de Azure Stack Edge Mini R


Tenga en cuenta estos límites cuando implemente y use su solución de Azure Stack Edge Mini R.

## <a name="azure-stack-edge-service-limits"></a>Límites del servicio de Azure Stack Edge

[!INCLUDE [azure-stack-edge-gateway-service-limits](../../includes/azure-stack-edge-gateway-service-limits.md)]

## <a name="azure-stack-edge-mini-r-device-limits"></a>Límites de dispositivos Azure Stack Edge Mini R

En la siguiente tabla se describen los límites de dispositivos Azure Stack Edge Mini R.

| Descripción | Límite|
|---|---:|
|No. de archivos por dispositivo | 100 millones <!--check with devs-->|
|No. de recursos compartidos por contenedor | 1|
|N.º máximo de puntos de conexión de uso compartido y puntos de conexión REST por dispositivo| 24 |
|N.º máximo de cuentas de almacenamiento con niveles por dispositivo| 24|
|Tamaño máximo de archivo escrito en un recurso compartido| 500 GB|
|Número máximo de grupos de recursos por dispositivo| 800|

## <a name="azure-storage-limits"></a>Límites de almacenamiento de Azure

[!INCLUDE [azure-stack-edge-gateway-storage-limits](../../includes/azure-stack-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Advertencias al cargar los datos

[!INCLUDE [azure-stack-edge-gateway-storage-data-upload-caveats](../../includes/azure-stack-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Límites de tamaño objeto y de tamaño de cuenta de almacenamiento en Azure

[!INCLUDE [azure-stack-edge-gateway-storage-acct-limits](../../includes/azure-stack-edge-gateway-storage-acct-limits.md)]

## <a name="azure-object-size-limits"></a>Límites de tamaño de objeto de Azure

[!INCLUDE [azure-stack-edge-gateway-storage-object-limits](../../includes/azure-stack-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Stack Edge](azure-stack-edge-gpu-deploy-prep.md)
