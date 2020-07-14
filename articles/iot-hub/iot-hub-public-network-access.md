---
title: Administración del acceso a la red pública para Azure IoT Hub
description: Documentación sobre cómo deshabilitar y habilitar el acceso a la red pública para IoT Hub
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/01/2020
ms.openlocfilehash: c82f98df8fb79fa10f2e30b219c1a02bb646e2de
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937474"
---
# <a name="managing-public-network-access-for-your-iot-hub"></a>Administración del acceso a la red pública para su instancia de IoT Hub

Para restringir el acceso solo al [punto de conexión privado para su instancia de IoT Hub en su red virtual](virtual-network-support.md), deshabilite el acceso a la red pública. Para ello, use Azure Portal o la API de `publicNetworkAccess`. 

## <a name="turn-off-public-network-access-using-azure-portal"></a>Desactivación del acceso a la red pública mediante Azure Portal

1. Visite [Azure Portal](https://portal.azure.com).
2. Vaya a su instancia de IoT Hub.
3. Seleccione **Redes** en el menú de la izquierda.
4. En "Allow public network access to" (Permitir acceso a la red pública para), seleccione **Deshabilitado**.
5. Seleccione **Guardar**.

:::image type="content" source="media/iot-hub-publicnetworkaccess/turn-off-public-network-access.png" alt-text="Imagen que muestra en Azure Portal dónde desactivar el acceso a la red pública" lightbox="media/iot-hub-publicnetworkaccess/turn-off-public-network-access.png":::

Para activar el acceso a la red pública, seleccione **Habilitado** y, a continuación, **Guardar**.

## <a name="ip-filter"></a>Filtro IP 

Si el acceso a la red pública está deshabilitado, todas las reglas de [Filtro IP](iot-hub-ip-filtering.md) se omiten. Esto se debe a que se bloquean todas las direcciones IP de la red pública de Internet. Para usar el filtro IP, utilice la opción **Selected IP ranges** (Intervalos IP seleccionados).

## <a name="bug-fix-with-built-in-event-hub-compatible-endpoint"></a>Corrección de errores con el punto de conexión compatible con el centro de eventos integrado

Hay un error con IoT Hub en el que el [punto de conexión compatible con el centro de eventos integrado](iot-hub-devguide-messages-read-builtin.md) sigue siendo accesible a través de la red pública de Internet cuando se deshabilita el acceso a la red pública para IoT Hub. Para obtener más información y ponerse en contacto con nosotros en relación con este error, consulte [Al deshabilitar el acceso de la red pública a Azure IoT Hub, se deshabilitará el acceso al punto de conexión integrado del centro de eventos integrado](https://azure.microsoft.com/updates/iot-hub-public-network-access-bug-fix).