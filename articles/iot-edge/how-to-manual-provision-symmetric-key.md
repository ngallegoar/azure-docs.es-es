---
title: 'Aprovisionamiento con claves simétricas: Azure IoT Edge | Microsoft Docs'
description: Después de la instalación, aprovisione un dispositivo IoT Edge con claves simétricas mediante su cadena de conexión y autentíquelo en IoT Hub
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/06/2020
ms.author: kgremban
ms.openlocfilehash: f5371539c1b45c14b519729c7c07003bf74847a0
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92043874"
---
# <a name="set-up-an-azure-iot-edge-device-with-symmetric-key-authentication"></a>Configuración de un dispositivo Azure IoT Edge con autenticación de claves simétricas

En este artículo se proporcionan los pasos para registrar un nuevo dispositivo IoT Edge en IoT Hub y configurarlo para que se autentique con claves simétricas.

Los pasos de este artículo le guían a través de un proceso denominado aprovisionamiento manual, donde cada dispositivo se conecta manualmente a su centro de IoT. La alternativa es el aprovisionamiento automático mediante IoT Hub Device Provisioning Service, que resulta útil cuando se tienen muchos dispositivos que aprovisionar.

<!--TODO: Add auto-provision info/links-->

En el caso del aprovisionamiento manual, tiene dos opciones para autenticar dispositivos IoT Edge:

* **Clave simétrica**: cuando se crea una nueva identidad de dispositivo en IoT Hub, el servicio crea dos claves. Debe colocar una de las claves en el dispositivo para que este la presente a IoT Hub al autenticarse.

  Este método de autenticación es más rápido para comenzar, pero no es tan seguro.

* **X.509 autofirmado**: se crean dos certificados de identidad X.509 y se colocan en el dispositivo. Cuando se crea una nueva identidad de dispositivo en IoT Hub, se proporcionan huellas digitales de ambos certificados. Cuando el dispositivo se autentica en IoT Hub, presenta sus certificados e IoT Hub puede comprobar que coinciden con las huellas digitales.

  Este método de autenticación es más seguro y se recomienda en escenarios de producción.

Este artículo le guía a través del proceso de registro y aprovisionamiento con autenticación de clave simétrica. Si quiere obtener información sobre cómo configurar un dispositivo con certificados X.509, vea [Configuración de un dispositivo Azure IoT Edge mediante autenticación de certificado X.509](how-to-manual-provision-x509.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de seguir los pasos de este artículo, debe tener un dispositivo con el entorno de ejecución de IoT Edge instalado. Si no es así, siga los pasos de [Instalación o desinstalación del entorno de ejecución de Azure IoT Edge](how-to-install-iot-edge.md).

## <a name="register-a-new-device"></a>Registro de un nuevo dispositivo

Todos los dispositivos que se conectan a IoT Hub tienen un identificador de dispositivo que se usa para realizar un seguimiento de las comunicaciones de nube a dispositivo o de dispositivo a nube. Configure un dispositivo con su información de conexión, que incluye el nombre de host del centro de IoT, el identificador de dispositivo y la información que el dispositivo usa para autenticarse en IoT Hub.

En el caso de la autenticación de clave simétrica, esta información se recopila en una *cadena de conexión* que se puede recuperar desde IoT Hub y luego colocar en el dispositivo IoT Edge.

Puede usar varias herramientas para registrar un nuevo dispositivo IoT Edge en IoT Hub y recuperar su cadena de conexión, en función de sus preferencias.

# <a name="portal"></a>[Portal](#tab/azure-portal)

### <a name="prerequisites-for-the-azure-portal"></a>Requisitos previos para Azure Portal

Un [centro de IoT](../iot-hub/iot-hub-create-through-portal.md) gratuito o estándar en la suscripción de Azure.

### <a name="create-an-iot-edge-device-in-the-azure-portal"></a>Creación de un dispositivo IoT Edge en Azure Portal

En IoT Hub de Azure Portal, los dispositivos IoT Edge se crean y administran por separado de los dispositivos IOT que no están habilitados para Edge.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.

1. En el panel izquierdo, seleccione **IoT Edge** en el menú y luego **Agregar un dispositivo IoT Edge**.

   ![Incorporación de un dispositivo IoT Edge desde Azure Portal](./media/how-to-manual-provision-symmetric-key/portal-add-iot-edge-device.png)

1. En la página **Crear un dispositivo**, proporcione la siguiente información:

   * Cree un identificador de dispositivo descriptivo.
   * Seleccione **Clave simétrica** como el tipo de autenticación.
   * Use la configuración predeterminada para la generación automática de claves de autenticación y para conectar el dispositivo nuevo a su centro.

1. Seleccione **Guardar**.

### <a name="view-iot-edge-devices-in-the-azure-portal"></a>Visualización de dispositivos IoT Edge en Azure Portal

Todos los dispositivos habilitados para Edge que se conectan al centro de IoT se enumeran en la página **IoT Edge**.

![Uso de Azure Portal para visualizar todos los dispositivos IoT Edge del centro de IoT](./media/how-to-manual-provision-symmetric-key/portal-view-devices.png)

### <a name="retrieve-the-connection-string-in-the-azure-portal"></a>Recuperación de la cadena de conexión en Azure Portal

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT.

1. En la página **IoT Edge** del portal, haga clic en el identificador de dispositivo en la lista de dispositivos de IoT Edge.
2. Copie el valor de la **cadena de conexión primaria** o la **cadena de conexión secundaria**.

# <a name="visual-studio-code"></a>Tener [Visual Studio Code](#tab/visual-studio-code)

### <a name="prerequisites-for-visual-studio-code"></a>Requisitos previos para Visual Studio Code

* Un [centro de IoT](../iot-hub/iot-hub-create-through-portal.md) gratuito o estándar en la suscripción de Azure
* Tener [Visual Studio Code](https://code.visualstudio.com/)
* [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) para Visual Studio Code

### <a name="sign-in-to-access-your-iot-hub"></a>Iniciar sesión para acceder a IoT Hub

Puede usar las extensiones de Azure IoT para Visual Studio Code para realizar operaciones con IoT Hub. Para que estas operaciones funcionen, debe iniciar sesión en su cuenta de Azure y seleccionar su instancia de IoT Hub.

1. En Visual Studio Code, abra la vista **Explorer** (Explorador).
1. En la parte inferior de Explorer, expanda la sección **Azure IoT Hub**.

   ![Sección Expansión de los dispositivos de Azure IoT Hub](./media/how-to-manual-provision-symmetric-key/azure-iot-hub-devices.png)

1. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub**. Si no ve el botón de los puntos suspensivos, mantenga el ratón sobre el encabezado o haga clic en él.
1. Elija **Select IoT Hub** (Seleccionar IoT Hub).
1. Si no ha iniciado sesión en su cuenta de Azure, siga las indicaciones para hacerlo.
1. Seleccione su suscripción a Azure.
1. Seleccione IoT Hub.

### <a name="create-an-iot-edge-device-with-visual-studio-code"></a>Creación de un dispositivo IoT Edge con Visual Studio Code

1. En el explorador de VS Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub).
1. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Si no ve el botón de los puntos suspensivos, mantenga el ratón sobre el encabezado o haga clic en él.
1. Seleccione **Create IoT Edge Device** (Crear un dispositivo de IoT Edge).
1. En el cuadro de texto que aparece, otorgue a su dispositivo un identificador.

En la pantalla de salida, consulte el resultado del comando. Se imprime la información del dispositivo, que incluye el valor de **deviceId** que proporcionó, y el valor de **connectionString** que puede usar para conectar su dispositivo físico a su instancia de IoT Hub.

En la pantalla de salida, consulte el resultado del comando. Se imprime la información del dispositivo, que incluye el valor de **deviceId** que proporcionó, y el valor de **connectionString** que puede usar para conectar su dispositivo físico a su instancia de IoT Hub.

### <a name="view-iot-edge-devices-with-visual-studio-code"></a>Creación de dispositivos IoT Edge con Visual Studio Code

Todos los dispositivos que se conectan a IoT Hub se enumeran en la sección **Azure IoT Hub** del explorador de Visual Studio Code. Los dispositivos de IoT Edge se distinguen de los dispositivos que no son Edge con un icono diferente y por el hecho de que los módulos **$edgeAgent** y **$edgeHub** se implementan en cada dispositivo IoT Edge.

![Uso de VS Code para visualizar todos los dispositivos IoT Edge del centro de IoT](./media/how-to-manual-provision-symmetric-key/view-devices.png)

### <a name="retrieve-the-connection-string-with-visual-studio-code"></a>Recuperación de la cadena de conexión con Visual Studio Code

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT.

1. Haga clic con el botón derecho en el identificador del dispositivo en la sección **Azure IoT Hub**.
1. Seleccione **Copy Device Connection String** (Copiar la cadena de conexión del dispositivo).

   La cadena de conexión se copiará al portapapeles.

También puede seleccionar **Get Device Info** (Obtener información del dispositivo), si hace clic con el botón derecho del mouse en el menú para ver toda la información del dispositivo, incluyendo la cadena de conexión, en la ventana de salida.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

### <a name="prerequisites-for-the-azure-cli"></a>Requisitos previos para la CLI de Azure

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) en la suscripción de Azure.
* La [CLI de Azure](/cli/azure/install-azure-cli) en su entorno. Como mínimo, la versión de la CLI de Azure debe ser la 2.0.70 o posterior. Use `az --version` para asegurarse. Esta versión admite comandos az extension e introduce la plataforma de comandos de Knack.
* La [extensión de IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension).

### <a name="create-an-iot-edge-device-with-the-azure-cli"></a>Creación de un dispositivo IoT Edge con la CLI de Azure

Use el comando [az iot hub device-identity create](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-create) para crear una nueva identidad de dispositivo en su centro de IoT. Por ejemplo:

   ```azurecli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Este comando incluye tres parámetros:

* `--device-id` o `-d`: proporcione un nombre descriptivo que sea único de su centro de IoT.
* `hub-name` o `-n`: proporcione el nombre de su centro de IoT.
* `--edge-enabled` o `--ee`: Declare que se trata de un dispositivo de IoT Edge.

   ![az iot hub device-identity create output](./media/how-to-manual-provision-symmetric-key/create-edge-device-cli.png)

### <a name="view-iot-edge-devices-with-the-azure-cli"></a>Creación de dispositivos IoT Edge con la CLI de Azure

Use el comando [az iot hub device-identity list](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-list) para ver todos los dispositivos en su centro de IoT. Por ejemplo:

   ```azurecli
   az iot hub device-identity list --hub-name [hub name]
   ```

Los dispositivos que están registrados como dispositivos IoT Edge tendrán la propiedad **capabilities.iotEdge** establecida en **true**.

### <a name="retrieve-the-connection-string-with-the-azure-cli"></a>Recuperación de la cadena de conexión con la CLI de Azure

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT. Use el comando [az iot hub device-identity show-connection-string](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-show-connection-string) para devolver la cadena de conexión para un único dispositivo:

   ```azurecli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

El valor del parámetro `device-id` distingue mayúsculas y minúsculas. No copie las comillas alrededor de la cadena de conexión.

---

## <a name="provision-an-iot-edge-device"></a>Aprovisionamiento de un dispositivo IoT Edge

Una vez que el dispositivo IoT Edge tiene una identidad en IoT Hub y una cadena de conexión que puede usar para la autenticación, hay que aprovisionar el propio dispositivo con esta información.

En un dispositivo Linux, la cadena de conexión se proporciona mediante la edición de un archivo config.yaml. En un dispositivo Windows, la cadena de conexión se proporciona mediante la ejecución de un script de PowerShell.

# <a name="linux"></a>[Linux](#tab/linux)

Abra el archivo de configuración en el dispositivo IoT Edge.

```bash
sudo nano /etc/iotedge/config.yaml
```

Busque las configuraciones de aprovisionamiento del archivo y quite la marca de comentario de la sección **Manual provisioning configuration using a connection string** (Configuración del aprovisionamiento manual mediante una cadena de conexión). 

Actualice el valor de **device_connection_string** con la cadena de conexión desde un dispositivo IoT Edge. Asegúrese de que las demás secciones de aprovisionamiento estén comentadas. Asegúrese de que la línea **provisioning:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios.

```yml
# Manual provisioning configuration using a connection string
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  dynamic_reprovisioning: false
```

Para pegar el contenido del portapapeles en Nano: `Shift+Right Click` o presione `Shift+Insert`.

Guarde y cierre el archivo.

   `CTRL + X`, `Y`, `Enter`

Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

```bash
sudo systemctl restart iotedge
```

# <a name="windows"></a>[Windows](#tab/windows)

1. En el dispositivo IoT Edge, ejecute PowerShell como administrador.

2. Use el comando [Initialize-IoTEdge](reference-windows-scripts.md#initialize-iotedge) para configurar el entorno de ejecución de IoT Edge en la máquina. El comportamiento predeterminado del comando es el aprovisionamiento manual con contenedores de Windows.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualConnectionString -ContainerOs Windows
   ```

   * Si va a usar contenedores de Linux, agregue el parámetro `-ContainerOs` a la marca. Sea coherente con la opción de contenedor seleccionada con el comando `Deploy-IoTEdge` ejecutado anteriormente.

      ```powershell
      . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
      Initialize-IoTEdge -ContainerOs Linux
      ```

   * Si ha descargado el script IoTEdgeSecurityDaemon.ps1 en el dispositivo para la instalación sin conexión o de una versión específica, asegúrese de hacer referencia a la copia local del script.

      ```powershell
      . <path>/IoTEdgeSecurityDaemon.ps1
      Initialize-IoTEdge -ManualX509
      ```

3. Cuando se le pida, especifique la cadena de conexión del dispositivo que ha recuperado en la sección anterior. La cadena de conexión del dispositivo asocia el dispositivo físico a un identificador de dispositivo de IoT Hub y proporciona información de autenticación.

   La cadena de conexión del dispositivo adopta el formato siguiente y no debe incluir comillas: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

Al aprovisionar un dispositivo manualmente, puede usar parámetros adicionales para modificar el proceso, lo que incluye:

* Dirigir el tráfico para que pase por un servidor proxy
* Declarar una imagen de contenedor de edgeAgent específica y proporcionar credenciales si se encuentra en un registro privado

Para obtener más información sobre estos parámetros adicionales, vea [Scripts de PowerShell para IoT Edge en Windows](reference-windows-scripts.md).

---

[!INCLUDE [Verify and troubleshoot installation](../../includes/iot-edge-verify-troubleshoot-install.md)]

## <a name="next-steps"></a>Pasos siguientes

Vaya a [Implementación de módulos de Azure IoT Edge](how-to-deploy-modules-portal.md) para obtener información sobre cómo implementar módulos en el dispositivo.