---
title: Uso de la extensión de Azure IoT para que la CLI de Azure interactúe con los dispositivos de la versión preliminar de IoT Plug and Play | Microsoft Docs
description: Instale la extensión de Azure IoT para la CLI de Azure y úsela para interactuar con los dispositivos IoT Plug and Play conectados a su centro de IoT.
author: dominicbetts
ms.author: dobett
ms.date: 07/20/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 3699213fe61c64d7677ba026a8df54ccbbfe4b33
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352022"
---
# <a name="install-and-use-the-azure-iot-extension-for-the-azure-cli"></a>Instalación y uso de la extensión de Azure IoT para la CLI de Azure

[La CLI de Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) es una herramienta de línea de comandos multiplataforma y de código abierto para la administración de recursos de Azure, como IoT Hub. La CLI de Azure está disponible en Windows, Linux y MacOS. También viene preinstalada en [Azure Cloud Shell](https://shell.azure.com). Permite administrar los recursos de Azure IoT Hub, las instancias de Device Provisioning Service y los centros vinculados sin instalar ninguna extensión.

La extensión de Azure IoT para la CLI de Azure es una herramienta de línea de comandos para interactuar con los dispositivos de la versión preliminar de IoT Plug and Play y probarlos. Puede usar la extensión para lo siguiente:

- Conectar con un dispositivo.
- Ver la telemetría que envía el dispositivo.
- Trabajar con las propiedades del dispositivo.
- Llamar a comandos de dispositivo.

En este artículo aprenderá a:

- Instalar y configurar la extensión de Azure IoT para la CLI de Azure.
- Usar la extensión para interactuar con los dispositivos y probarlos.

## <a name="install-azure-iot-extension-for-the-azure-cli"></a>Instalación de la extensión de Azure IoT para la CLI de Azure

### <a name="step-1---install-the-azure-cli"></a>Paso 1: Instalación de la CLI de Azure

Siga las [instrucciones de instalación](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) para instalar la CLI de Azure en su entorno. Para obtener la mejor experiencia, la CLI de Azure debe ser de la versión 2.9.1 o posterior. Use `az -–version` para asegurarse.

### <a name="step-2---install-iot-extension"></a>Paso 2: Instalación de la extensión de IoT

En el [archivo Léame de la extensión de IoT](https://github.com/Azure/azure-iot-cli-extension) se describen varias maneras de instalarla. La manera más sencilla es ejecutar `az extension add --name azure-iot`. Después de la instalación, puede usar `az extension list` para validar las extensiones instaladas actualmente o `az extension show --name azure-iot` para ver los detalles acerca de la extensión de IoT. En el momento de escribir este documento, el número de versión de la extensión es `0.9.7`.

Para quitar la extensión, puede usar `az extension remove --name azure-iot`.

## <a name="use-azure-iot-extension-for-the-azure-cli"></a>Uso de la extensión de Azure IoT para la CLI de Azure

### <a name="prerequisites"></a>Requisitos previos

Para iniciar sesión en su suscripción a Azure, ejecute el siguiente comando:

```azurecli
az login
```

> [!NOTE]
> Si usa Azure Cloud Shell, inicia sesión automáticamente y no es necesario ejecutar el comando anterior.

Para usar la extensión de Azure IoT para la CLI de Azure, necesita:

- Un centro de Azure IoT. Hay muchas maneras de agregar un centro de IoT a la suscripción de Azure, por ejemplo, [crear un centro de IoT mediante la CLI de Azure](../iot-hub/iot-hub-create-using-cli.md). Necesitará la cadena de conexión del centro de IoT para ejecutar los comandos de la extensión de Azure IoT. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- Un dispositivo registrado en su centro de IoT. Puede usar el siguiente comando de CLI de Azure para registrar un dispositivo: asegúrese de reemplazar los marcadores de posición `{YourIoTHubName}` y `{YourDeviceID}` por sus valores:

    ```azurecli
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

### <a name="interact-with-a-device"></a>Interactuación con un dispositivo

Puede usar la extensión para ver e interactuar con los dispositivos de IoT Plug and Play que estén conectados a un centro de IoT. La extensión funciona con el gemelo digital que representa el dispositivo de IoT Plug and Play.

#### <a name="list-devices"></a>Enumerar los dispositivos.

Enumerar todos los dispositivos de una instancia de IoT Hub:

```azurecli
az iot hub device-identity list --hub-name {YourIoTHubName}
```

#### <a name="view-digital-twin"></a>Ver gemelo digital

Ver el gemelo digital de un dispositivo:

```azurecli
az iot pnp twin show -n {iothub_name} -d {device_id}
```

#### <a name="properties"></a>Propiedades

Establecer el valor de una propiedad de lectura y escritura:

```azurecli
az iot pnp twin update --hub-name {iothub_name} --device-id {device_id} --json-patch '{"op":"add", "path":"/thermostat1/targetTemperature", "value": 54}'
```

#### <a name="commands"></a>Comandos:

Invocar un comando:

```azurecli
az iot pnp twin invoke-command --cn getMaxMinReport -n {iothub_name} -d {device_id} --component-path thermostat1
```

#### <a name="digital-twin-events"></a>Eventos gemelos digitales

Supervise todos los eventos gemelos digitales de IoT Plug and Play desde un dispositivo e interfaz específicos que vayan al grupo de consumidores del centro de eventos **$Default**:

```azurecli
az iot hub monitor-events -n {iothub_name} -d {device_id} -i {interface_id}
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo de procedimientos, ha aprendido a instalar y usar la extensión de Azure IoT para la CLI de Azure a fin de interactuar con los dispositivos Plug and Play. El siguiente paso sugerido es obtener información sobre cómo usar [Azure IoT Explorer con los dispositivos](./howto-use-iot-explorer.md).
