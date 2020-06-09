---
title: 'Tutorial de grabación de vídeo basada en eventos en la nube y reproducción desde la nube: Azure'
description: En este tutorial, obtendrá información sobre cómo usar Live Video Analytics en IoT Edge para realizar una grabación de vídeo basada en eventos en la nube y la reproducción desde la nube.
ms.topic: tutorial
ms.date: 05/27/2020
ms.openlocfilehash: 92367634a2f5785ecbb102db1e03f3d5f12d744e
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300848"
---
# <a name="tutorial-event-based-video-recording-to-cloud-and-playback-from-cloud"></a>Tutorial: Grabación de vídeo basada en eventos en la nube y reproducción desde la nube

En este tutorial, aprenderá a usar Live Video Analytics en IoT Edge para grabar de forma selectiva partes de un origen de vídeo en directo en Media Services en la nube. En este tutorial, este caso de uso se conoce como [grabación de vídeo basada en eventos](event-based-video-recording-concept.md) (EVR). Para ello, se usará un modelo de inteligencia artificial de detección de objetos para buscar objetos en el vídeo y grabar clips de vídeo solo cuando se detecte un tipo determinado de objeto. También aprenderá cómo reproducir los clips de vídeo grabados mediante Media Services. Esto resulta útil en diversos escenarios en los que es necesario mantener un archivo de clips de vídeo de interés.

> [!div class="checklist"]
> * Configuración de los recursos correspondientes
> * Examen del código que realiza la EVR
> * Ejecución del código de ejemplo
> * Examen de los resultados y visualización del vídeo

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Sugerencias antes de la lectura  

Se recomienda leer las siguientes páginas de documentación:

* [Introducción a Live Video Analytics en IoT Edge](overview.md)
* [Terminología de Live Video Analytics en IoT Edge](terminology.md)
* [Concepto de grafo multimedia](media-graph-concept.md) 
* [Grabación de vídeo basada en eventos](event-based-video-recording-concept.md)
* [Tutorial: Desarrollo de módulos IoT Edge para dispositivos Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux)
* [Cómo editar deployment.*.template.json](https://github.com/microsoft/vscode-azure-iot-edge/wiki/How-to-edit-deployment.*.template.json)
* Sección [Declaración de rutas del manifiesto de implementación de IoT Edge](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes)

## <a name="prerequisites"></a>Requisitos previos

Los requisitos previos de este tutorial son los siguientes:

* [Visual Studio Code](https://code.visualstudio.com/) en la máquina de desarrollo con la extensión de [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) y la extensión de [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).

    > [!TIP]
    > Es posible que se le pida que instale Docker. Puede omitir este mensaje.
* [.NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/thank-you/sdk-3.1.201-windows-x64-installer) instalado en el equipo de desarrollo.
* Complete los pasos de [Script de configuración de los recursos de Live Video Analytics](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) y [Configuración del entorno](https://review.docs.microsoft.com/en-us/azure/media-services/live-video-analytics-edge/detect-motion-emit-events-quickstart?branch=release-preview-media-services-lva#set-up-the-environment).

Al final de los pasos anteriores, tendrá determinados recursos de Azure implementados en la suscripción de Azure, entre los que se incluyen:

* IoT Hub
* Cuenta de almacenamiento
* Cuenta de Azure Media Services
* Máquina virtual Linux en Azure, con el [entorno de ejecución de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) instalado

## <a name="concepts"></a>Conceptos

La grabación de vídeo basada en eventos (EVR) hace referencia al proceso de grabación de vídeo desencadenado por un evento. Ese evento podría generarse a partir del procesamiento de la propia señal de vídeo (por ejemplo, al detectar un objeto en movimiento en el vídeo) o desde un origen independiente (por ejemplo, la apertura de una puerta). Como alternativa, puede desencadenar la grabación solo cuando un servicio de inferencia detecta que se ha producido un evento específico.  En este tutorial se usará un vídeo de vehículos en movimiento en una autovía y se grabarán clips de vídeo cada vez que se detecta un camión.

![Grafo multimedia](./media/event-based-video-recording-tutorial/overview.png)

El diagrama anterior es una representación gráfica de un [grafo multimedia](media-graph-concept.md) y los módulos adicionales que llevan a cabo el escenario deseado. Hay cuatro módulos IoT Edge implicados:

* El módulo Live Video Analytics en IoT Edge.
* Un módulo IoT Edge que ejecuta un modelo de inteligencia artificial mediante un punto de conexión HTTP. Este módulo de inteligencia artificial utiliza el modelo [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx), que es capaz de detectar muchos tipos de objetos.
* Un módulo personalizado para filtrar y hacer un recuento de los objetos (llamado Object Counter en el diagrama anterior) que se va a crear e implementar en este tutorial.
* Un [módulo simulador RTSP](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) para simular una cámara RTSP.
    
    Como se muestra en el diagrama, se usará un nodo de [origen RTSP](media-graph-concept.md#rtsp-source) en el grafo multimedia para capturar el vídeo en directo simulado (de tráfico en una autopista) y enviar el vídeo a dos rutas de acceso.

* La primera ruta de acceso dirige a un nodo [procesador de filtros de velocidad de fotogramas](media-graph-concept.md#frame-rate-filter-processor) que genera fotogramas de vídeo con la velocidad de fotogramas especificada (reducida). Esos fotogramas de vídeo se envían a un nodo de extensión HTTP que, a su vez, transmite los fotogramas (como imágenes) al módulo de inteligencia artificial (YOLO v3, que es un detector de objetos) y recibe los resultados, que serán los objetos (vehículos en tráfico) detectados por el modelo. A continuación, el nodo de extensión HTTP publica los resultados mediante el nodo receptor de mensajes de IoT Hub en el centro de IoT Edge.
* El módulo del contador de objetos está configurado para recibir mensajes desde el centro de IoT Edge, que incluye los resultados de la detección de objetos (vehículos en tráfico). El módulo comprueba estos mensajes en busca de objetos de un tipo determinado (configurado mediante un valor). Cuando se encuentra un objeto de este tipo, este módulo envía un mensaje al centro de IoT Edge. A continuación, los mensajes de "objeto encontrado" se enrutan al nodo de origen de IoT Hub del grafo multimedia. Tras recibir este tipo de mensaje, el nodo de origen de IoT Hub del grafo multimedia desencadena el nodo del [procesador de la puerta de señales](media-graph-concept.md#signal-gate-processor), lo que hace que este último se abra durante un tiempo configurado. El vídeo fluye a través de la puerta al nodo receptor del recurso durante esa duración. Después, esa parte del streaming en vivo se graba mediante el nodo [receptor del recurso](media-graph-concept.md#asset-sink) en un [recurso](terminology.md#asset) de la cuenta de Azure Media Services.

## <a name="set-up-your-development-environment"></a>Configurado su entorno de desarrollo

Antes de comenzar, compruebe que ha completado el tercer punto de [Requisitos previos](#prerequisites). Una vez finalizado el script de configuración de recursos, haga clic en las llaves para exponer la estructura de carpetas. Verá que se han creado algunos archivos en el directorio ~/clouddrive/lva-sample.

![Configuración de la aplicación](./media/quickstarts/clouddrive.png)

Entre los que son de interés para este tutorial se incluyen:

* ~/clouddrive/lva-sample/edge-deployment/.env: contiene las propiedades que utiliza Visual Studio Code para implementar módulos en un dispositivo IoT Edge.
* ~/clouddrive/lva-sample/appsetting.json: lo utiliza Visual Studio Code para ejecutar el código de ejemplo.

Necesitará estos archivos para los pasos siguientes.

1. Clone el repositorio desde aquí https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp.
1. Inicie Visual Studio Code y abra la carpeta donde descargó el repositorio.
1. En Visual Studio Code, vaya a la carpeta "src/cloud-to-device-console-app" y cree un archivo llamado "appsettings.json". Este archivo contendrá la configuración necesaria para ejecutar el programa.
1. Copie el contenido del archivo ~/clouddrive/lva-sample/appsettings.json. El texto debe tener el siguiente aspecto:

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```

    La cadena de conexión de IoT Hub permite usar Visual Studio Code para enviar comandos a los módulos IoT Edge mediante Azure IoT Hub.
    
1. A continuación, vaya a la carpeta "src/edge" y cree un archivo llamado ".env".
1. Copie el contenido del archivo ~/clouddrive/lva-sample/.env. El texto debe tener el siguiente aspecto:

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    INPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    OUTPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/output"  
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry username>"      
    ```

## <a name="examine-the-template-file"></a>Examen del archivo de plantilla 

En el paso anterior, ha iniciado Visual Studio Code y ha abierto la carpeta que contiene el código de ejemplo.

En Visual Studio Code, vaya a "src/edge". Verá el archivo .env que ha creado, así como algunos archivos de plantilla de implementación. Esta plantilla define los módulos IoT Edge que se van a implementar en el dispositivo perimetral (la máquina virtual Linux de Azure). El archivo. env contiene valores para las variables que se usan en estas plantillas, como las credenciales de Media Services.

Abra "src/edge/deployment.objectCounter.template.json". Observe que hay cuatro entradas en la sección "modules", correspondientes a los elementos enumerados anteriormente (en la sección Conceptos):

* lvaEdge: módulo de Live Video Analytics en IoT Edge
* yolov3: módulo de inteligencia artificial creado con el modelo YOLO V3
* rtspsim: simulador RTSP
* objectCounter: módulo que busca objetos específicos en los resultados de yolov3

En el módulo objectCounter, observe la cadena (${MODULES.objectCounter}) que se usa para el valor "image", que se basa en el [tutorial](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux) sobre el desarrollo de un módulo IoT Edge. Visual Studio Code reconocerá automáticamente que el código del módulo del contador de objetos está en "src/edge/modules/objectCounter". 

Consulte [esta](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) sección sobre cómo declarar rutas en el manifiesto de implementación de IoT Edge y, a continuación, examine las rutas del archivo JSON de la plantilla. Observe que:

* LVAToObjectCounter se usa para enviar eventos específicos a un punto de conexión específico en el módulo objectCounter.
* ObjectCounterToLVA se usa para enviar un evento desencadenador a un punto de conexión específico (que debe ser el nodo de origen de IoT Hub) en el módulo lvaEdge.
* objectCounterToIoTHub se usa como herramienta de depuración para ayudarle a ver la salida de objectCounter al ejecutar este tutorial.

> [!NOTE]
> Compruebe las propiedades deseadas para el módulo objectCounter, que están configuradas para buscar objetos etiquetados como "camión", con un nivel de confianza de al menos el 50 %.

## <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>Generación e implementación del manifiesto de implementación de IoT Edge 

El manifiesto de implementación define los módulos que se implementan en un dispositivo IoT Edge y los valores de configuración de dichos módulos. Siga estos pasos para generar un manifiesto de este tipo a partir del archivo de plantilla y, a continuación, impleméntelo en el dispositivo IoT Edge.

Con Visual Studio Code, siga [estas](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux#build-and-push-your-solution) instrucciones para iniciar sesión en Docker y "compilar e insertar la solución IoT Edge", pero con src/edge/deployment.objectCounter.template.json para este paso.

![Compilación e inserción de la solución IoT Edge](./media/event-based-video-recording-tutorial/build-push.png)

Esto creará el módulo objectCounter para el recuento de objetos e insertará la imagen en la instancia de Azure Container Registry (ACR).

* Compruebe que ha definido las variables de entorno CONTAINER_REGISTRY_USERNAME_myacr y CONTAINER_REGISTRY_PASSWORD_myacr en el archivo .env.

El paso anterior creará el manifiesto de implementación de IoT Edge en src/edge/config/deployment.objectCounter.amd64.json. Haga clic con el botón derecho en el archivo y haga clic en "Create Deployment for Single Device" (Crear una implementación para un dispositivo individual).

![Crear una implementación para un dispositivo individual](./media/quickstarts/create-deployment-single-device.png)

Si este es su primer tutorial con Live Video Analytics en IoT Edge, Visual Studio Code le pedirá que especifique la cadena de conexión de IoT Hub. Puede copiarla desde el archivo appsettings.json.

A continuación, Visual Studio Code le pedirá que seleccione un dispositivo IoT Hub. Seleccione su dispositivo IoT Edge (debería ser "lva-sample-device").

En esta fase, se ha iniciado la implementación de los módulos perimetrales en el dispositivo IoT Edge.
En unos 30 segundos, actualice la instancia de Azure IoT Hub en la sección inferior izquierda de Visual Studio Code y verá que hay 4 módulos implementados (tenga en cuenta de nuevo los nombres: lvaEdge, rtspsim, yolov3 y objectCounter).

![4 módulos implementados](./media/event-based-video-recording-tutorial/iot-hub.png)

## <a name="prepare-for-monitoring-events"></a>Preparación de la supervisión de eventos

Para ver los eventos del módulo contador de objetos y del módulo Live Video Analytics en IoT Edge, siga estos pasos:

1. Abra el panel del explorador en Visual Studio Code y busque Azure IoT Hub en la esquina inferior izquierda.
1. Expanda el nodo Devices (Dispositivos).
1. Haga clic con el botón derecho en Iva-sample-device y elija la opción **Start Monitoring Built-in Event Endpoint** (Iniciar la supervisión del punto de conexión de eventos integrado).

![Iniciar la supervisión del punto de conexión de eventos integrado](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-program"></a>Ejecución del programa

1. En Visual Studio Code, vaya a "src/cloud-to-device-console-app/operations.json".

1. En el nodo GraphTopologySet, edite lo siguiente:

    `"topologyUrl" : "https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json"`
    
1. A continuación, en los nodos GraphInstanceSet y GraphTopologyDelete, edite:

    `"topologyName" : "EVRtoAssetsOnObjDetect"`
1. Inicie una sesión de depuración (presione F5). Empezará a ver algunos mensajes impresos en la ventana de terminal.

1. El archivo operations.json comienza con llamadas a GraphTopologyList y GraphInstanceList. Si ha limpiado los recursos después de los inicios rápidos o los tutoriales anteriores, devolverá listas vacías y, a continuación, entrará en pausa para que presione Entrar, como se muestra a continuación:

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
    1. Al presionar la tecla "Entrar" en la ventana de terminal, se realiza el siguiente conjunto de llamadas de método directo.
     * Una llamada a GraphTopologySet con el elemento topologyUrl anterior.
     * Una llamada a GraphInstanceSet con el siguiente cuerpo.
     
        ```
        {
          "@apiVersion": "1.0",
          "name": "Sample-Graph-1",
          "properties": {
            "topologyName": "EVRtoAssetsOnObjDetect",
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
    
     * Una llamada a GraphInstanceActivate para iniciar la instancia del grafo e iniciar el flujo de vídeo.
     * Una segunda llamada a GraphInstanceList para mostrar que la instancia del grafo está realmente en estado de ejecución.
     
1. La salida de la ventana de TERMINAL se pondrá en pausa ahora con el mensaje "Press Enter to continue" (Presione Entrar para continuar). No presione "Entrar" en este momento. Puede desplazarse hacia arriba para ver las cargas de la respuesta JSON para los métodos directos que ha invocado.

1. Si ahora cambia a la ventana de salida de Visual Studio Code, verá los mensajes que se envían a IoT Hub mediante el módulo Live Video Analytics en IoT Edge.

     * Estos mensajes se describen en la sección siguiente.
     
1. La instancia del grafo seguirá en ejecución y grabará el vídeo: el simulador RTSP mantendrá el bucle en el vídeo de origen. Revise los mensajes como se describe en la sección siguiente y, a continuación, para detener la instancia, vuelva a la ventana de terminal y presione "Entrar". Se realiza la siguiente serie de llamadas para la limpieza de recursos:

     * Una llamada a GraphInstanceDeactivate para desactivar la instancia del grafo.
     * Una llamada a GraphInstanceDelete para eliminar la instancia.
     * Una llamada a GraphTopologyDelete para eliminar la topología.
     * Una llamada final a GraphTopologyList para mostrar que ahora la lista está vacía.

## <a name="interpret-the-results"></a>Interpretación de los resultados 

Al ejecutar el grafo multimedia, el módulo Live Video Analytics en IoT Edge envía determinados eventos de diagnóstico y de funcionamiento al centro de IoT Edge. Estos eventos son los mensajes que aparecen en la ventana de salida de Visual Studio Code, los cuales contienen una sección "body" y una sección "applicationProperties". Para comprender lo que representan estas secciones, consulte [Creación y lectura de mensajes de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).

En los mensajes siguientes, se definen las propiedades de la aplicación y el contenido del cuerpo mediante el módulo Live Video Analytics.

## <a name="diagnostic-events"></a>Eventos de diagnóstico

### <a name="mediasessionestablished-event"></a>Evento MediaSessionEstablished 

Cuando se crean instancias del grafo multimedia, el nodo de origen RTSP intenta conectarse al servidor RTSP que se ejecuta en el contenedor del simulador RTSP. Si se realiza correctamente, imprimirá este evento. Observe que el tipo de evento es Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished.

```
[IoTHubMonitor] [5:53:17 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 XXX.XX.XX.XX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-05-17T17:53:16.981Z",
    "dataVersion": "1.0"
  }
}
```


* El mensaje es un evento de diagnóstico, MediaSessionEstablished, que indica que el nodo de origen RTSP (sujeto) ha podido establecer la conexión con el simulador RTSP y comenzar a recibir una fuente en directo (simulada).

* La sección "subject" de applicationProperties hace referencia al nodo de la topología de grafo desde la que se generó el mensaje. En este caso, el mensaje se origina en el nodo de origen RTSP.

* "eventType" en applicationProperties indica que se trata de un evento de diagnóstico.

* "eventTime" indica la hora en la que se produjo el evento, que es la hora en la que el vídeo de tráfico (archivo MKV) empezó a llegar al módulo como streaming en vivo.

* "body" contiene datos sobre el evento de diagnóstico que, en este caso, es el detalle de [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol).


## <a name="operational-events"></a>Eventos operativos

Después de que el grafo multimedia se ejecute durante un tiempo, podría recibir un evento del módulo del contador de objetos. 

```
[IoTHubMonitor] [5:53:44 PM] Message received from [lva-sample-device/objectCounter]:
{
  "body": {
    "count": 2
  },
  "applicationProperties": {
    "eventTime": "2020-05-17T17:53:44.062Z"
  }
}
```

El elemento applicationProperties contiene eventTime, que es la hora a la que el módulo del contador de objetos observó que los resultados del módulo YOLO v3 contenían objetos de interés (camiones).

Es posible que aparezcan más de estos eventos cuando se detecten otros camiones en el vídeo.

### <a name="recordingstarted-event"></a>Evento RecordingStarted

Casi inmediatamente después de que el contador de objetos envíe el evento, verá un evento de tipo Microsoft.Media.Graph.Operational.RecordingStarted.

```
[IoTHubMonitor] [5:53:46 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
    "eventTime": " 2020-05-17T17:53:46.132Z",
    "dataVersion": "1.0"
  }
}
```

La sección "subject" de applicationProperties hace referencia al nodo receptor del recurso del grafo que generó este mensaje. La sección "body" contiene información sobre la ubicación de salida, que en este caso es el nombre del recurso de Azure Media Services en el que se graba el vídeo. Anote este valor.

### <a name="recordingavailable-event"></a>Evento RecordingAvailable

Cuando el nodo receptor del recurso ha cargado el vídeo en el recurso, emite este evento de tipo Microsoft.Media.Graph.Operational.RecordingAvailable.

```
[IoTHubMonitor] [5:54:15 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingAvailable",
    "eventTime": "2020-05-17T17:54:15.808Z",
    "dataVersion": "1.0"
  }
}
```

Este evento indica que se han escrito suficientes datos en el recurso para que los reproductores y clientes inicien la reproducción del vídeo. La sección "subject" de applicationProperties hace referencia al nodo receptor del recurso del grafo que generó este mensaje. La sección "body" contiene información sobre la ubicación de salida, que en este caso es el nombre del recurso de Azure Media Services en el que se graba el vídeo.

### <a name="recordingstopped-event"></a>Evento RecordingStopped

Si examina la configuración de activación (maximumActivationTime) del nodo del procesador de la puerta de señales en la [topología](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json), verá que la puerta está configurada para cerrarse después de que se hayan enviado 30 segundos de vídeo. Por tanto, aproximadamente 30 segundos después del evento RecordingStarted, debería ver un evento de tipo Microsoft.Media.Graph.Operational.RecordingStopped, que indica que el nodo receptor del recurso ha detenido la grabación de vídeo en el recurso.

```
[IoTHubMonitor] [5:54:15 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStopped",
    "eventTime": "2020-05-17T17:54:15.040Z",
    "dataVersion": "1.0"
  }
}
```

Este evento indica que se ha detenido la grabación. La sección "subject" de applicationProperties hace referencia al nodo receptor del recurso del grafo que generó este mensaje. La sección "body" contiene información sobre la ubicación de salida, que en este caso es el nombre del recurso de Azure Media Services en el que se graba el vídeo.

## <a name="media-services-asset"></a>Recurso de Media Services  

Puede examinar el recurso de Media Services que creó el grafo; para ello, inicie sesión en Azure Portal y visualice el vídeo.

1. Abra el explorador web y vaya a [Azure Portal](https://portal.azure.com/). Introduzca sus credenciales para iniciar sesión en el portal. La vista predeterminada es el panel del servicio.
1. Busque la cuenta de Media Services entre los recursos que tiene en la suscripción y abra la hoja Cuenta.
1. Haga clic en Recursos en la lista de Media Services.

    ![Recursos](./media/continuous-video-recording-tutorial/assets.png)
1. En la lista encontrará un recurso llamado sampleAssetFromEVR-LVAEdge-{DateTime}: este es el nombre proporcionado en la propiedad outputLocation del evento RecordingStarted. El elemento assetNamePattern de la topología determina cómo se genera este nombre.
1. Haga clic en el recurso.
1. En la página de detalles del recurso, haga clic en **Crear nuevo** bajo el cuadro de texto Dirección URL de streaming.

    ![Nuevo recurso](./media/continuous-video-recording-tutorial/new-asset.png)

1. En el asistente que se abre, acepte las opciones predeterminadas y pulse "Agregar". Para más información, consulte [Reproducción de vídeo](video-playback-concept.md).

    > [!TIP]
    > Asegúrese de que el [punto de conexión de streaming está en ejecución](../latest/streaming-endpoint-concept.md).
1. El reproductor debería cargar el vídeo y debería poder pulsar **Reproducir** para verlo.

> [!NOTE]
> Puesto que el origen de vídeo era un contenedor que simula una fuente de cámara, las marcas de tiempo del vídeo están relacionadas a cuándo activó y desactivó la instancia del grafo. Si usa los controles de reproducción integrados en el tutorial [Reproducción de las grabaciones de varios días](playback-multi-day-recordings-tutorial.md), puede ver que las marcas de tiempo del vídeo se muestran en la pantalla.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si va a probar los otros tutoriales, debe mantener los recursos creados. En caso contrario, vaya a Azure Portal, vaya a los grupos de recursos, seleccione el grupo de recursos en el que ejecutó este tutorial y elimine el grupo de recursos.

## <a name="next-steps"></a>Pasos siguientes

* Use una [cámara IP](https://en.wikipedia.org/wiki/IP_camera) compatible con RTSP en lugar de utilizar el simulador RTSP. Puede buscar cámaras IP con compatibilidad con RTSP en la página de productos [compatibles con ONVIF](https://www.onvif.org/conformant-products/), buscando dispositivos que cumplan con los perfiles G, S o T.
* Use un dispositivo Linux AMD64 o x64 (en lugar de usar una máquina virtual Linux de Azure). El dispositivo debe estar en la misma red que la cámara IP. Puede seguir las instrucciones de [Instalación del entorno de ejecución de Azure IoT Edge en sistemas Linux basados en Debian](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) y, a continuación, seguir las instrucciones de la guía de inicio rápido [Implementación del primer módulo IoT Edge en un dispositivo virtual Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) para registrar el dispositivo en Azure IoT Hub.
