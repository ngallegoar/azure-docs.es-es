---
title: 'Desarrollo y depuración de módulos en Visual Studio: Azure IoT Edge'
description: Use Visual Studio con Azure IoT Tools para desarrollar un módulo de IoT Edge en C o C# e insertarlo desde su instancia de IoT Hub en un dispositivo IoT, tal y como se configura en un manifiesto de implementación.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 3/27/2020
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: 1883ffdff20bbbef8efec1440854f01a21a281dc
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95994526"
---
# <a name="use-visual-studio-2019-to-develop-and-debug-modules-for-azure-iot-edge"></a>Uso de Visual Studio 2019 para desarrollar y depurar módulos para Azure IoT Edge

Puede convertir la lógica de negocios en módulos de Azure IoT Edge. En este artículo se muestra cómo usar Visual Studio 2019 como herramienta principal para desarrollar y depurar módulos.

Las herramientas de Azure IoT Edge para Visual Studio proporcionan las siguientes ventajas:

- Creación, edición, compilación y ejecución de soluciones y módulos de Azure IoT Edge en su equipo de desarrollo local.
- Implementación de la solución de Azure IoT Edge en un dispositivo de Azure IoT Edge gracias a Azure IoT Hub.
- Programe los módulos de Azure IoT en C o C# disfrutando de las ventajas de desarrollo en Visual Studio.
- Administración de dispositivos de Azure IoT Edge y módulos sin interfaz de usuario.

En este artículo se muestra cómo usar las herramientas de Azure IoT Edge para Visual Studio 2019 con el fin de desarrollar los módulos de IoT Edge. También aprenderá a implementar el proyecto en el dispositivo de Azure IoT Edge. Actualmente, Visual Studio 2019 proporciona compatibilidad con módulos escritos en C y C#. Las arquitecturas de dispositivo compatibles son Windows x64 y Linux x64 o ARM32. Para obtener más información sobre los sistemas operativos, los lenguajes y las arquitecturas compatibles, consulte [Compatibilidad de lenguaje y arquitectura](module-development.md#language-and-architecture-support).
  
## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por hecho que usa una máquina virtual que ejecuta Windows como máquina de desarrollo. En equipos Windows, puede desarrollar módulos de Windows o Linux. Para desarrollar módulos de Windows, use un equipo Windows que ejecute la versión 1809, la compilación 17763 o una más reciente. Para desarrollar módulos de Linux, use un equipo Windows que cumpla los [requisitos de Docker Desktop](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).

En este artículo se usa Visual Studio 2019 como herramienta de desarrollo principal, por lo que debe instalar Visual Studio. Asegúrese de incluir las cargas de trabajo de **desarrollo de Azure** y **desarrollo de escritorio con C++** en la instalación de Visual Studio 2019. También puede [modificar Visual Studio 2019](/visualstudio/install/modify-visual-studio?view=vs-2019) para agregar las cargas de trabajo necesarias.

Una vez que Visual Studio 2019 esté listo, también necesitará las siguientes herramientas y componentes:

- Descargue e instale las [herramientas de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) desde Visual Studio Marketplace para crear un proyecto de IoT Edge en Visual Studio 2019.

> [!TIP]
> Si usa Visual Studio 2017, descargue e instale [Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) para VS 2017 en Visual Studio Marketplace.

- Descargue e instale [Docker Community Edition](https://docs.docker.com/install/) en su equipo de desarrollo para compilar y ejecutar las imágenes del módulo. Deberá configurar Docker CE para que se ejecute ya sea en modo de contenedor de Linux o en modo de contenedor de Windows.

- Configure el entorno local de desarrollo para depurar, ejecutar y probar la solución de IoT Edge mediante la instalación de la [herramienta de desarrollo de Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Instale [Python (2.7 o 3.6+) y Pip](https://www.python.org/) y el paquete **iotedgehubdev** mediante la ejecución del siguiente comando en el terminal. Asegúrese de que la versión de la herramienta de desarrollo de Azure IoT EdgeHub es posterior a la 0.3.0.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- Clone el repositorio e instale el administrador de bibliotecas de Vcpkg y, después, instale el **paquete azure-iot-sdk-c** para Windows.

  ```cmd
  git clone https://github.com/Microsoft/vcpkg
  cd vcpkg
  bootstrap-vcpkg.bat
  ```

  ```cmd
  vcpkg.exe install azure-iot-sdk-c:x64-windows
  vcpkg.exe --triplet x64-windows integrate install
  ```

- [Azure Container Registry](../container-registry/index.yml) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags).

  > [!TIP]
  > Puede usar un registro de Docker local con fines de prototipo y prueba en lugar de un registro en la nube.

- Para probar el módulo en un dispositivo, necesitará una instancia de IoT Hub con al menos un dispositivo IoT Edge. Para usar el equipo como un dispositivo IoT Edge, siga los pasos descritos en el inicio rápido para [Linux](quickstart-linux.md) o [Windows](quickstart.md). Si ejecuta el demonio de IoT Edge en su equipo de desarrollo, es posible que deba detener EdgeHub y EdgeAgent para empezar el desarrollo en Visual Studio.

### <a name="check-your-tools-version"></a>Comprobación de la versión de herramientas

1. En el menú **Extensiones**, seleccione **Administrar extensiones**. Expanda **Instalado > Herramientas** y encontrará **Azure IoT Edge Tools para Visual Studio** y **Cloud Explorer para Visual Studio**.

1. Observe la versión instalada. Puede comparar esta versión con la más reciente de Visual Studio Marketplace ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS2019), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools))

1. Si su versión es anterior a la que está disponible en Visual Studio Marketplace, actualice las herramientas en Visual Studio como se muestra en la siguiente sección.

### <a name="update-your-tools"></a>Actualización de las herramientas

1. En la ventana **Administrar extensiones**, expanda **Actualizaciones > Visual Studio Marketplace**, elija **Azure IoT Edge Tools** o **Cloud Explorer para Visual Studio** y seleccione **Actualizar**.

1. Después de descargar la actualización de las herramientas, cierre Visual Studio para desencadenar la actualización de las herramientas mediante el programa de instalación de VSIX.

1. En el instalador, presione **Aceptar** para iniciar y **Modificar** para actualizar las herramientas.

1. Una vez completada la actualización, seleccione **Cerrar** y reinicie Visual Studio.

### <a name="create-an-azure-iot-edge-project"></a>Creación de un proyecto de Azure IoT Edge

La plantilla de proyecto de Azure IoT Edge en Visual Studio crea un proyecto que se puede implementar en dispositivos de Azure IoT Edge en Azure IoT Hub. En primer lugar, cree una solución de Azure IoT Edge y, luego, genere el primer módulo en esa solución. Cada solución de IoT Edge puede contener más de un módulo.

> [!TIP]
> La estructura del proyecto de IoT Edge creada por Visual Studio no es igual que en Visual Studio Code.

1. En el diálogo de proyecto nuevo de Visual Studio, busque y seleccione el proyecto de **Azure IoT Edge** y haga clic en **Siguiente**. En la ventana de configuración del proyecto, escriba un nombre para el proyecto y especifique la ubicación y, luego, seleccione **Crear**. El nombre de proyecto predeterminado es **AzureIoTEdgeApp1**.

   ![Creación de un proyecto](./media/how-to-visual-studio-develop-csharp-module/create-new.png)

1. En la ventana **Agregar la aplicación y el módulo de IoT Edge**, seleccione **Módulo C#** o **Módulo C** y, después, especifique el nombre y el repositorio de imágenes del módulo. Visual Studio completa automáticamente el nombre del módulo como **localhost:5000 /<el nombre de su módulo\>** . Reemplácelo por su propia información de registro. Si usa un registro de Docker local para realizar pruebas, **localhost** puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a **_\<registry name\>_ .azurecr.io**. Simplemente reemplace la sección **localhost:5000** de la cadena para que el resultado final se parezca a **\<*registry name*\>.azurecr.io/ _\<your module name\>_**. El nombre de módulo predeterminado es **IotEdgeModule1**

   ![Adición de la aplicación y el módulo](./media/how-to-visual-studio-develop-csharp-module/add-application-and-module.png)

1. Seleccione **Aceptar** para crear la solución de Azure IoT Edge con un módulo que use C# o C.

Ahora tiene un proyecto **AzureIoTEdgeApp1.Linux.Amd64** o **AzureIoTEdgeApp1.Windows.Amd64** y también uno **IotEdgeModule1** en la solución. Cada proyecto **AzureIoTEdgeApp1** tiene un archivo `deployment.template.json`, que define los módulos que quiere compilar e implementar para la solución de IoT Edge y, además, define las rutas entre los módulos. La solución predeterminada tiene un módulo **SimulatedTemperatureSensor** y otro **IotEdgeModule1**. El módulo **SimulatedTemperatureSensor** genera datos simulados para el módulo **IotEdgeModule1**, mientras que el código predeterminado del módulo **IotEdgeModule1** canaliza directamente los mensajes recibidos a Azure IoT Hub.

Para ver cómo funciona el sensor de temperatura simulado, vea el [código fuente de SimulatedTemperatureSensor.csproj](https://github.com/Azure/iotedge/tree/master/edge-modules/SimulatedTemperatureSensor).

El proyecto **IotEdgeModule1** es una aplicación de consola de .NET Core 2.1, si se trata de un módulo de C#. Contiene los archivos de Docker que necesita para que el dispositivo de IoT Edge se ejecute con un contenedor de Windows o Linux. El archivo `module.json` describe los metadatos de un módulo. El código del módulo real, que toma el SDK de dispositivo IoT de Azure como dependencia, se encuentra en los archivos `Program.cs` o `main.c`.

## <a name="develop-your-module"></a>Desarrollo de su módulo

El código del módulo predeterminado que se incluye con la solución se encuentra en **IotEdgeModule1** > **Program.cs** (para C#) o en **main.c** (para C). El módulo y el archivo `deployment.template.json` se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo con el fin de iniciar la prueba sin tocar el código. El módulo se compila para recibir entradas de un origen (en este caso, el módulo **SimulatedTemperatureSensor** que simula datos) y canalizarlo a Azure IoT Hub.

Cuando tenga todo listo para personalizar la plantilla del módulo con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como la seguridad, la administración de dispositivos y la fiabilidad.

## <a name="initialize-iotedgehubdev-with-iot-edge-device-connection-string"></a>Inicialización de iotedgehubdev con una cadena de conexión de dispositivo IoT Edge

1. Copie la cadena de conexión de cualquier dispositivo de IoT Edge desde **cadena de conexión principal** en Visual Studio Cloud Explorer. Asegúrese de no copiar la cadena de conexión de un dispositivo que no sea Edge, ya que el icono de dispositivo IoT Edge es diferente al del dispositivo que no es Edge.

   ![Copia de la cadena de conexión del dispositivo Edge](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

1. Vaya al menú **Herramientas**, seleccione **Azure IoT Edge Tools** > **Setup IoT Edge Simulator** (Configurar el simulador de IoT Edge), pegue la cadena de conexión y haga clic en **Aceptar**.

   ![Apertura de la ventana de la cadena de conexión del dispositivo Edge establecida](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

1. Escriba la cadena de conexión del primer paso y, a continuación, seleccione **Aceptar**.

> [!NOTE]
> Debe seguir estos pasos solo una vez en el equipo de desarrollo, ya que los resultados se aplican automáticamente a todas las soluciones de Azure IoT Edge posteriores. Puede realizar nuevamente este procedimiento si necesita cambiar a una cadena de conexión diferente.

## <a name="build-and-debug-single-module"></a>Compilación y depuración de un solo módulo

Por lo general, querrá probar y depurar los módulos antes de que se ejecuten en una solución entera con varios de ellos.

1. En **Explorador de soluciones**, haga clic con el botón derecho en **IotEdgeModule1** y seleccione **Establecer como proyecto de inicio** en el menú contextual.

   ![Establecimiento del proyecto de inicio](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

1. Presione **F5** o haga clic en el siguiente botón para ejecutar el módulo, puede tardar unos 10&ndash;20 segundos la primera vez que se ejecute.

   ![Ejecución del módulo](./media/how-to-visual-studio-develop-csharp-module/run-module.png)

1. Si el módulo se inicializa correctamente, verá que una aplicación de consola de .NET Core se inicia.

   ![Módulo en ejecución](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

1. Si el desarrollo se realiza con C#, establezca un punto de interrupción en la función `PipeMessage()` del archivo **Program.cs**; si es con C, establezca un punto de interrupción en la función `InputQueue1Callback()` del archivo **main.c**. Luego, envíe un mensaje para probarla mediante la ejecución del comando siguiente en un shell de **Git Bash** o **WSL Bash**. (No puede ejecutar el comando `curl` en PowerShell ni desde el símbolo del sistema).

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Depuración de un solo módulo](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Debe desencadenarse el punto de interrupción. Puede inspeccionar las variables en la ventana **Variables locales** de Visual Studio.

   > [!TIP]
   > También puede usar [PostMan](https://www.getpostman.com/) u otras herramientas de API para enviar mensajes en lugar de `curl`.

1. Presione **Ctrl + F5** o haga clic en el botón Detener para detener la depuración.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Compilación y depuración de la solución IoT Edge con varios módulos

Cuando haya terminado de desarrollar el módulo, querrá ejecutar y depurar una solución completa de varios módulos.

1. En **Explorador de soluciones**, agregue un segundo módulo a la solución; para ello, haga clic con el botón derecho en **AzureIoTEdgeApp1** y seleccione **Agregar** > **New IoT Edge Module** (Nuevo módulo de IoT Edge). El nombre predeterminado del segundo módulo es **IotEdgeModule2** y actuará como otro módulo de canalización.

1. Abra el archivo `deployment.template.json` y verá que **IotEdgeModule2** se ha agregado a la sección **modules**. Reemplace la sección **routes** por lo siguiente. Si ha personalizado los nombres de módulo, asegúrese de actualizarlos para que coincidan.

    ```json
        "routes": {
          "IotEdgeModule1ToIoTHub": "FROM /messages/modules/IotEdgeModule1/outputs/* INTO $upstream",
          "sensorToIotEdgeModule1": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IotEdgeModule1/inputs/input1\")",
          "IotEdgeModule2ToIoTHub": "FROM /messages/modules/IotEdgeModule2/outputs/* INTO $upstream",
          "sensorToIotEdgeModule2": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IotEdgeModule2/inputs/input1\")"
        },
    ```

1. Haga clic con el botón derecho en **AzureIoTEdgeApp1** y seleccione **Set as StartUp Project** (Establecer como proyecto de inicio) en el menú contextual.

1. Cree los puntos de interrupción y presione **F5** para poder ejecutar y depurar varios módulos a la vez. Verá varias ventanas de aplicación de consola de .NET Core; cada una de ellas representa un módulo distinto.

   ![Depuración de varios módulos](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

1. Presione **Ctrl + F5** o seleccione el botón Detener para detener la depuración.

## <a name="build-and-push-images"></a>Compilación e inserción de imágenes

1. Asegúrese de que **AzureIoTEdgeApp1** es el proyecto de inicio. Seleccione **Depurar** o **Versión** como configuración para compilar las imágenes del módulo.

    > [!NOTE]
    > Al elegir **Depurar**, Visual Studio usará `Dockerfile.(amd64|windows-amd64).debug` para compilar las imágenes de Docker. Esto incluye el depurador de la línea de comandos de .NET Core VSDBG en la imagen de contenedor al realizar la compilación. Para los módulos de IoT Edge listos para producción, se recomienda usar la configuración de **Versión** que usa `Dockerfile.(amd64|windows-amd64)` sin VSDBG.

1. Si usa un registro privado como Azure Container Registry (ACR), use el siguiente comando de Docker para iniciar sesión en él.  Puede obtener el nombre de usuario y la contraseña en la página **Claves de acceso** del registro en Azure Portal. Si usa un registro local, puede [Run a local registry](https://docs.docker.com/registry/deploying/#run-a-local-registry) (Ejecutar un registro local).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```

1. Si usa un registro privado como Azure Container Registry, deberá agregar la información de inicio de sesión del registro a la configuración en tiempo de ejecución que se encuentra en el archivo `deployment.template.json`. Reemplace los marcadores de posición por su nombre de usuario administrador, contraseña y nombre de registro reales de ACR.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

1. En **Explorador de soluciones**, haga clic con el botón derecho en **AzureIoTEdgeApp1** y seleccione **Compilación e inserción de módulos IoT Edge** para compilar e insertar las imágenes de Docker para cada módulo.

## <a name="deploy-the-solution"></a>Implementación de la solución

En el artículo de la guía de inicio rápido que siguió para configurar el dispositivo de IoT Edge, implementó un módulo mediante Azure Portal. También puede implementar módulos mediante Cloud Explorer para Visual Studio. Ya tiene un manifiesto de implementación listo para su escenario (el archivo `deployment.json`) y todo lo que necesita hacer es seleccionar el dispositivo que recibirá la implementación.

1. Abra **Cloud Explorer** al hacer clic en **Ver** > **Cloud Explorer**. Asegúrese de haber iniciado sesión en Visual Studio 2019.

1. En **Cloud Explorer**, expanda su suscripción, busque Azure IoT Hub y el dispositivo de Azure IoT Edge que desea implementar.

1. Haga clic con el botón derecho en el dispositivo IoT Edge para crear una implementación para este. Vaya al manifiesto de implementación configurado para la plataforma, el cual se encuentra en la carpeta **config** de la solución de Visual Studio, por ejemplo, `deployment.arm32v7.json`.

1. Haga clic en el botón Actualizar para ver los nuevos módulos en ejecución junto con el módulo **SimulatedTemperatureSensor**, **$edgeAgent** y **$edgeHub**.

## <a name="view-generated-data"></a>Visualización de datos generados

1. Para supervisar el mensaje de D2C de un determinado dispositivo IoT Edge, selecciónelo en el centro de IoT en **Cloud Explorer** y, luego, haga clic en **Iniciar la supervisión del punto de conexión de eventos integrado** en la ventana **Acción**.

1. Para detener la supervisión de datos, seleccione **Detener la supervisión del punto de conexión de eventos integrado** en la ventana **Acción**.

## <a name="next-steps"></a>Pasos siguientes

Para desarrollar módulos personalizados para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).