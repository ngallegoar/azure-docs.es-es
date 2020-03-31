---
title: Uso de la extensión de Azure IoT para que la CLI de Azure interactúe con los dispositivos de la versión preliminar de IoT Plug and Play | Microsoft Docs
description: Instale la extensión de Azure IoT para la CLI de Azure y úsela para interactuar con los dispositivos IoT Plug and Play conectados a su centro de IoT.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 12/26/2019
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: b5907c0fb127947e90352e68b2726a22f5afea0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234684"
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
- Usar la extensión para administrar interfaces en el repositorio del modelo.

## <a name="install-azure-iot-extension-for-the-azure-cli"></a>Instalación de la extensión de Azure IoT para la CLI de Azure

### <a name="step-1---install-the-azure-cli"></a>Paso 1: Instalación de la CLI de Azure

Siga las [instrucciones de instalación](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) para instalar la CLI de Azure en su entorno. Para usar todos los comandos siguientes, la versión de la CLI de Azure debe ser 2.0.73 o posterior. Use `az -–version` para asegurarse.

### <a name="step-2---install-iot-extension"></a>Paso 2: Instalación de la extensión de IoT

En el [archivo Léame de la extensión de IoT](https://github.com/Azure/azure-iot-cli-extension) se describen varias maneras de instalarla. La manera más sencilla es ejecutar `az extension add --name azure-iot`. Después de la instalación, puede usar `az extension list` para validar las extensiones instaladas actualmente o `az extension show --name azure-iot` para ver los detalles acerca de la extensión de IoT. Para quitar la extensión, puede usar `az extension remove --name azure-iot`.

## <a name="use-azure-iot-extension-for-the-azure-cli"></a>Uso de la extensión de Azure IoT para la CLI de Azure

### <a name="prerequisites"></a>Prerequisites

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

- Algunos comandos requieren la cadena de conexión para el repositorio de modelos de una empresa. Se creará un repositorio de modelos para su empresa la primera vez que [incorpore un elemento al portal de Azure Certified for IoT](howto-onboard-portal.md). Un tercero podría compartir su cadena de conexión del repositorio de modelos para proporcionarle acceso a sus interfaces y modelos.

### <a name="interact-with-a-device"></a>Interactuación con un dispositivo

Puede usar la extensión para ver e interactuar con los dispositivos de IoT Plug and Play que estén conectados a un centro de IoT. La extensión funciona con el gemelo digital que representa el dispositivo de IoT Plug and Play.

#### <a name="list-devices-and-interfaces"></a>Enumeración de dispositivos e interfaces

Enumerar todos los dispositivos de una instancia de IoT Hub:

```azurecli
az iot hub device-identity list --hub-name {YourIoTHubName}
```

Enumerar todas las interfaces registradas por un dispositivo de IoT Plug and Play:

```azurecli
az iot dt list-interfaces --hub-name {YourIoTHubName} --device-id {YourDeviceID}
```

#### <a name="properties"></a>Propiedades

Se enumeran todas las propiedades y los valores de propiedad de una interfaz en un dispositivo:

```azurecli
az iot dt list-properties --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --source private --repo-login "{YourCompanyModelRepoConnectionString}"
```

Establecer el valor de una propiedad de lectura y escritura:

```azurecli
az iot dt update-property --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface-payload {JSONPayload or FilePath}
```

Un archivo de carga de ejemplo para establecer la propiedad **name** en la interfaz **sensor** de un dispositivo para **Contoso** tiene la apariencia siguiente:

```json
{
  "sensor": {
    "properties": {
      "name": {
        "desired": {
          "value": "Contoso"
        }
      }
    }
  }
}
```

#### <a name="commands"></a>Comandos:

Enumerar todos los comandos de una interfaz en un dispositivo:

```azurecli
az iot dt list-commands --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --source private --repo-login {YourCompanyModelRepoConnectionString}
```

Sin el parámetro `--repo-login`, este comando usa el repositorio del modelo público.

Invocar un comando:

```azurecli
az iot dt invoke-command --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --cn {CommandName} --command-payload {CommandPayload or FilePath}
```

#### <a name="digital-twin-events"></a>Eventos gemelos digitales

Supervise todos los eventos gemelos digitales de IoT Plug and Play desde un dispositivo e interfaz específicos que vayan al grupo de consumidores del centro de eventos **$Default**:

```azurecli
az iot dt monitor-events --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID}
```

Supervise todos los eventos gemelos digitales de IoT Plug and Play desde un dispositivo e interfaz específicos que vayan a un grupo de consumidores concreto:

```azurecli
az iot dt monitor-events --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --consumer-group {YourConsumerGroup}
```

### <a name="manage-interfaces-in-a-model-repository"></a>Administración de interfaces en un repositorio de modelos

Los siguientes comandos usan el repositorio público de modelos de IoT Plug and Play. Para usar un repositorio de modelos de empresa, agregue el argumento `--login` con la cadena de conexión del repositorio de modelos.

Enumerar las interfaces en el repositorio de modelos de IoT Plug and Play:

```azurecli
az iot pnp interface list
```

Mostrar una interfaz en el repositorio de modelos de IoT Plug and Play:

```azurecli
az iot pnp interface show --interface {YourInterfaceId}
```

Crear una interfaz en el repositorio de modelos de empresa de IoT Plug and Play:

```azurecli
az iot pnp interface create --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

No puede crear directamente una interfaz en el repositorio de modelos públicos.

Actualizar una interfaz en el repositorio de modelos de empresa de IoT Plug and Play:

```azurecli
az iot pnp interface update --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

No puede actualizar directamente una interfaz en el repositorio de modelos públicos.

Publique una interfaz desde el repositorio de modelo de empresa de IoT Plug and Play en el repositorio de modelos públicos. Esta operación hace que la interfaz sea inmutable:

```azurecli
az iot pnp interface publish --interface {YourInterfaceID} --login {YourCompanyModelRepoConnectionString}
```

Solo los asociados de Microsoft pueden publicar interfaces en el repositorio de modelos públicos.

### <a name="manage-device-capability-models-in-a-model-repository"></a>Administración de modelos de funcionalidad de dispositivos en un repositorio de modelos

Los siguientes comandos usan el repositorio público de modelos de IoT Plug and Play. Para usar un repositorio de modelos de empresa, agregue el argumento `--login` con la cadena de conexión del repositorio de modelos.

Enumerar los modelos de funcionalidad de dispositivos en el repositorio de modelos públicos de IoT Plug and Play:

```azurecli
az iot pnp capability-model list
```

Mostrar un modelo de funcionalidad de dispositivos en el repositorio de modelos públicos de IoT Plug and Play:

```azurecli
az iot pnp capability-model show --model {YourModelID}
```

Crear un modelo de funcionalidad de dispositivos en un repositorio de modelos de empresa de IoT Plug and Play:

```azurecli
az iot pnp capability-model create --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

No puede crear directamente un modelo en el repositorio de modelos públicos.

Actualizar un modelo de funcionalidad de dispositivos en un repositorio de modelos de empresa de IoT Plug and Play:

```azurecli
az iot pnp capability-model update --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

No puede actualizar directamente un modelo en el repositorio de modelos públicos.

Publique un modelo de funcionalidad del dispositivo desde el repositorio de modelos de empresa de IoT Plug and Play en el repositorio de modelos públicos. Esta operación hace que el modelo sea inmutable:

```azurecli
az iot pnp capability-model publish --model {YourModelID} --login {YourCompanyModelRepoConnectionString}
```

Solo los asociados de Microsoft pueden publicar modelos en el repositorio de modelos públicos.

## <a name="next-steps"></a>Pasos siguientes

En este artículo de procedimientos, ha aprendido a instalar y usar la extensión de Azure IoT para la CLI de Azure a fin de interactuar con los dispositivos Plug and Play. El siguiente paso sugerido es obtener información sobre cómo [administrar modelos](./howto-manage-models.md).
