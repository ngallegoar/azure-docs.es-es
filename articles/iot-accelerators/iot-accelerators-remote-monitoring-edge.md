---
title: Tutorial de la detección de anomalías en el perímetro en una solución de Azure | Microsoft Docs
description: En este tutorial aprenderá a supervisar los dispositivos de IoT Edge mediante el acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/08/2018
ms.topic: tutorial
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: f5ffc7472bb0744d76b5ceaacc95f74af22e762e
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92739804"
---
# <a name="tutorial-detect-anomalies-at-the-edge-with-the-remote-monitoring-solution-accelerator"></a>Tutorial: Detección de anomalías mediante IoT Edge con el acelerador de soluciones de supervisión remota

En este tutorial aprenderá a configurar la solución de supervisión remota para dar respuesta a las anomalías que detecta un dispositivo IoT Edge. Los dispositivos IoT Edge le permiten procesar datos de telemetría en el perímetro para reducir el volumen de telemetría que se envía a la solución y para permitir una respuesta más rápida a los eventos de los dispositivos. Para más información sobre las ventajas del procesamiento con IoT Edge, consulte [¿Qué es Azure IoT Edge?](../iot-edge/about-iot-edge.md).

Para presentar el procesamiento perimetral con la solución remota, en este tutorial se usa un dispositivo simulado de bomba de petróleo. La bomba de petróleo la administra una organización denominada Contoso y está conectada al acelerador de soluciones de supervisión remota. Los sensores de la bomba de petróleo miden la temperatura y la presión. Los trabajadores de Contoso saben que un aumento anómalo de la temperatura puede hacer que el bombeo de petróleo se ralentice. Los trabajadores de Contoso no necesitan supervisar la temperatura del dispositivo si esta se encuentra dentro de su intervalo normal.

Contoso desea implementar un módulo de inteligencia perimetral en la bomba de petróleo que detecte anomalías de temperatura. Otro módulo perimetral envía alertas a la solución de supervisión remota. Si se recibe una alerta, un trabajador de Contoso puede enviar a un técnico de mantenimiento. Contoso puede configurar también una acción automatizada como, por ejemplo, enviar un correo electrónico, que se ejecute cuando la solución reciba una alerta.

El diagrama siguiente muestra los componentes clave en el escenario del tutorial:

![En el diagrama se muestra el conector de la bomba de aceite conectado al módulo de Azure Stream Analytics en IoT Edge en el dispositivo IoT Edge para datos de telemetría y comandos. Los datos de telemetría filtrados van al dispositivo IoT Edge en el acelerador de la solución de supervisión remota en la nube. La nube también contiene la implementación y el paquete. La implementación implementa el entorno de ejecución de IoT Edge en el dispositivo.](media/iot-accelerators-remote-monitoring-edge/overview.png)

En este tutorial, hizo lo siguiente:

>[!div class="checklist"]
> * Agregar un dispositivo IoT Edge a la solución
> * Crear un manifiesto de IoT Edge
> * Importar el manifiesto como un paquete que define los módulos que se van a ejecutar en el dispositivo
> * Implementar el paquete en el dispositivo IoT Edge
> * Ver las alertas del dispositivo

En el dispositivo IoT Edge:

* El entorno de ejecución recibe el paquete e instala los módulos.
* El módulo de Stream Analytics detecta anomalías de temperatura en la bomba y envía comandos para resolver el problema.
* El módulo de Stream Analytics reenvía los datos filtrados al acelerador de soluciones.

Este tutorial usa una máquina virtual Linux como dispositivo IoT Edge. También se instala un módulo Edge para simular el dispositivo de bomba de petróleo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="add-an-iot-edge-device"></a>Incorporación de un dispositivo de IoT Edge

Se necesitan dos pasos para agregar un dispositivo IoT Edge al acelerador de soluciones de supervisión remota. Esta sección le indica cómo:

* Agregar un dispositivo IoT Edge en la página **Device Explorer** de la interfaz de usuario web de supervisión remota.
* Instalar el entorno de ejecución de IoT Edge en una máquina virtual Linux.

### <a name="add-an-iot-edge-device-to-your-solution"></a>Agregar un dispositivo IoT Edge a la solución

Para agregar un dispositivo IoT Edge al acelerador de soluciones de supervisión remota, vaya a la página **Device Explorer** de la interfaz de usuario web y haga clic en **+ Nuevo dispositivo** .

En el panel **Nuevo dispositivo** , elija **Dispositivo IoT Edge** y escriba **oil-pump** como identificador de dispositivo. Puede dejar los valores predeterminados para las demás opciones de configuración. A continuación, haga clic en **Aplicar** :

[![Adición de un dispositivo IoT Edge](./media/iot-accelerators-remote-monitoring-edge/addedgedevice-inline.png)](./media/iot-accelerators-remote-monitoring-edge/addedgedevice-expanded.png#lightbox)

Tome nota de la cadena de conexión del dispositivo; la necesitará en la siguiente sección de este tutorial.

Al registrar un dispositivo con el centro de IoT en el acelerador de soluciones de supervisión remota, este aparece en la lista **Device Explorer** de la interfaz de usuario web:

[![Nuevo dispositivo IoT Edge](./media/iot-accelerators-remote-monitoring-edge/newedgedevice-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newedgedevice-expanded.png#lightbox)

Para facilitar la administración de dispositivos IoT Edge en la solución, cree un grupo de dispositivos y agregue el dispositivo IoT Edge:

1. Seleccione el dispositivo **oil-pump** en la lista de la página **Device Explorer** y, a continuación, haga clic en **Trabajos** .

1. Cree un trabajo para agregar la etiqueta **IsEdge** al dispositivo mediante la siguiente configuración:

    | Configuración | Valor |
    | ------- | ----- |
    | Trabajo     | Etiquetas  |
    | Nombre del trabajo | AddEdgeTag |
    | Clave     | IsOilPump |
    | Valor   | Y     |
    | Tipo    | Texto  |

    [![Agregar etiqueta](./media/iot-accelerators-remote-monitoring-edge/addtag-inline.png)](./media/iot-accelerators-remote-monitoring-edge/addtag-expanded.png#lightbox)

1. Haga clic en **Aplicar** y en **Cerrar** .

1. En la página **Device Explorer** , haga clic en **Administrar grupos de dispositivos** .

1. Haga clic en **Crear un nuevo grupo de dispositivos** . Cree un nuevo grupo de dispositivos con la siguiente configuración:

    | Configuración | Valor |
    | ------- | ----- |
    | Nombre    | OilPumps |
    | Campo   | Tags.IsOilPump |
    | Operator | = Equals |
    | Valor    | Y |
    | Tipo     | Texto |

    [![Crear grupo de dispositivos](./media/iot-accelerators-remote-monitoring-edge/createdevicegroup-inline.png)](./media/iot-accelerators-remote-monitoring-edge/createdevicegroup-expanded.png#lightbox)

1. Haga clic en **Save** (Guardar).

El dispositivo IoT Edge ya está en el grupo **OilPumps** .

### <a name="install-the-edge-runtime"></a>Instalación del entorno de ejecución de IoT Edge

Un dispositivo Edge requiere la instalación del entorno de ejecución de IoT Edge. En este tutorial va a instalar este entorno en una máquina virtual Linux de Azure para probar el escenario. Los pasos siguientes usan Azure Cloud Shell en la instalación y configuración de la máquina virtual:

1. Para crear una máquina virtual Linux en Azure, ejecute los comandos siguientes. Puede usar una ubicación cercana a la que se encuentre:

    ```azurecli-interactive
    az group create \
      --name IoTEdgeDevices \
      --location eastus
    az vm create \
      --resource-group IoTEdgeDevices \
      --name EdgeVM \
      --image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest \
      --admin-username azureuser \
      --generate-ssh-keys \
      --size Standard_B1ms
    ```

1. Para configurar el tiempo de ejecución de Edge con la cadena de conexión del dispositivo, ejecute el comando siguiente con la cadena de conexión de dispositivo que anotó anteriormente:

    ```azurecli-interactive
    az vm run-command invoke \
      --resource-group IoTEdgeDevices \
      --name EdgeVM \
      --command-id RunShellScript \
      --scripts 'sudo /etc/iotedge/configedge.sh "YOUR_DEVICE_CONNECTION_STRING"'
    ```

    No olvide incluir la cadena de conexión entre comillas dobles.

Con ello ya ha instalado y configurado el entorno de ejecución de Azure IoT Edge en un dispositivo Linux. Más adelante en este tutorial, usará la solución de supervisión remota para implementar módulos de IoT Edge en este dispositivo.

## <a name="create-an-edge-manifest"></a>Crear un manifiesto de IoT Edge

Para simular el dispositivo de bomba de petróleo, deberá agregar los módulos siguientes al dispositivo Edge:

* Módulo de simulación de temperatura.
* Detección de anomalías en Azure Stream Analytics.

Los pasos siguientes muestran cómo crear un manifiesto de implementación de Edge que incluye estos módulos. Más adelante en este tutorial, importará este manifiesto como un paquete en el acelerador de soluciones de supervisión remota.

### <a name="create-the-azure-stream-analytics-job"></a>Creación del trabajo de Azure Stream Analytics

Puede definir el trabajo de Stream Analytics en el portal antes de empaquetarlo como un módulo de Edge.

1. En Azure Portal, cree una cuenta de almacenamiento de Azure con las opciones predeterminadas en el grupo de recursos **IoTEdgeDevices** . Anote el nombre que eligió.

1. En Azure Portal, cree un **trabajo de Stream Analytics** en el grupo de recursos **IoTEdgeDevices** . Use los siguientes valores de configuración:

    | Opción | Value |
    | ------ | ----- |
    | Nombre del trabajo | EdgeDeviceJob |
    | Subscription | Su suscripción de Azure |
    | Resource group | IoTEdgeDevices |
    | Location | Este de EE. UU. |
    | Entorno de hospedaje | Borde |
    | Unidades de streaming | 1 |

1. Abra el trabajo **EdgeDeviceJob** de Stream Analytics en el portal, haga clic en Entradas, y agregue una entrada de flujo de **Edge Hub** denominada **telemetría** .

1. En el trabajo **EdgeDeviceJob** de Stream Analytics en el portal, haga clic en **Salidas** y agregue una salida de **Edge Hub** denominada **salida** .

1. En el trabajo **EdgeDeviceJob** de Stream Analytics en el portal, haga clic en **Salidas** y agregue una segunda salida de **Edge Hub** denominada **alerta** .

1. En el trabajo **EdgeDeviceJob** de Stream Analytics en el portal, haga clic en **Consulta** y agregue la siguiente instrucción **select** :

    ```sql
    SELECT  
    avg(machine.temperature) as temperature, 
    'F' as temperatureUnit 
    INTO output 
    FROM telemetry TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second, 5) 
    SELECT 'reset' as command 
    INTO alert 
    FROM telemetry TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second, 3) 
    HAVING avg(machine.temperature) > 400
    ```

1. En el trabajo **EdgeDeviceJob** de Stream Analytics en el portal, haga clic en **Configuración de cuenta de almacenamiento** . Agregue la cuenta de almacenamiento que agregó al grupo de recursos **IoTEdgeDevices** al comienzo de esta sección. Cree un nuevo contenedor denominado **edgeconfig** .

La siguiente captura de pantalla muestra el trabajo de Stream Analytics guardado:

[![Trabajo de Stream Analytics](./media/iot-accelerators-remote-monitoring-edge/streamjob-inline.png)](./media/iot-accelerators-remote-monitoring-edge/streamjob-expanded.png#lightbox)

Ahora ya ha definido un trabajo de Stream Analytics para ejecutarlo en el dispositivo Edge. El trabajo calcula la temperatura media a lo largo de una ventana de 5 segundos. El trabajo también envía una alerta si la temperatura media en una ventana de 3 segundos es superior a 400.

### <a name="create-the-iot-edge-deployment"></a>Creación de la implementación de IoT Edge

A continuación, puede crear un manifiesto de implementación de IoT Edge que defina los módulos que se van a ejecutar en el dispositivo Edge. En la sección siguiente, importará este manifiesto como un paquete en la solución de supervisión remota.

1. En Azure Portal, vaya al centro de IoT de la solución de supervisión remota. Puede encontrar el centro de IoT en el grupo de recursos que tiene el mismo nombre que la solución de supervisión remota.

1. En el centro de IoT, haga clic en **IoT Edge** en la sección **Administración automática de dispositivos** . Haga clic en **Agregar una implementación de IoT Edge** .

1. En la página **Crear implementación > Nombre y etiqueta** , escriba el nombre **oil-pump-device** . Haga clic en **Next** .

1. En la página **Crear implementación > Agregar módulos** , haga clic en **+ Agregar** . Elija **Módulo IoT Edge** .

1. En el panel **Módulos personalizados de IoT Edge** , escriba **temperatureSensor** como nombre y **asaedgedockerhubtest/asa-edge-test-module:sensor-ad-linux-amd64** como identificador URI de la imagen. Haga clic en **Save** (Guardar).

1. En la página **Crear implementación > Agregar módulos** , haga clic en **+ Agregar** para agregar un segundo módulo. Seleccione **Módulo de Azure Stream Analytics** .

1. En el panel **Implementación de Edge** , seleccione la suscripción y el trabajo **EdgeDeviceJob** que creó en la sección anterior. Haga clic en **Save** (Guardar).

1. En la página **Crear implementación > Agregar módulos** , haga clic en **Siguiente** .

1. En la página **Crear implementación > Especificar rutas** , agregue el código siguiente:

    ```sql
    {
      "routes": { 
        "alertsToReset": "FROM /messages/modules/EdgeDeviceJob/outputs/alert INTO BrokeredEndpoint(\"/modules/temperatureSensor/inputs/control\")", 
        "telemetryToAsa": "FROM /messages/modules/temperatureSensor/* INTO BrokeredEndpoint(\"/modules/EdgeDeviceJob/inputs/telemetry\")", 
        "ASAToCloud": "FROM /messages/modules/EdgeDeviceJob/* INTO $upstream" 
      } 
    }
    ```

    Este código enruta la salida del módulo de Stream Analytics a las ubicaciones correctas.

    Haga clic en **Next** .

1. En la página **Crear implementación > Especificar métricas** , haga clic en **Siguiente** .

1. En la página **Crear implementación > Dispositivos de destino** , escriba 10 como la prioridad. Haga clic en **Next** .

1. En la página **Crear implementación > Revisar la implementación** , haga clic en **Enviar** :

    [![Revisión de la implementación](./media/iot-accelerators-remote-monitoring-edge/reviewdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/reviewdeployment-expanded.png#lightbox)

1. En la página principal de **IoT Edge** , haga clic en **Implementaciones de IoT Edge** . Puede ver que **oil-pump-device** está en la lista de implementaciones.

1. Haga clic en la implementación **oil-pump-device** y, después, en **Descargar el manifiesto de IoT Edge** . Guarde el archivo como **oil-pump-device.json** en una ubicación adecuada en la máquina local. Lo necesitará en la sección siguiente de este tutorial.

Ahora ya ha creado un manifiesto de IoT Edge para importar en la solución de supervisión remota como un paquete. Normalmente, un desarrollador crea los módulos de IoT Edge y el archivo de manifiesto.

## <a name="import-a-package"></a>Importación de un paquete

En esta sección, va a importar el manifiesto de Edge como un paquete en la solución de supervisión remota.

1. En la interfaz de usuario web de supervisión remota, vaya a la página **Paquetes** y haga clic en **+ Nuevo paquete** :

    [![Nuevo paquete](./media/iot-accelerators-remote-monitoring-edge/newpackage-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newpackage-expanded.png#lightbox)

1. En el panel **Nuevo paquete** , seleccione **Manifiesto de Edge** como tipo de paquete, haga clic en **Examinar** para buscar el archivo **oil-pump-device.json** en la máquina local y haga clic en **Cargar** :

    [![Carga del paquete](./media/iot-accelerators-remote-monitoring-edge/uploadpackage-inline.png)](./media/iot-accelerators-remote-monitoring-edge/uploadpackage-expanded.png#lightbox)

    La lista de paquetes incluye ahora el paquete **oil-pump-device.json** .

En la siguiente sección va a crear una implementación que aplicará el paquete al dispositivo Edge.

## <a name="deploy-a-package"></a>Implementación de un paquete

Ahora ya está listo para implementar el paquete en el dispositivo.

1. En la interfaz de usuario web de supervisión remota, vaya a la página **Implementaciones** y haga clic en **+ Nueva implementación** :

    [![Nueva implementación](./media/iot-accelerators-remote-monitoring-edge/newdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newdeployment-expanded.png#lightbox)

1. En el panel **Nueva implementación** , cree una implementación con la siguiente configuración:

    | Opción | Valor |
    | ------ | ----- |
    | Nombre   | OilPumpDevices |
    | Tipo de paquete | Manifiesto de Edge |
    | Paquete | oil-pump-device.json |
    | Grupo de dispositivos | OilPumps |
    | Priority | 10 |

    [![Crear implementación](./media/iot-accelerators-remote-monitoring-edge/createdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/createdeployment-expanded.png#lightbox)

    Haga clic en **Aplicar** .

Deberá esperar unos minutos para que el paquete se implemente en el dispositivo y para que los datos de telemetría empiecen a fluir desde este.

[![Implementación activa](./media/iot-accelerators-remote-monitoring-edge/deploymentactive-inline.png)](./media/iot-accelerators-remote-monitoring-edge/deploymentactive-expanded.png#lightbox)

La página **Implementaciones** muestra las métricas siguientes:

* **Objetivo** muestra el número de dispositivos en el grupo de dispositivos.
* **Aplicado** muestra el número de dispositivos en los que se ha aplicado el contenido de la implementación.
* **Correctos** : el número de dispositivos correctos de Edge en el informe de implementación del entorno de ejecución del cliente de IoT Edge.
* **Con error** : el número de dispositivos con error de Edge en el informe de implementación del entorno de ejecución del cliente de IoT Edge.

## <a name="monitor-the-device"></a>Supervisión del dispositivo

Puede ver los datos de telemetría de temperatura del dispositivo de bomba de petróleo en la interfaz de usuario web de supervisión remota:

1. Vaya a la página **Device Explorer** y seleccione el dispositivo de la bomba de petróleo.
1. En la sección **Telemetría** del panel **Detalles del dispositivo** , haga clic en **Temperatura** :

    [![Ver datos de telemetría](./media/iot-accelerators-remote-monitoring-edge/viewtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-edge/viewtelemetry-expanded.png#lightbox)

Puede ver cómo la temperatura aumenta hasta que alcanza un umbral. El módulo de Edge de Stream Analytics detecta cuando la temperatura alcanza este umbral y envía un comando al dispositivo para reducir la temperatura inmediatamente. Todo este proceso se realiza en el dispositivo sin comunicarse con la nube.

Si desea notificar a los operadores cuando se alcance el umbral, puede crear una regla en la interfaz de usuario web de supervisión remota:

1. Vaya a la página **Reglas** y haga clic en **+ Nueva regla** .
1. Cree una nueva regla con la siguiente configuración:

    | Opción | Valor |
    | ------ | ----- |
    | Nombre de la regla | Temperatura de la bomba de petróleo |
    | Descripción | La temperatura de la bomba de petróleo es superior a 300 |
    | Grupo de dispositivos | OilPumps |
    | Cálculo | Instantánea |
    | Campo | temperatura |
    | Operator | > |
    | Valor | 300 |
    | Nivel de gravedad | Información |

    [![Crear regla](./media/iot-accelerators-remote-monitoring-edge/newrule-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newrule-expanded.png#lightbox)

    Haga clic en **Aplicar** .

1. Vaya a la página **Panel** . Aparece una alerta en el panel **Alertas** cuando la temperatura del dispositivo **oil-pump** es superior a 300.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se muestra cómo agregar y configurar un dispositivo de IoT Edge en el acelerador de soluciones de supervisión remota. Para más información sobre cómo trabajar con paquetes de IoT Edge en la solución de supervisión remota, consulte la siguiente guía de procedimientos:

> [!div class="nextstepaction"]
> [Importación de un paquete de IoT Edge en el acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-import-edge-package.md)

Para más información acerca de cómo instalar el entorno de ejecución de IoT Edge, consulte [Instalar el entorno de ejecución de Azure IoT Edge en Linux (x64)](../iot-edge/how-to-install-iot-edge-linux.md).

Para más información acerca de Azure Stream Analytics en dispositivos de Edge, consulte [Implementación de Azure Stream Analytics como un módulo de IoT Edge](../iot-edge/tutorial-deploy-stream-analytics.md).
