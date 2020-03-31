---
title: Límites y cuotas de IoT Plug and Play (versión preliminar) | Microsoft Docs
description: Comprenda los límites, las cuotas y las limitaciones que se aplican al usar IoT Plug and Play (versión preliminar).
author: miagdp
ms.author: miag
ms.date: 12/26/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 48ecaaba6d956efd9da75d0582fa06d231cb3f80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75531384"
---
# <a name="iot-plug-and-play-preview-limits-quotas-and-throttles"></a>Límites, cuotas y limitaciones de IoT Plug and Play (versión preliminar)

En este artículo se explican los límites, las cuotas y las limitaciones específicos de IoT Plug and Play que se aplican en la versión preliminar pública. También se aplican [cuotas y limitaciones de IoT Hub](../iot-hub/iot-hub-devguide-quotas-throttling.md) existentes.

## <a name="iot-hub"></a>IoT Hub

En la versión preliminar pública, los límites y las cuotas siguientes se aplican a un centro de IoT:

| Límites, restricciones y limitaciones | Value | Notas |
|-----|-----|-----|
| Número de modelos de funcionalidad del dispositivo (DCM) o interfaces que se pueden registrar por centro | 1\.500 ||
| Número máximo de interfaces que se pueden registrar por dispositivo | 40 ||
| Número máximo de DCM que se pueden registrar por dispositivo | 1 ||
| Tamaño máximo del archivo de interfaz o DCM | 512 caracteres ||
| Tamaño máximo de un nombre de interfaz | 256 caracteres ||
| Tamaño máximo de un nombre de propiedad  | 64 bytes, 7 niveles en profundidad (y el primer nivel está reservado para `$iotin`) | Caracteres permitidos: a-z, A-Z, 0-9 (no como primer carácter) y subrayado. |
| Tamaño máximo de un valor de propiedad | 512 bytes ||
| Tamaño máximo de un nombre de comando | 100 bytes ||
| Tamaño del dispositivo gemelo | Igual que [Límites de IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) ||
| Resolución de llamadas API por las SKU (independientemente de las unidades) | 100 solicitudes por segundo ||

## <a name="model-repository"></a>Repositorio de modelos

En la versión preliminar pública, los límites y las cuotas siguientes se aplican a un repositorio de modelos:

| Límites, restricciones y limitaciones| Value |
|-----|-----|
| Número de repositorios de modelos de empresa por inquilino de Azure Active Directory | 1 |
| Número de claves de autorización por repositorio de modelos | 10  |
| Número de modelos (DCM o interfaces) por repositorio de modelos de empresa| 1\.500  |
| Número de modelos (DCM o interfaces) en el repositorio de modelos público por inquilino de Azure Active Directory| 1\.500  |
| Número de DCM o interfaces que se van a eliminar en un repositorio de modelos de empresa | 10 consultas por segundo (QPS)|
| Número de repositorios de modelos que un inquilino crea o actualiza| 1 QPS |
| Número de claves de autorización que se crean, actualizan o eliminan en un repositorio de modelos | 1 QPS|
| Número de DCM que se crean en un repositorio de modelos de empresa | 10 QPS |
| Número de interfaces que se crean en un repositorio de modelos de empresa | 10 QPS|
| Número de DCM que se crean en el repositorio de modelos público | 10 QPS|
| Número de interfaces que se crean en el repositorio de modelos público | 10 QPS|

## <a name="parser-library"></a>Biblioteca de analizador

La biblioteca del analizador sigue los límites que se aplican al [lenguaje de definición de gemelos digitales](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL).

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso sugerido sirve para aprender a [conectarse con un dispositivo IoT Plug and Play e interactuar con él](./howto-develop-solution.md).
