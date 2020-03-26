---
title: 'Tutorial: Creación e implementación de módulos personalizados: Machine Learning en Azure IoT Edge'
description: En este ejemplo se indica cómo crear e implementar los módulos de IoT Edge que procesan datos desde dispositivos hoja a través de un modelo de Machine Learning y, después, envían la información a IoT Hub.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3cba7781ac80ae567b2bfd54c4131429ed94b90f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75772370"
---
# <a name="tutorial-create-and-deploy-custom-iot-edge-modules"></a>Tutorial: Creación e implementación de módulos personalizados de IoT Edge

> [!NOTE]
> Este artículo forma parte de un tutorial sobre el uso de Azure Machine Learning en IoT Edge. Si ha llegado directamente a este artículo, le recomendamos que comience con el [primer el artículo](tutorial-machine-learning-edge-01-intro.md) de la serie para obtener los mejores resultados.

En este artículo, crearemos tres módulos de IoT Edge que reciben mensajes desde dispositivos hoja, ejecutan los datos a través de un modelo de Machine Learning y, después, reenvían la información a IoT Hub.

El centro de IoT Edge facilita la comunicación entre módulos. Al usar el centro de IoT Edge como un agente de mensajes, los módulos seguirán siendo independientes entre sí. Los módulos solo necesitan especificar las entradas en las que aceptan mensajes y las salidas en las que los escriben.

Queremos conseguir que el dispositivo IoT Edge haga cuatro cosas:

* Reciba datos desde dispositivos hoja
* Prediga la vida útil restante (RUL) del dispositivo que envió los datos
* Envíe un mensaje solo con la RUL del dispositivo a IoT Hub (esta función se podría modificar para solo enviar los datos si la RUL se sitúa por debajo de cierto nivel)
* Guarde los datos del dispositivo hoja en un archivo local en el dispositivo IoT Edge. Este archivo de datos se carga periódicamente en IoT Hub a través de la carga de archivos para refinar el entrenamiento del modelo de Machine Learning. El uso de la carga de archivos en lugar del streaming de mensajes constante es más rentable.

Para realizar estas tareas, usamos tres módulos personalizados:

* **Clasificador RUL**: el módulo turboFanRulClassifier que creamos en [Train and deploy an Azure Machine Learning model](tutorial-machine-learning-edge-04-train-model.md) (Entrenar e implementar un modelo de Azure Machine Learning) es un módulo de Machine Learning estándar, que expone una entrada denominada "amlInput" y una salida denominada "amlOutput". La entrada "amlInput" espera que sus datos sean exactamente iguales a la entrada que se envía al servicio web basado en ACI. Del mismo modo, la salida "amlOutput" devuelve los mismos datos que el servicio web.

* **Escritor Avro:** este módulo recibe mensajes en la entrada "avroModuleInput" y conserva el mensaje en formato Avro en el disco para su posterior carga en IoT Hub.

* **Módulo de enrutador:** El módulo de enrutador recibe mensajes de los dispositivos de hoja de bajada y, después, da formato y envía los mensajes al clasificador. A continuación, el módulo recibe los mensajes desde el clasificador y reenvía el mensaje al módulo escritor Avro. Por último, el módulo envía simplemente la predicción de la RUL a IoT Hub.

  * Entradas:
    * **deviceInput:** recibe mensajes de dispositivos hoja
    * **rulInput:** recibe mensajes de "amlOutput"

  * Salidas:
    * **classify:** envía mensajes a "amlInput"
    * **writeAvro:** envía mensajes a "avroModuleInput"
    * **toIotHub:** envía mensajes a $upstream, que pasa los mensajes a la instancia conectada de IoT Hub

En el diagrama siguiente se muestran los módulos, entradas, salidas y rutas de IoT Edge Hub para la solución completa:

![Diagrama de arquitectura de IoT Edge de tres módulos](media/tutorial-machine-learning-edge-06-custom-modules/modules-diagram.png)

Los pasos descritos en este artículo los realiza normalmente un desarrollador en la nube.

## <a name="create-a-new-iot-edge-solution"></a>Crear una nueva solución de IoT Edge

Durante la ejecución del segundo de nuestros dos cuadernos de Azure Notebooks, creamos y publicamos una imagen de contenedor que contiene nuestro modelo RUL. Azure Machine Learning, como parte del proceso de creación de imágenes, empaqueta ese modelo para que la imagen se pueda implementar como un módulo de Azure IoT Edge. En este paso, se va a crear una solución de Azure IoT Edge con el módulo "Azure Machine Learning" y, después, se orientará el módulo a la imagen que publicamos mediante Azure Notebooks.

1. Abra una sesión de Escritorio remoto en la máquina de desarrollo.

2. Abra la carpeta **C:\\origen\\IoTEdgeAndMlSample** en Visual Studio Code.

3. Haga clic con el botón derecho en el panel del explorador (en el espacio en blanco) y seleccione **New IoT Edge Solution** (Nueva solución de IoT Edge).

    ![Creación de la nueva solución de IoT Edge](media/tutorial-machine-learning-edge-06-custom-modules/new-edge-solution-command.png)

4. Acepte el nombre de la solución predeterminada **EdgeSolution**.

5. Elija **Azure Machine Learning** como la plantilla del módulo.

6. Asigne un nombre al módulo **turbofanRulClassifier**.

7. Elija un área de trabajo de Machine Learning.

8. Seleccione la imagen que creó al ejecutar el cuaderno de Azure Notebooks.

9. Examine la solución y observe los archivos que se han creado:

   * **deployment.template.json:** este archivo contiene la definición de cada uno de los módulos de la solución. Hay tres secciones importantes en este archivo:

     * **Credenciales del registro:** define el conjunto de instancias de Container Registry personalizadas que se usan en la solución. Ahora mismo, debería contener el registro del área de trabajo de Machine Learning, que es donde se almacenó la imagen de Azure Machine Learning. Puede tener el número que quiera de instancias de Container Registry, pero por motivos de simplicidad usaremos este único registro para todos los módulos.

       ```json
       "registryCredentials": {
         "<your registry>": {
           "username": "$CONTAINER_REGISTRY_USERNAME_<your registry>",
           "password": "$CONTAINER_REGISTRY_PASSWORD_<your registry>",
           "address": "<your registry>.azurecr.io"
         }
       }
       ```

     * **Módulos:** esta sección contiene el conjunto de módulos definidos por el usuario que se incluyen con esta solución. Observará que esta sección contiene actualmente dos módulos: SimulatedTemperatureSensor y turbofanRulClassifier. SimulatedTemperatureSensorr se instaló con la plantilla de Visual Studio Code, pero no se necesita para esta solución. Puede eliminar la definición del módulo SimulatedTemperatureSensor en la sección de módulos. Tenga en cuenta que la definición del módulo turbofanRulClassifier apunta a la imagen de su instancia de Container Registry. A medida que se agregan más módulos a la solución, se mostrarán en esta sección.

       ```json
       "modules": {
         "SimulatedTemperatureSensor": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
             "createOptions": {}
           }
         },
         "turbofanRulClassifier": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "<your registry>.azurecr.io/edgemlsample:1",
             "createOptions": {}
           }
         }
       }
       ```

     * **Rutas:** trabajaremos bastante con rutas en este tutorial. Las rutas definen cómo se comunican los módulos entre sí. Las dos rutas definidas por la plantilla no coinciden con el enrutamiento necesario. La primera ruta envía todos los datos desde cualquier salida del clasificador a IoT Hub ($upstream). La otra ruta es para SimulatedTemperatureSensor, que se acaba de eliminar. Elimine las dos rutas predeterminadas.

       ```json
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
             "turbofanRulClassifierToIoTHub": "FROM /messages/modules/turbofanRulClassifier/outputs/\* INTO $upstream",
             "sensorToturbofanRulClassifier": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\\"/modules/turbofanRulClassifier/inputs/input1\\")"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       }
       ```

   * **deployment.debug.template.json:** este archivo es la versión de depuración de deployment.template.json. Debemos reflejar todos los cambios de deployment.template.json en este archivo.

   * **.env:** este archivo es donde debe proporcionar el nombre de usuario y la contraseña para acceder al registro.

      ```env
      CONTAINER_REGISTRY_USERNAME_<your registry name>=<ACR username>
      CONTAINER_REGISTRY_PASSWORD_<your registry name>=<ACR password>
      ```

10. Haga clic con el botón derecho en el archivo deployment.template.json en el explorador de Visual Studio Code y seleccione **Build IoT Edge Solution** (Compilar la solución IoT Edge).

11. Tenga en cuenta que este comando crea una carpeta de configuración con un archivo deployment.amd64.json. Este archivo es la plantilla de implementación concreta de la solución.

## <a name="add-router-module"></a>Adición del módulo de enrutador

A continuación, agregamos el módulo de enrutador a nuestra solución. El módulo de enrutador controla varias responsabilidades de nuestra solución:

* **Recibir mensajes de dispositivos hoja:** conforme lleguen mensajes al dispositivo IoT Edge desde dispositivos de bajada, el módulo de enrutador recibe el mensaje y comienza la orquestación del enrutamiento del mensaje.
* **Enviar mensajes al módulo Clasificador RUL:** cuando se recibe un mensaje nuevo de un dispositivo de bajada, el módulo de enrutador transforma el mensaje al formato que espera el Clasificador RUL. El enrutador envía el mensaje al Clasificador RUL para obtener una predicción de RUL. Una vez que el clasificador ha realizado una predicción, devuelve el mensaje al módulo de enrutador.
* **Enviar mensajes de RUL a IoT Hub:** cuando el enrutador recibe mensajes desde el clasificador, transforma el mensaje para que contenga solo la información esencial, el id. de dispositivo y la vida útil restante, y envía el mensaje abreviado a IoT Hub. Otro perfeccionamiento, que no hemos hecho, enviaría mensajes a la instancia de IoT Hub solo cuando la predicción del RUL caiga por debajo de un umbral (por ejemplo, cuando el RUL sea inferior a 100 ciclos). Filtrar de este modo podría reducir el volumen de mensajes y los costos de IoT Hub.
* **Enviar mensajes al módulo escritor Avro:** para conservar todos los datos enviados por el dispositivo de bajada, el módulo de enrutador envía todo el mensaje recibido desde el clasificador al módulo de escritor de Avro, que se conservará y cargará los datos mediante la carga de archivos de IoT Hub.

> [!NOTE]
> La descripción de las responsabilidades del módulo puede hacer que el procesamiento parezca secuencial, pero el flujo está basado en mensajes/eventos. Por eso es necesario un módulo de orquestación como nuestro módulo de enrutador.

### <a name="create-module-and-copy-files"></a>Creación del módulo y copia de archivos

1. Haga clic con el botón derecho en la carpeta de módulos de Visual Studio Code y elija **Agregar módulo de IoT Edge**.

2. Elija **Módulo C#** .

3. Asigne un nombre al módulo **turbofanRulClassifier**.

4. Cuando el repositorio de imágenes de Docker se lo solicite, use el registro del área de trabajo de Machine Learning (puede encontrar el registro en el nodo registryCredentials del archivo *deployment.template.json*). Este valor es la dirección completa del registro, como **\<su registro\>.azurecr.io/turbofanrouter**.

    > [!NOTE]
    > En este artículo usamos la instancia de Azure Container Registry que creó el área de trabajo de Azure Machine Learning, la cual usamos para entrenar e implementar nuestro clasificador. Esto es únicamente para su comodidad. Se podría haber creado una nueva instancia de Container Registry y publicar nuestros módulos allí.

5. Abra una nueva ventana de terminal en Visual Studio Code (**Vista** > **Terminal**) y copie los archivos del directorio de módulos.

    ```cmd
    copy c:\source\IoTEdgeAndMlSample\EdgeModules\modules\turbofanRouter\*.cs c:\source\IoTEdgeAndMlSample\EdgeSolution\modules\turbofanRouter\
    ```

6. Cuando se le solicite sobrescribir program.cs, presione `y` y, después, `Enter`.

### <a name="build-router-module"></a>Compilación del módulo de enrutador

1. En Visual Studio Code, seleccione **Terminal** > **Configure Default Build Task** (Configurar tarea de compilación predeterminada).

2. Haga clic en **Create tasks.json file from template** (Crear archivo tasks.json desde plantilla).

3. Haga clic en **.NET Core**.

4. Cuando se abra el archivo tasks.json, reemplace el contenido con:

    ```json
    {
      // See https://go.microsoft.com/fwlink/?LinkId=733558
      // for the documentation about the tasks.json format
      "version": "2.0.0",
      "tasks": [
        {
          "label": "build",
          "command": "dotnet",
          "type": "shell",
          "group": {
            "kind": "build",
            "isDefault": true
          },
          "args": [
            "build",
            "${workspaceFolder}/modules/turbofanRouter"
          ],
          "presentation": {
            "reveal": "always"
          },
          "problemMatcher": "$msCompile"
        }
      ]
    }
    ```

5. Guarde y cierre el archivo tasks.json.

6. Ejecute la compilación con `Ctrl + Shift + B` o **Terminal** > **Run Build Task** (Ejecutar la tarea de compilación).

### <a name="set-up-module-routes"></a>Configurar las rutas del módulo

Como se mencionó anteriormente, el entorno de ejecución de Azure IoT Edge usa rutas configuradas en el archivo *deployment.template.json* para administrar las comunicaciones entre módulos acoplados débilmente. En esta sección, profundizamos en cómo configurar las rutas para el módulo turbofanRouter. Cubriremos primero las rutas de entrada y luego pasaremos a las salidas.

#### <a name="inputs"></a>Entradas

1. En el método Init() de Program.cs, registramos dos devoluciones de llamadas para el módulo:

   ```csharp
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromLeafDevice, LeafDeviceInputMessageHandler, ioTHubModuleClient);
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromClassifier, ClassifierCallbackMessageHandler, ioTHubModuleClient);
   ```

2. La primera devolución de llamada escucha los mensajes enviados al receptor **deviceInput**. En el diagrama anterior, vemos que queremos enrutar los mensajes desde cualquier dispositivo hoja a esta entrada. En el archivo *deployment.template.json*, agregue una ruta que indique al centro de Edge que enrute cualquier mensaje recibido por el dispositivo IoT Edge que no se haya enviado por un módulo IoT Edge en la entrada denominada "deviceInput" en el módulo turbofanRouter:

   ```json
   "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")"
   ```

3. A continuación, agregue una ruta para los mensajes desde el módulo rulClassifier en el módulo turbofanRouter:

   ```json
   "classifierToRouter": "FROM /messages/modules/turbofanRulClassifier/outputs/amloutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")"
   ```

#### <a name="outputs"></a>Salidas

Agregue cuatro rutas adicionales para el parámetro de ruta $edgeHub, para controlar las salidas del módulo de enrutador.

1. El archivo Program.cs define el método SendMessageToClassifier(), que usa al cliente de módulo para enviar un mensaje al clasificador de RUL con la ruta:

   ```json
   "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/turbofanRulClassifier/inputs/amlInput\")"
   ```

2. SendRulMessageToIotHub() usa el cliente de módulo para enviar solo los datos de RUL del dispositivo a IoT Hub a través de la ruta:

   ```json
   "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream"
   ```

3. SendMessageToAvroWriter() usa el cliente de módulo para enviar el mensaje con los datos de RUL agregados al módulo avroFileWriter.

   ```json
   "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")"
   ```

4. HandleBadMessage() envía los mensajes con errores a IoT Hub, donde se pueden enrutar para su uso posterior.

   ```json
   "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
   ```

Con todas las rutas tomadas en conjunto, el nodo "$edgeHub" debería tener un aspecto similar al siguiente JSON:

```json
"$edgeHub": {
  "properties.desired": {
    "schemaVersion": "1.0",
    "routes": {
      "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")",
      "classifierToRouter": "FROM /messages/modules/turbofanRulClassifier/outputs/amlOutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")",
      "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/turbofanRulClassifier/inputs/amlInput\")",
      "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream",
      "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")",
      "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
    },
    "storeAndForwardConfiguration": {
      "timeToLiveSecs": 7200
    }
  }
}
```

> [!NOTE]
> La adición del módulo turbofanRouter crea la siguiente ruta adicional: `turbofanRouterToIoTHub": "FROM /messages/modules/turbofanRouter/outputs/* INTO $upstream`. Quite esta ruta y deje solo las rutas enumeradas anteriormente en el archivo deployment.template.json.

#### <a name="copy-routes-to-deploymentdebugtemplatejson"></a>Copia de las rutas en el archivo deployment.debug.template.json

Por último, para mantener sincronizados los archivos, refleje los cambios realizados en deployment.template.json en deployment.debug.template.json.

## <a name="add-avro-writer-module"></a>Adición del módulo escritor Avro

El módulo escritor Avro tiene dos responsabilidades en nuestra solución: almacenar mensajes y cargar archivos.

* **Almacenar mensajes**: cuando el módulo escritor Avro recibe un mensaje, lo escribe en el sistema de archivos locales en formato Avro. Usamos un montaje de enlace, que se monta en un directorio (en este caso /data/avrofiles) en una ruta de acceso del contenedor del módulo. Este montaje permite al módulo escribir en una ruta de acceso local (/avrofiles) y que estos archivos sean accesibles directamente desde el dispositivo IoT Edge.

* **Cargar archivos**: el módulo escritor Avro utiliza la característica de carga de archivos de Azure IoT Hub para cargar archivos en una cuenta de almacenamiento de Azure. Una vez que un archivo se cargue correctamente, el módulo elimina el archivo del disco

### <a name="create-module-and-copy-files"></a>Creación del módulo y copia de archivos

1. En la paleta de comandos, busque y seleccione **Python: Select Interpreter** (Seleccionar intérprete).

1. Elija el intérprete que se encuentra en C:\\Python37.

1. Vuelva a abrir la paleta de comandos y busque y seleccione **Terminal: Seleccionar el shell predeterminado**.

1. Cuando se le solicite, elija **Símbolo del sistema**.

1. Abra un nuevo shell de terminal, **Terminal** > **Nuevo terminal**.

1. Haga clic con el botón derecho en la carpeta de módulos de Visual Studio Code y elija **Agregar módulo de IoT Edge**.

1. Elija **Módulo de Python**.

1. Asigne el nombre "avroFileWriter" al módulo.

1. Cuando se le solicite su repositorio de imágenes de Docker, use el mismo registro que usó al agregar el módulo de enrutador.

1. Copie archivos desde el módulo de ejemplo a la solución.

   ```cmd
   copy C:\source\IoTEdgeAndMlSample\EdgeModules\modules\avroFileWriter\*.py C:\source\IoTEdgeAndMlSample\EdgeSolution\modules\avroFileWriter\
   ```

1. Si se le solicita sobrescribir main.py, escriba `y` y, después, presione `Enter`.

1. Tenga en cuenta que se han agregado a la solución filemanager.py y schema.py, y main.py se ha actualizado.

> [!NOTE]
> Al abrir un archivo de Python se le pedirá que instale pylint. No es necesario instalar el linter para completar este tutorial.

### <a name="bind-mount-for-data-files"></a>Montaje de enlace para archivos de datos

Como se mencionó en la introducción, el módulo escritor se basa en la presencia de un montaje de enlace para escribir los archivos de Avro en el sistema de archivos del dispositivo.

#### <a name="add-directory-to-device"></a>Adición de un directorio al dispositivo

1. Conéctese a la VM del dispositivo IoT Edge con SSH.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Cree el directorio que contendrá los mensajes del dispositivo hoja guardados.

   ```bash
   sudo mkdir -p /data/avrofiles
   ```

3. Actualice los permisos del directorio para que el contenedor pueda escribir.

   ```bash
   sudo chmod ugo+rw /data/avrofiles
   ```

4. Valide el directorio, ahora tendrá permisos de escritura (w) para el usuario, grupo y propietario.

   ```bash
   ls -la /data
   ```

   ![Permisos de directorio para avrofiles](media/tutorial-machine-learning-edge-06-custom-modules/avrofiles-directory-permissions.png)

#### <a name="add-directory-to-the-module"></a>Adición de un directorio al módulo

Para agregar el directorio al contenedor del módulo, modificaremos las instancias de Dockerfiles asociadas con el módulo avroFileWriter. Hay tres archivos Dockerfiles asociados con el módulo: Dockerfile.amd64, Dockerfile.amd64.debug y Dockerfile.arm32v7. Estos archivos deben mantenerse sincronizados en caso de que se quiera depurar o implementar en un dispositivo de arm32. En aras de este artículo, céntrese solo en Dockerfile.amd64.

1. En el equipo de desarrollo, abra el archivo **Dockerfile.amd64**.

2. Modifique el archivo para que tenga el siguiente aspecto:

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

   Los comandos `mkdir` y `chown` indican al proceso de compilación de Docker que cree un directorio de nivel superior llamado /avrofiles en la imagen y, después, que convierta a moduleuser en el propietario de ese directorio. Es importante que estos comandos se inserten después de que el usuario del módulo se agregue a la imagen con el comando `useradd` y antes de que el contexto pase a moduleuser (USER moduleuser).

3. Haga los cambios correspondientes en Dockerfile.amd64.debug y Dockerfile.arm32v7.

#### <a name="update-the-module-configuration"></a>Actualización de la configuración del módulo

El paso final de la creación del enlace es actualizar los archivos deployment.template.json (y deployment.debug.template.json) con la información de enlace.

1. Guarde deployment.template.json.

2. Modifique la definición del módulo de avroFileWriter agregando el parámetro `Binds` que dirija el directorio del contenedor /avrofiles al directorio local en el dispositivo Edge. La definición del módulo debe coincidir con este ejemplo:

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles"
           ]
         }
       }
     }
   }
   ```

3. Realice los cambios correspondientes en deployment.debug.template.json.

### <a name="bind-mount-for-access-to-configyaml"></a>Montaje de enlace para acceder a config.yaml

Es necesario agregar un enlace más para el módulo de escritor. Este enlace proporciona al módulo acceso para leer la cadena de conexión desde el archivo /etc/iotedge/config.yaml en el dispositivo IoT Edge. Se necesita la cadena de conexión para crear una instancia de IoTHubClient de modo que podamos llamar al método upload\_blob\_async para cargar archivos en IoT Hub. Los pasos para agregar este enlace son similares a los de la sección anterior.

#### <a name="update-directory-permission"></a>Actualización del permiso de directorio

1. Conéctese al dispositivo IoT Edge con SSH.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Agregue el permiso de lectura al archivo config.yaml.

   ```bash
   sudo chmod +r /etc/iotedge/config.yaml
   ```

3. Confirme que los permisos están correctamente configurados.

   ```bash
   ls -la /etc/iotedge/
   ```

4. Asegúrese de que los permisos para config.yaml son **-r--r--r--** .

#### <a name="add-directory-to-module"></a>Adición de un directorio al módulo

1. En el equipo de desarrollo, abra el archivo **Dockerfile.amd64**.

2. Agregue un conjunto adicional de comandos `mkdir` y `chown` al archivo para que tenga el aspecto siguiente:

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig

   USER moduleuser

   CMD "python3", "-u", "./main.py"]
   ```

3. Haga los cambios correspondientes en Dockerfile.amd64.debug y Dockerfile.arm32v7.

#### <a name="update-the-module-configuration"></a>Actualización de la configuración del módulo

1. Abra el archivo **deployment.template.json**.

2. Modifique la definición del módulo de avroFileWriter agregando una segunda línea al parámetro `Binds` que dirija el directorio del contenedor (/app/iotconfig) al directorio local en el dispositivo (/etc/iotedge).

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles",
             "/etc/iotedge:/app/iotconfig"
           ]
         }
       }
     }
   }
   ```

3. Realice los cambios correspondientes en deployment.debug.template.json.

## <a name="install-dependencies"></a>Instalar dependencias

El módulo escritor toma una dependencia en las dos bibliotecas de Python, fastavro y PyYAML. Es necesario instalar las dependencias en nuestro equipo de desarrollo e indicarle al proceso de compilación de Docker que las instale en la imagen de nuestro módulo.

### <a name="pyyaml"></a>PyYAML

1. En el equipo de desarrollo, abra el archivo **requirements.txt** y agregue pyyaml.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   ```

2. Abra el archivo **Dockerfile.amd64** y agregue un comando `pip install` para actualizar setuptools.

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && \
       apt-get install -y --no-install-recommends libcurl4-openssl-dev python3-pip libboost-python1.58-dev libpython3-dev && \
       rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   RUN pip install -U pip setuptools
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

3. Haga los cambios correspondientes en Dockerfile.amd64.debug. <!--may not be necessary. Add 'if needed'?-->

4. Instale pyyaml localmente abriendo un terminal en Visual Studio Code y escribiendo.

   ```cmd
   pip install pyyaml
   ```

### <a name="fastavro"></a>Fastavro

1. En requirements.txt, agregue fastavro después de pyyaml.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   fastavro
   ```

2. Instale fastavro en el equipo de desarrollo mediante el terminal de Visual Studio Code.

   ```cmd
   pip install fastavro
   ```

## <a name="reconfigure-iot-hub"></a>Reconfiguración de IoT Hub

Mediante la introducción de los módulos y el dispositivo IoT Edge en el sistema, hemos cambiado nuestras expectativas sobre qué datos se enviarán al concentrador y su finalidad. Es necesario volver a configurar el enrutamiento en el concentrador para tratar con nuestra nueva realidad.

> [!NOTE]
> El concentrador se vuelve a configurar antes de implementar los módulos porque algunas de las opciones de configuración del concentrador, específicamente la carga de archivos, debe configurarse correctamente para que el módulo avroFileWriter se ejecute correctamente

### <a name="set-up-route-for-rul-messages-in-iot-hub"></a>Configuración de ruta para los mensajes de RUL en IoT Hub

Con el enrutador y el clasificador preparados, se espera recibir mensajes regulares que contengan solo el id. del dispositivo y la predicción de RUL del dispositivo. Queremos enrutar los datos de RUL a su propia ubicación de almacenamiento donde podemos supervisar el estado de los dispositivos, compilar informes y activar alertas según sea necesario. Al mismo tiempo, queremos que los datos del dispositivo que se envían directamente a través de un dispositivo hoja que aún no se ha vinculado a nuestro dispositivo IoT Edge se sigan enrutando a la ubicación de almacenamiento actual.

#### <a name="create-a-rul-message-route"></a>Creación de una ruta de mensajes RUL

1. En Azure Portal, navegue a IoT Hub.

2. En el panel de navegación izquierdo, elija **Enrutamiento de mensajes**.

3. Seleccione **Agregar**.

4. Asigne un nombre a la ruta **RulMessageRoute**.

5. Seleccione **Agregar** junto al selector **Punto de conexión** y elija **Almacenamiento de blobs**.

6. En el formulario **Add a storage endpoint** (Agregar punto de conexión de almacenamiento), asigne un nombre al punto de conexión **ruldata**.

7. Seleccione **Seleccionar un contenedor**.

8. Elija la cuenta de almacenamiento que se usó a lo largo de este tutorial, que se denomina **iotedgeandml\<sufijo único\>** .

9. Elija el contenedor **ruldata** y haga clic en **Seleccionar**.

10. Haga clic en **Crear** para crear el punto de conexión de almacenamiento.

11. Para la **Consulta de enrutamiento**, escriba la siguiente consulta:

    ```sql
    IS_DEFINED($body.PredictedRul) AND NOT IS_DEFINED($body.OperationalSetting1)
    ```

12. Expanda la sección **Prueba** y, después, la sección **Cuerpo del mensaje**. Reemplace el mensaje con este ejemplo de nuestros mensajes esperados:

    ```json
    {
      "ConnectionDeviceId": "aaLeafDevice_1",
      "CorrelationId": "b27e97bb-06c5-4553-a064-e9ad59c0fdd3",
      "PredictedRul": 132.62721409309165,
      "CycleTime": 64.0
    }
    ```

13. Seleccione **Probar ruta**. Si la prueba es correcta, vera "The message matched the query" (El mensaje coincidió con la consulta).

14. Haga clic en **Save**(Guardar).

#### <a name="update-turbofandevicetostorage-route"></a>Actualización de la ruta turbofanDeviceToStorage

No queremos enrutar los nuevos datos de predicción a la ubicación de almacenamiento anterior, por lo que es necesario actualizar la ruta.

1. En la página **Enrutamiento de mensajes** de IoT Hub, seleccione la pestaña **Rutas**.

2. Seleccione **turbofanDeviceDataToStorage**, o el nombre que haya asignado a la ruta de datos inicial del dispositivo.

3. Actualización de la consulta de enrutamiento

   ```sql
   IS_DEFINED($body.OperationalSetting1)
   ```

4. Expanda la sección **Prueba** y, después, la sección **Cuerpo del mensaje**. Reemplace el mensaje con este ejemplo de nuestros mensajes esperados:

   ```json
   {
     "Sensor13": 2387.96,
     "OperationalSetting1": -0.0008,
     "Sensor6": 21.61,
     "Sensor11": 47.2,
     "Sensor9": 9061.45,
     "Sensor4": 1397.86,
     "Sensor14": 8140.39,
     "Sensor18": 2388.0,
     "Sensor12": 522.87,
     "Sensor2": 642.42,
     "Sensor17": 391.0,
     "OperationalSetting3": 100.0,
     "Sensor1": 518.67,
     "OperationalSetting2": 0.0002,
     "Sensor20": 39.03,
     "DeviceId": 19.0,
     "Sensor5": 14.62,
     "PredictedRul": 212.00132402791962,
     "Sensor8": 2388.01,
     "Sensor16": 0.03,
     "CycleTime": 42.0,
     "Sensor21": 23.3188,
     "Sensor15": 8.3773,
     "Sensor3": 1580.09,
     "Sensor10": 1.3,
     "Sensor7": 554.57,
     "Sensor19": 100.0
   }
   ```

5. Seleccione **Probar ruta**. Si la prueba es correcta, vera "The message matched the query" (El mensaje coincidió con la consulta).

6. Seleccione **Guardar**.

### <a name="configure-file-upload"></a>Configuración de la carga de archivos

Configure la característica de carga de archivos de IoT Hub para habilitar el módulo de escritor de archivos para la carga de archivos al almacenamiento.

1. En el navegador izquierdo de IoT Hub, elija **Carga de archivos**.

2. Seleccione **Contenedor de Azure Storage**.

3. Seleccione la cuenta de almacenamiento en la lista.

4. Seleccione el contenedor **uploadturbofanfiles** y haga clic en **Seleccionar**.

5. Seleccione **Guardar**. El portal le avisa cuando se completa la operación de guardar.

> [!Note]
> No vamos a activar la notificación de carga para este tutorial, pero consulte [Recepción de una notificación de carga de archivos](../iot-hub/iot-hub-java-java-file-upload.md#receive-a-file-upload-notification) para obtener más información acerca de cómo usar la notificación de carga de archivos.

## <a name="build-publish-and-deploy-modules"></a>Creación, publicación e implementación de módulos

Ahora que hemos modificado la configuración, ya está todo listo para compilar imágenes y publicarlas en Azure Container Registry. El proceso de compilación usa el archivo deployment.template.json para determinar qué módulos deben compilarse. La configuración de cada módulo, incluida la versión, se encuentra en el archivo module.json de la carpeta del módulo. En primer lugar, el proceso de compilación se ejecuta en una compilación de Docker en los archivos Dockerfile que coinciden con la configuración actual que se encuentra en el archivo module.json para crear una imagen. A continuación, publica la imagen en el registro desde el archivo module.json con una etiqueta de versión que coincide con la del archivo module.json. Por último, genera un manifiesto de implementación específico de la configuración (por ejemplo, deployment.amd64.json), que se implementará en el dispositivo IoT Edge. El dispositivo IoT Edge lee la información del manifiesto de implementación y, basándose en las instrucciones, descargará los módulos, y configurará las rutas y las propiedades que quiera. Este método de implementación tiene dos efectos secundarios que debería tener en cuenta:

* **Retardo de implementación:** puesto que el entorno de ejecución de Azure IoT Edge debe reconocer el cambio a sus propiedades deseadas antes de comenzar a volver a configurar, puede pasar algún tiempo desde que se implementan los módulos hasta que el entorno de ejecución los detecta y empieza a actualizar el dispositivo IoT Edge.

* **Versiones del módulo:** si publica una nueva versión de un contenedor del módulo en la instancia de Container Registry utilizando las mismas etiquetas de versión que el módulo anterior, el entorno de ejecución no descargará la nueva versión del módulo. Realiza una comparación de la etiqueta de la versión de la imagen local y la imagen deseada desde el manifiesto de implementación. Si esas versiones coinciden, el entorno de ejecución no realiza ninguna acción. Por lo tanto, es importante aumentar la versión del módulo cada vez que quiera implementar nuevos cambios. Aumente la versión cambiando la propiedad **version** en la propiedad **tag** del archivo module.json para el módulo que se va a cambiar. A continuación, compile y publique la solución.

    ```json
    {
      "$schema-version": "0.0.1",
      "description": "",
      "image": {
        "repository": "<your registry>.azurecr.io/avrofilewriter",
        "tag": {
          "version": "0.0.1",
          "platforms": {
            "amd64": "./Dockerfile.amd64",
            "amd64.debug": "./Dockerfile.amd64.debug",
            "arm32v7": "./Dockerfile.arm32v7"
          }
        },
        "buildOptions": []
      },
      "language": "python"
    }
    ```

### <a name="build-and-publish"></a>Compilación y publicación

1. En Visual Studio Code en su VM de desarrollo, abra una ventana de terminal de Visual Studio Code e inicie sesión en la instancia de Container Registry.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

1. En Visual Studio Code, haga clic con el botón derecho en el archivo deployment.template.json y seleccione **Build and Push IoT Edge solution** (Compilar e insertar solución IoT Edge).

### <a name="view-modules-in-the-registry"></a>Vista de módulos en el registro

Una vez que la compilación se complete correctamente, se podrá usar Azure Portal para revisar los módulos publicados.

1. En Azure Portal, vaya al área de trabajo de Azure Machine Learning y haga clic en el hipervínculo **Registro**.

    ![Navegación al registro desde el área de trabajo de Machine Learning Service](media/tutorial-machine-learning-edge-06-custom-modules/follow-registry-link.png)

2. En el registro del lado de navegación, seleccione **Repositorios**.

3. Tenga en cuenta que los módulos que creó, **avrofilewriter** y **turbofanrouter**, aparecen como repositorios.

4. Seleccione **turbofanrouter** y tenga en cuenta que ha publicado una imagen etiquetada como 0.0.1-amd64.

   ![Visualización de la primera versión etiquetada de turbofanrouter](media/tutorial-machine-learning-edge-06-custom-modules/tagged-image-turbofanrouter-repo.png)

### <a name="deploy-modules-to-iot-edge-device"></a>Implementación de módulos en un dispositivo IoT Edge

Hemos compilado y configurado los módulos en nuestra solución, ahora los implementaremos en el dispositivo IoT Edge.

1. En Visual Studio Code, haga clic en el archivo **deployment.amd64.json** de la carpeta config.

2. Seleccione **Crear una implementación para un dispositivo individual**.

3. Elija el dispositivo IoT Edge, **aaTurboFanEdgeDevice**.

4. Actualice el panel de dispositivos de Azure IoT Hub en el explorador de Visual Studio Code. Debería ver implementados los tres nuevos módulos, que aún no se están ejecutando.

5. Vuelva a actualizar después de unos minutos y verá los módulos en ejecución.

   ![Visualización de los módulos en ejecución en Visual Studio Code](media/tutorial-machine-learning-edge-06-custom-modules/view-running-modules-list.png)

> [!NOTE]
> Los módulos pueden tardar varios minutos en iniciarse y establecerse en un estado de ejecución estable. Durante ese tiempo, es posible que vea que los módulos se inician y detienen mientras intentan establecer una conexión con el módulo del centro de IoT Edge.

## <a name="diagnosing-failures"></a>Diagnóstico de errores

En esta sección, se comparten algunas técnicas para comprender qué ha ido mal con un módulo o módulos. A menudo, un error suele detectarse en primer lugar desde el estado de Visual Studio Code.

### <a name="identify-failed-modules"></a>Identificación de módulos con errores

* **Visual Studio Code:** consulte el panel de dispositivos de Azure IoT Hub. Si la mayoría de los módulos está en un estado de ejecución, pero uno está detenido, deberá investigar porqué lo está. Si todos los módulos están detenidos durante un largo período de tiempo, esto también puede indicar un error.

* **Azure Portal:** si navega a IoT Hub en el portal y, después, busca la página de detalles del dispositivo (en IoT Edge, profundice en el dispositivo) es posible que descubra que un módulo ha detectado un error o que nunca ha notificado nada a IoT Hub.

### <a name="diagnosing-from-the-device"></a>Diagnóstico desde el dispositivo

Al iniciar sesión en el dispositivo IoT Edge, puede obtener acceso a una gran cantidad de información acerca del estado de los módulos. El principal mecanismo que usamos son los comandos de Docker que permiten examinar los contenedores y las imágenes en el dispositivo.

1. Enumere todos los contenedores en ejecución. Se espera ver un contenedor por módulo con un nombre que se corresponde con dicho módulo. Además, este comando muestra la imagen del contenedor, incluida la versión para que coincida con sus expectativas. También puede enumerar las imágenes sustituyendo "image" por "container" en el comando.

   ```bash
   sudo docker container ls
   ```

2. Obtenga los registros de un contenedor. Este comando da como resultado lo que se ha escrito en StdErr y StdOut en el contenedor. Este comando funciona para los contenedores que se iniciaron y, después, desaparecieron por algún motivo. También es útil para comprender qué está pasando con los contenedores edgeAgent o edgeHub.

   ```bash
   sudo docker container logs <container name>
   ```

3. Inspeccione un contenedor. Este comando proporciona una gran cantidad de información sobre la imagen. Los datos se pueden filtrar según lo que busca. Por ejemplo, si quiere ver si son correctos los enlaces en avroFileWriter puede usar el comando:

   ```bash
   sudo docker container inspect -f "{{ json .Mounts }}" avroFileWriter | python -m json.tool
   ```

4. Conéctese a un contenedor en ejecución. Este comando puede ser útil si quiere examinar el contenedor mientras se está ejecutando:

   ```bash
   sudo docker exec -it avroFileWriter bash
   ```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, hemos creado una solución de IoT Edge en Visual Studio Code con tres módulos, un clasificador, un enrutador y un sistema de escritura/carga de archivos. Configuramos las rutas para permitir que los módulos se comuniquen entre sí en el dispositivo Edge, modificamos la configuración del dispositivo Edge y actualizamos los archivos Dockerfiles para instalar las dependencias y agregar montajes de enlace a los contenedores de los módulos. A continuación, hemos actualizado la configuración de IoT Hub para enrutar los mensajes según el tipo y controlar las cargas de archivos. Con todo preparado, implementamos los módulos en el dispositivo IoT Edge y garantizamos que los módulos se estaban ejecutando correctamente.

Puedes encontrar más información en las páginas siguientes:

* [Obtenga información sobre cómo implementar módulos y establecer rutas en IoT Edge](module-composition.md)
* [Sintaxis de las consultas de enrutamiento de mensajes de IoT Hub](../iot-hub/iot-hub-devguide-routing-query-syntax.md)
* [IoT Hub message routing: now with routing on message body](https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/) (Enrutamiento de mensajes de IoT Hub: ahora con enrutamiento en el cuerpo del mensaje)
* [Carga de archivos con IoT Hub](../iot-hub/iot-hub-devguide-file-upload.md)
* [Carga de archivos de un dispositivo a la nube con IoT Hub](../iot-hub/iot-hub-python-python-file-upload.md)

Continúe con el siguiente artículo para empezar a enviar datos y ver la solución en acción.

> [!div class="nextstepaction"]
> [Send data via transparent gateway](tutorial-machine-learning-edge-07-send-data-to-hub.md) (Envío de datos mediante una puerta de enlace transparente)
