---
title: 'Aprovisionamiento con certificados X.509: Azure IoT Edge | Microsoft Docs'
description: Después de la instalación, aprovisione un dispositivo IoT Edge con sus certificados de identidad de dispositivo y autentíquelo en IoT Hub.
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 10/06/2020
ms.author: kgremban
ms.openlocfilehash: b1aa12bd73772b5d6332a36d749ec4d7d10d4026
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92048192"
---
# <a name="set-up-an-azure-iot-edge-device-with-x509-certificate-authentication"></a>Configuración de un dispositivo Azure IoT Edge con autenticación de certificados X.509

En este artículo se proporcionan los pasos para registrar un dispositivo IoT Edge nuevo en IoT Hub y configurarlo para que se autentique con certificados X.509.

Los pasos descritos en este artículo lo guiarán a través de un proceso llamado aprovisionamiento manual, donde se conecta manualmente cada dispositivo a su centro de IoT. La alternativa es el aprovisionamiento automático mediante IoT Hub Device Provisioning Service, lo que resulta útil cuando se tienen muchos dispositivos para aprovisionar.

<!--TODO: Add auto-provision info/links-->

Para el aprovisionamiento manual, tiene dos opciones para autenticar dispositivos IoT Edge:

* **Clave simétrica**: cuando se crea una identidad de dispositivo nueva en IoT Hub, el servicio crea dos claves. Debe colocar una de las claves en el dispositivo y este presenta la clave a IoT Hub al autenticarse.

  Este método de autenticación es más rápido para comenzar, pero no es tan seguro.

* **X.509 autofirmado**: cree dos certificados de identidad X.509 y colóquelos en el dispositivo. Cuando se crea una identidad de dispositivo nueva en IoT Hub, se proporcionan huellas digitales desde ambos certificados. Cuando el dispositivo se autentica en IoT Hub, presenta sus certificados e IoT Hub puede comprobar que coinciden con las huellas digitales.

  Este método de autenticación es más seguro y se recomienda para escenarios de producción.

Este artículo lo guiará a través del proceso de registro y aprovisionamiento con autenticación de certificados X.509. Si quiere aprender a configurar un dispositivo con claves simétricas, consulte [Configuración de un dispositivo Azure IoT Edge con autenticación de claves simétricas](how-to-manual-provision-symmetric-key.md).

## <a name="prerequisites"></a>Requisitos previos

Antes de seguir los pasos de este artículo, debe tener instalado un dispositivo con entorno de ejecución de Azure IoT Edge instalado. Si no es así, siga los pasos descritos en [Instalación o desinstalación del entorno de ejecución de Azure IoT Edge](how-to-install-iot-edge.md).

El aprovisionamiento manual con certificados X.509 requiere IoT Edge, versión 1.0.10 o posterior.

## <a name="create-certificates-and-thumbprints"></a>Creación de certificados y huellas digitales



<!-- TODO -->

## <a name="register-a-new-device"></a>Registro de un dispositivo nuevo

Todos los dispositivos que se conectan a un centro de IoT tienen un id. de dispositivo que se usa para realizar un seguimiento de las comunicaciones de la nube al dispositivo o del dispositivo a la nube. Configure un dispositivo con su información de conexión, que incluye el nombre de host del centro de IoT, el id. de dispositivo y la información que el dispositivo usa para autenticarse en IoT Hub.

En el caso de la autenticación mediante certificados X.509, esta información se proporciona en forma de *huellas digitales* tomadas de los certificados de identidad del dispositivo. Estas huellas digitales se proporcionan a IoT Hub en el momento de registrar el dispositivo para que el servicio pueda reconocer el dispositivo al conectarse.

Puede usar varias herramientas para registrar un dispositivo IoT Edge nuevo en IoT Hub y cargar sus huellas digitales de certificado. 

# <a name="portal"></a>[Portal](#tab/azure-portal)

### <a name="prerequisites-for-the-azure-portal"></a>Requisitos previos para Azure Portal

Un [centro de IoT](../iot-hub/iot-hub-create-through-portal.md) gratuito o estándar en la suscripción de Azure.

### <a name="create-an-iot-edge-device-in-the-azure-portal"></a>Creación de un dispositivo IoT Edge en Azure Portal

En IoT Hub de Azure Portal, los dispositivos IoT Edge se crean y administran por separado de los dispositivos IOT que no están habilitados para Edge.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.

1. En el panel izquierdo, seleccione **IoT Edge** en el menú y, a continuación, seleccione **Agregar un dispositivo IoT Edge**.

   ![Agrear un dispositivo IoT Edge desde Azure Portal](./media/how-to-manual-provision-symmetric-key/portal-add-iot-edge-device.png)

1. En la página **Crear un dispositivo**, proporcione la información siguiente:

   * Cree un id. de dispositivo descriptivo. Anote este id. de dispositivo porque lo usará en la sección siguiente.
   * Seleccione **X.509 autofirmado** como el tipo de autenticación.
   * Proporcione las huellas digitales de certificado de identidad principal y secundaria. Los valores de huella digital son 40 caracteres hexadecimales para hashes de tipo SHA-1 o 64 caracteres hexadecimales para hashes de tipo SHA-256.

1. Seleccione **Guardar**.

### <a name="view-iot-edge-devices-in-the-azure-portal"></a>Visualización de dispositivos IoT Edge en Azure Portal

Todos los dispositivos habilitados para Edge que se conectan al centro de IoT se enumeran en la página **IoT Edge**.

![Visualización de todos los dispositivos IoT Edge en su centro de IoT](./media/how-to-manual-provision-symmetric-key/portal-view-devices.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

### <a name="prerequisites-for-the-azure-cli"></a>Requisitos previos para la CLI de Azure

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) en la suscripción de Azure.
* La [CLI de Azure](/cli/azure/install-azure-cli) en su entorno. Como mínimo, la versión de la CLI de Azure debe ser la 2.0.70. Use `az --version` para asegurarse. Esta versión admite comandos az extension e introduce la plataforma de comandos de Knack.
* La [extensión de IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension).

### <a name="create-an-iot-edge-device-with-the-azure-cli"></a>Creación de un dispositivo IoT Edge con la CLI de Azure

Use el comando [az iot hub device-identity create](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-create) para crear una nueva identidad de dispositivo en su centro de IoT. Por ejemplo:

   ```azurecli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled --auth-method x509_thumbprint --primary-thumbprint [SHA thumbprint] --secondary-thumbprint [SHA thumbprint]
   ```

Este comando incluye varios parámetros:

* `--device-id` o `-d`: proporcione un nombre descriptivo que sea único de su centro de IoT. Anote este id. de dispositivo porque lo usará en la sección siguiente.
* `hub-name` o `-n`: proporcione el nombre de su centro de IoT.
* `--edge-enabled` o `--ee`: declare que se trata de un dispositivo IoT Edge.
* `--auth-method` o `--am`: declare el tipo de autorización que usará el dispositivo. En este caso, vamos a usar huellas digitales de certificado X.509.
* `--primary-thumbprint` o `--ptp`: proporcione una huella digital de certificado X.509 que se usará como clave principal.
* `--secondary-thumbprint` o `--stp`: proporcione una huella digital de certificado X.509 que se usará como clave secundaria.

### <a name="view-iot-edge-devices-with-the-azure-cli"></a>Creación de dispositivos IoT Edge con la CLI de Azure

Use el comando [az iot hub device-identity list](/cli/azure/ext/azure-iot/iot/hub/device-identity#ext-azure-iot-az-iot-hub-device-identity-list) para ver todos los dispositivos en su centro de IoT. Por ejemplo:

   ```azurecli
   az iot hub device-identity list --hub-name [hub name]
   ```

Agregue la marca `--edge-enabled` o `--ee` para mostrar solo los dispositivos IoT Edge de su centro de IoT.

Los dispositivos que están registrados como dispositivos IoT Edge tendrán la propiedad **capabilities.iotEdge** establecida en **true**.

--- 

## <a name="configure-an-iot-edge-device"></a>Configurar un dispositivo IoT Edge

Una vez que el dispositivo IoT Edge tiene una identidad en IoT Hub, debe configurar el dispositivo con su identidad de nube, así como sus certificados de identidad.

Para proporcionar esta información en un dispositivo Linux, edite un archivo config.yaml. Para proporcionar esta información en un dispositivo Windows, ejecute un script de PowerShell.

# <a name="linux"></a>[Linux](#tab/linux)

1. Abra el archivo de configuración en el dispositivo IoT Edge.

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

1. Busque la sección de configuraciones de aprovisionamiento del archivo. 

1. Marque como comentario la sección **Manual provisioning configuration using a connection string** (Configuración del aprovisionamiento manual mediante una cadena de conexión).

1. Quite la marca de comentario de la sección **Manual provisioning configuration using an X.509 identity certificate** (Configuración del aprovisionamiento manual mediante un certificado digital X.509). Asegúrese de que la línea **provisioning:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios.

   ```yml
   # Manual provisioning configuration using a connection string
   provisioning:
     source: "manual"
     authentication:
       method: "x509"
       iothub_hostname: "<REQUIRED IOTHUB HOSTNAME>"
       device_id: "<REQUIRED DEVICE ID PROVISIONED IN IOTHUB>"
       identity_cert: "<REQUIRED URI TO DEVICE IDENTITY CERTIFICATE>"
       identity_pk: "<REQUIRED URI TO DEVICE IDENTITY PRIVATE KEY>"
     dynamic_reprovisioning: false
   ```

1. Actualice los campos siguientes:

   * **iothub_hostname**: nombre de host del centro de IoT al que se conectará el dispositivo. Por ejemplo, `{IoT hub name}.azure-devices.net`.
   * **device_id**: el id. que proporcionó al registrar el dispositivo.
   * **identity_cert**: URI a un certificado de identidad en el dispositivo. Por ejemplo, `file:///path/identity_certificate.pem`.
   * **identity_pk**: URI al archivo de clave privada para el certificado de identidad proporcionado. Por ejemplo, `file:///path/identity_key.pem`.

1. Guarde y cierre el archivo.

   `CTRL + X`, `Y`, `Enter`

1. Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

   ```bash
   sudo systemctl restart iotedge
   ```

# <a name="windows"></a>[Windows](#tab/windows)

1. En el dispositivo IoT Edge, ejecute PowerShell como administrador.

2. Use el comando [Initialize-IoTEdge](reference-windows-scripts.md#initialize-iotedge) para configurar el entorno de ejecución de Azure IoT Edge en el equipo.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ManualX509
   ```

   * Si va a usar contenedores de Linux, agregue el parámetro `-ContainerOs` a la marca. Sea coherente con la opción de contenedor que eligió con el comando `Deploy-IoTEdge` ejecutado anteriormente.

      ```powershell
      . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
      Initialize-IoTEdge -ManualX509 -ContainerOs Linux
      ```

   * Si descargó el script IoTEdgeSecurityDaemon.ps1 en el dispositivo para la instalación sin conexión o de una versión específica, asegúrese de hacer referencia a la copia local del script.

      ```powershell
      . <path>/IoTEdgeSecurityDaemon.ps1
      Initialize-IoTEdge -ManualX509
      ```

3. Cuando se le solicite, proporcione la siguiente información:

   * **IotHubHostName**: nombre de host del centro de IoT al que se conectará el dispositivo. Por ejemplo, `{IoT hub name}.azure-devices.net`.
   * **DeviceId**: el id. que proporcionó al registrar el dispositivo.
   * **X509IdentityCertificate**: ruta de acceso absoluta a un certificado de identidad en el dispositivo. Por ejemplo, `C:\path\identity_certificate.pem`.
   * **X509IdentityPrivateKey**: ruta de acceso absoluta al archivo de clave privada para el certificado de identidad proporcionado. Por ejemplo, `C:\path\identity_key.pem`.

Al aprovisionar un dispositivo manualmente, puede usar parámetros adicionales para modificar el proceso, incluyendo:

* Dirigir el tráfico para que pase por un servidor proxy
* Declarar una imagen de contenedor de edgeAgent específica y proporcionar las credenciales si se encuentra en un registro privado

Para más información sobre estos parámetros adicionales, consulte [Scripts de PowerShell para IoT Edge en Windows](reference-windows-scripts.md).

---

[!INCLUDE [Verify and troubleshoot installation](../../includes/iot-edge-verify-troubleshoot-install.md)]

## <a name="next-steps"></a>Pasos siguientes

Siga [implementando módulos de IoT Edge](how-to-deploy-modules-portal.md) para información sobre cómo implementar módulos en el dispositivo.