---
title: 'Entrada de datos de telemetría y conectividad de dispositivo: Azure Digital Twins | Microsoft Docs'
description: Aprenda a conectar, incorporar y enviar telemetría desde un dispositivo IoT en Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: 5c2c519ece9806b92c3e455d5f550bc2abfc9f3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75862482"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Entrada de datos de telemetría y conectividad de dispositivo

Los datos de telemetría enviados por los dispositivos y sensores son la columna de cualquier solución de IoT. La manera de representar estos diferentes recursos y de administrarlos en el contexto de una ubicación es una de las principales preocupaciones en el desarrollo de aplicaciones de IoT. Azure Digital Twins simplifica el proceso de desarrollar soluciones de IoT al unir dispositivos y sensores con un grafo de inteligencia espacial.

Para empezar, cree un recurso de Azure IoT Hub en la raíz del grafo espacial. El recurso IoT Hub permite a todos los dispositivos situados debajo del espacio raíz enviar mensajes. Después de crear la instancia de IoT Hub, registre los dispositivos con sensores dentro de la instancia de Digital Twins. Los dispositivos pueden enviar datos a un servicio de Digital Twins mediante el [SDK de dispositivo IoT de Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks).

Para obtener una guía paso a paso sobre cómo incorporar dispositivos, lea el [tutorial para implementar y configurar Digital Twins](tutorial-facilities-setup.md). En resumen, los pasos son:

- Implementar una instancia de Digital Twins desde [Azure Portal](https://portal.azure.com)
- Crear espacios en el grafo
- Crear un recurso de IoT Hub y asignarlo a un espacio en el grafo
- Crear dispositivos y sensores en el grafo y asignarlos a los espacios creados en los pasos anteriores
- Crear un buscador de coincidencias para filtrar los mensajes de telemetría en función de las condiciones
- Crear una [función definida por el usuario](concepts-user-defined-functions.md) y asignarla a un espacio en el grafo para el procesamiento personalizado de los mensajes de telemetría
- Asignar un rol para permitir que la función definida por el usuario acceda a los datos del grafo
- Obtener la cadena de conexión del dispositivo de IoT Hub de las API de administración de Digital Twins
- Configurar la cadena de conexión de dispositivo en el dispositivo con el SDK de dispositivo IoT de Azure

En las siguientes secciones, va a aprender a obtener la cadena de conexión del dispositivo de IoT Hub de la API de administración de Digital Twins. También aprenderá a utilizar el formato de mensajes de telemetría de IoT Hub para enviar telemetría basada en sensores. Digital Twins requiere que cada pieza de telemetría que recibe esté asociada a un sensor dentro del grafo espacial. Este requisito garantiza que los datos se procesen y enruten dentro del contexto espacial apropiado.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>Obtención de la cadena de conexión del dispositivo de IoT Hub de la API de administración

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Haga una llamada GET en la API de dispositivo con un parámetro `includes=ConnectionString` para obtener la cadena de conexión del dispositivo de IoT Hub. Puede filtrar por GUID de dispositivo o por el identificador de hardware para encontrar el dispositivo específico.

```plaintext
YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_GUID?includes=ConnectionString
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_DEVICE_GUID* | El identificador del dispositivo |

```plaintext
YOUR_MANAGEMENT_API_URL/devices?HardwareIds=YOUR_DEVICE_HARDWARE_ID&includes=ConnectionString
```

| Valor del parámetro | Reemplazar por |
| --- | --- |
| *YOUR_DEVICE_HARDWARE_ID* | El id. del hardware del dispositivo |

En la carga de la respuesta, copie la propiedad **connectionString** del dispositivo. Se usa cuando se llama al SDK de dispositivo IoT de Azure para enviar datos a Digital Twins.

## <a name="device-to-cloud-message"></a>Mensaje del dispositivo a la nube

Puede personalizar la carga y el formato del mensaje del dispositivo para ajustarse a las necesidades de su solución. Utilice cualquier contrato de datos que se pueda serializar en una matriz de bytes o secuencia que sea compatible con la [clase de mensaje de cliente de dispositivo IoT de Azure, Message(byte[] byteArray)](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___). El mensaje puede estar en un formato binario personalizado de su elección, siempre que descodifique el contrato de datos en una función definida por el usuario correspondiente. Hay solo un requisito para un mensaje del dispositivo a la nube. Mantenga un conjunto de propiedades para asegurarse de que el mensaje se enruta correctamente al motor de procesamiento.

### <a name="telemetry-properties"></a>Propiedades de telemetría

 El contenido de la carga de un **mensaje** pueden ser datos arbitrarios con un tamaño de hasta 256 KB. Se esperan algunos requisitos para las propiedades del tipo [`Message.Properties`](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet). La tabla muestra las propiedades obligatorias y opcionales admitidas por el sistema.

| Nombre de propiedad | Value | Obligatorio | Descripción |
|---|---|---|---|
| **DigitalTwins-Telemetry** | 1.0 | Sí | Valor constante que identifica un mensaje en el sistema. |
| **DigitalTwins-SensorHardwareId** | `string(72)` | Sí | Identificador único del sensor que envía el **mensaje**. Este valor debe coincidir con la propiedad **HardwareId** de un objeto para que el sistema lo procese. Por ejemplo, `00FF0643BE88-CO2`. |
| **CreationTimeUtc** | `string` | No | Cadena de fecha con formato según [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) que identifica el tiempo de muestreo de la carga. Por ejemplo, `2018-09-20T07:35:00.8587882-07:00`. |
| **CorrelationId** | `string` | No | UUID que puede usarse para el seguimiento de eventos en todo el sistema. Por ejemplo, `cec16751-ab27-405d-8fe6-c68e1412ce1f`.

### <a name="send-your-message-to-digital-twins"></a>Envío de mensajes a Digital Twins

Use la llamada [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) o [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet) de DeviceClient para enviar el mensaje a Digital Twins.

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre el procesamiento de datos y funcionalidades de las funciones definidas por el usuario de Azure Digital Twins, lea [Procesamiento de datos y funciones definidas por el usuario con Azure Digital Twins](concepts-user-defined-functions.md).
