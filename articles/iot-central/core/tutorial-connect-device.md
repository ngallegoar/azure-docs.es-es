---
title: 'Tutorial: Conexión de una aplicación cliente Node.js genérica a Azure IoT Central | Microsoft Docs'
description: En este tutorial se muestra cómo los desarrolladores de dispositivos pueden conectar un dispositivo que ejecuta una aplicación cliente Node.js a la aplicación de Azure IoT Central. Para crear una plantilla de dispositivo, importe un modelo de funcionalidad de dispositivo y agregue vistas que permitan interactuar con un dispositivo conectado
author: dominicbetts
ms.author: dobett
ms.date: 02/26/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 1bcfc949eff0639dd1b4a063687e2c198f480ea3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77624341"
---
# <a name="tutorial-create-and-connect-a-nodejs-client-application-to-your-azure-iot-central-application-nodejs"></a>Tutorial: Creación y conexión de un aplicación cliente Node.js genérica a una aplicación de Azure IoT Central (Node.js)

En este tutorial se muestra cómo los desarrolladores de dispositivos pueden conectar una aplicación cliente Node.js a la aplicación de Azure IoT Central. La aplicación Node.js simula el comportamiento de un dispositivo real. Para que un sensor ambiental cree una _plantilla de dispositivo_ en IoT Central, se usa un _modelo de funcionalidad del dispositivo_ de muestra. Agregue vistas a la plantilla de dispositivo para visualizar la telemetría de este, administrar sus propiedades y usar comandos para controlar los dispositivos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Importar un modelo de funcionalidad de dispositivo para crear una plantilla de dispositivo.
> * Agregar las vistas predeterminada y personalizada a una plantilla de dispositivo.
> * Publicar una plantilla de dispositivo y agregar un dispositivo real a la aplicación de IoT Central.
> * Crear y ejecutar el código del dispositivo Node.js y ver si se conecta a la aplicación de IoT Central.
> * Especificar los datos de telemetría simulados que envía el dispositivo.
> * Usar una vista para administrar las propiedades del dispositivo.
> * Llamar a comandos para controlar el dispositivo.

## <a name="prerequisites"></a>Prerrequisitos

Necesitará lo siguiente para completar los pasos de este artículo:

* Una aplicación de Azure IoT Central creada a partir de la plantilla **Custom application** (Aplicación personalizada). Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
* Una máquina de desarrollo que tenga instalada la versión 10.0.0 de [Node.js](https://nodejs.org/) o una posterior. Puede ejecutar `node --version` en la línea de comandos para comprobar la versión. Node.js está disponible para una amplia variedad de sistemas operativos. En las instrucciones de este tutorial se da por hecho que se ejecuta el comando **node** en el símbolo del sistema de Windows. Puede usar Node.js en una gran variedad de sistemas operativos.

## <a name="create-a-device-template"></a>Creación de una plantilla de dispositivo

Cree una carpeta denominada `environmental-sensor` en la máquina local.

Descargue el archivo JSON del [modelo de funcionalidad del sensor ambiental](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) y guárdelo en la carpeta `environmental-sensor`.

Use un editor de texto para reemplazar las dos instancias de `{YOUR_COMPANY_NAME_HERE}` por el nombre de la empresa en el archivo `EnvironmentalSensorInline.capabilitymodel.json` que ha descargado.

En la aplicación de Azure IoT Central, cree una plantilla de dispositivo llamada *Environmental sensor* (Sensor ambiental); para ello, importe el archivo del modelo de funcionalidad del dispositivo `EnvironmentalSensorInline.capabilitymodel.json`:

![Plantilla de dispositivo con el modelo de funcionalidad del dispositivo importado](./media/tutorial-connect-device/device-template.png)

El modelo de funcionalidad del dispositivo incluye dos interfaces: la **Device Information** (Información de dispositivo) estándar y la **Environmenal Sensor** (Sensor ambiental) personalizada. La interfaz **Environmental Sensor** (Sensor ambiental) define las siguientes funcionalidades:

| Tipo | Display Name (Nombre para mostrar) | Descripción |
| ---- | ------------ | ----------- |
| Propiedad | Device State (Estado del dispositivo)     | Estado del dispositivo. Hay dos estados disponibles: en línea/sin conexión. |
| Propiedad | Nombre del cliente    | Nombre del cliente que opera actualmente el dispositivo. |
| Propiedad | Nivel de brillo | Nivel de brillo de la iluminación en el dispositivo. Se puede especificar como 1 (alto), 2 (intermedio), 3 (bajo). |
| Telemetría | Temperatura | Temperatura actual en el dispositivo. |
| Telemetría | Humedad    | Humedad actual en el dispositivo. |
| Get-Help | blink          | Comienza a parpadear el LED durante un tiempo determinado. |
| Get-Help | turnon         | Enciende la luz LED en el dispositivo. |
| Get-Help | turnoff        | Apaga la luz LED en el dispositivo. |
| Get-Help | rundiagnostics | Este comando inicia un diagnóstico. |

Para personalizar la forma en que se muestra la propiedad **Device State** (Estado del dispositivo) en la aplicación de IoT Central, seleccione **Customize** (Personalizar) en la plantilla de dispositivo. Expanda la entrada **Device State** (Estado del dispositivo), escriba _Online_ (En línea) para **True name** (Nombre real) y _Offline_ (Sin conexión) para **False name** (Nombre falso). Guarde los cambios:

![Personalización de la plantilla de dispositivo](media/tutorial-connect-device/customize-template.png)

## <a name="create-views"></a>Creación de vistas

Las vistas permiten interactuar con los dispositivos conectados a la aplicación de IoT Central. Por ejemplo, puede tener vistas que muestren telemetría, vistas que muestren propiedades y vistas que permitan editar las propiedades de escritura y de la nube. Las vistas forman parte de una plantilla de dispositivo.

Para agregar vistas predeterminadas a la plantilla de dispositivo **Environmental sensor** (Sensor ambiental), vaya a la plantilla de dispositivo, seleccione **Views** (Vistas) y seleccione el icono **Generate Default views** (Generar vistas predeterminadas). Asegúrese de que **Overview** (Información general) y **About** (Acerca de) se encuentran en estado **On** (Activo) y seleccione **Generate default dashboard views** (Generar vistas de panel predeterminadas). Ahora tiene dos vistas predeterminadas definidas en la plantilla.

La interfaz **Environmental Sensor** (Sensor ambiental) incluye dos propiedades con posibilidad de escritura: **Customer Name** (Nombre de cliente) y **Brightness Level** (Nivel de brillo). Para crear una vista, puede editar estas propiedades:

1. Seleccione **Views** (Vistas) y el icono **Editing device and cloud data** (Edición de los datos del dispositivo y de la nube).

1. Escriba _Properties_ (Propiedades) como nombre del formulario.

1. Seleccione las propiedades **Brightness Level** (Nivel de brillo) y **Customer Name** (Nombre de cliente). Después, seleccione **Add section** (Agregar sección).

1. Guarde los cambios.

![Incorporación de una vista para habilitar la edición de propiedades](media/tutorial-connect-device/properties-view.png)

## <a name="publish-the-template"></a>Publicación de la plantilla

Para agregar un dispositivo a la aplicación de IoT Central que usa la plantilla de dispositivo **Environmental sensor** (Sensor ambiental), primero debe publicar la plantilla.

En la plantilla de dispositivo, seleccione **Publish** (Publicar). En el panel que muestra los cambios que se van a publicar, seleccione **Publish** (Publicar).

Para comprobar que la plantilla está lista para usar, vaya a la página **Devices** (Dispositivos) en la aplicación de IoT Central. En la sección **Devices** (Dispositivos) se muestra una lista de los dispositivos publicados en la aplicación:

![Plantillas publicadas en la página de dispositivos](media/tutorial-connect-device/published-templates.png)

## <a name="add-a-real-device"></a>Adición de un dispositivo real

En la aplicación de Azure IoT Central, agregue un dispositivo real a la plantilla de dispositivo que creó en la sección anterior:

1. En la página **Devices** (Dispositivos), seleccione la plantilla de dispositivo **Environmental Sensor** (Sensor ambiental).

1. Seleccione **+ Nuevo**.

1. Asegúrese de que la opción **Simulado** esté **desactivada**. Seleccione **Crear**.

Haga clic en el nombre de dispositivo y seleccione **Connect** (Conectar). Anote la información de conexión del dispositivo en la página **Device Connection** (Conexión de dispositivos): **ID scope** (Ámbito de Id.), **Device ID** (Id. de dispositivo) y **Primary key** (Clave principal). Necesitará estos valores al crear el código del dispositivo:

![Información de conexión del dispositivo](./media/tutorial-connect-device/device-connection.png)

### <a name="create-a-nodejs-application"></a>Creación de una aplicación Node.js

En los pasos siguientes se muestra cómo crear una aplicación cliente Node.js que implementa el dispositivo real que se agregó a la aplicación. Esta aplicación Node.js simula el comportamiento de un dispositivo real.

1. Vaya a la carpeta `environmental-sensor` que ha creado anteriormente en el entorno de la línea de comandos.

1. Para inicializar el proyecto de Node.js e instalar las dependencias necesarias, ejecute los siguientes comandos y acepte todas las opciones predeterminadas al ejecutar `npm init`:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt azure-iot-provisioning-device-mqtt azure-iot-security-symmetric-key --save
    ```

1. Cree un archivo llamado **environmentalSensor.js** en la carpeta `environmental-sensor`.

1. Agregue las siguientes instrucciones `require` al principio del archivo **environmentalSensor.js**:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var iotHubTransport = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var ProvisioningTransport = require('azure-iot-provisioning-device-mqtt').Mqtt;
    var SymmetricKeySecurityClient = require('azure-iot-security-symmetric-key').SymmetricKeySecurityClient;
    var ProvisioningDeviceClient = require('azure-iot-provisioning-device').ProvisioningDeviceClient;
    ```

1. Agregue las siguientes declaraciones de variable al archivo:

    ```javascript
    var provisioningHost = 'global.azure-devices-provisioning.net';
    var idScope = '{your Scope ID}';
    var registrationId = '{your Device ID}';
    var symmetricKey = '{your Primary Key}';
    var provisioningSecurityClient = new SymmetricKeySecurityClient(registrationId, symmetricKey);
    var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvisioningTransport(), provisioningSecurityClient);
    var hubClient;
    var targetTemperature = 0;
    var ledOn = true;
    ```

    Actualice los marcadores de posición `{your Scope ID}`, `{your Device ID}` y `{your Primary Key}` con los valores que anotó anteriormente. En este ejemplo, se inicializará `targetTemperature` a cero, pero puede usar la lectura actual del dispositivo o un valor del dispositivo gemelo.

1. Para enviar las medidas de telemetría a la aplicación de Azure IoT Central, agregue la siguiente función al archivo:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temp = targetTemperature + (Math.random() * 15);
      var humid = 70 + (Math.random() * 10);
      var data = JSON.stringify({
        temp: temp,
        humid: humid,
        });
      var message = new Message(data);
      hubClient.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

1. Para enviar las propiedades de dispositivo a la aplicación de Azure IoT Central, agregue la siguiente función al archivo:

    ```javascript
    // Send device reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Para definir y controlar las propiedades con posibilidad de escritura a las que responda el dispositivo, agregue el código siguiente:

    ```javascript
    // Add any writeable properties your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'name': (newValue, callback) => {
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'brightness': (newValue, callback) => {
        setTimeout(() => {
            callback(newValue, 'completed');
      }, 5000);
      }
    };

    // Handle writeable property changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              sendDeviceProperties(twin, patch);
            });
          }
        }
      });
    }
    ```

1. Agregue el código siguiente para controlar los comandos enviados desde la aplicación de IoT Central:

    ```javascript
    // Handle blink command
    function onBlink(request, response) {
      console.log('Received asynchronous call to blink');
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          console.log('Blinking LED every ' + request.payload  + ' seconds');
        }
      });
    }

    // Handle LED turn on command
    function turnOn(request, response) {
      console.log('Received synchronous call to turn on LED');
      if(!ledOn){
        console.log('Turning on the LED');
        ledOn = true;
      }
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        }
      });
    }

    // Handle LED turn off command
    function turnOff(request, response) {
      console.log('Received synchronous call to turn off LED');
      if(ledOn){
        console.log('Turning off the LED');
        ledOn = false;
      }
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        }
      });
    }

    // Handle sensor diagnostics command
    function diagnostics(request, response) {
      console.log('Received asynchronous call to run diagnostics');
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          console.log('Running diagnostics...');
        }
      });
    }
    ```

1. Agregue el código siguiente para completar la conexión a Azure IoT Central y enlazar las funciones en el código de cliente:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');
        // Create handlers for commands
        hubClient.onDeviceMethod('blink', onBlink);
        hubClient.onDeviceMethod('turnon', turnOn);
        hubClient.onDeviceMethod('turnoff', turnOff);
        hubClient.onDeviceMethod('rundiagnostics', diagnostics);
        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);
        // Get device twin from Azure IoT Central.
        hubClient.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send Environmental Sensor device properties once on device start up.
            var properties = {
              state: 'true'
            };
            sendDeviceProperties(twin, properties);
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (register and connect to Azure IoT Central).
    provisioningClient.register((err, result) => {
      if (err) {
        console.log('Error registering device: ' + err);
      } else {
        console.log('Registration succeeded');
        console.log('Assigned hub=' + result.assignedHub);
        console.log('DeviceId=' + result.deviceId);
        var connectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';SharedAccessKey=' + symmetricKey;
        hubClient = Client.fromConnectionString(connectionString, iotHubTransport);
        hubClient.open(connectCallback);
      }
    });
    ```

## <a name="run-your-nodejs-application"></a>Ejecución de la aplicación de Node.js

Para iniciar la aplicación cliente del dispositivo, ejecute el comando siguiente en el entorno de la línea de comandos:

```cmd/sh
node environmentalSensor.js
```

Verá que el dispositivo se conecta a la aplicación de Azure IoT Central y comienza a enviar telemetría:

![Ejecución de la aplicación cliente](media/tutorial-connect-device/run-application.png)

Como operador de la aplicación de Azure IoT Central, puede:

* Ver la telemetría enviada por el dispositivo en la página **Overview** (Información general):

    ![Ver datos de telemetría](media/tutorial-connect-device/view-telemetry.png)

* Actualizar los valores de las propiedades con posibilidad de escritura en la página **Properties** (Propiedades):

    ![Actualización de propiedades](media/tutorial-connect-device/update-properties.png)

    ![Actualización del dispositivo de las propiedades](media/tutorial-connect-device/update-properties-device.png)

* Llamar al comando desde la página **Commands** (Comandos):

    ![Llamada al comando blink](media/tutorial-connect-device/call-command.png)

    ![Llamada al comando blink del dispositivo](media/tutorial-connect-device/call-command-device.png)

* Ver las propiedades del dispositivo en la página **About** (Acerca de):

    ![Ver propiedades](media/tutorial-connect-device/about-properties.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los modelos de funcionalidad del dispositivo y cómo crear sus propias plantillas de dispositivo, continúe con la guía paso a paso:

> [!div class="nextstepaction"]
> [Definición de un nuevo tipo de dispositivo IoT](./howto-set-up-template.md)
