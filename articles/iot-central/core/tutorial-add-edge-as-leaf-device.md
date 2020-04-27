---
title: Incorporación de un dispositivo Azure IoT Edge a Azure IoT Central | Microsoft Docs
description: Como operador, agregue un dispositivo Azure IoT Edge a la aplicación Azure IoT Central.
author: rangv
ms.author: rangv
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: c60cf4b90b089d271c0ccd91031420efe9017b1e
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758167"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application"></a>Tutorial: Incorporación de un dispositivo Azure IoT Edge a la aplicación Azure IoT Central

*Este artículo se aplica a los generadores de soluciones y a los desarrolladores de dispositivos.*

En este tutorial se explica cómo configurar y agregar un dispositivo Azure IoT Edge a la aplicación de Azure IoT Central. En el tutorial se usa una máquina virtual Linux habilitada para IoT Edge de Azure Marketplace para simular un dispositivo IoT Edge. El dispositivo IoT Edge usa un módulo que genera datos de telemetría ambiental simulados. Puede ver los datos de telemetría en un panel en la aplicación de IoT Central.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una plantilla de dispositivo para un dispositivo IoT Edge
> * Crear un dispositivo IoT Edge en IoT Central
> * Implementar un dispositivo IoT Edge simulado en una máquina virtual Linux

## <a name="prerequisites"></a>Prerrequisitos

Complete el inicio rápido [Creación de una aplicación de Azure IoT Central](./quick-deploy-iot-central.md) para crear una aplicación de IoT Central mediante la plantilla **Aplicación personalizada > Aplicación personalizada**.

Para completar los pasos de este tutorial, deberá tener una suscripción de Azure activa.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

Descargue el archivo de manifiesto de IoT Edge desde GitHub. Haga clic con el botón derecho en el siguiente vínculo y seleccione **Guardar vínculo como**: [EnvironmentalSensorManifest.json](https://raw.githubusercontent.com/Azure-Samples/iot-central-docs-samples/master/iotedge/EnvironmentalSensorManifest.json)

## <a name="create-device-template"></a>Creación de la plantilla de dispositivo

En esta sección, creará una plantilla de dispositivo para un dispositivo IoT Edge que se conecta a la aplicación de IoT Central. Para empezar a trabajar, importe un manifiesto de IoT Edge y, a continuación, modifique la plantilla para agregar las definiciones de telemetría y las vistas:

### <a name="import-manifest-to-create-template"></a>Importación de un manifiesto para crear una plantilla

Para crear una plantilla de dispositivo a partir de un manifiesto de IoT Edge:

1. En la aplicación de IoT Central, vaya a **Plantillas de dispositivo** y seleccione **+ Nueva**.

1. En la página **Seleccionar tipo de plantilla**, seleccione el icono **Azure IoT Edge**. Después, seleccione **Next: Customize** (Personalizar)

1. En la página **Cargar un manifiesto de implementación de Azure IoT Edge**, seleccione **Examinar** para cargar el archivo **EnvironmentalSensorManifest.json** que descargó anteriormente. Después, seleccione **Next: Review** (Siguiente: revisar).

1. En la página **Revisar**, seleccione **Crear**.

1. Una vez creada la plantilla, cambie su nombre a *Environmental Sensor Edge Device*.

1. Seleccione la interfaz **Administrar** en el módulo **SimulatedTemperatureSensor** para ver las dos propiedades definidas en el manifiesto:

![Plantilla de dispositivo creada a partir de un manifiesto de IoT Edge](./media/tutorial-add-edge-as-leaf-device/imported-manifest.png)

### <a name="add-telemetry-to-manifest"></a>Adición de telemetría al manifiesto

Un manifiesto de IoT Edge no define la telemetría que envía un módulo. Debe agregar las definiciones de telemetría a la plantilla de dispositivo. El módulo **SimulatedTemperatureSensor** envía mensajes de telemetría que tienen un aspecto similar al del siguiente código JSON:

```json
{
    "machine": {
        "temperature": 75.0,
        "pressure": 40.2
    },
    "ambient": {
        "temperature": 23.0,
        "humidity": 30.0
    },
    "timeCreated": ""
}
```

Para agregar las definiciones de telemetría a la plantilla de dispositivo:

1. Seleccione la interfaz **Administrar** en la plantilla **Environmental Sensor Edge Device**.

1. Seleccione **+ Agregar funcionalidad**. Escriba *machine* (máquina) como **Nombre para mostrar** y asegúrese de que **Tipo de funcionalidad** es **Telemetría**.

1. Seleccione **Objeto** como tipo de esquema y, a continuación, seleccione **Definir**. En la página de definición de objeto, agregue *temperature* (temperatura) y *pressure* (presión) como atributos de tipo **Doble** y, a continuación, seleccione **Aplicar**.

1. Seleccione **+ Agregar funcionalidad**. Escriba *ambient* (ambiental) como **Nombre para mostrar** y asegúrese de que **Tipo de funcionalidad** es **Telemetría**.

1. Seleccione **Objeto** como tipo de esquema y, a continuación, seleccione **Definir**. En la página de definición de objeto, agregue *temperature* (temperatura) y *humidity* (humedad) como atributos de tipo **Doble** y, a continuación, seleccione **Aplicar**.

1. Seleccione **+ Agregar funcionalidad**. Escriba *timeCreated* (hora de creación) como **Nombre para mostrar** y asegúrese de que **Tipo de funcionalidad** es **Telemetría**.

1. Seleccione **DateTime** (Fecha y hora) como el tipo de esquema.

1. Seleccione **Guardar** para actualizar la plantilla.

La interfaz **Administrar** ahora incluye los tipos de telemetría **machine**, **ambient** y **timeCreated**:

![Interfaz con los tipos de telemetría machine y ambient](./media/tutorial-add-edge-as-leaf-device/manage-interface.png)

### <a name="add-views-to-template"></a>Adición de vistas a la plantilla

La plantilla de dispositivo todavía no tiene una vista que permita al operador ver la telemetría del dispositivo IoT Edge. Para agregar una vista a la plantilla de dispositivo:

1. Seleccione **Vistas** en la plantilla **Environmental Sensor Edge Device**.

1. En la página **Seleccionar para agregar una nueva vista**, seleccione el icono **Visualizar el dispositivo**.

1. Cambie el nombre de la vista a *View IoT Edge device telemetry* (Ver telemetría del dispositivo IoT Edge).

1. Seleccione los tipos de telemetría **ambient** y **machine**. A continuación, seleccione **Agregar icono**.

1. Seleccione **Guardar** para guardar la vista **View IoT Edge device telemetry**.

![Plantilla de dispositivo con vista de telemetría](./media/tutorial-add-edge-as-leaf-device/template-telemetry-view.png)

### <a name="publish-the-template"></a>Publicación de la plantilla

Antes de poder agregar un dispositivo que use la plantilla **Environmental Sensor Edge Device**, debe publicar la plantilla.

Vaya a la plantilla **Environmental Sensor Edge Device** y seleccione **Publicar**. A continuación, seleccione **Publicar** para publicar la plantilla:

![Publicación de la plantilla de dispositivo](./media/tutorial-add-edge-as-leaf-device/publish-template.png)

## <a name="add-iot-edge-device"></a>Adición de un dispositivo IoT Edge

Ahora que ha publicado la plantilla **Environmental Sensor Edge Device**, puede agregar un dispositivo a la aplicación de IoT Central:

1. En la aplicación de IoT Central, vaya a la página **Dispositivos** y seleccione **Environmental Sensor Edge Device** en la lista de plantillas disponibles.

1. Seleccione **+** para agregar un nuevo dispositivo desde la plantilla. En la página **Crear nuevo dispositivo**, seleccione **Crear**.

Ahora tiene un nuevo dispositivo con el estado de **Registrado**:

![Publicación de la plantilla de dispositivo](./media/tutorial-add-edge-as-leaf-device/new-device.png)

### <a name="get-the-device-credentials"></a>Obtención de las credenciales del dispositivo

Cuando implemente el dispositivo IoT Edge más adelante en este tutorial, necesitará las credenciales que permiten al dispositivo conectarse a la aplicación de IoT Central. Para obtener las credenciales del dispositivo:

1. En la página **Dispositivo**, seleccione el dispositivo que ha creado.

1. Seleccione **Conectar**.

1. En la página **Conexión del dispositivo**, tome nota de los valores **Ámbito del identificador**, **Identificador de dispositivo** y **Clave principal**. Utilizará estos valores más adelante.

1. Seleccione **Cerrar**.

Ya ha terminado de configurar la aplicación de IoT Central para habilitar la conexión de un dispositivo IoT Edge.

## <a name="deploy-an-iot-edge-device"></a>Implementación de un dispositivo IoT Edge

En este tutorial se usa una máquina virtual Linux habilitada para Azure IoT Edge, creada en Azure para simular un dispositivo IoT Edge. Para crear la máquina virtual habilitada para IoT Edge:

1. Vaya a [Azure IoT Edge en Ubuntu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) en Azure Marketplace. A continuación, seleccione **Obténgalo ahora**.

1. En la página **Crear esta aplicación en Azure**, seleccione **Continuar**. Este vínculo le lleva a Azure Portal, donde es posible que tenga que iniciar sesión en la suscripción de Azure.

1. En la página **Azure IoT Edge en Ubuntu** de Azure Portal, seleccione **Crear**.

1. En la página **Crear una máquina virtual > Básico**:

    - Seleccione su suscripción a Azure.
    - Cree un nuevo grupo de recursos llamado **iot-edge-devices**.
    - Use el nombre de máquina virtual: **iotedgevm**.
    - Elija la región más cercana.
    - Establezca el tipo de autenticación en **Contraseña**.
    - Elija un nombre de usuario y una contraseña.
    - Puede dejar las restantes opciones con sus valores predeterminados.
    - Seleccione **Revisar + crear**.

1. Una vez completada la validación, seleccione **Crear**.

Después de unos minutos, cuando la implementación se complete, seleccione **Ir al recurso**.

### <a name="provision-vm-as-an-iot-edge-device"></a>Aprovisionamiento de una máquina virtual como un dispositivo IoT Edge 

Para aprovisionar una máquina virtual como un dispositivo IoT Edge:

1. En la sección **Soporte técnico y solución de problemas**, seleccione **Serial Console**.

1. Presione **Entrar** para ver el símbolo del sistema `login:`. Escriba su nombre de usuario y contraseña para iniciar sesión.

1. Ejecute el siguiente comando para comprobar la versión del entorno de ejecución de IoT Edge. En el momento de escribir este documento, la versión es la 1.0.8:

    ```bash
    sudo iotedge --version
    ```

1. Use el editor `nano` para abrir el archivo config.yaml de IoT Edge:

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

1. Desplácese hacia abajo hasta que vea `# Manual provisioning configuration`. Convierta en comentario las tres líneas siguientes, tal como se muestra en el siguiente fragmento de código:

    ```yaml
    # Manual provisioning configuration
    #provisioning:
    #  source: "manual"
    #  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
    ```

1. Desplácese hacia abajo hasta que vea `# DPS symmetric key provisioning configuration`. Quite las marcas de comentario de las siguientes ocho líneas, como se muestra en el siguiente fragmento de código:

    ```yaml
    # DPS symmetric key provisioning configuration
    provisioning:
      source: "dps"
      global_endpoint: "https://global.azure-devices-provisioning.net"
      scope_id: "{scope_id}"
      attestation:
        method: "symmetric_key"
        registration_id: "{registration_id}"
        symmetric_key: "{symmetric_key}"
    ```

1. Reemplace `{scope_id}` por el valor de **Ámbito del identificador** que anotó anteriormente.

1. Reemplace `{registration_id}` por el valor de **Identificador de dispositivo** que anotó anteriormente.

1. Reemplace `{symmetric_key}` por el valor de **Clave principal** que anotó anteriormente.

1. Guarde los cambios (**Ctrl-O**) y salga (**Ctrl-X**) del editor `nano`.

1. Ejecute el siguiente comando para reiniciar el demonio de IoT Edge:

    ```bash
    sudo systemctl restart iotedge
    ```

1. Ejecute el siguiente comando para comprobar el estado de los módulos de IoT Edge:

    ```bash
    iotedge list
    ```

    El resultado tendrá un aspecto similar al siguiente:

    ```bash
    NAME                        STATUS           DESCRIPTION      CONFIG
    SimulatedTemperatureSensor  running          Up 20 seconds    mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
    edgeAgent                   running          Up 27 seconds    mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub                     running          Up 22 seconds    mcr.microsoft.com/azureiotedge-hub:1.0
    ```

## <a name="view-the-telemetry"></a>Visualización de la telemetría

El dispositivo IoT Edge simulado ahora está en ejecución en la máquina virtual. En la aplicación de IoT Central, el estado del dispositivo ahora es de **Aprovisionado** en la página **Dispositivos**:

![Dispositivo aprovisionado](./media/tutorial-add-edge-as-leaf-device/provisioned-device.png)

Puede ver los datos de telemetría en la página **Ver datos de telemetría del dispositivo IoT Edge**:

![Telemetría de dispositivo](./media/tutorial-add-edge-as-leaf-device/device-telemetry-view.png)

En la página **Módulos** se muestra el estado de los módulos de IoT Edge:

![Telemetría de dispositivo](./media/tutorial-add-edge-as-leaf-device/edge-module-status.png)

## <a name="next-steps"></a>Pasos siguientes

Como desarrollador de dispositivos, ahora que ha aprendido a trabajar y a administrar dispositivos IoT Edge en IoT Central, este es el siguiente paso sugerido:

<!-- Next how-tos in the sequence -->

> [!div class="nextstepaction"]
> [Conexión a Azure IoT Central](./concepts-get-connected.md)
