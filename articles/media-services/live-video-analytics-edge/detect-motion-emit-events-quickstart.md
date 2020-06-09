---
title: 'Detección de movimiento y emisión de eventos: Azure'
description: En este inicio rápido se muestra cómo usar Live Video Analytics en IoT Edge para detectar movimiento y emitir eventos con una llamada mediante programación a métodos directos.
ms.topic: quickstart
ms.date: 05/29/2020
ms.openlocfilehash: 4986ea13bec5382a8e0ef791e75442e4333e4356
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261592"
---
# <a name="quickstart-detect-motion-and-emit-events"></a>Inicio rápido: Detección de movimiento y emisión de eventos

Este inicio rápido le guiará por los pasos necesarios para empezar a usar Live Video Analytics en IoT Edge. Usa una máquina virtual de Azure como un dispositivo IoT Edge y una secuencia de vídeo en directo simulada. Después de completar los pasos de configuración, podrá ejecutar una secuencia de vídeo en directo simulada mediante un grafo de elementos multimedia que detecta e informa de cualquier movimiento en esa secuencia. El diagrama siguiente muestra una representación gráfica de ese grafo de elementos multimedia.

![Flujo de Live Video Analytics basado en la detección de movimiento](./media/analyze-live-video/motion-detection.png) 

Este artículo se basa en un [ejemplo de código](https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp) escrito en C#.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Visual Studio Code](https://code.visualstudio.com/) en la máquina con las siguientes extensiones:
    1. [Herramientas de Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    2. [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [SDK de .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1) instalado en el sistema

> [!TIP]
> Al instalar la extensión de Azure IoT Tools, es posible que se le pida que instale Docker. Si lo desea, puede ignorar este aviso.

## <a name="set-up-azure-resources"></a>Configuración de los recursos de Azure

Este tutorial requiere los siguientes servicios de Azure.

* IoT Hub
* Cuenta de almacenamiento
* Cuenta de Azure Media Services
* Máquina virtual Linux en Azure, con el [entorno de ejecución de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) instalado

Para este inicio rápido, se recomienda usar el [script de configuración de recursos de Live Video Analytics](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) para implementar los recursos de Azure mencionados anteriormente en su suscripción de Azure. Para ello, siga estos pasos:

1. Vaya a https://shell.azure.com.
1. Si es la primera vez que usa Cloud Shell, se le pedirá que seleccione una suscripción para crear una cuenta de almacenamiento y recursos compartidos de Microsoft Azure Files. Seleccione "Crear almacenamiento" para crear una cuenta de almacenamiento para almacenar la información de la sesión de Cloud Shell. Esta cuenta de almacenamiento es independiente de la que creará el script para usarla con su cuenta de Azure Media Services.
1. Seleccione "Bash" como su entorno en la lista desplegable del lado izquierdo de la ventana del shell.

    ![Selector de entorno](./media/quickstarts/env-selector.png)

1. Ejecute el siguiente comando.

    ```
    bash -c "$(curl -sL https://aka.ms/lva-edge/setup-resources-for-samples)"
    ```

    Si el script se completa correctamente, debería ver todos los recursos mencionados anteriormente en su suscripción.

1. Una vez finalizado el script, haga clic en las llaves para exponer la estructura de carpetas. Verá que se han creado algunos archivos en el directorio ~/clouddrive/lva-sample. Archivos de interés en este inicio rápido:

     * ~/clouddrive/lva-sample/edge-deployment/.env: contiene las propiedades que utiliza Visual Studio Code para implementar módulos en un dispositivo IoT Edge.
     * ~/clouddrive/lva-sample/appsetting.json: lo utiliza Visual Studio Code para ejecutar el código de ejemplo.
     
Los necesitará para actualizar los archivos en Visual Studio Code más tarde en este inicio rápido. Es posible que desee copiarlos en un archivo local por el momento.


 ![Configuración de la aplicación](./media/quickstarts/clouddrive.png)

## <a name="set-up-your-development-environment"></a>Configurado su entorno de desarrollo

1. Clone el repositorio desde aquí https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp.
1. Inicie Visual Studio Code y abra la carpeta donde descargó el repositorio.
1. En Visual Studio Code, vaya a la carpeta "src/cloud-to-device-console-app" y cree un archivo llamado "appsettings.json". Este archivo contendrá la configuración necesaria para ejecutar el programa.
1. Copie el contenido del archivo ~/clouddrive/lva-sample/appsettings.json generado en la sección anterior (consulte el paso 5).

    El texto debe tener el siguiente aspecto:

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. A continuación, vaya a la carpeta "src/edge" y cree un archivo llamado ".env".
1. Copie el contenido del archivo "/clouddrive/lva-sample/edge-deployment/.env". El texto debe tener el siguiente aspecto:

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    INPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    OUTPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry username>"      
    ```

## <a name="examine-the-sample-files"></a>Examen de los archivos de ejemplo

1. En Visual Studio Code, vaya a "src/edge". Verá el archivo .env que ha creado junto con algunos archivos de plantilla de implementación.

    La plantilla de implementación hace referencia al manifiesto de implementación del dispositivo perimetral con algunos valores de marcador de posición. El archivo .env contiene los valores de esas variables.
1. A continuación, vaya a la carpeta "src/cloud-to-device-console-app". Aquí podrá ver el archivo appsettings.json que creó junto con algunos otros archivos:

    * c2d-console-app.csproj: archivo de proyecto para Visual Studio Code.
    * operations.json: este archivo muestra las distintas operaciones que desea que ejecute el programa.
    * Program.cs: código del programa de ejemplo, que hace lo siguiente:
    
        * Carga la configuración de la aplicación.
        * Invoca los métodos directos que expone el módulo Live Video Analytics en IoT Edge. Puede usar el módulo para analizar secuencias de vídeo en directo mediante la invocación de sus [métodos directos](direct-methods.md). 
        * Se pone en pausa para que pueda examinar la salida del programa en la ventana de terminal y los eventos generados por el módulo en la ventana de salida.
        * Invoca los métodos directos para la limpieza de los recursos.   

## <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>Generación e implementación del manifiesto de implementación de IoT Edge

El manifiesto de implementación define los módulos que se implementan en un dispositivo IoT Edge y los valores de configuración de dichos módulos. Siga estos pasos para generar un manifiesto de este tipo a partir del archivo de plantilla y, a continuación, impleméntelo en el dispositivo IoT Edge.

1. Abra Visual Studio Code.
1. Establezca la cadena de conexión de IoT Hub; para ello, haga clic en el icono "More actions" (Más acciones) situado junto al panel AZURE IOT HUB en la esquina inferior izquierda. Puede copiar la cadena del archivo src/cloud-to-device-console-app/appsettings.json. 

    ![Establecimiento de la cadena de conexión de IoT](./media/quickstarts/set-iotconnection-string.png)
1. A continuación, haga clic con el botón derecho en "src/edge/deployment.template.json" y haga clic en "Generate IoT Edge Deployment Manifest" (Generar manifiesto de implementación de IoT Edge).
    ![Generación de un manifiesto de implementación de IoT Edge](./media/quickstarts/generate-iot-edge-deployment-manifest.png)

    Esto debe crear un archivo de manifiesto en la carpeta src/edge/config llamado "deployment.amd64.json".
1. Haga clic con el botón derecho en "src/edge/config/deployment.amd64.json", haga clic en "Create Deployment for Single Device" (Crear una implementación para un dispositivo individual) y seleccione el nombre del dispositivo perimetral.

    ![Crear una implementación para un dispositivo individual](./media/quickstarts/create-deployment-single-device.png)
1. A continuación, aparecerá el mensaje "Select an IoT Hub device" (Seleccione un dispositivo IoT Hub). Seleccione lva-sample-device en la lista desplegable.
1. En unos 30 segundos, actualice la instancia de Azure IoT Hub en la sección inferior izquierda y debería ver que el dispositivo IoT Edge tiene implementados los siguientes módulos:

    * Live Video Analytics en IoT Edge (nombre de módulo "lvaEdge")
    * Simulador RTSP (nombre de módulo "rtspsim")

El módulo del simulador RTSP simula una secuencia de vídeo en directo mediante un archivo de vídeo almacenado que se copió en el dispositivo perimetral cuando ejecutó el [script de configuración de los recursos de Live Video Analytics](https://github.com/Azure/live-video-analytics/tree/master/edge/setup). En esta fase, ha implementado los módulos pero no hay grafos de elementos multimedia activos.

## <a name="prepare-for-monitoring-events"></a>Preparación de la supervisión de eventos

Va a utilizar el módulo de Live Video Analytics en IoT Edge para detectar el movimiento en la secuencia de vídeo en directo entrante y enviar eventos a IoT Hub. Para ver estos eventos, siga estos pasos:

1. Abra el panel del explorador en Visual Studio Code y busque Azure IoT Hub en la esquina inferior izquierda.
1. Expanda el nodo Devices (Dispositivos).
1. Haga clic con el botón derecho en Iva-sample-device y elija la opción "Iniciar la supervisión del punto de conexión de eventos integrado".

    ![Iniciar la supervisión del punto de conexión de eventos integrado](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-sample-program"></a>Ejecución del programa de ejemplo

Siga los pasos siguientes para ejecutar el código de ejemplo.
1. En Visual Studio Code, vaya a "src/cloud-to-device-console-app/operations.json".
1. En el nodo GraphTopologySet, compruebe lo siguiente:

    ` "topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/motion-detection/topology.json"`
1. A continuación, en los nodos GraphInstanceSet y GraphTopologyDelete, asegúrese de que el valor de topologyName coincide con el valor de la propiedad "name" en la topología de grafo anterior:

    `"topologyName" : "MotionDetection"`
    
1. Inicie una sesión de depuración (presione F5). Empezará a ver algunos mensajes impresos en la ventana de terminal.
1. El archivo operations.json comienza con llamadas a GraphTopologyList y GraphInstanceList. Si ha limpiado los recursos después de los inicios rápidos anteriores, devolverá listas vacías y, a continuación, entrará en pausa para que presione Entrar.

    ```
    --------------------------------------------------------------------------
    Executing operation GraphTopologyList
    -----------------------  Request: GraphTopologyList  --------------------------------------------------
    {
        "@apiVersion": "1.0"
    }
    ---------------  Response: GraphTopologyList - Status: 200  ---------------
    {
        "value": []
    }
    --------------------------------------------------------------------------
    Executing operation WaitForInput
    Press Enter to continue
    ```
1. Al presionar la tecla "Entrar" en la ventana de terminal, se realiza el siguiente conjunto de llamadas de método directo
     
     * Una llamada a GraphTopologySet con el elemento topologyUrl anterior
     * Una llamada a GraphInstanceSet con el siguiente cuerpo
     
     ```
     {
       "@apiVersion": "1.0",
       "name": "Sample-Graph",
       "properties": {
         "topologyName": "MotionDetection",
         "description": "Sample graph description",
         "parameters": [
           {
             "name": "rtspUrl",
             "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
           },
           {
             "name": "rtspUserName",
             "value": "testuser"
           },
           {
             "name": "rtspPassword",
             "value": "testpassword"
           }
         ]
       }
     }
     ```
     
     * Una llamada a GraphInstanceActivate para iniciar la instancia del grafo e iniciar el flujo de vídeo
     * Una segunda llamada a GraphInstanceList para mostrar que la instancia de grafo está ciertamente en ejecución
1. La salida de la ventana de TERMINAL se pondrá en pausa ahora con el mensaje "Press Enter to continue" (Presione Entrar para continuar). No presione "Entrar" en este momento. Puede desplazarse hacia arriba para ver las cargas de la respuesta JSON para los métodos directos que ha invocado.
1. Si ahora cambia a la ventana de salida de Visual Studio Code, verá los mensajes que se envían a IoT Hub mediante el módulo Live Video Analytics en IoT Edge.
     * Estos mensajes se describen en la sección siguiente.
1. El grafo de elementos multimedia seguirá en ejecución e imprimirá los resultados: el simulador RTSP mantendrá el bucle en el vídeo de origen. Para detener el grafo de elementos multimedia, vuelva a la ventana de terminal y presione "Entrar". Se realiza la siguiente serie de llamadas para la limpieza de recursos:
     * Una llamada a GraphInstanceDeactivate para desactivar la instancia del grafo.
     * Una llamada a GraphInstanceDelete para eliminar la instancia
     * Una llamada a GraphTopologyDelete para eliminar la topología
     * Una llamada final a GraphTopologyList para mostrar que ahora la lista está vacía

## <a name="interpret-results"></a>Interpretación de los resultados

Cuando se ejecuta el grafo de elementos multimedia, los resultados del nodo del procesador de detección del movimiento se envían mediante el nodo de receptor de IoT Hub a IoT Hub. Los mensajes que aparecen en la ventana de salida de Visual Studio Code contienen una sección "body" y una sección "applicationProperties". Para comprender lo que estas secciones representan, consulte [este artículo](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).

En los mensajes siguientes, se definen las propiedades de la aplicación y el contenido del cuerpo mediante el módulo Live Video Analytics.

## <a name="mediasession-established-event"></a>Evento MediaSessionEstablished

Cuando se crean instancias del grafo de elementos multimedia, el nodo de origen RTSP intenta conectarse al servidor RTSP que se ejecuta en el contenedor rtspsim-live555. Si se realiza correctamente, imprimirá este evento:

```
[IoTHubMonitor] [9:42:18 AM] Message received from [lvaedgesample/lvaEdge]:  
{  
"body": {
"sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 xxx.xxx.xxx.xxx\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets={SPS}\r\na=control:track1\r\n"  
},  
"applicationProperties": {  
    "dataVersion": "1.0",  
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",  
    "subject": "/graphInstances/GRAPHINSTANCENAMEHERE/sources/rtspSource",  
    "eventType": "Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished",  
    "eventTime": "2020-04-09T16:42:18.1280000Z"  
    }  
}
```

* El mensaje es un evento de diagnóstico, MediaSessionEstablished, que indica que el nodo de origen RTSP (sujeto) ha podido establecer la conexión con el simulador RTSP y comenzar a recibir una fuente en directo (simulada).
* La sección "subject" de applicationProperties hace referencia al nodo de la topología de grafo desde la que se generó el mensaje. En este caso, el mensaje se origina en el nodo de origen RTSP.
* "eventType" en applicationProperties indica que se trata de un evento de diagnóstico.
* "eventTime" indica la hora a la que se produjo el evento.
* "body" contiene datos sobre el evento de diagnóstico que, en este caso, es el detalle de [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol).


## <a name="motion-detection-event"></a>Evento Detección de movimiento

Cuando se detecta movimiento, el módulo de Live Video Analytics en Edge envía un evento de inferencia. El tipo se establece en "movimiento"para indicar que es un resultado procedente del procesador de detección del movimiento y eventTime le indica a qué hora (UTC) se produjo el movimiento. Aquí tiene un ejemplo:

```
  {  
  "body": {  
    "timestamp": 142843967343090,
    "inferences": [  
      {  
        "type": "motion",  
        "motion": {  
          "box": {  
            "l": 0.573222,  
            "t": 0.492537,  
            "w": 0.141667,  
            "h": 0.074074  
          }  
        }  
      }  
    ]  
  },  
  "applicationProperties": {  
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",  
    "subject": "/graphInstances/GRAPHINSTANCENAME/processors/md",  
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",  
    "eventTime": "2020-04-17T20:26:32.7010000Z",
    "dataVersion": "1.0"  
  }  
}  
```

* La sección "subject" de applicationProperties hace referencia al nodo del grafo de elementos multimedia desde el que se generó el mensaje. En este caso, el mensaje se origina en el nodo del procesador de detección del movimiento.
* "eventType" en applicationProperties indica que se trata de un evento de análisis.
* "eventTime" indica la hora a la que se produjo el evento.
"body" contiene datos sobre el evento de análisis. En este caso, el evento es un evento de inferencia y, por lo tanto, el cuerpo contiene datos de "marca de tiempo" e "inferencias".
* Los datos de "inferences" indican que el "tipo" es "movimiento" y contienen datos adicionales sobre el evento "movimiento".
* La sección "box" contiene las coordenadas de un rectángulo delimitador alrededor del objeto en movimiento. Los valores se normalizan según el ancho y el alto del vídeo en píxeles (por ejemplo, ancho de 1920 y alto de 1080).

    ```
    l - distance from left of image
    t - distance from top of image
    w - width of bounding box
    h - height of bounding box
    ```
    
## <a name="cleanup-resources"></a>Limpieza de recursos

Si su intención es probar los demás inicios rápidos, debería conservar los recursos creados. En caso contrario, vaya a Azure Portal, vaya a los grupos de recursos, seleccione el grupo de recursos en el que ejecutó este inicio rápido y elimine todos los recursos.

## <a name="next-steps"></a>Pasos siguientes

Ejecute los demás inicios rápidos, como el de detección de un objeto, en una fuente de vídeo en directo.        
