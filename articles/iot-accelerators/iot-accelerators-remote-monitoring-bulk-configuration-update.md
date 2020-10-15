---
title: Administración de dispositivos conectados a supervisión remota de forma masiva en Azure | Microsoft Docs
description: En este tutorial aprenderá a administrar los dispositivos conectados a una solución de supervisión remota de forma masiva.
author: Philmea
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: philmea
ms.openlocfilehash: 57d2ac4f1e80c24a750e0e5f8eb22850718594a1
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92071037"
---
# <a name="tutorial-manage-your-connected-devices-in-bulk"></a>Tutorial: Administración de los dispositivos conectados de forma masiva

En este tutorial, usará el acelerador de soluciones de supervisión remota para administrar la configuración de los dispositivos conectados.

Como operador de Contoso, deberá configurar un grupo de dispositivos con una nueva versión de firmware. No desea tener que actualizar el firmware en cada dispositivo individualmente. Para actualizar el firmware en un grupo de dispositivos, puede usar la administración automática de dispositivos y grupos de dispositivos en el acelerador de soluciones de supervisión remota. Cualquier dispositivo que agregue al grupo de dispositivos obtiene el firmware más reciente tan pronto como el dispositivo se ponga en línea.

En este tutorial, hizo lo siguiente:

>[!div class="checklist"]
> * Creación de un grupo de dispositivos
> * Preparación y hospedaje del firmware
> * Creación de la configuración de un dispositivo en Azure Portal
> * Importación de la configuración de un dispositivo en una solución de supervisión remota
> * Implementación de la configuración de los dispositivos en el grupo de dispositivos
> * Supervisión de la implementación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

<!--
If this is going to be a tutorial - we need to split this include into two so that we can accommodate the additional prerequisites:

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]
-->

## <a name="prerequisites"></a>Prerrequisitos

Para seguir este tutorial, necesita una instancia implementada de la solución de supervisión remota en la suscripción de Azure.

Si aún no ha implementado el acelerador de soluciones de supervisión remota, debe completar la guía de inicio rápido [Implementación de una solución de supervisión remota basada en la nube](quickstart-remote-monitoring-deploy.md).

Necesitará una cuenta de Azure Storage para hospedar los archivos de firmware. Puede usar una cuenta de almacenamiento existente o [crear una nueva](../storage/common/storage-account-create.md) en su suscripción.

En este tutorial se usa un dispositivo [IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) como dispositivo de ejemplo.

Necesita tener el siguiente software instalado en la máquina local:

* [Visual Studio Code (VS Code)](https://code.visualstudio.com/).
* La extensión VS Code de [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench).

Antes de comenzar:

* Asegúrese de que la versión del [cargador de arranque en el dispositivo de IoT DevKit sea 1.4.0 o posterior](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/).
* Asegúrese de que la versión del SDK de IoT DevKit sea la misma que la del cargador de arranque. Puede actualizar el SDK de IoT DevKit con Azure IoT Workbench en VS Code. Abra la paleta de comandos y escriba **Arduino: Board Manager**. Para más información, consulte [Setting up a development environment](../iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started.md#prepare-the-development-environment) (Configuración de un entorno de desarrollo).

También tiene que conectar al menos un dispositivo IoT DevKit al acelerador de soluciones de supervisión remota. Si no ha conectado este tipo de dispositivo, consulte [Conexión de MXChip IoT DevKit AZ3166 al acelerador de la solución Supervisión remota de IoT](./iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoring-v2.md).

## <a name="navigate-to-the-dashboard"></a>Vaya al panel.

Para ver el panel de la solución de supervisión remota en el explorador, vaya primero a [Aceleradores de soluciones de IoT de Microsoft Azure](https://www.azureiotsolutions.com/Accelerators#dashboard). Se le pedirá que inicie sesión con sus credenciales de suscripción de Azure.

A continuación, haga clic en **Iniciar** en el icono del acelerador de soluciones de supervisión remota que implementó en el [Inicio rápido](quickstart-remote-monitoring-deploy.md).

## <a name="create-a-device-group"></a>Creación de un grupo de dispositivos

Para actualizar automáticamente el firmware en un grupo de dispositivos, los dispositivos deben ser miembros de un grupo de dispositivos en la solución de supervisión remota:

1. En la página **Dispositivos**, seleccione todos los dispositivos **IoT DevKit** que ha conectado al acelerador de soluciones. A continuación, haga clic en **Trabajos**.

1. En el panel **Trabajos**, seleccione **Etiqueta**, establezca el nombre del trabajo en **AddDevKitTag** y, a continuación, agregue una etiqueta de texto llamada **IsDevKitDevice** con el valor **Y**. A continuación, haga clic en **Aplicar**.

1. Ahora puede usar los valores de etiqueta para crear un grupo de dispositivos. En la página **Dispositivos**, haga clic en **Administrar grupos de dispositivos**.

1. Cree un filtro de texto que use el nombre de etiqueta **IsDevKitDevice** y el valor **Y** en la condición. Guarde el grupo de dispositivos como **Dispositivos IoT DevKit**.

Más adelante en este tutorial, usará este grupo de dispositivos para aplicar una configuración de dispositivo que actualice el firmware de todos los miembros.

## <a name="prepare-and-host-the-firmware"></a>Preparación y hospedaje del firmware

La extensión de VS Code [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) incluye el código de dispositivo de ejemplo para la actualización del firmware.

### <a name="open-the-firmware-ota-sample-in-vs-code"></a>Apertura del ejemplo Firmware OTA en VS Code

1. Asegúrese de que IoT DevKit no está conectado al equipo. Inicie VS Code y luego conecte el DevKit al equipo.

1. Presione **F1** para abrir la paleta de comandos y escriba y seleccione **IoT Workbench: Examples** (IoT Workbench: ejemplos). A continuación, seleccione **IoT DevKit** como placa.

1. Busque **Firmware OTA** y haga clic en **Abrir ejemplo**. Se abre una nueva ventana de VS Code y muestra la carpeta del proyecto **firmware_ota**:

    ![IoT Workbench, seleccione Ejemplo Firmware OTA](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-firmware-example.png)

### <a name="build-the-new-firmware"></a>Compilación del nuevo firmware

La versión inicial del firmware del dispositivo es 1.0.0. El nuevo firmware debe tener un número de versión posterior.

1. En VS Code, abra el archivo **FirmwareOTA.ino** y cambie `currentFirmwareVersion` de `1.0.0` a `1.0.1`:

    ![Cambio de la versión de firmware](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Abra la paleta de comandos y escriba y seleccione **IoT Workbench: Device** (IoT Workbench: dispositivo). A continuación, seleccione **Device Compile** (Compilación del dispositivo) para compilar el código:

    ![Compilación del dispositivo](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-compile.png)

    VS Code guarda el archivo compilado en la carpeta del proyecto `.build`. Según la configuración, VS Code puede ocultar la carpeta `.build` en la vista del explorador.

### <a name="generate-the-crc-value-and-calculate-the-firmware-file-size"></a>Generación del valor CRC y cálculo del tamaño de archivo del firmware

1. Abra la paleta de comandos y escriba y seleccione **IoT Workbench: Device** (IoT Workbench: dispositivo). A continuación, seleccione **Generate CRC** (Generar CRC).

    ![Generación de CRC](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-crc.png)

1. VS Code genera el valor CRC, el nombre de archivo y la ruta de acceso del firmware, y el tamaño del archivo en la ventana de salida, y los imprime. Anote estos valores para usarlos más adelante:

    ![Información de CRC](media/iot-accelerators-remote-monitoring-bulk-configuration-update/crc-info.png)

### <a name="upload-the-firmware-to-the-cloud"></a>Carga del firmware en la nube

Use su cuenta de almacenamiento de Azure para hospedar el nuevo archivo de firmware en la nube.

1. Vaya a la cuenta de almacenamiento en Azure Portal. En la sección Servicios, haga clic en **Blobs**. Cree un contenedor público llamado **firmware** para almacenar los archivos de firmware:

    ![Creación de la carpeta](media/iot-accelerators-remote-monitoring-bulk-configuration-update/blob-folder.png)

1. Para cargar el archivo de firmware en el contenedor, seleccione el contenedor **firmware** y haga clic en **Cargar**.

1. Seleccione **FirmwareOTA.ino.bin**. Tomó nota de la ruta de acceso completa de este archivo en la sección anterior.

1. Una vez completada la carga del archivo de firmware, tome nota de su dirección URL.

### <a name="build-and-upload-the-original-firmware-to-the-iot-devkit-device"></a>Creación y carga del firmware original en el dispositivo de IoT DevKit

1. En VS Code, abra el archivo **FirmwareOTA.ino** y cambie `currentFirmwareVersion` de nuevo a `1.0.0`:

    ![Versión 1.0.0](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Abra la paleta de comandos y escriba y seleccione **IoT Workbench: Device** (IoT Workbench: dispositivo). A continuación, seleccione **Device Upload** (Carga del dispositivo):

    ![Carga del dispositivo](media/iot-accelerators-remote-monitoring-bulk-configuration-update/device-upload.png)

1. VS Code comprueba y carga el código en el dispositivo IoT DevKit.

1. Cuando la carga finaliza, el dispositivo de IoT DevKit se reinicia. Cuando finaliza el reinicio, la pantalla de IoT DevKit muestra **FW version: 1.0.0**, y que está buscando nuevo firmware:

    ![La fotografía muestra el dispositivo IoT DevKit, y su pantalla indica la versión de firmware y que está buscando nuevo firmware.](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-1.jpg)

## <a name="create-a-device-configuration"></a>Creación de la configuración de dispositivos

La configuración de los dispositivos especifica el estado deseado de estos. Normalmente, un desarrollador [crea la configuración](../iot-hub/iot-hub-automatic-device-management.md#create-a-configuration) en la página **IoT device configuration** (Configuración del dispositivo IoT) de Azure Portal. La configuración de un dispositivo es un documento JSON que especifica el estado deseado de los dispositivos y un conjunto de métricas.

Guarde la configuración siguiente como un archivo denominado **firmware-update.json** en la máquina local. Reemplace los marcadores de posición `YOURSTRORAGEACCOUNTNAME`, `YOURCHECKSUM` y `YOURPACKAGESIZE` por los valores que anotó anteriormente:

```json
{
  "id": "firmware-update",
  "schemaVersion": null,
  "labels": {
    "Version": "1.0.1",
    "Devices": "IoT DevKit"
  },
  "content": {
    "deviceContent": {
      "properties.desired.firmware": {
        "fwVersion": "1.0.1",
        "fwPackageURI": "https://YOURSTRORAGEACCOUNTNAME.blob.core.windows.net/firmware/FirmwareOTA.ino.bin",
        "fwPackageCheckValue": "YOURCHECKSUM",
        "fwSize": YOURPACKAGESIZE
      }
    }
  },
  "targetCondition": "",
  "priority": 10,
  "systemMetrics": {
    "results": {
      "targetedCount": 0,
      "appliedCount": 0
    },
    "queries": {
      "targetedCount": "",
      "appliedCount": "select deviceId from devices where configurations.[[firmware-update]].status = 'Applied'"
    }
  },
  "metrics": {
    "results": {
      "Current": 1
    },
    "queries": {
      "Current": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Current' AND properties.reported.firmware.type='IoTDevKit'",
      "Downloading": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Downloading' AND properties.reported.firmware.type='IoTDevKit'",
      "Verifying": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Verifying' AND properties.reported.firmware.type='IoTDevKit'",
      "Applying": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Applying' AND properties.reported.firmware.type='IoTDevKit'",
      "Error": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Error' AND properties.reported.firmware.type='IoTDevKit'"
    }
  }
}
```

Use este archivo de configuración en la sección siguiente.

## <a name="import-a-configuration"></a>Importación de una configuración

En esta sección, va a importar la configuración de un dispositivo como un paquete en el acelerador de soluciones de supervisión remota. Normalmente, un operador realiza esta tarea.

1. En la interfaz de usuario web de supervisión remota, vaya a la página **Paquetes** y haga clic en **+ Nuevo paquete**:

    ![Nuevo paquete](media/iot-accelerators-remote-monitoring-bulk-configuration-update/packagepage.png)

1. En el panel **Nuevo paquete**, elija **Configuración del dispositivo** como tipo de paquete y **Firmware** como tipo de configuración. Haga clic en **Examinar** para encontrar el archivo **firmware-update.json** en la máquina local y, a continuación, haga clic en **Cargar**:

    ![Carga del paquete](media/iot-accelerators-remote-monitoring-bulk-configuration-update/uploadpackage.png)

1. La lista de paquetes incluye ahora el paquete **firmware-update**.

## <a name="deploy-the-configuration-to-your-devices"></a>Implementación de la configuración en los dispositivos

En esta sección, creará y ejecutará una implementación que aplica la configuración del dispositivo a los dispositivos de IoT DevKit.

1. En la interfaz de usuario web de supervisión remota, vaya a la página **Implementaciones** y haga clic en **+ Nueva implementación**:

    ![Nueva implementación](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentpage.png)

1. En el panel **Nueva implementación**, cree una implementación con la siguiente configuración:

    |Opción|Value|
    |---|---|
    |Nombre|Implementación de la actualización de firmware|
    |Tipo de paquete|Configuración del dispositivo|
    |Tipo de configuración|Firmware|
    |Paquete|firmware-update.json|
    |Grupo de dispositivos|Dispositivos de IoT DevKit|
    |Priority|10|

    ![Creación de una implementación](media/iot-accelerators-remote-monitoring-bulk-configuration-update/newdeployment.png)

    Haga clic en **Aplicar**. Verá una nueva implementación en la página **Implementaciones** que muestra las métricas siguientes:

    * **Objetivo** muestra el número de dispositivos en el grupo de dispositivos.
    * **Applied** (Se aplicó) muestra el número de dispositivos que se actualizaron con el contenido de la configuración.
    * **Succeeded** (Correcto) muestra el número de dispositivos en la implementación que informaron de que el proceso fue correcto.
    * **Failed** (Con error) muestra el número de dispositivos en la implementación que informaron de que el proceso no se pudo realizar.

## <a name="monitor-the-deployment"></a>Supervisión de la implementación

Después de unos minutos, el IoT DevKit recupera la información del nuevo firmware y empieza a descargarlo en el dispositivo:

![La fotografía muestra el dispositivo IoT DevKit, y su pantalla indica la nueva versión de firmware que se está descargando.](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-2.jpg)

Según la velocidad de la red, la descarga puede tardar hasta un par de minutos. Una vez descargado el firmware, el dispositivo comprueba el tamaño de archivo y el valor CRC. La pantalla de MXChip muestra **passed** si la comprobación es correcta.

![La fotografía muestra el dispositivo IoT DevKit, y su pantalla indica la versión de firmware y que la comprobación ha sido correcta.](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-3.jpg)

Si la comprobación es correcta, el dispositivo se reinicia. Verá una cuenta atrás de **5** a **0** antes de que ocurra el reinicio.

![La fotografía muestra el dispositivo IoT DevKit preparando el reinicio.](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-4.jpg)

Después del reinicio, el cargador de arranque de IoT DevKit actualiza el firmware a la nueva versión. Podría tardar varios segundos. Durante esta fase, el LED RGB del dispositivo está en rojo y la pantalla, en blanco.

![La fotografía muestra el dispositivo IoT DevKit realizando la actualización. No hay ninguna indicación, pero el LED RGB aparece iluminado en rojo.](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-5.jpg)

Cuando el reinicio haya finalizado, el dispositivo de IoT DevKit estará ejecutando la versión 1.0.1 del firmware.

![La fotografía muestra el dispositivo IoT DevKit, y su pantalla indica la nueva versión de firmware.](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-6.jpg)

En la página **Implementaciones**, haga clic en una implementación para ver el estado de los dispositivos a medida que se actualizan. Puede ver el estado de cada uno en el grupo de dispositivos y las métricas personalizadas que ha definido.

![Detalles de implementación](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentstatus.png)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha explicado cómo actualizar el firmware de un grupo de dispositivos conectados a la solución. Para actualizarlos, la solución utiliza la administración automática de dispositivos. Para más información acerca de la característica de administración automática de dispositivos en el centro de IoT subyacente de la solución, consulte [Configuración y supervisión de dispositivos IoT a escala mediante Azure Portal](../iot-hub/iot-hub-automatic-device-management.md).