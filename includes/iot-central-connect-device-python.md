---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 11/24/2020
ms.openlocfilehash: cd7d4d3653dc775a949da229e4005d297f7810aa
ms.sourcegitcommit: b8a175b6391cddd5a2c92575c311cc3e8c820018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126128"
---
## <a name="prerequisites"></a>Requisitos previos

Necesitará lo siguiente para completar los pasos de este artículo:

* Una aplicación de Azure IoT Central creada a partir de la plantilla **Custom application** (Aplicación personalizada). Para más información, consulte la [guía de inicio rápido para crear una aplicación](../articles/iot-central/core/quick-deploy-iot-central.md). La aplicación se debe haber creado el 14 de julio de 2020 o después.
* Una máquina de desarrollo que tenga instalada la versión 3.7 de [Python](https://www.python.org/) o una posterior. Puede ejecutar `python --version` en la línea de comandos para comprobar la versión. Python está disponible para una amplia variedad de sistemas operativos. En las instrucciones de este tutorial se da por hecho que se ejecuta el comando **python** en el símbolo del sistema de Windows.
* Una copia local del repositorio de GitHub de [SDK de IoT de Microsoft Azure para Python](https://github.com/Azure/azure-iot-sdk-python) que contiene el código de ejemplo. Use este vínculo para descargar una copia del repositorio: [Descargar el archivo ZIP](https://github.com/Azure/azure-iot-sdk-python/archive/master.zip). Después, descomprima el archivo en una ubicación conveniente en la máquina local.

## <a name="review-the-code"></a>Revisión del código

En la copia del SDK de IoT de Microsoft Azure para Python que descargó anteriormente, abra el archivo *azure-iot-sdk-python/azure-iot-device/samples/pnp/simple_thermostat.py* en un editor de texto.

Al ejecutar el ejemplo para conectarse a IoT Central, usa Device Provisioning Service (DPS) para registrar el dispositivo y generar una cadena de conexión. En el ejemplo se recupera la información de conexión de DPS que necesita del entorno de línea de comandos.

La función `main`:

* Usa DPS para aprovisionar el dispositivo. La información de aprovisionamiento incluye el identificador de modelo.
* Crea un objeto `Device_client` y establece el identificador del modelo `dtmi:com:example:Thermostat;1` antes de abrir la conexión.
* Envía la propiedad `maxTempSinceLastReboot` a IoT Central.
* Crea un agente de escucha para el comando `getMaxMinReport`.
* Crea un agente de escucha de propiedad para realizar escuchas de las actualizaciones de las propiedades de escritura.
* Inicia un bucle para enviar telemetría de temperatura cada 10 segundos.

```python
async def main():
    switch = os.getenv("IOTHUB_DEVICE_SECURITY_TYPE")
    if switch == "DPS":
        provisioning_host = (
            os.getenv("IOTHUB_DEVICE_DPS_ENDPOINT")
            if os.getenv("IOTHUB_DEVICE_DPS_ENDPOINT")
            else "global.azure-devices-provisioning.net"
        )
        id_scope = os.getenv("IOTHUB_DEVICE_DPS_ID_SCOPE")
        registration_id = os.getenv("IOTHUB_DEVICE_DPS_DEVICE_ID")
        symmetric_key = os.getenv("IOTHUB_DEVICE_DPS_DEVICE_KEY")

        registration_result = await provision_device(
            provisioning_host, id_scope, registration_id, symmetric_key, model_id
        )

        if registration_result.status == "assigned":

            device_client = IoTHubDeviceClient.create_from_symmetric_key(
                symmetric_key=symmetric_key,
                hostname=registration_result.registration_state.assigned_hub,
                device_id=registration_result.registration_state.device_id,
                product_info=model_id,
            )
        else:
            raise RuntimeError(
                "Could not provision device. Aborting Plug and Play device connection."
            )

    elif switch == "connectionString":

        # ...

    # Connect the client.
    await device_client.connect()

    max_temp = 10.96  # Initial Max Temp otherwise will not pass certification
    await device_client.patch_twin_reported_properties({"maxTempSinceLastReboot": max_temp})

    listeners = asyncio.gather(
        execute_command_listener(
            device_client,
            method_name="getMaxMinReport",
            user_command_handler=max_min_handler,
            create_user_response_handler=create_max_min_report_response,
        ),
        execute_property_listener(device_client),
    )

    async def send_telemetry():
        global max_temp
        global min_temp
        current_avg_idx = 0

        while True:
            current_temp = random.randrange(10, 50)
            if not max_temp:
                max_temp = current_temp
            elif current_temp > max_temp:
                max_temp = current_temp

            if not min_temp:
                min_temp = current_temp
            elif current_temp < min_temp:
                min_temp = current_temp

            avg_temp_list[current_avg_idx] = current_temp
            current_avg_idx = (current_avg_idx + 1) % moving_window_size

            temperature_msg1 = {"temperature": current_temp}
            await send_telemetry_from_thermostat(device_client, temperature_msg1)
            await asyncio.sleep(8)

    send_telemetry_task = asyncio.create_task(send_telemetry())

    # ...
```

La función `provision_device` usa DPS para aprovisionar el dispositivo y registrarlo con IoT Central. La función incluye el identificador del modelo de dispositivo en la carga de aprovisionamiento:

```python
async def provision_device(provisioning_host, id_scope, registration_id, symmetric_key, model_id):
    provisioning_device_client = ProvisioningDeviceClient.create_from_symmetric_key(
        provisioning_host=provisioning_host,
        registration_id=registration_id,
        id_scope=id_scope,
        symmetric_key=symmetric_key,
    )
    provisioning_device_client.provisioning_payload = {"modelId": model_id}
    return await provisioning_device_client.register()
```

La función `execute_command_listener` controla las solicitudes de comando, ejecuta la función `max_min_handler` cuando el dispositivo recibe el comando `getMaxMinReport` y ejecuta la función `create_max_min_report_response` para generar la respuesta:

```python
async def execute_command_listener(
    device_client, method_name, user_command_handler, create_user_response_handler
):
    while True:
        if method_name:
            command_name = method_name
        else:
            command_name = None

        command_request = await device_client.receive_method_request(command_name)
        print("Command request received with payload")
        print(command_request.payload)

        values = {}
        if not command_request.payload:
            print("Payload was empty.")
        else:
            values = command_request.payload

        await user_command_handler(values)

        response_status = 200
        response_payload = create_user_response_handler(values)

        command_response = MethodResponse.create_from_method_request(
            command_request, response_status, response_payload
        )

        try:
            await device_client.send_method_response(command_response)
        except Exception:
            print("responding to the {command} command failed".format(command=method_name))
```

`async def execute_property_listener` controla las actualizaciones de las propiedades de escritura como `targetTemperature` y genera la respuesta JSON:

```python
async def execute_property_listener(device_client):
    ignore_keys = ["__t", "$version"]
    while True:
        patch = await device_client.receive_twin_desired_properties_patch()  # blocking call

        print("the data in the desired properties patch was: {}".format(patch))

        version = patch["$version"]
        prop_dict = {}

        for prop_name, prop_value in patch.items():
            if prop_name in ignore_keys:
                continue
            else:
                prop_dict[prop_name] = {
                    "ac": 200,
                    "ad": "Successfully executed patch",
                    "av": version,
                    "value": prop_value,
                }

        await device_client.patch_twin_reported_properties(prop_dict)
```

La función `send_telemetry_from_thermostat` envía los mensajes de telemetría a IoT Central:

```python
async def send_telemetry_from_thermostat(device_client, telemetry_msg):
    msg = Message(json.dumps(telemetry_msg))
    msg.content_encoding = "utf-8"
    msg.content_type = "application/json"
    print("Sent message")
    await device_client.send_message(msg)
```

## <a name="get-connection-information"></a>Obtención de información sobre la conexión

[!INCLUDE [iot-central-connection-configuration](iot-central-connection-configuration.md)]

## <a name="run-the-code"></a>Ejecución del código

Para ejecutar la aplicación de ejemplo, abra un entorno de línea de comandos y vaya a la carpeta *azure-iot-sdk-python/azure-iot-device/samples/pnp* que contiene el archivo de ejemplo *simple_thermostat.py*.

[!INCLUDE [iot-central-connection-environment](iot-central-connection-environment.md)]

Instale los paquetes necesarios:

```cmd/sh
pip install azure-iot-device
```

Ejecución del ejemplo:

```cmd/sh
python simple_thermostat.py
```

La salida siguiente muestra el registro del dispositivo y la conexión a IoT Central. El ejemplo envía la propiedad `maxTempSinceLastReboot` antes de comenzar a enviar telemetría:

```cmd/sh
Device was assigned
iotc-.......azure-devices.net
sample-device-01
Listening for command requests and property updates
Press Q to quit
Sending telemetry for temperature
Sent message
Sent message
Sent message
```

[!INCLUDE [iot-central-monitor-thermostat](iot-central-monitor-thermostat.md)]

Puede ver cómo responde el dispositivo a los comandos y las actualizaciones de propiedades:

```cmd/sh
Sent message
the data in the desired properties patch was: {'targetTemperature': {'value': 86.3}, '$version': 2}
Sent message

...

Sent message
Command request received with payload
2020-10-14T08:00:00.000Z
Will return the max, min and average temperature from the specified time 2020-10-14T08:00:00.000Z to the current time
Done generating
{"avgTemp": 31.5, "endTime": "2020-10-16T10:07:41.580722", "maxTemp": 49, "minTemp": 12, "startTime": "2020-10-16T10:06:21.580632"}
```
