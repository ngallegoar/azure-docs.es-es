---
title: Administración de modelos de la versión preliminar de IoT Plug and Play en el repositorio | Microsoft Docs
description: Cómo administrar modelos de funcionalidad del dispositivo en el repositorio mediante el portal de Azure Certified for IoT, la CLI de Azure y Visual Studio Code.
author: Philmea
manager: philmea
ms.service: iot-pnp
services: iot-pnp
ms.topic: how-to
ms.date: 12/26/2019
ms.author: philmea
ms.openlocfilehash: 78406175090521af70381f61f4e33dfb6c35ac8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159156"
---
# <a name="manage-models-in-the-repository"></a>Administración de modelos en el repositorio

En el repositorio de modelos de la versión preliminar de IoT Plug and Play se almacenan las interfaces y los modelos de funcionalidad del dispositivo. En este repositorio, los desarrolladores de soluciones podrán encontrar y detectar las interfaces y los modelos.

Existen tres herramientas que se pueden usar para administrar el repositorio:

- El portal de Azure Certified for IoT
- La CLI de Azure
- Visual Studio Code

## <a name="model-repositories"></a>Repositorios de modelos

Existen dos tipos de repositorio de modelos donde almacenar las interfaces y los modelos de funcionalidad del dispositivo:

- Hay un único _repositorio público_ donde se almacenan las interfaces y los modelos de funcionalidad del dispositivo correspondientes a los dispositivos incluidos en el [catálogo de dispositivos Azure Certified for IoT](https://aka.ms/iotdevcat). En este repositorio se almacenan también las [interfaces comunes](./concepts-common-interfaces.md) y los [DCM e interfaces que publican los asociados de Microsoft](./howto-onboard-portal.md). Para más información sobre cómo certificar un dispositivo y agregar su modelo de funcionalidad de dispositivo al repositorio público, vea el tutorial [Certificación del dispositivo de IoT Plug and Play](./tutorial-certification-test.md).
- Existen varios _repositorios de empresa_. Los repositorios de empresa de la organización se crean automáticamente al [incorporarse al portal de Azure Certified for IoT](./howto-onboard-portal.md). Puede usar el repositorio de empresa para almacenar las interfaces y los modelos de funcionalidad del dispositivo durante el desarrollo y las pruebas.

## <a name="azure-certified-for-iot-portal"></a>Portal de Azure Certified for IoT

En el [portal de Azure Certified for IoT](https://preview.catalog.azureiotsolutions.com) se pueden realizar las siguientes tareas:

- [Completar el proceso de certificación del dispositivo IoT](./tutorial-certification-test.md).
- Buscar modelos de funcionalidad del dispositivo de IoT Plug and Play. Estos modelos se pueden usar para [crear rápidamente dispositivos listos para IoT e integrarlos con soluciones](./quickstart-connect-pnp-device-solution-node.md).

## <a name="azure-cli"></a>Azure CLI

La CLI de Azure pone a disposición comandos para administrar interfaces y modelos de funcionalidad del dispositivo en los repositorios de modelo público y de empresa de IoT Plug and Play. Para obtener más información, consulte la guía paso a paso [Instalación y uso de la extensión de Azure IoT para la CLI de Azure](./howto-install-pnp-cli.md).

## <a name="visual-studio-code"></a>Visual Studio Code

Para abrir la vista **Model Repository** (Repositorio de modelos) en Visual Studio Code:

1. Abra Visual Studio Code, use **Ctrl+Mayús+P**, escriba y seleccione **IoT Plug and Play: Open Model Repository** (IoT Plug and Play: Abrir el repositorio de modelos).

1. Puede optar entre **Open Public Model Repository** (Abrir repositorio de modelos público) u **Open Organizational Model Repository** (Abrir repositorio de modelos de empresa). Si elige el repositorio de modelos de empresa, debe especificar la cadena de conexión del repositorio de modelos. Encontrará esta cadena de conexión en el [portal de Azure Certified for IoT](https://preview.catalog.azureiotsolutions.com), en la pestaña **Connection strings** (Cadenas de conexión) del **repositorio de la empresa**.

1. Se abre una pestaña nueva en la vista **Model Repository** (Repositorio de modelos).

    Use esta vista para agregar, descargar y eliminar interfaces y modelos de funcionalidad del dispositivo. Puede usar un filtro para encontrar elementos concretos dentro de la lista.

1. Para cambiar entre el repositorio de modelos de empresa y el repositorio de modelos público, use **Ctrl+Mayús+P**, escriba y seleccione **IoT Plug and Play: Sign out Model Repository** (IoT Plug and Play: Cerrar sesión en el repositorio de modelos). Luego, vuelva a usar el comando **IoT Plug and Play: Open Model Repository** (IoT Plug and Play: Abrir el repositorio de modelos).

> [!NOTE]
> En VS Code, el repositorio de modelos público es de solo lectura. Los asociados de Microsoft pueden actualizar el repositorio público en el [portal de Azure Certified for IoT](https://preview.catalog.azureiotsolutions.com).

## <a name="next-steps"></a>Pasos siguientes

El siguiente procedimiento sugerido sirve para aprender a [enviar un dispositivo IoT Plug and Play para certificarlo](tutorial-certification-test.md).
