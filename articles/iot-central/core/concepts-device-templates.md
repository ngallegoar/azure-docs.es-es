---
title: ¿Qué son las plantillas de dispositivos en Azure IoT Central | Microsoft Docs
description: Las plantillas de dispositivos de Azure IoT Central permiten especificar el comportamiento de los dispositivos conectados a la aplicación. Una plantilla de dispositivo especifica la telemetría, las propiedades y los comandos que el dispositivo debe implementar. Una plantilla de dispositivo también define la interfaz de usuario del dispositivo en IoT Central como los formularios y paneles que utiliza un operador.
author: dominicbetts
ms.author: dobett
ms.date: 11/05/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: device-developer
ms.openlocfilehash: e82a377d62184c8ae1d2e8f076b228e36005887a
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94992682"
---
# <a name="what-are-device-templates"></a>¿Qué son las plantillas de dispositivo?

_Este artículo se aplica a desarrolladores de dispositivos y generadores de soluciones._

Una plantilla de dispositivo en Azure IoT Central es un plano técnico que define las características y los comportamientos de un tipo de dispositivo que se conecta a la aplicación. Por ejemplo, la plantilla de dispositivo define la telemetría que un dispositivo envía para que IoT Central pueda crear visualizaciones que usen las unidades y los tipos de datos correctos.

Un generador de soluciones agrega plantillas de dispositivos a una aplicación de IoT Central. Un desarrollador de dispositivos escribe el código del dispositivo que implementa los comportamientos definidos en la plantilla de dispositivo.

En una plantilla de dispositivo se incluyen las secciones siguientes:

- _Un modelo de dispositivo_. Esta parte de la plantilla de dispositivo define cómo interactúa el dispositivo con la aplicación. Un desarrollador de dispositivos implementa los comportamientos definidos en el modelo.
    - _Componente predeterminado_. Cada modelo de dispositivo tiene un componente predeterminado. La interfaz del componente predeterminado describe las funcionalidades que son específicas del modelo de dispositivo.
    - _Componentes_. Un modelo de dispositivo puede incluir componentes además del componente predeterminado para describir las funcionalidades del dispositivo. Cada componente tiene una interfaz que describe las funcionalidades del componente. Las interfaces de los componentes se pueden reutilizar en otros modelos de dispositivo. Por ejemplo, varios modelos de dispositivos de teléfono pueden usar la misma interfaz de cámara.
    - _Interfaces heredadas_. Un modelo de dispositivo contiene una o más interfaces que amplían las funcionalidades del componente predeterminado.
- _Propiedades de la nube_. Esta parte de la plantilla de dispositivo permite al desarrollador de soluciones especificar los metadatos del dispositivo que se van a almacenar. Las propiedades de la nube nunca se sincronizan con los dispositivos y solo existen en la aplicación. Las propiedades de la nube no afectan al código que escribe el desarrollador de dispositivos para implementar el modelo de dispositivo.
- _Personalizaciones_. Esta parte de la plantilla de dispositivo permite que el desarrollador de soluciones reemplace algunas de las definiciones del modelo de dispositivo. Las personalizaciones son útiles si el desarrollador de soluciones desea restringir el modo en que la aplicación controla un valor, como cambiar el nombre para mostrar de una propiedad o el color usado para mostrar un valor de telemetría. Las personalizaciones no afectan al código que escribe el desarrollador de dispositivos para implementar el modelo de dispositivo.
- _Vistas_. Esta parte de la plantilla de dispositivo permite que el desarrollador de soluciones defina visualizaciones para ver los datos del dispositivo y los formularios para administrar y controlar un dispositivo. Las vistas usan el modelo de dispositivo, las propiedades de la nube y las personalizaciones. Las vistas no afectan al código que escribe el desarrollador de dispositivos para implementar el modelo de dispositivo.

## <a name="device-models"></a>Modelos de dispositivos

Un modelo de dispositivo define cómo interactúa un dispositivo con la aplicación de IoT Central. El desarrollador de dispositivos debe asegurarse de que el dispositivo implementa los comportamientos definidos en el modelo de dispositivo para que IoT Central pueda supervisar y administrar el dispositivo. Un modelo de dispositivo se compone de una o varias _interfaces_, y cada interfaz puede definir una colección de tipos de _telemetría_, _propiedades del dispositivo_ y _comandos_. Un desarrollador de soluciones puede importar un archivo JSON, que defina el modelo de dispositivo, a una plantilla de dispositivo, o usar la interfaz de usuario web en IoT Central para crear o editar un modelo de dispositivo. Los cambios en un modelo de dispositivo realizados mediante la interfaz de usuario web requieren la [cambio de versión de la plantilla de dispositivo](./howto-version-device-template.md).

Un desarrollador de soluciones también puede exportar un archivo JSON que contenga el modelo de dispositivo. Un desarrollador de dispositivos puede usar este documento JSON para comprender cómo debe comunicarse el dispositivo con la aplicación de IoT Central.

El archivo JSON que define el modelo de dispositivo usa el [Lenguaje de definición de gemelos digitales (DTDL) V2](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md). IoT Central espera que el archivo JSON contenga el modelo de dispositivo con las interfaces definidas insertadas, en lugar de en archivos independientes.

Habitualmente, los dispositivos IoT constan de:

- Elementos personalizados, que son las cosas que hacen que el dispositivo sea único.
- Elementos estándar, que son cosas comunes a todos los dispositivos.

Estos elementos se denominan _interfaces_  en un modelo de dispositivo. Las interfaces definen los detalles de cada uno de los elementos que implementa el dispositivo. Las interfaces se pueden reutilizar entre los modelos de dispositivo. En DTDL, un componente hace referencia a una interfaz definida en un archivo DTDL independiente.

En el ejemplo siguiente se muestra el esquema del modelo de dispositivo para un dispositivo de control de temperatura. El componente predeterminado incluye definiciones para `workingSet`, `serialNumber` y `reboot`. El modelo de dispositivo también incluye las interfaces `thermostat` y `deviceInformation`:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:TemperatureController;1",
  "@type": "Interface",
  "displayName": "Temperature Controller",
  "description": "Device with two thermostats and remote reboot.",
  "contents": [
    {
      "@type": [
        "Telemetry", "DataSize"
      ],
      "name": "workingSet",
      "displayName": "Working Set",
      "description": "Current working set of the device memory in KiB.",
      "schema": "double",
      "unit" : "kibibyte"
    },
    {
      "@type": "Property",
      "name": "serialNumber",
      "displayName": "Serial Number",
      "description": "Serial number of the device.",
      "schema": "string"
    },
    {
      "@type": "Command",
      "name": "reboot",
      "displayName": "Reboot",
      "description": "Reboots the device after waiting the number of seconds specified.",
      "request": {
        "name": "delay",
        "displayName": "Delay",
        "description": "Number of seconds to wait before rebooting the device.",
        "schema": "integer"
      }
    },
    {
      "@type" : "Component",
      "schema": "dtmi:com:example:Thermostat;1",
      "name": "thermostat",
      "displayName": "Thermostat",
      "description": "Thermostat One."
    },
    {
      "@type": "Component",
      "schema": "dtmi:azure:DeviceManagement:DeviceInformation;1",
      "name": "deviceInformation",
      "displayName": "Device Information interface",
      "description": "Optional interface with basic device hardware information."
    }
  ]
}
```

Una interfaz tiene varios campos obligatorios:

- `@id`: un identificador único en forma de nombre de recursos uniforme simple.
- `@type`: declara que este objeto es una interfaz.
- `@context`: especifica la versión de DTDL que se usa para la interfaz.
- `contents`: enumera las propiedades, la telemetría y los comandos que componen el dispositivo. Las funcionalidades pueden definirse en varias interfaces.

Hay algunos campos opcionales que puede usar para agregar más detalles al modelo de funcionalidad, como el de nombre para mostrar y el de descripción.

Todas las entradas de la lista de interfaces de la sección de implementaciones tienen:

- `name`: el nombre de programación de la interfaz.
- `schema`: la interfaz que implementa el modelo de funcionalidad.

## <a name="interfaces"></a>Interfaces

DTDL permite describir las funcionalidades del dispositivo. Las funcionalidades relacionadas se agrupan en interfaces. Las interfaces describen las propiedades, la telemetría y los comandos que implementa un elemento del dispositivo:

- `Properties`. Las propiedades son campos de datos que representan el estado de un dispositivo. Se usan para representar el estado duradero del dispositivo, como el estado de encendido-apagado de una bomba de líquido refrigerante. Las propiedades también pueden representar propiedades básicas del dispositivo, como la versión del firmware del dispositivo. Las propiedades se pueden declarar como de solo lectura o de escritura. Solo los dispositivos pueden actualizar el valor de una propiedad de solo lectura. Un operador puede establecer el valor de una propiedad grabable que se vaya a enviar a un dispositivo.
- `Telemetry`. Los campos de telemetría representan las medidas de los sensores. Cada vez que un dispositivo toma una medida de un sensor, debe enviar un evento de telemetría que contenga los datos del sensor.
- `Commands`. Los comandos representan métodos que los usuarios del dispositivo pueden ejecutar en el dispositivo. Por ejemplo, un comando de restablecimiento o un comando para encender o apagar un ventilador.

En el ejemplo siguiente se muestra la definición de interfaz de un termostato:

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:Thermostat;1",
  "@type": "Interface",
  "displayName": "Thermostat",
  "description": "Reports current temperature and provides desired temperature control.",
  "contents": [
    {
      "@type": [
        "Telemetry",
        "Temperature"
      ],
      "name": "temperature",
      "displayName" : "Temperature",
      "description" : "Temperature in degrees Celsius.",
      "schema": "double",
      "unit": "degreeCelsius"
    },
    {
      "@type": [
        "Property",
        "Temperature"
      ],
      "name": "targetTemperature",
      "schema": "double",
      "displayName": "Target Temperature",
      "description": "Allows to remotely specify the desired target temperature.",
      "unit" : "degreeCelsius",
      "writable": true
    },
    {
      "@type": [
        "Property",
        "Temperature"
      ],
      "name": "maxTempSinceLastReboot",
      "schema": "double",
      "unit" : "degreeCelsius",
      "displayName": "Max temperature since last reboot.",
      "description": "Returns the max temperature since last device reboot."
    },
    {
      "@type": "Command",
      "name": "getMaxMinReport",
      "displayName": "Get Max-Min report.",
      "description": "This command returns the max, min and average temperature from the specified time to the current time.",
      "request": {
        "name": "since",
        "displayName": "Since",
        "description": "Period to return the max-min report.",
        "schema": "dateTime"
      },
      "response": {
        "name" : "tempReport",
        "displayName": "Temperature Report",
        "schema": {
          "@type": "Object",
          "fields": [
            {
              "name": "maxTemp",
              "displayName": "Max temperature",
              "schema": "double"
            },
            {
              "name": "minTemp",
              "displayName": "Min temperature",
              "schema": "double"
            },
            {
              "name" : "avgTemp",
              "displayName": "Average Temperature",
              "schema": "double"
            },
            {
              "name" : "startTime",
              "displayName": "Start Time",
              "schema": "dateTime"
            },
            {
              "name" : "endTime",
              "displayName": "End Time",
              "schema": "dateTime"
            }
          ]
        }
      }
    }
  ]
}
```

En este ejemplo se muestran dos propiedades (una de solo lectura y otra editable), un tipo de telemetría y un comando. Una descripción de campo mínima tiene:

- `@type` para especificar el tipo de funcionalidad: `Telemetry`, `Property`o `Command`.  En algunos casos, el tipo incluye un tipo semántico para permitir que IoT Central realice algunas suposiciones sobre cómo controlar el valor.
- `name` para el valor de telemetría.
- `schema` para especificar el tipo de datos para la telemetría o la propiedad. Este valor puede ser un tipo primitivo, como doble, entero, booleano o cadena. También se admiten tipos de objetos complejos, matrices y mapas.

Los campos opcionales, como el nombre para mostrar y la descripción, permiten agregar más detalles a la interfaz y las funcionalidades.

## <a name="properties"></a>Propiedades

De forma predeterminada, las propiedades son de solo lectura. Las propiedades de solo lectura significan que el dispositivo informa de las actualizaciones de valores de propiedad a la aplicación de IoT Central. La aplicación de IoT Central no puede establecer el valor de una propiedad de solo lectura.

Una propiedad también se puede marcar como editable en una interfaz. Un dispositivo puede recibir una actualización de una propiedad editable desde la aplicación de IoT Central, así como notificar las actualizaciones de los valores de propiedad a la aplicación.

No es preciso que los dispositivos estén conectados para establecer los valores de propiedad. Los valores actualizados se transfieren la siguiente vez que el dispositivo se conecta a la aplicación. Este comportamiento se aplica a tanto a las propiedades de solo lectura como a las de escritura.

No use las propiedades para enviar datos de telemetría desde un dispositivo. Por ejemplo, una propiedad de solo lectura como m`temperatureSetting=80` debe significar que la temperatura del dispositivo se ha establecido en 80 y que el dispositivo está intentando llegar a esta temperatura o quedarse en ella.

En el caso de las propiedades de escritura, la aplicación de dispositivo devuelve un código de estado deseado, una versión y una descripción para indicar si ha recibido y aplicado el valor de propiedad.

## <a name="telemetry"></a>Telemetría

IoT Central le permite ver la telemetría en paneles y gráficos, y usar reglas para desencadenar acciones cuando se alcanzan los umbrales. IoT Central usa la información del modelo de dispositivo, como los tipos de datos, las unidades y los nombres para mostrar, para determinar cómo se muestran los valores de telemetría.

Puede usar la característica de exportación de datos de IoT Central para transmitir la telemetría a otros destinos, como Storage o Event Hubs.

## <a name="commands"></a>Comandos:

Un comando debe ejecutarse en un plazo máximo de 30 segundos de manera predeterminada y el dispositivo debe estar conectado cuando llegue el comando. Si el dispositivo no responde a tiempo o no está conectado, se produce un error en el comando.

Los comandos pueden tener parámetros de solicitud y devolver una respuesta.

### <a name="offline-commands"></a>Comandos sin conexión

Puede elegir comandos de cola si un dispositivo está sin conexión; para ello, habilite la opción **Queue if offline** (Poner en cola si no está conectado) en un comando de la plantilla de dispositivo.

Los comandos sin conexión son notificaciones unidireccionales al dispositivo desde la solución. Los comandos sin conexión pueden tener parámetros de solicitud, pero no devuelven una respuesta.

> [!NOTE]
> Esta opción solo está disponible en la interfaz de usuario web de IoT Central. Esta configuración no se incluye si exporta un modelo o una interfaz desde la plantilla de dispositivo.

## <a name="cloud-properties"></a>Propiedades de la nube

Las propiedades de la nube forman parte de la plantilla de dispositivo, pero no forman parte del modelo de dispositivo. Las propiedades de la nube permiten al desarrollador de soluciones especificar los metadatos de los dispositivos que se van a almacenar en la aplicación de IoT Central. Las propiedades de la nube no afectan al código que escribe el desarrollador de dispositivos para implementar el modelo de dispositivo.

Un desarrollador de soluciones puede agregar propiedades de la nube a los paneles y las vistas, junto a las propiedades del dispositivo, para permitir que un operador administre los dispositivos conectados a la aplicación. Un desarrollador de soluciones también puede usar las propiedades de la nube como parte de una definición de regla para que un operador pueda editar un valor de umbral.

## <a name="customizations"></a>Personalizaciones

Las personalizaciones forman parte de la plantilla de dispositivo, pero no forman parte del modelo de dispositivo. Las personalizaciones permiten al desarrollador de soluciones reemplazar algunas de las definiciones del modelo de dispositivo. Por ejemplo, un desarrollador de soluciones puede cambiar el nombre para mostrar de un tipo de telemetría o propiedad. Un desarrollador de soluciones también puede usar personalizaciones para agregar una validación, como la longitud mínima o máxima de una propiedad de dispositivo de cadena.

Las personalizaciones pueden afectan al código que escribe el desarrollador de dispositivos para implementar el modelo de dispositivo. Por ejemplo, una personalización podría establecer longitudes de cadena mínima y máxima, o valores numéricos mínimo y máximo para la telemetría.

## <a name="views"></a>Vistas

Un desarrollador de soluciones crea vistas que permiten a los operadores supervisar y administrar los dispositivos conectados. Las vistas forman parte de la plantilla de dispositivo, por lo que una vista se asocia a un tipo de dispositivo específico. Una vista puede incluir:

- Gráficos para trazar la telemetría.
- Iconos para mostrar las propiedades de un dispositivo de solo lectura.
- Iconos para permitir que el operador edite las propiedades grabables del dispositivo.
- Iconos para permitir que el operador edite las propiedades de la nube.
- Iconos para permitir que el operador llame a comandos, incluidos los comandos que esperan una carga útil.
- Iconos para mostrar etiquetas, imágenes o texto de Markdown.

La telemetría, las propiedades y los comandos que puede agregar a una vista vienen determinados por el modelo de dispositivo, las propiedades de la nube y las personalizaciones de la plantilla de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Como desarrollador de dispositivos, ahora que conoce las plantillas de dispositivo, los siguientes pasos sugeridos consisten en leer [Cargas de telemetría, propiedades y comandos](./concepts-telemetry-properties-commands.md) para obtener más información sobre los datos que un dispositivo intercambia con IoT Central.

Como desarrollador de soluciones, el siguiente paso sugerido es leer [Definición de un nuevo tipo de dispositivo IoT en la aplicación de Azure IoT Central](./howto-set-up-template.md) para obtener más información sobre cómo crear una plantilla de dispositivo.
