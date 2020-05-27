---
title: 'Guía de inicio rápido: Creación de un dispositivo Azure IoT Edge en Linux | Microsoft Docs'
description: En esta guía de inicio rápido, obtenga información sobre cómo crear un dispositivo IoT Edge y luego implementar código creado previamente de manera remota desde Azure Portal.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/06/2019
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: e015999d8c1f60340fb30609c6563f770c5c824f
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726237"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-virtual-linux-device"></a>Inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo virtual Linux

Pruebe Azure IoT Edge en este inicio rápido mediante la implementación de código en contenedor en un dispositivo de IoT Edge virtual. IoT Edge permite administrar de forma remota el código de los dispositivos para que pueda enviar más cargas de trabajo al perímetro. En este inicio rápido, se recomienda usar una máquina virtual de Azure para el dispositivo IoT Edge, que le permite crear rápidamente una máquina de pruebas con todos los requisitos previos instalados y, luego, eliminarla cuando haya terminado.

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

1. Cree un centro de IoT Hub.
2. Registre un dispositivo IoT Edge en su instancia de IoT Hub.
3. Instale e inicie el entorno de ejecución de IoT Edge en el dispositivo virtual.
4. Implemente un módulo de manera remota en un dispositivo IoT Edge.

![Diagrama: Inicio rápido de la arquitectura para el dispositivo y la nube](./media/quickstart-linux/install-edge-full.png)

Este inicio rápido le guía en el proceso de creación de una máquina virtual Linux que está configurada para ser un dispositivo de IoT Edge. Después, puede implementar un módulo desde Azure Portal en el dispositivo. El módulo que se implementa en esta guía de inicio rápido es un sensor simulado que genera datos de temperatura, humedad y presión. Los otros tutoriales de Azure IoT Edge se basan en el trabajo que se realiza aquí mediante la implementación de módulos que analizan los datos simulados para obtener información empresarial.

Si no tiene una suscripción activa a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de comenzar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Usará la CLI de Azure para completar muchos de los pasos de esta guía de inicio rápido; además, Azure IoT cuenta con una extensión para habilitar funcionalidad adicional.

Agregue la extensión de Azure IoT a la instancia de Cloud Shell.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```
   
[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prerequisites"></a>Prerrequisitos

Recursos en la nube:

* Un grupo de recursos para administrar todos los recursos que se van a usar en esta guía de inicio rápido.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

Dispositivo de IoT Edge:

* Una máquina virtual o dispositivo de Linux que actúa como dispositivo de IoT Edge. Debe utilizar la instancia que proporciona Microsoft de [Azure IoT Edge en la máquina virtual Ubuntu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu), que preinstala todo lo necesario para ejecutar IoT Edge en un dispositivo. Acepte los términos de uso y cree esta máquina virtual con los comandos siguientes:

   ```azurecli-interactive
   az vm image terms accept --urn microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest --admin-username azureuser --generate-ssh-keys
   ```

   La nueva máquina virtual puede tardar unos minutos en crearse e iniciarse.

   Cuando cree una nueva máquina virtual, tome nota del valor de **publicIpAddress**, que se proporciona como parte de la salida del comando create. Utilice esta dirección IP pública para conectarse a la máquina virtual más adelante en la guía de inicio rápido.

* Si prefiere ejecutar el entorno de ejecución de Azure IoT Edge en su propio dispositivo, siga las instrucciones en [Instalación del entorno de ejecución de Azure IoT Edge en Linux](how-to-install-iot-edge-linux.md).

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Para empezar el inicio rápido, cree un centro de IoT con la CLI de Azure.

![Diagrama: Creación de un centro de IoT en la nube](./media/quickstart-linux/create-iot-hub.png)

El nivel gratuito de IoT Hub funciona para esta guía de inicio rápido. Si ha usado IoT Hub en el pasado y ya tiene un centro gratis creado, puede usar ese centro de IoT. Cada suscripción no puede tener más de un centro de IoT gratuito.

El código siguiente crea un centro **F1** gratis en el grupo de recursos **IoTEdgeResources**. Reemplace `{hub_name}` por un nombre único para su centro de IoT.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   Si se produce un error porque ya hay un centro gratis en la suscripción, cambie la SKU a **S1**. Si recibe un error que le indica que el nombre de IoT Hub no está disponible, significa que alguien más ya tiene un centro con ese nombre. Pruebe con uno nuevo.

## <a name="register-an-iot-edge-device"></a>Registro de un dispositivo de IoT Edge

Registre un dispositivo de IoT Edge con la instancia de IoT Hub recién creada.

![Diagrama: Registro de un dispositivo con una identidad de IoT Hub](./media/quickstart-linux/register-device.png)

Cree una identidad para el dispositivo de IoT Edge, con el fin de que pueda comunicarse con su centro de IoT. La identidad del dispositivo reside en la nube, y se usa una cadena de conexión de dispositivo única para asociar un dispositivo físico a una identidad de dispositivo.

Dado que los dispositivos de IoT Edge se comportan y se pueden administrar de manera diferente a los dispositivos de IoT típicos, declare que esta identidad es para un dispositivo de IoT Edge con la marca `--edge-enabled`.

1. En Azure Cloud Shell, escriba el comando siguiente para crear un dispositivo denominado **myEdgeDevice** en el centro.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

   Si recibe un error acerca de las claves de directiva de iothubowner, asegúrese de que Cloud Shell ejecuta la versión más reciente de la extensión azure-iot.

2. Recupere la cadena de conexión del dispositivo, que vincula el dispositivo físico con su identidad en IoT Hub.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. Copie el valor de la clave `connectionString` desde la salida JSON y guárdelo. Este valor es la cadena de conexión del dispositivo. Esta cadena de conexión se usará para configurar el entorno de ejecución de IoT Edge en la sección siguiente.

   ![Recuperar la cadena de conexión de la salida de la CLI](./media/quickstart/retrieve-connection-string.png)

## <a name="configure-your-iot-edge-device"></a>Configuración de un dispositivo de IoT Edge

Inicie el entorno de ejecución de Azure IoT Edge en el dispositivo de IoT Edge.

![Diagrama: Inicio del runtime en el dispositivo](./media/quickstart-linux/start-runtime.png)

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Tiene tres componentes. El **demonio de seguridad de IoT Edge** se inicia cada vez que se inicia un dispositivo IoT Edge y arranca el dispositivo mediante el inicio del agente de este. El **agente de IoT Edge** facilita la implementación y supervisión de los módulos en el dispositivo IoT Edge, incluido el centro de IoT Edge. El **centro de IoT Edge** administra las comunicaciones entre los módulos del dispositivo de IoT Edge y entre el dispositivo y la instancia de IoT Hub.

Durante la configuración del entorno en tiempo de ejecución, tendrá que proporcionar una cadena de conexión del dispositivo. Use la cadena que recuperó de la CLI de Azure. Esta cadena asocia el dispositivo físico con la identidad del dispositivo IoT Edge en Azure.

### <a name="set-the-connection-string-on-the-iot-edge-device"></a>Establecimiento de la cadena de conexión en el dispositivo IoT Edge

Si usa Azure IoT Edge en la máquina virtual Ubuntu según se describió en los requisitos previos, el dispositivo ya tiene instalado el entorno de ejecución de Azure IoT Edge. Solo tiene que configurar el dispositivo con la cadena de conexión de dispositivo que recuperó en la sección anterior. Esta operación se puede hacer de forma remota, sin tener que conectarse a la máquina virtual. Ejecute el siguiente comando, pero reemplace`{device_connection_string}` por su propia cadena.

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunShellScript --script "/etc/iotedge/configedge.sh '{device_connection_string}'"
   ```

Si ejecuta IoT Edge en una máquina local o en un dispositivo ARM32 o ARM64, tiene que instalar el entorno de ejecución de Azure IoT Edge y sus requisitos previos en el dispositivo. Siga las instrucciones de [Instalación del entorno de ejecución de Azure IoT Edge en Linux](how-to-install-iot-edge-linux.md) y vuelva a este inicio rápido.

### <a name="view-the-iot-edge-runtime-status"></a>Visualización del estado del entorno de ejecución de Azure IoT Edge

El resto de los comandos de este inicio rápido tienen lugar en el propio dispositivo de IoT Edge, con el fin de que pueda ver lo que sucede en el dispositivo. Si usa una máquina virtual, conéctese a ella ahora mediante la dirección IP pública que generó el comando de creación. También puede encontrar la dirección IP pública en la página de información general de la máquina virtual en Azure Portal. Use el siguiente comando para conectarse a la máquina virtual. Reemplace `{azureuser}` si ha usado un nombre de usuario que no sea el que se sugiere en los requisitos previos. Reemplace `{publicIpAddress}` por la dirección de su máquina.

   ```console
   ssh azureuser@{publicIpAddress}
   ```

Compruebe que el runtime se ha instalado y configurado correctamente en el dispositivo de IoT Edge.

>[!TIP]
>Necesita privilegios elevados para ejecutar comandos `iotedge`. Cuando cierre la sesión en su máquina y la inicie de nuevo por primera vez después de instalar el entorno de ejecución de IoT Edge, sus permisos se actualizarán automáticamente. Hasta entonces, use `sudo` delante de los comandos.

1. Compruebe que el demonio de seguridad de IoT Edge se ejecuta como un servicio del sistema.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Comprobación de cómo el demonio de IoT Edge se ejecuta como un servicio del sistema](./media/quickstart-linux/iotedged-running.png)

2. Si necesita solucionar problemas del servicio, recupere los registros del servicio.

   ```bash
   journalctl -u iotedge
   ```

3. Vea los módulos que se ejecutan en el dispositivo.

   ```bash
   sudo iotedge list
   ```

   ![Visualización de un módulo en el dispositivo](./media/quickstart-linux/iotedge-list-1.png)

El dispositivo de IoT Edge está ya configurado. Está preparado para ejecutar módulos implementados en la nube.

## <a name="deploy-a-module"></a>Implementación de un módulo

Administre el dispositivo Azure IoT Edge desde la nube para implementar un módulo que enviará datos de telemetría a IoT Hub.
![Diagrama: Implementación del módulo desde la nube al dispositivo](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Visualización de datos generados

En esta guía de inicio rápido, ha creado un nuevo dispositivo de IoT Edge y ha instalado el runtime de IoT Edge en él. Luego, ha usado Azure Portal para implementar un módulo de IoT Edge para que se ejecute en el dispositivo sin tener que realizar cambios en el propio dispositivo.

En este caso, el módulo que ha insertado crea los datos de ejemplo que puede usar para las pruebas. El módulo del sensor de temperatura simulado genera datos de entorno que se pueden utilizar con fines de prueba más tarde. El sensor simulado está supervisando una máquina y el entorno alrededor de esta. Por ejemplo, este sensor podría estar en una sala de servidores, en una fábrica o en un aerogenerador. El mensaje incluye la temperatura y la humedad ambiental, la temperatura y la presión de la máquina, y una marca de tiempo. Los tutoriales de IoT Edge usan los datos creados por este módulo como datos de prueba con fines de análisis.

Abra de nuevo el símbolo del sistema en su dispositivo IoT Edge, o utilice la conexión SSH de la CLI de Azure. Confirme que el módulo implementado desde la nube se está ejecutando en el dispositivo IoT Edge:

   ```bash
   sudo iotedge list
   ```

   ![Ver tres módulos en el dispositivo](./media/quickstart-linux/iotedge-list-2.png)

Vea los mensajes que se envían desde el módulo del sensor de temperatura:

   ```bash
   sudo iotedge logs SimulatedTemperatureSensor -f
   ```

   >[!TIP]
   >Los comandos de IoT Edge distinguen mayúsculas de minúsculas en los nombres de los módulos.

   ![Ver los datos desde el módulo](./media/quickstart-linux/iotedge-logs.png)

También puede ver los mensajes que llegan a su centro de IoT mediante la [extensión de Azure IoT Hub para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea continuar con los tutoriales de IoT Edge, puede usar el dispositivo que ha registrado y configurado en esta guía de inicio rápido. En caso contrario, puede eliminar los recursos de Azure que creó para evitar gastos.

Si ha creado una máquina virtual y un centro de IoT en un nuevo grupo de recursos, puede eliminar dicho grupo y todos los recursos asociados. Vuelva a comprobar el contenido del grupo de recursos para asegurarse de que no haya nada que desee conservar. Si no desea eliminar todo el grupo, puede eliminar recursos individuales en su lugar.

Quite el grupo **IoTEdgeResources**.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un dispositivo IoT Edge y ha usado la interfaz en la nube de Azure IoT Edge para implementar el código en el dispositivo. Ahora tiene un dispositivo de prueba que genera datos sin procesar acerca de su entorno.

El siguiente paso es configurar el entorno de desarrollo local para que pueda empezar a crear módulos de IoT Edge que ejecutan la lógica de negocios.

> [!div class="nextstepaction"]
> [Empezar a desarrollar módulos de IoT Edge para dispositivos Linux](tutorial-develop-for-linux.md)
