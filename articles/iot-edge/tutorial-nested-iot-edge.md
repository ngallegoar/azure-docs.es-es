---
title: 'Tutorial: Creación de una jerarquía de dispositivos IoT Edge con Azure IoT Edge'
description: En este tutorial se muestra cómo crear una estructura jerárquica de dispositivos IoT Edge mediante puertas de enlace.
author: v-tcassi
manager: philmea
ms.author: v-tcassi
ms.date: 11/10/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
monikerRange: '>=iotedge-2020-11'
ms.openlocfilehash: 28b34ecaf51406b35c67d3838714691390f5adf7
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96453051"
---
# <a name="tutorial-create-a-hierarchy-of-iot-edge-devices-preview"></a>Tutorial: Creación de una jerarquía de dispositivos IoT Edge (versión preliminar)

Implemente nodos de Azure IoT Edge en redes organizadas en capas jerárquicas. Cada capa de una jerarquía es un dispositivo de puerta de enlace que controla los mensajes y las solicitudes de los dispositivos de la capa que se encuentra debajo.

>[!NOTE]
>Esta característica requiere IoT Edge versión 1.2, la cual se encuentra en versión preliminar pública, y la ejecución de contenedores Linux.

Puede estructurar una jerarquía de dispositivos para que solo la capa superior tenga conectividad a la nube y las capas inferiores solo puedan comunicarse con las capas superior e inferior adyacentes. Estas capas de red constituyen la base de la mayoría de las redes industriales que siguen la [norma ISA-95](https://en.wikipedia.org/wiki/ANSI/ISA-95).

El objetivo de este tutorial es crear una jerarquía de dispositivos IoT Edge que simula un entorno de producción. Al final, implementará el [módulo del sensor de temperatura simulado](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.simulated-temperature-sensor) en un dispositivo de nivel inferior sin acceso a Internet mediante la descarga de imágenes de contenedor a través de la jerarquía.

Para lograr este objetivo, este tutorial le guiará a través de la creación de una jerarquía de dispositivos IoT Edge, la implementación de contenedores del entorno de ejecución de Azure IoT Edge en los dispositivos y la configuración local de estos. En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Crear y definir las relaciones en una jerarquía de dispositivos IoT Edge.
> * Configurar el entorno de ejecución de Azure IoT Edge en los dispositivos de la jerarquía.
> * Instalar certificados coherentes en la jerarquía de dispositivos.
> * Agregar cargas de trabajo a los dispositivos de la jerarquía.
> * Use un módulo de proxy de API para enrutar de forma segura el tráfico HTTP a través de un único puerto desde los dispositivos del nivel inferior.

En este tutorial, se definen los siguientes niveles de red:

* **Nivel superior**: Los dispositivos IoT Edge de este nivel se pueden conectar directamente a la nube.

* **Nivel inferior**: Los dispositivos IoT Edge de este nivel no se pueden conectar directamente a la nube. Deben pasar por uno o más dispositivos IoT Edge intermediarios para enviar y recibir datos.

En este tutorial se usa una jerarquía de dos dispositivos para simplificar. Un dispositivo, **topLayerDevice**, representa un dispositivo del nivel superior de la jerarquía, el cual puede conectarse directamente a la nube. También se hace referencia a este dispositivo como **dispositivo primario**. El otro dispositivo, **lowerLayerDevice**, representa un dispositivo del nivel inferior de la jerarquía, el cual no puede conectarse directamente a la nube. También se hace referencia a este dispositivo como **dispositivo secundario**. Puede agregar dispositivos de nivel inferior adicionales para representar el entorno de producción. La configuración de los dispositivos de nivel inferior adicionales sigue la configuración de **lowerLayerDevice**.

## <a name="prerequisites"></a>Requisitos previos

Para crear una jerarquía de dispositivos IoT Edge, necesitará:

* Un equipo (Windows o Linux) con conectividad a Internet.
* Dos dispositivos Linux para configurar como dispositivos IoT Edge. Si no tiene dispositivos disponibles, puede usar [máquinas virtuales de Azure](../virtual-machines/linux/index.yml).
* Una cuenta de Azure con una suscripción válida. Si no tiene una [suscripción a Azure](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing), cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure.
* La versión v2.3.1 de la CLI de Azure con la extensión v0.10.6 de Azure IoT u otra versión posterior. Este tutorial usa [Azure Cloud Shell](../cloud-shell/overview.md). Si no está familiarizado con Azure Cloud Shell, [consulte un inicio rápido para más información](./quickstart-linux.md#use-azure-cloud-shell).

También puede probar este escenario siguiendo el [ejemplo con scripts de Azure IoT Edge para el IoT industrial](https://aka.ms/iotedge-nested-sample), que implementa máquinas virtuales de Azure como dispositivos preconfigurados para simular un entorno de fábrica.

## <a name="configure-your-iot-edge-device-hierarchy"></a>Configuración de la jerarquía de dispositivos IoT Edge

### <a name="create-a-hierarchy-of-iot-edge-devices"></a>Creación de una jerarquía de dispositivos IoT Edge

El primer paso, la creación de los dispositivos IoT Edge, puede realizarse mediante Azure Portal o la CLI de Azure. En este tutorial se creará una jerarquía de dos dispositivos IoT Edge: **topLayerDevice** y el dispositivo secundario **lowerLayerDevice**.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. En [Azure Portal](https://ms.portal.azure.com/), vaya a IoT Hub.

1. En el menú del panel izquierdo, en **Administración automática de dispositivos**, seleccione **IoT Edge**.

1. Seleccione **+ Agregar un dispositivo IoT Edge**. Este dispositivo será el dispositivo de nivel superior, por lo que debe especificar un identificador de dispositivo único adecuado. Seleccione **Guardar**.

1. Seleccione **+ Agregar un dispositivo IoT Edge** de nuevo. Este dispositivo será el dispositivo de nivel inferior perimetral, por lo que debe especificar un identificador de dispositivo único adecuado.

1. Elija **Establecer un dispositivo primario**, elija el dispositivo de nivel superior en la lista de dispositivos y seleccione **Aceptar**. Seleccione **Guardar**.

   ![Configuración del elemento primario para el dispositivo de nivel inferior](./media/tutorial-nested-iot-edge/set-parent-device.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/), escriba el comando siguiente para crear un dispositivo IoT Edge en el centro de conectividad. Este dispositivo será el dispositivo de nivel superior, por lo que debe especificar un identificador de dispositivo único adecuado:

   ```azurecli-interactive
   az iot hub device-identity create --device-id {top_layer_device_id} --edge-enabled --hub-name {hub_name}
   ```

1. Escriba el siguiente comando para crear el dispositivo IoT Edge secundario y crear la relación primario-secundario entre los dispositivos:

    ```azurecli-interactive
    az iot hub device-identity create --device-id {lower_layer_device_id} --edge-enabled --pd {top_layer_device_id} --hub-name {iothub_name}
    ```

---

Tome nota de cada cadena de conexión del dispositivo IoT Edge. Se usarán más adelante.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. En [Azure Portal](https://ms.portal.azure.com/), vaya a la sección **IoT Edge** de IoT Hub.

1. Haga clic en el identificador de dispositivo de uno de los dispositivos de la lista.

1. Seleccione **Copiar** en el campo **Cadena de conexión principal** y guárdelo en un lugar de su elección.

1. Repita el procedimiento con el resto de dispositivos.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/), para cada dispositivo, escriba el siguiente comando para recuperar la cadena de conexión del dispositivo y guardarla en un lugar de su elección:

   ```azurecli-interactive
   az iot hub device-identity connection-string show --device-id {device_id} --hub-name {hub_name}
   ```

---

### <a name="create-certificates"></a>Creación de certificados

Todos los dispositivos de un [escenario de puerta de enlace](iot-edge-as-gateway.md) necesitan un certificado compartido para configurar conexiones seguras entre ellos. Use los pasos siguientes para crear certificados de demostración para los dispositivos de este escenario.

Para crear certificados de demostración en un dispositivo Linux, debe clonar los scripts de generación y configurarlos para que se ejecuten localmente en bash.

1. Clone el repositorio de Git de IoT Edge que contiene los scripts para generar los certificados de demostración.

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

1. Vaya al directorio en el que quiere trabajar. Todos los archivos de certificados y claves se crearán en este directorio.

1. Copie los archivos de configuración y de script del repositorio de IoT Edge clonado en el directorio de trabajo.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

1. Cree el certificado de entidad de certificación raíz y un certificado intermedio.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Este comando de script crea varios archivos de certificado y clave, pero nosotros vamos a usar el siguiente archivo como **certificado de entidad de certificación raíz** para la jerarquía de la puerta de enlace:

   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`  

1. Cree dos conjuntos de certificados y claves privadas de entidad de certificación para los dispositivos IoT Edge con el comando siguiente: un conjunto para el dispositivo del nivel superior y otro para el del inferior. Proporcione nombres fáciles de recordar para que los certificados de la entidad de certificación puedan distinguirlos entre sí.

   ```bash
   ./certGen.sh create_edge_device_ca_certificate "top-layer-device"
   ./certGen.sh create_edge_device_ca_certificate "lower-layer-device"
   ```

   Este comando de script crea varios archivos de certificado y de clave, pero vamos a usar el siguiente par de certificado y clave en cada dispositivo IoT Edge y a esto se hace referencia en el archivo config.yaml:

   * `<WRKDIR>/certs/iot-edge-device-<CA cert name>-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-<CA cert name>.key.pem`

Cada dispositivo necesita una copia del certificado de la entidad de certificación raíz y una copia de su propio certificado y clave privada. Puede usar una unidad USB o una [copia de archivo seguro](https://www.ssh.com/ssh/scp/) para trasladar los certificados a cada dispositivo.

1. Una vez transferidos los certificados, instale la entidad de certificación raíz para cada dispositivo.

   ```bash
   sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```

   La salida de este comando debe indicar que se ha agregado un certificado en /etc/ssl/certs.

### <a name="install-iot-edge-on-the-devices"></a>Instalación de IoT Edge en los dispositivos

Instale IoT Edge siguiendo estos pasos en ambos dispositivos.

1. Actualice las listas de paquetes en el dispositivo.

   ```bash
   sudo apt-get update
   ```

1. Instale el motor de Moby.

   ```bash
   sudo apt-get install moby-engine
   ```

1. Instalación de hsmlib y el demonio de IoT Edge <!-- Update with proper image links on release -->

   ```bash
   curl -L https://github.com/Azure/azure-iotedge/releases/download/1.2.0-rc2/libiothsm-std_1.2.0.rc2-1-1_debian9_amd64.deb -o libiothsm-std.deb
   curl -L https://github.com/Azure/azure-iotedge/releases/download/1.2.0-rc2/iotedge_1.2.0_rc2-1_debian9_amd64.deb -o iotedge.deb
   sudo dpkg -i ./libiothsm-std.deb
   sudo dpkg -i ./iotedge.deb
   ```

### <a name="configure-the-iot-edge-runtime"></a>Configuración del runtime de IoT Edge

Configure el entorno de ejecución de Azure IoT Edge con estos pasos en ambos dispositivos. La configuración del entorno de ejecución de Azure IoT Edge para los dispositivos consta de cuatro pasos que se realizan todos mediante la edición del archivo de configuración de IoT Edge:

1. Para aprovisionar manualmente cada dispositivo agregue la cadena de conexión del dispositivo al archivo de configuración.

1. Termine de configurar los certificados del dispositivo; para ello, apunte el archivo de configuración hacia el certificado, la clave privada y el certificado de la entidad de certificación raíz del dispositivo.

1. Arranque el sistema con el agente de IoT Edge.

1. Actualice el parámetro **hostname** del dispositivo del **nivel superior** y actualice los parámetros **hostname** y **parent_hostname** de los dispositivos del **nivel inferior**.

Complete estos pasos y reinicie el servicio IoT Edge para configurar los dispositivos.

1. En cada dispositivo, abra el archivo de configuración de IoT Edge.

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

1. Busque las configuraciones de aprovisionamiento del archivo y quite la marca de comentario de la sección **Manual provisioning configuration using a connection string** (Configuración del aprovisionamiento manual mediante una cadena de conexión).

1. Actualice el valor de **device_connection_string** con la cadena de conexión desde un dispositivo IoT Edge. Asegúrese de que las demás secciones de aprovisionamiento estén comentadas. Asegúrese de que la línea **provisioning:** no tiene ningún espacio en blanco delante y de que los elementos anidados muestran una sangría de dos espacios.

   ```yml
   # Manual provisioning configuration using a connection string
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
     dynamic_reprovisioning: false
   ```

   >[!TIP]
   >Para pegar el contenido del portapapeles en Nano: `Shift+Right Click` o presione `Shift+Insert`.

1. Busca la sección **certificados**. Quite las marcas de comentarios y actualice los tres campos de certificado para que apunten a los certificados que creó en la sección anterior y que se movieron al dispositivo IoT Edge. Proporcione las rutas de acceso del URI del archivo, las cuales adoptan el formato `file:///<path>/<filename>`.

   ```yml
   certificates:
     device_ca_cert: "<File URI path to the device CA certificate unique to this device.>"
     device_ca_pk: "<File URI path to the device CA private key unique to this device.>"
     trusted_ca_certs: "<File URI path to the root CA certificate shared by all devices in the gateway hierarchy.>"
   ```

1. Para el dispositivo del **nivel superior**, busque el parámetro **hostname**. Actualice el valor para que sea el nombre de dominio completo (FQDN) o la dirección IP del dispositivo IoT Edge. Use el valor que elija de forma coherente en los dispositivos de la jerarquía.

   ```yml
   hostname: <device fqdn or IP>
   ```

1. Para los dispositivos IoT Edge de los **niveles inferiores**, actualice el archivo de configuración para que apunte hacia el FQDN o la dirección IP del dispositivo primario, para que coincida con lo que haya en el campo **hostname** del dispositivo primario. En el caso de los dispositivos IoT Edge del **nivel superior**, deje este parámetro como comentario.

   ```yml
   parent_hostname: <parent device fqdn or IP>
   ```

   > [!NOTE]
   > En el caso de las jerarquías con más de un nivel inferior, actualice el campo *parent_hostname* con el FQDN del dispositivo del nivel inmediatamente inferior.

1. Para el dispositivo del **nivel superior**, busque la sección **Agente** de YAML y actualice el valor de la imagen a la versión correcta del agente de IoT Edge. En este caso, apuntaremos al agente de IoT Edge del nivel superior en la instancia de Azure Container Registry con la versión preliminar pública de la imagen del agente de IoT Edge disponible.

   ```yml
   agent:
     name: "edgeAgent"
     type: "docker"
     env: {}
     config:
       image: "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc2"
       auth: {}
   ```

1. Para los dispositivos IoT Edge de los **niveles inferiores**, actualice el nombre de dominio del valor de la imagen para que apunte al FQDN o la dirección IP del dispositivo primario seguido del puerto del proxy de la API, el 8000. Agregará el módulo de proxy de API en la sección siguiente.

   ```yml
   agent:
     name: "edgeAgent"
     type: "docker"
     env: {}
     config:
       image: "<parent_device_fqdn_or_ip>:8000/azureiotedge-agent:1.2.0-rc2"
       auth: {}
   ```

1. Guarde y cierre el archivo.

   `CTRL + X`, `Y`, `Enter`

1. Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

   ```bash
   sudo systemctl restart iotedge
   ```

## <a name="deploy-modules-to-the-top-layer-device"></a>Implementación de módulos en el dispositivo de nivel superior

El resto de los pasos para completar la configuración del entorno de ejecución de Azure IoT Edge e implementar las cargas de trabajo se realiza desde la nube mediante Azure Portal o la CLI de Azure.

# <a name="portal"></a>[Portal](#tab/azure-portal)

En el [Portal de Azure](https://ms.portal.azure.com/):

1. Vaya a su instancia de IoT Hub.

1. En el menú del panel izquierdo, en **Administración automática de dispositivos**, seleccione **IoT Edge**.

1. Haga clic en el id. de dispositivo del dispositivo perimetral del **nivel superior** en la lista de dispositivos.

1. En la barra superior, seleccione **Establecer módulos**.

1. Seleccione la opción **Configuración del entorno de ejecución**, situada junto al icono de engranaje.

1. En **Centro de Microsoft Edge**, en el campo de imagen, escriba `mcr.microsoft.com/azureiotedge-hub:1.2.0-rc2`.

   ![Edición de la imagen del Centro de Microsoft Edge](./media/tutorial-nested-iot-edge/edge-hub-image.png)

1. Agregue las siguientes variables de entorno al módulo del Centro de Microsoft Edge:

    | Nombre | Value |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

   ![Edición de las variables de entorno del Centro de Microsoft Edge](./media/tutorial-nested-iot-edge/edge-hub-environment-variables.png)

1. En **Agente de Edge**, en el campo de imagen, escriba `mcr.microsoft.com/azureiotedge-agent:1.2.0-rc2`. Seleccione **Guardar**.

1. Agregue el módulo del registro de Docker al dispositivo de nivel superior. Seleccione **+ Agregar** y elija **Módulo IoT Edge** en la lista desplegable. Proporcione el nombre `registry` para el módulo del registro de Docker y escriba `registry:latest` para el URI de la imagen. A continuación, agregue variables de entorno y cree opciones que apunten el módulo del registro local hacia el registro de contenedor de Microsoft desde el que descargar las imágenes de contenedor y proporcionar estas imágenes en el registro: 5000.

1. En la pestaña Variables de entorno, escriba el siguiente par nombre-valor de la variable de entorno:

    | Nombre | Value |
    | - | - |
    | `REGISTRY_PROXY_REMOTEURL` | `https://mcr.microsoft.com` |

1. En la pestaña Opciones de creación del contenedor, escriba el siguiente JSON:

   ```json
   {
    "HostConfig": {
        "PortBindings": {
            "5000/tcp": [
                {
                    "HostPort": "5000"
                }
            ]
         }
      }
   }
   ```

1. A continuación, agregue el módulo de proxy de API al dispositivo del nivel superior. Seleccione **+ Agregar** y elija **Módulo de Marketplace** en la lista desplegable. Busque `IoT Edge API Proxy` y seleccione el módulo. El proxy de API de IoT Edge usa el puerto 8000 y está configurado para usar el módulo del registro denominado `registry` en el puerto 5000 de forma predeterminada.

1. Seleccione **Revisar y crear** y, luego, **Crear** para completar la implementación. El entorno de ejecución de Azure IoT Edge del dispositivo del nivel superior, que tiene acceso a Internet, extraerá y ejecutará las configuraciones de la **versión preliminar pública** para el centro y el agente de IoT Edge.

   ![Implementación finalizada que contiene el centro de Edge, el agente de Edge, el módulo del registro y el módulo de proxy de API](./media/tutorial-nested-iot-edge/complete-top-layer-deployment.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/), escriba el comando siguiente para crear un archivo deployment.json:

   ```azurecli-interactive
   code deploymentTopLayer.json
   ```

1. Copie el contenido del JSON siguiente en el archivo deployment.json, guárdelo y ciérrelo.

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "dockerContainerRegistry": {
                           "settings": {
                               "image": "registry:latest",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5000/tcp\":[{\"HostPort\":\"5000\"}]}}}"
                           },
                           "type": "docker",
                           "version": "1.0",
                           "env": {
                               "REGISTRY_PROXY_REMOTEURL": {
                                   "value": "https://mcr.microsoft.com"
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       },
                       "IoTEdgeAPIProxy": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-api-proxy",
                               "createOptions": "{\"HostConfig\": {\"PortBindings\": {\"8000/tcp\": [{\"HostPort\":\"8000\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "NGINX_DEFAULT_PORT": {
                                   "value": "8000"
                               },
                               "DOCKER_REQUEST_ROUTE_ADDRESS": {
                                   "value": "registry:5000"
                               },
                               "BLOB_UPLOAD_ROUTE_ADDRESS": {
                                   "value": "AzureBlobStorageonIoTEdge:11002"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25",
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc2",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-hub:1.2.0-rc2",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "experimentalFeatures__enabled": {
                                   "value": "true"
                               },
                               "experimentalFeatures__nestedEdgeEnabled": {
                                   "value": "true"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

1. Escriba el siguiente comando para crear una implementación en el dispositivo perimetral del nivel superior:

   ```azurecli-interactive
   az iot edge set-modules --device-id <top_layer_device_id> --hub-name <iot_hub_name> --content ./deploymentTopLayer.json
   ```

---

## <a name="deploy-modules-to-the-lower-layer-device"></a>Implementación de módulos en el dispositivo de nivel inferior

Puede usar Azure Portal o la CLI de Azure para implementar cargas de trabajo desde la nube en los dispositivos de **nivel inferior**.

# <a name="portal"></a>[Portal](#tab/azure-portal)

En el [Portal de Azure](https://ms.portal.azure.com/):

1. Vaya a su instancia de IoT Hub.

1. En el menú del panel izquierdo, en **Administración automática de dispositivos**, seleccione **IoT Edge**.

1. Haga clic en el id. del dispositivo del nivel inferior en la lista de dispositivos IoT Edge.

1. En la barra superior, seleccione **Establecer módulos**.

1. Seleccione la opción **Configuración del entorno de ejecución**, situada junto al icono de engranaje.

1. En **Centro de Microsoft Edge**, en el campo de imagen, escriba `$upstream:8000/azureiotedge-hub:1.2.0-rc2`.

1. Agregue las siguientes variables de entorno al módulo del Centro de Microsoft Edge:

    | Nombre | Value |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

1. En **Agente de Edge**, en el campo de imagen, escriba `$upstream:8000/azureiotedge-agent:1.2.0-rc2`. Seleccione **Guardar**.

1. Agregue el módulo del sensor de temperatura. Seleccione **+ Agregar** y elija **Módulo de Marketplace** en la lista desplegable. Busque `Simulated Temperature Sensor` y seleccione el módulo.

1. En **Módulos de IoT Edge**, seleccione el módulo `Simulated Temperature Sensor` que acaba de agregar y actualice su URI de imagen para que apunte a `$upstream:8000/azureiotedge-simulated-temperature-sensor:1.0`.

1. Seleccione **Guardar**, **Revisar y crear**, y **Crear** para completar la implementación.

   ![Implementación finalizada que contiene el centro de Edge, el agente de Edge y el sensor de temperatura simulado](./media/tutorial-nested-iot-edge/complete-lower-layer-deployment.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. En [Azure Cloud Shell](https://shell.azure.com/), escriba el comando siguiente para crear un archivo deployment.json:

   ```azurecli-interactive
   code deploymentLowerLayer.json
   ```

1. Copie el contenido del JSON siguiente en el archivo deployment.json, guárdelo y ciérrelo.

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "simulatedTemperatureSensor": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-simulated-temperature-sensor:1.0",
                               "createOptions": ""
                           },
                           "type": "docker",
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25",
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-agent:1.2.0-rc2",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-hub:1.2.0-rc2",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "experimentalFeatures__enabled": {
                                   "value": "true"
                               },
                               "experimentalFeatures__nestedEdgeEnabled": {
                                   "value": "true"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

1. Escriba el siguiente comando para crear una implementación de módulos establecida en el dispositivo del nivel inferior:

   ```azurecli-interactive
   az iot edge set-modules --device-id <lower_layer_device_id> --hub-name <iot_hub_name> --content ./deploymentLowerLayer.json

---

Notice that the image URI that we used for the simulated temperature sensor module pointed to `$upstream:8000` instead of to a container registry. We configured this device to not have direct connections to the cloud, because it's in a lower layer. To pull container images, this device requests the image from its parent device instead. At the top layer, the API proxy module routes this container request to the registry module, which handles the image pull.

On the device details page for your lower layer IoT Edge device, you should now see the temperature sensor module listed along the system modules as **Specified in deployment**. It may take a few minutes for the device to receive its new deployment, request the container image, and start the module. Refresh the page until you see the temperature sensor module listed as **Reported by device**.

## View generated data

The **Simulated Temperature Sensor** module that you pushed generates sample environment data. It sends messages that include ambient temperature and humidity, machine temperature and pressure, and a timestamp.

You can watch the messages arrive at your IoT hub by using the [Azure IoT Hub extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).

You can also view these messages through the [Azure Cloud Shell](https://shell.azure.com/):

   ```azurecli-interactive
   az iot hub monitor-events -n <iothub_name> -d <lower-layer-device-name>
   ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo.

Para eliminar los recursos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después seleccione **Grupos de recursos**.

2. Seleccione el nombre del grupo de recursos que contiene los recursos de prueba de IoT Edge. 

3. Revise la lista de los recursos contenidos en el grupo de recursos. Si desea eliminar todos ellos, puede seleccionar **Eliminar grupo de recursos**. Si desea eliminar solo algunos de ellos, puede hacer clic en cada recurso para eliminarlos individualmente. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, configuró dos dispositivos IoT Edge como puertas de enlace y estableció uno como el dispositivo primario del otro. Después, ha mostrado la extracción de una imagen de contenedor en el dispositivo secundario a través de una puerta de enlace. También puede probar este escenario siguiendo el [ejemplo con scripts de Azure IoT Edge para el IoT industrial](https://aka.ms/iotedge-nested-sample), que implementa máquinas virtuales de Azure como dispositivos preconfigurados para simular un entorno de fábrica.

Para ver cómo Azure IoT Edge puede crear más soluciones para la empresa, siga con los otros tutoriales.

> [!div class="nextstepaction"]
> [Implementación de un modelo de Azure Machine Learning como módulo](tutorial-deploy-machine-learning.md)