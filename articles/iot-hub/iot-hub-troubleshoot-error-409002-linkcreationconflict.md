---
title: Solución de problemas del error 409002 de Azure IoT Hub LinkCreationConflict
description: Corrección del error 409002 LinkCreationConflict
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom: amqp
ms.openlocfilehash: 20d39b1f5a11f20eb5d12f34337787b382c820f6
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758741"
---
# <a name="409002-linkcreationconflict"></a>409002 LinkCreationConflict

En este artículo se describen las causas y las soluciones de los errores **409002 LinkCreationConflict**.

## <a name="symptoms"></a>Síntomas

Verá el error **409002 LinkCreationConflict** en los registros de diagnóstico junto con la desconexión del dispositivo o el error del mensaje de la nube al dispositivo. 

<!-- When using AMQP? -->

## <a name="cause"></a>Causa

Por lo general, este error se produce cuando IoT Hub detecta que un cliente tiene más de una conexión. De hecho, cuando llega una nueva solicitud de conexión para un dispositivo con una conexión existente, IoT Hub cierra la conexión existente con este error.

### <a name="cause-1"></a>Causa 1

Lo más frecuentes es un problema independiente (como [404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)) que hace que el dispositivo se desconecte. El dispositivo intenta restablecer la conexión inmediatamente, pero IoT Hub sigue teniendo en cuenta el dispositivo conectado. IoT Hub cierra la conexión anterior y registra el error.

### <a name="cause-2"></a>Causa 2

La lógica del dispositivo defectuosa hace que el dispositivo establezca la conexión cuando ya hay una abierta.

## <a name="solution"></a>Solución

Este error normalmente aparece como un efecto secundario de un problema transitorio diferente, así que busque otros errores en los registros para solucionar el problema. En caso contrario, asegúrese de emitir una nueva solicitud de conexión solo si la conexión se anula.
