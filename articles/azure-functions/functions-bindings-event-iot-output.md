---
title: Enlace de salida de Azure IoT Hub para Azure Functions
description: Aprenda a escribir mensajes en secuencias de Azure IoT Hub mediante Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: d4dbf43fb5684d829e581be29832e94ad46b2936
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91871786"
---
# <a name="azure-iot-hub-output-binding-for-azure-functions"></a>Enlace de salida de Azure IoT Hub para Azure Functions

En este artículo se explica cómo trabajar con enlaces de salida de Azure Functions para IoT Hub. La compatibilidad de IoT Hub se basa en el [enlace de Azure Event Hubs](functions-bindings-event-hubs.md).

Para obtener información sobre los detalles de instalación y configuración, vea la [información general](functions-bindings-event-iot.md).

> [!IMPORTANT]
> Aunque los ejemplos de código siguientes usan la API de Event Hubs, la sintaxis proporcionada es aplicable para las funciones de IoT Hub.

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs-output.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Responder a los eventos enviados a una secuencia de eventos del centro de eventos (desencadenador)](./functions-bindings-event-iot-trigger.md)
