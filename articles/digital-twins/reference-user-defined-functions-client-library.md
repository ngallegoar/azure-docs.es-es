---
title: Referencia de la biblioteca cliente de funciones definidas por el usuario (Azure Digital Twins) | Microsoft Docs
description: Documentación de referencia de la biblioteca cliente de funciones definidas por el usuario de Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: article
ms.date: 01/17/2020
ms.custom: seodec18
ms.openlocfilehash: bd6095daca51ddca0cfb4b34ca86e763df9a3d02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76276809"
---
# <a name="user-defined-functions-client-library-reference"></a>Referencia de la biblioteca cliente de funciones definidas por el usuario

En este documento se proporciona información de referencia para la biblioteca cliente de funciones definidas por el usuario de Azure Digital Twins.

## <a name="helper-methods"></a>Métodos auxiliares

La biblioteca cliente define los métodos auxiliares para las operaciones utilizadas con frecuencia.

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Dado un identificador de espacio, esta función recupera el espacio del grafo.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Identificador del espacio |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Dado un identificador de sensor, esta función recupera el sensor del grafo.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Identificador del sensor |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Dado un identificador de dispositivo, esta función recupera el dispositivo del grafo.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Identificador de dispositivo |

### <a name="getsensorvaluesensorid-datatype--value"></a>getSensorValue(sensorId, dataType) ⇒ `value`

Dado un identificador de sensor y su tipo de datos, esta función recupera el valor actual de ese sensor.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | Identificador del sensor |
| *dataType*  | `string` | Tipo de datos del sensor |

### <a name="getspacevaluespaceid-valuename--value"></a>getSpaceValue(spaceId, valueName) ⇒ `value`

Dado un identificador de espacio y el nombre del valor, esta función recupera el valor actual de esa propiedad de espacio.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | Identificador del espacio |
| *valueName* | `string` | Nombre de la propiedad de espacio |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>getSensorHistoryValues(sensorId, dataType) ⇒ `value[]`

Dado un identificador de sensor y su tipo de datos, esta función recupera el historial de valores de ese sensor.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador del sensor |
| *dataType* | `string` | Tipo de datos del sensor |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>getSpaceHistoryValues(spaceId, dataType) ⇒ `value[]`

Dado un identificador de espacio y el nombre del valor, esta función recupera el historial de valores de esa propiedad de espacio.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |
| *valueName* | `string` | Nombre de la propiedad de espacio |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Dado un identificador de espacio, esta función recupera los espacios secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Dado un identificador de espacio, esta función recupera los sensores secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Dado un identificador de espacio, esta función recupera los dispositivos secundarios de ese espacio primario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Dado un identificador de espacio, esta función recupera los sensores secundarios de ese dispositivo primario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Identificador de dispositivo |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Dado un identificador de espacio, esta función recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | Identificador del espacio |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Dado un identificador de sensor, esta función recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Identificador del sensor |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Dado un identificador de dispositivo, esta función recupera su espacio primario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Identificador de dispositivo |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Dado un identificador de sensor, esta función recupera su dispositivo primario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Identificador del sensor |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>getSpaceExtendedProperty(spaceId, propertyName) ⇒ `extendedProperty`

Dado un identificador de espacio, esta función recupera la propiedad y su valor desde el espacio.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |
| *propertyName* | `string` | Nombre de la propiedad de espacio |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>getSensorExtendedProperty(sensorId, propertyName) ⇒ `extendedProperty`

Dado un identificador de sensor, esta función recupera la propiedad y su valor desde el sensor.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador del sensor |
| *propertyName* | `string` | Nombre de la propiedad del sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>getDeviceExtendedProperty(deviceId, propertyName) ⇒ `extendedProperty`

Dado un identificador de dispositivo, esta función recupera la propiedad y su valor desde el espacio.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Identificador de dispositivo |
| *propertyName* | `string` | Nombre de la propiedad del dispositivo |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue(sensorId, dataType, value)

Esta función establece un valor en el objeto de sensor con el tipo de datos indicado.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador del sensor |
| *dataType*  | `string` | Tipo de datos del sensor |
| *value*  | `string` | Value |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue(spaceId, dataType, value)

Esta función establece un valor en el objeto de espacio con el tipo de datos indicado.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador del espacio |
| *dataType* | `string` | Tipo de datos |
| *value* | `string` | Value |

### <a name="logmessage"></a>log(Message)

Esta función registra el mensaje siguiente dentro de la función definida por el usuario.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *message* | `string` | Mensaje que se va a registrar |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification(topologyObjectId, topologyObjectType, payload)

Esta función envía una notificación personalizada para que se distribuya.

**Tipo**: función global

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Identificador de objeto gráfico. Algunos ejemplos son el espacio, el sensor y el identificador de dispositivo.|
| *topologyObjectType*  | `string` | Algunos ejemplos son el sensor y el dispositivo.|
| *payload*  | `string` | La carga útil de JSON que se envía con la notificación. |

## <a name="return-types"></a>Tipos de valores devueltos

Debajo se describen los modelos de respuesta devueltos por métodos auxiliares de referencia de cliente.

### <a name="space"></a>Space

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "Space",
  "friendlyName": "Conference Room",
  "typeId": 0,
  "parentSpaceId": "00000000-0000-0000-0000-000000000001",
  "subtypeId": 0
}
```

### <a name="space-methods"></a>Métodos del espacio

#### <a name="parent--space"></a>Parent() ⇒ `space`

Esta función devuelve el espacio primario del espacio actual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Esta función devuelve los sensores secundarios del espacio actual.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Esta función devuelve los dispositivos secundarios del espacio actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Esta función devuelve la propiedad extendida y su valor para el espacio actual.

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nombre de la propiedad extendida |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Esta función devuelve el valor del espacio actual.

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Nombre del valor |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Esta función devuelve los valores históricos del espacio actual.

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Nombre del valor |

#### <a name="notifypayload"></a>Notify(Payload)

Esta función envía una notificación con la carga útil especificada.

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | la carga útil de JSON que se incluye en la notificación |

### <a name="device"></a>Dispositivo

```JSON
{
  "id": "00000000-0000-0000-0000-000000000002",
  "name": "Device",
  "friendlyName": "Temperature Sensing Device",
  "description": "This device contains a sensor that captures temperature readings.",
  "type": "None",
  "subtype": "None",
  "typeId": 0,
  "subtypeId": 0,
  "hardwareId": "ABC123",
  "gatewayId": "ABC",
  "spaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Métodos de dispositivo

#### <a name="parent--space"></a>Parent() ⇒ `space`

Esta función devuelve el espacio primario del dispositivo actual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Esta función devuelve los sensores secundarios del dispositivo actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Esta función devuelve la propiedad extendida y su valor para el dispositivo actual.

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nombre de la propiedad extendida |

#### <a name="notifypayload"></a>Notify(Payload)

Esta función envía una notificación con la carga útil especificada.

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | la carga útil de JSON que se incluye en la notificación |

### <a name="sensor"></a>Sensor

```JSON
{
  "id": "00000000-0000-0000-0000-000000000003",
  "port": "30",
  "pollRate": 3600,
  "dataType": "Temperature",
  "dataSubtype": "None",
  "type": "Classic",
  "portType": "None",
  "dataUnitType": "FahrenheitTemperature",
  "spaceId": "00000000-0000-0000-0000-000000000000",
  "deviceId": "00000000-0000-0000-0000-000000000001",
  "portTypeId": 0,
  "dataUnitTypeId": 0,
  "dataTypeId": 0,
  "dataSubtypeId": 0,
  "typeId": 0  
}
```

### <a name="sensor-methods"></a>Métodos del sensor

#### <a name="space--space"></a>Space() ⇒ `space`

Esta función devuelve el espacio primario del sensor actual.

#### <a name="device--device"></a>Device() ⇒ `device`

Esta función devuelve el dispositivo primario del sensor actual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Esta función devuelve la propiedad extendida y su valor para el sensor actual.

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nombre de la propiedad extendida |

#### <a name="value--value"></a>Value() ⇒ `value`

Esta función devuelve el valor del sensor actual.

#### <a name="history--value"></a>History() ⇒ `value[]`

Esta función devuelve los valores históricos del sensor actual.

#### <a name="notifypayload"></a>Notify(Payload)

Esta función envía una notificación con la carga útil especificada.

| Parámetro  | Tipo                | Descripción  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | la carga útil de JSON que se incluye en la notificación |

### <a name="value"></a>Value

```JSON
{
  "dataType": "Temperature",
  "value": "70",
  "createdTime": ""
}
```

### <a name="extended-property"></a>Propiedad extendida

```JSON
{
  "name": "OccupancyStatus",
  "value": "Occupied"
}
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [funciones definidas por el usuario de Azure Digital Twins](./concepts-user-defined-functions.md).

- Obtenga información sobre la [creación de funciones definidas por el usuario](./how-to-user-defined-functions.md).

- Obtenga información sobre la [depuración de funciones definidas por el usuario](./how-to-diagnose-user-defined-functions.md).
