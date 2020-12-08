---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 11/24/2020
ms.openlocfilehash: 4b19ae1b584a2d300e4144e79ef76245c71035cf
ms.sourcegitcommit: b8a175b6391cddd5a2c92575c311cc3e8c820018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126137"
---
## <a name="prerequisites"></a>Requisitos previos

Necesitará lo siguiente para completar los pasos de este artículo:

* Una aplicación de Azure IoT Central creada a partir de la plantilla **Custom application** (Aplicación personalizada). Para más información, consulte la [guía de inicio rápido para crear una aplicación](../articles/iot-central/core/quick-deploy-iot-central.md). La aplicación se debe haber creado el 14 de julio de 2020 o después.
* Una máquina de desarrollo con la versión 6 de [Node.js](https://nodejs.org/) o una posterior instalada. Puede ejecutar `node --version` en la línea de comandos para comprobar la versión. En las instrucciones de este tutorial se da por hecho que se ejecuta el comando **node** en el símbolo del sistema de Windows. No obstante, puede usar Node.js en muchos otros sistemas operativos.
* Una copia local del repositorio de GitHub de [SDK de IoT de Microsoft Azure para Node.js](https://github.com/Azure/azure-iot-sdk-node) que contiene el código de ejemplo. Use este vínculo para descargar una copia del repositorio: [Descargar el archivo ZIP](https://github.com/Azure/azure-iot-sdk-node/archive/master.zip). Después, descomprima el archivo en una ubicación conveniente en la máquina local.

## <a name="review-the-code"></a>Revisión del código

En la copia del SDK de IoT de Microsoft Azure para Node.js que descargó anteriormente, abra el archivo *azure-iot-sdk-node/device/samples/pnp/simple_thermostat.js* en un editor de texto.

Al ejecutar el ejemplo para conectarse a IoT Central, usa Device Provisioning Service (DPS) para registrar el dispositivo y generar una cadena de conexión. En el ejemplo se recupera la información de conexión de DPS que necesita del entorno de línea de comandos.

El método `main` realiza las acciones siguientes:

* Crea un objeto `client` y establece el identificador del modelo `dtmi:com:example:Thermostat;1` antes de abrir la conexión.
* Crea un controlador de comandos.
* Inicia un bucle para enviar telemetría de temperatura cada 10 segundos.
* Envía la propiedad `maxTempSinceLastReboot` a IoT Central. IoT Central omite la propiedad `serialNumber` porque no forma parte del modelo de dispositivo.
* Crea un controlador de propiedades de escritura.

```javascript
async function main() {

  // ...

  // fromConnectionString must specify a transport, coming from any transport package.
  const client = Client.fromConnectionString(deviceConnectionString, Protocol);

  let resultTwin;
  try {
    // Add the modelId here
    await client.setOptions(modelIdObject);
    await client.open();

    client.onDeviceMethod(commandMaxMinReport, commandHandler);

    // Send Telemetry every 10 secs
    let index = 0;
    intervalToken = setInterval(() => {
      sendTelemetry(client, index).catch((err) => console.log('error', err.toString()));
      index += 1;
    }, telemetrySendInterval);

    // attach a standard input exit listener
    attachExitHandler(client);

    // Deal with twin
    try {
      resultTwin = await client.getTwin();
      const patchRoot = createReportPropPatch({ serialNumber: deviceSerialNum });
      const patchThermostat = createReportPropPatch({
        maxTempSinceLastReboot: deviceTemperatureSensor.getMaxTemperatureValue()
      });

      // the below things can only happen once the twin is there
      updateComponentReportedProperties(resultTwin, patchRoot);
      updateComponentReportedProperties(resultTwin, patchThermostat);

      // Setup the handler for desired properties
      desiredPropertyPatchHandler(resultTwin);

    } catch (err) {
      console.error('could not retrieve twin or report twin properties\n' + err.toString());
    }
  } catch (err) {
    console.error('could not connect Plug and Play client or could not attach interval function for telemetry\n' + err.toString());
  }
}
```

La función `provisionDevice` muestra cómo el dispositivo usa DPS para registrarse y conectarse a IoT Central. La carga incluye el identificador de modelo:

```javascript
async function provisionDevice(payload) {
  var provSecurityClient = new SymmetricKeySecurityClient(registrationId, symmetricKey);
  var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvProtocol(), provSecurityClient);

  if (!!(payload)) {
    provisioningClient.setProvisioningPayload(payload);
  }

  try {
    let result = await provisioningClient.register();
    deviceConnectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';SharedAccessKey=' + symmetricKey;
  } catch (err) {
    console.error("error registering device: " + err.toString());
  }
}
```

La función `sendTelemetry` muestra cómo el dispositivo envía la telemetría de temperatura a IoT Central. El método `getCurrentTemperatureObject` devuelve un objeto similar a `{ temperature: 45.6 }`:

```javascript
async function sendTelemetry(deviceClient, index) {
  console.log('Sending telemetry message %d...', index);
  const msg = new Message(
    JSON.stringify(
      deviceTemperatureSensor.updateSensor().getCurrentTemperatureObject()
    )
  );
  msg.contentType = 'application/json';
  msg.contentEncoding = 'utf-8';
  await deviceClient.sendEvent(msg);
}
```

El método `main` usa los dos métodos siguientes para enviar la propiedad `maxTempSinceLastReboot` a IoT Central. El método `main` llama a `createReportPropPatch` con un objeto similar a `{maxTempSinceLastReboot: 80.9}`:

```javascript
const createReportPropPatch = (propertiesToReport) => {
  let patch;
  patch = { };
  patch = propertiesToReport;
  return patch;
};

const updateComponentReportedProperties = (deviceTwin, patch) => {
  deviceTwin.properties.reported.update(patch, function (err) {
    if (err) throw err;
    console.log('Properties have been reported for component');
  });
};
```

El método `main` usa los dos métodos siguientes para controlar las actualizaciones de la propiedad de escritura _target temperature_ de IoT Central. Observe cómo `propertyUpdateHandle` compila la respuesta con la versión y el código de estado:

```javascript
const desiredPropertyPatchHandler = (deviceTwin) => {
  deviceTwin.on('properties.desired', (delta) => {
    const versionProperty = delta.$version;

    Object.entries(delta).forEach(([propertyName, propertyValue]) => {
      if (propertyName !== '$version') {
        propertyUpdateHandler(deviceTwin, propertyName, null, propertyValue, versionProperty);
      }
    });
  });
};

const propertyUpdateHandler = (deviceTwin, propertyName, reportedValue, desiredValue, version) => {
  console.log('Received an update for property: ' + propertyName + ' with value: ' + JSON.stringify(desiredValue));
  const patch = createReportPropPatch(
    { [propertyName]:
      {
        'value': desiredValue,
        'ac': 200,
        'ad': 'Successfully executed patch for ' + propertyName,
        'av': version
      }
    });
  updateComponentReportedProperties(deviceTwin, patch);
  console.log('updated the property');
};
```

El método `main` usa los dos métodos siguientes para controlar las llamadas al comando `getMaxMinReport`. El método `getMaxMinReportObject` genera el informe como un objeto JSON:

```javascript
const commandHandler = async (request, response) => {
  switch (request.methodName) {
  case commandMaxMinReport: {
    console.log('MaxMinReport ' + request.payload);
    await sendCommandResponse(request, response, 200, deviceTemperatureSensor.getMaxMinReportObject());
    break;
  }
  default:
    await sendCommandResponse(request, response, 404, 'unknown method');
    break;
  }
};

const sendCommandResponse = async (request, response, status, payload) => {
  try {
    await response.send(status, payload);
    console.log('Response to method \'' + request.methodName +
              '\' sent successfully.' );
  } catch (err) {
    console.error('An error ocurred when sending a method response:\n' +
              err.toString());
  }
};
```

## <a name="get-connection-information"></a>Obtención de información sobre la conexión

[!INCLUDE [iot-central-connection-configuration](iot-central-connection-configuration.md)]

## <a name="run-the-code"></a>Ejecución del código

Para ejecutar la aplicación de ejemplo, abra un entorno de línea de comandos y vaya a la carpeta *azure-iot-sdk-node/device/samples/pnp* que contiene el archivo de ejemplo *simple_thermostat.js*.

[!INCLUDE [iot-central-connection-environment](iot-central-connection-environment.md)]

Instale los paquetes necesarios:

```cmd/sh
npm install
```

Ejecución del ejemplo:

```cmd/sh
node simple_thermostat.js
```

La salida siguiente muestra el registro del dispositivo y la conexión a IoT Central. Después, el ejemplo envía la propiedad `maxTempSinceLastReboot` antes de comenzar a enviar telemetría:

```output
registration succeeded
assigned hub=iotc-.......azure-devices.net
deviceId=sample-device-01
payload=undefined
Connecting using connection string HostName=iotc-........azure-devices.net;DeviceId=sample-device-01;SharedAccessKey=Ci....=
Enabling the commands on the client
Please enter q or Q to exit sample.
The following properties will be updated for root interface:
{ maxTempSinceLastReboot: 55.20309427428496 }
Properties have been reported for component
Sending telemetry message 0...
Sending telemetry message 1...
Sending telemetry message 2...
Sending telemetry message 3...
```

[!INCLUDE [iot-central-monitor-thermostat](iot-central-monitor-thermostat.md)]

Puede ver cómo responde el dispositivo a los comandos y las actualizaciones de propiedades:

```output
MaxMinReport 2020-10-15T12:00:00.000Z
Response to method 'getMaxMinReport' sent successfully.

...

Received an update for property: targetTemperature with value: {"value":86.3}
The following properties will be updated for root interface:
{
  targetTemperature: {
    value: { value: 86.3 },
    ac: 200,
    ad: 'Successfully executed patch for targetTemperature',
    av: 2
  }
}
```
