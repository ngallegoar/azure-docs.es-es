---
title: 'Guía para desarrolladores: IoT Plug and Play (versión preliminar) | Microsoft Docs'
description: Descripción del modelado0 de dispositivos para desarrolladores de IoT Plug and Play
author: dominicbetts
ms.author: dobett
ms.date: 12/26/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 5fda51e6d2f62b9cbef0fcac22d5bb2ea0df905b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605225"
---
# <a name="iot-plug-and-play-preview-modeling-developer-guide"></a>Guía para desarrolladores de modelado de IoT Plug and Play (versión preliminar)

IoT Plug and Play (versión preliminar) permite crear dispositivos que anuncian sus funcionalidades en aplicaciones de Azure IoT. Los dispositivos IoT Plug and Play no requieren configuración manual cuando un cliente los conecta a aplicaciones habilitadas para IoT Plug and Play. IoT Central es un ejemplo de aplicación habilitada para IoT Plug and Play.

Para compilar un dispositivo IoT Plug and Play, es preciso crear una descripción del dispositivo. La descripción se realiza con un lenguaje de definición simple denominado lenguaje de definición de gemelos digitales (DTDL).

## <a name="device-capability-model"></a>Modelo de funcionalidad del dispositivo

Con DTDL, se crea un _modelo de funcionalidad del dispositivo_ para describir las partes del dispositivo. Habitualmente, los dispositivos IoT constan de:

- Elementos personalizados, que son las cosas que hacen que el dispositivo sea único.
- Elementos estándar, que son cosas comunes a todos los dispositivos.

Estos elementos se denominan _interfaces_ en un modelo de funcionalidad del dispositivo. Las interfaces definen los detalles de cada uno de los elementos que implementa el dispositivo.

En el ejemplo siguiente se muestra el modelo de funcionalidad del dispositivo de un dispositivo termostato:

```json
{
  "@id": "urn:example:Thermostat_T_1000:1",
  "@type": "CapabilityModel",
  "implements": [
    {
      "name": "thermostat",
      "schema": "urn:example:Thermostat:1"
    },
    {
      "name": "urn_azureiot_deviceManagement_DeviceInformation",
      "schema": "urn:azureiot:deviceManagement:DeviceInformation:1"
    }
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

Un modelo de funcionalidad tiene varios campos obligatorios:

- `@id`: un identificador único en forma de nombre de recursos uniforme simple.
- `@type`: declara que este objeto es un modelo de funcionalidad.
- `@context`: especifica la versión de DTDL que se usa para el modelo de funcionalidad.
- `implements`: enumera las interfaces que implementa el dispositivo.

Todas las entradas de la lista de interfaces de la sección de implementaciones tienen:

- `name`: el nombre de programación de la interfaz.
- `schema`: la interfaz que implementa el modelo de funcionalidad.

Hay campos opcionales adicionales que puede usar para agregar más detalles al modelo de funcionalidad, como el de nombre para mostrar y el de descripción. Las interfaces que se declaran dentro de un modelo de funcionalidad se pueden considerar componentes del dispositivo. En el caso de la versión preliminar pública, la lista de interfaces puede tener solo una entrada por esquema.

## <a name="interface"></a>Interfaz

Con DTDL se describen las funcionalidades del dispositivo mediante interfaces. Las interfaces describen las _propiedades_, la _telemetría_  y los _comandos_ que implementa un elemento del dispositivo:

- `Properties`. Las propiedades son campos de datos que representan el estado de un dispositivo. Se usan para representar el estado duradero del dispositivo, como el estado de encendido-apagado de una bomba de líquido refrigerante. Las propiedades también pueden representar propiedades básicas del dispositivo, como la versión del firmware del dispositivo. Las propiedades se pueden declarar como de solo lectura o de escritura.
- `Telemetry`. Los campos de telemetría representan las medidas de los sensores. Cada vez que un dispositivo toma una medida de un sensor, debe enviar un evento de telemetría que contenga los datos del sensor.
- `Commands`. Los comandos representan métodos que los usuarios del dispositivo pueden ejecutar en el dispositivo. Por ejemplo, un comando de restablecimiento o un comando para encender o apagar un ventilador.

En el ejemplo siguiente se muestra la interfaz de un dispositivo termostato:

```json
{
  "@id": "urn:example:Thermostat:1",
  "@type": "Interface",
  "contents": [
    {
      "@type": "Telemetry",
      "name": "temperature",
      "schema": "double"
    }
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

Una interfaz tiene varios campos obligatorios:

- `@id`: un identificador único en forma de nombre de recursos uniforme simple.
- `@type`: declara que este objeto es una interfaz.
- `@context`: especifica la versión de DTDL que se usa para la interfaz.
- `contents`: enumera las propiedades, la telemetría y los comandos que componen el dispositivo.

En este sencillo ejemplo, hay solo un campo de telemetría. Una descripción de campo mínima tiene:

- `@type`: especifica el tipo de funcionalidad: `Telemetry`, `Property`o `Command`.
- `name`: proporciona el nombre del valor de telemetría.
- `schema`: especifica el tipo de datos de la telemetría. Este valor puede ser un tipo primitivo, como doble, entero, booleano o cadena. También se admiten tipos de objetos complejos, matrices y mapas.

Otros campos opcionales, como nombre para mostrar y descripción, permiten agregar más detalles a la interfaz y a las funcionalidades.

### <a name="properties"></a>Propiedades

De forma predeterminada, las propiedades son de solo lectura. Las propiedades de solo lectura significan que el dispositivo informa de las actualizaciones de valores de propiedad a su centro de IoT. Su centro de IoT Hub no puede establecer el valor de una propiedad de solo lectura.

Una propiedad también se puede marcar como editable en una interfaz. Un dispositivo puede recibir una actualización de una propiedad editable desde su centro de IoT, así como notificar las actualizaciones de los valores de propiedad al centro.

No es preciso que los dispositivos estén conectados para establecer los valores de propiedad. Los valores actualizados se transfieren la siguiente vez que el dispositivo se conecta al centro. Este comportamiento se aplica a tanto a las propiedades de solo lectura como a las de escritura.

No use las propiedades para enviar datos de telemetría desde un dispositivo. Por ejemplo, una propiedad de solo lectura como m`temperatureSetting=80` debe significar que la temperatura del dispositivo se ha establecido en 80 y que el dispositivo está intentando llegar a esta temperatura o quedarse en ella.

En el caso de las propiedades de escritura, la aplicación de dispositivo devuelve un código de estado deseado, una versión y una descripción para indicar si ha recibido y aplicado el valor de propiedad.

### <a name="telemetry"></a>Telemetría

De manera predeterminada, IoT Hub enruta todos los mensajes de telemetría desde los dispositivos a su [punto de conexión orientado al servicio integrado (**messages/events**)](../iot-hub/iot-hub-devguide-messages-read-builtin.md) que es compatible con [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/).

Puede usar las [reglas de enrutamiento y los puntos de conexión personalizados de IoT Hub](../iot-hub/iot-hub-devguide-messages-d2c.md) para enviar datos de telemetría a otros destinos, como el almacenamiento de blobs u otros centros de eventos. Las reglas de enrutamiento usan las propiedades de los mensajes para seleccionar mensajes.

### <a name="commands"></a>Comandos:

Los comandos son sincrónicos o asincrónicos. Un comando sincrónico debe ejecutarse en un plazo máximo de 30 segundos de forma predeterminada y el dispositivo debe estar conectado cuando llegue el comando. Si el dispositivo no responde a tiempo o no está conectado, se produce un error en el comando.

Use comandos asincrónicos para las operaciones de ejecución prolongada. El dispositivo envía la información del progreso mediante mensajes de telemetría. Estos mensajes tienen las siguientes propiedades del encabezado:

- `iothub-command-name`: el nombre del comando, por ejemplo `UpdateFirmware`.
- `iothub-command-request-id`: el identificador de solicitud generado en el servidor y enviado al dispositivo en la llamada inicial.
- `iothub-interface-id`:  el identificador de la interfaz en la que está definido este comando, por ejemplo `urn:example:AssetTracker:1`.
 `iothub-interface-name`: el nombre de instancia de esta interfaz, por ejemplo `myAssetTracker`.
- `iothub-command-statuscode`: el código de estado que devuelve el dispositivo, por ejemplo `202`.

## <a name="register-a-device"></a>Registrar un dispositivo

IoT Plug and Play facilita la publicidad de las funcionalidades de cualquier dispositivo. Con IoT Plug and Play, después de que el dispositivo se conecte a IoT Hub, debe registrar el modelo de funcionalidad del dispositivo. El registro permite a los clientes usar las funcionalidades de IoT Plug and Play del dispositivo.

En esta guía se muestra cómo registrar un dispositivo mediante el SDK de dispositivo IoT de Azure para C.

Para cada interfaz que implemente el dispositivo, es preciso crear una interfaz y conectarla a su implementación.

En el caso de la interfaz del termostato que se ha mostrado anteriormente, mediante el SDK de C, creará y conectará la interfaz del termostato a su implementación:

```c
DIGITALTWIN_INTERFACE_HANDLE thermostatInterfaceHandle;

DIGITALTWIN_CLIENT_RESULT result = DigitalTwin_InterfaceClient_Create(
    "thermostat",
    "urn:example:Thermostat:1",
    null, null,
    &thermostatInterfaceHandle);

result = DigitalTwin_Interface_SetCommandsCallbacks(
    thermostatInterfaceHandle,
    commandsCallbackTable);

result = DigitalTwin_Interface_SetPropertiesUpdatedCallbacks(
    thermostatInterfaceHandle,
    propertiesCallbackTable);

```

Repita este código en cada interfaz que implemente el dispositivo.

Después de crear una interfaz, registre el modelo de funcionalidad del dispositivo y las interfaces en su centro de IoT:

```c
DIGITALTWIN_INTERFACE_CLIENT_HANDLE interfaces[2];
interfaces[0] = thermostatInterfaceHandle;
interfaces[1] = deviceInfoInterfaceHandle;

result = DigitalTwin_DeviceClient_RegisterInterfacesAsync(
    digitalTwinClientHandle, // The handle for the connection to Azure IoT
    "urn:example:Thermostat_T_1000:1",
    interfaces, 2,
    null, null);
```

## <a name="use-a-device"></a>Uso de un dispositivo

IoT Plug and Play le permite usar dispositivos que han registrado sus funcionalidades en su centro de IoT. Por ejemplo, puede acceder directamente a las propiedades y los comandos de un dispositivo.

Para usar un dispositivo IoT Plug and Play que esté conectado a su centro de IoT, use la API REST de IoT Hub o uno de los SDK de lenguaje de IoT. En los ejemplos siguientes se usa la API REST de IoT Hub. La versión actual de la API es `2019-07-01-preview`. Anexe `?api-version=2019-07-01-preview` a las llamadas a la API REST.

Para obtener el valor de una propiedad de dispositivo, como la versión del firmware (`fwVersion`) en la `DeviceInformation` interfaz del termostato, se usa la API REST de gemelos digitales.

Si se llama al dispositivo termostato `t-123`, logrará que su dispositivo implemente todas las propiedades con una llamada GET de la API REST:

```REST
GET /digitalTwins/t-123/interfaces
```

En general, con esta plantilla de API REST en la que `{device-id}` es el identificador del dispositivo se accede a todas las propiedades de todas las interfaces:

```REST
GET /digitalTwins/{device-id}/interfaces
```

Si conoce el nombre de la interfaz, como `deviceInformation`, y desea obtener las propiedades de esa interfaz en concreto, asigne el ámbito de la solicitud a una interfaz específica por el nombre:

```REST
GET /digitalTwins/t-123/interfaces/deviceInformation
```

En general, se puede acceder a las propiedades de una interfaz específica a través de esta plantilla de API REST, donde `device-id` es el identificador del dispositivo y `{interface-name}` es el nombre de la interfaz:

```REST
GET /digitalTwins/{device-id}/interfaces/{interface-name}
```

Puede llamar directamente a los comandos de dispositivo IoT Plug and Play. Si la interfaz `Thermostat` del `t-123` dispositivo tiene un comando `restart` en el que se puede usar una llamada POST de API REST para llamarlo:

```REST
POST /digitalTwins/t-123/interfaces/thermostat/commands/restart
```

En general, se puede llamar a los comandos mediante esta plantilla de API REST:

- `device-id`: el identificador del dispositivo.
- `interface-name`: el nombre de la interfaz de la sección de implementaciones del modelo de funcionalidad del dispositivo.
- `command-name`: el nombre del comando.

```REST
/digitalTwins/{device-id}/interfaces/{interface-name}/commands/{command-name}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido acerca del modelado de dispositivos, estos son algunos recursos adicionales:

- [Lenguaje de definición de gemelos digitales (DTDL)](https://aka.ms/DTDL)
- [SDK para dispositivos C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [API REST de IoT](https://docs.microsoft.com/rest/api/iothub/device)
