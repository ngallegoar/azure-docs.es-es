---
title: Migración de Azure IoT Hub a la configuración de diagnóstico | Microsoft Docs
description: Se describe cómo actualizar Azure IoT Hub para usar la configuración de diagnóstico de Azure en lugar de la supervisión de operaciones para controlar el estado de las operaciones de IoT Hub en tiempo real.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: kgremban
ms.openlocfilehash: ab07da38c01b052a4220274fb059683a22950a3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75750701"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migración de la supervisión de operaciones de IoT Hub a la configuración de diagnóstico

Los clientes que usan la [supervisión de operaciones](iot-hub-operations-monitoring.md) para realizar un seguimiento del estado de las operaciones en IoT Hub pueden migrar dicho flujo de trabajo a la [configuración de diagnóstico de Azure](../azure-monitor/platform/platform-logs-overview.md), una característica de Azure Monitor. La configuración de diagnóstico proporciona información de diagnóstico a nivel de recurso para muchos servicios de Azure.

**La funcionalidad de la supervisión de operaciones de IoT Hub está en desuso** y, por tanto, se ha quitado del portal. En este artículo se explican los pasos para migrar las cargas de trabajo de la supervisión de operaciones a la configuración de diagnóstico. Para más información sobre la escala de tiempo de desuso, consulte [Monitor your Azure IoT solutions with Azure Monitor and Azure Resource Health](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/) (Supervisión de las soluciones de Azure IoT con Azure Monitor y Azure Resource Health).

## <a name="update-iot-hub"></a>Actualización de IoT Hub

Para actualizar IoT Hub en Azure Portal, primero active la configuración de diagnóstico y luego desactive la supervisión de operaciones.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Desactivación de la supervisión de operaciones

> [!NOTE]
> A partir del 11 de marzo de 2019, la característica de supervisión de operaciones se quita de la interfaz de Azure Portal de IoT Hub. Los pasos siguientes ya no se aplican. Para realizar la migración, asegúrese de que las categorías adecuadas están activadas en la configuración de diagnóstico anterior de Azure Monitor.

Cuando haya probado la nueva configuración de diagnóstico en el flujo de trabajo, puede desactivar la característica de supervisión de operaciones. 

1. En el menú de IoT Hub, seleccione **Supervisión de operaciones**.

2. En cada categoría de supervisión, seleccione **Ninguno**.

3. Guarde los cambios de la supervisión de operaciones.

## <a name="update-applications-that-use-operations-monitoring"></a>Actualización de las aplicaciones que usan la supervisión de operaciones

Los esquemas para la supervisión de operaciones y la configuración de diagnóstico varían ligeramente. Es importante que actualice las aplicaciones que utilizan la supervisión de operaciones actualmente para asignarlas al esquema que la configuración de diagnóstico utiliza. 

Además, la configuración de diagnóstico ofrece cinco categorías nuevas para seguimiento. Después de actualizar las aplicaciones para el esquema existente, agregue también las nuevas categorías:

* Operaciones gemelas de la nube al dispositivo
* Operaciones gemelas del dispositivo a la nube
* Consultas gemelas
* Operaciones de trabajos
* Métodos directos

Para las estructuras de esquema específicas, consulte la [información sobre el esquema de la configuración de diagnóstico](iot-hub-monitor-resource-health.md#understand-the-logs).

## <a name="monitoring-device-connect-and-disconnect-events-with-low-latency"></a>Supervisión de los eventos de conexión y desconexión del dispositivo con baja latencia

Para supervisar los eventos de conexión y desconexión de dispositivos, se recomienda suscribirse al [evento **dispositivo desconectado**](iot-hub-event-grid.md#event-types) en Event Grid para obtener alertas y supervisar el estado de conexión del dispositivo. Siga este [tutorial](iot-hub-how-to-order-connection-state-events.md) para aprender a integrar eventos de dispositivo conectado y dispositivo desconectado de IoT Hub en la solución de IoT.

## <a name="next-steps"></a>Pasos siguientes

[Supervisión del mantenimiento de Azure IoT Hub y diagnóstico de problemas rápidamente](iot-hub-monitor-resource-health.md)
