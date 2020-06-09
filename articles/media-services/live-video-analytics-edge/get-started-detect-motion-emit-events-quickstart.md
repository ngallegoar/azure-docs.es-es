---
title: 'Introducción a Live Video Analytics en IoT Edge: Azure'
description: En este inicio rápido se muestra una introducción al uso de Live Video Analytics en IoT Edge y a la detección de movimiento en una secuencia de vídeo en directo.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: 307a81938be3e25b8a6a07bb3696ca3b7647c0aa
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261570"
---
# <a name="quickstart-get-started---live-video-analytics-on-iot-edge"></a>Inicio rápido: Introducción: Live Video Analytics on IoT Edge

Este inicio rápido le guiará por los pasos necesarios para empezar a usar Live Video Analytics en IoT Edge. Usa una máquina virtual de Azure como un dispositivo IoT Edge y una secuencia de vídeo en directo simulada. Después de completar los pasos de configuración, podrá ejecutar una secuencia de vídeo en directo simulada mediante un grafo de elementos multimedia que detecta e informa de cualquier movimiento en esa secuencia. El diagrama siguiente muestra una representación gráfica de ese grafo de elementos multimedia.

![Flujo de Live Video Analytics basado en la detección de movimiento](./media/analyze-live-video/motion-detection.png)

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Visual Studio Code](https://code.visualstudio.com/) en la máquina de desarrollo con la [extensión de Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* La red a la que está conectada la máquina de desarrollo debe permitir el protocolo AMQP a través del puerto 5671 (de modo que Azure IoT Tools pueda comunicarse con Azure IoT Hub).

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
    
Si el script se completa correctamente, debería ver todos los recursos mencionados anteriormente en su suscripción. Como parte de la salida del script, se generará una tabla de recursos que mostrará el nombre del centro de IoT. Busque el tipo de recurso **"Microsoft.Devices/IotHubs"** y anote el nombre. Lo necesitará en el siguiente paso. El script también generará algunos archivos de configuración en el directorio ~/clouddrive/lva-sample/: los necesitará más adelante en el inicio rápido.

## <a name="deploy-modules-on-your-edge-device"></a>Implementación de módulos en el dispositivo perimetral

En Cloud Shell, ejecute el comando siguiente:

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content ~/clouddrive/lva-sample/edge-deployment/deployment.amd64.json
```

El comando anterior implementará los siguientes módulos en el dispositivo perimetral (la máquina virtual Linux):

* Live Video Analytics en IoT Edge (nombre de módulo "lvaEdge")
* Simulador RTSP (nombre de módulo "rtspsim")

El módulo del simulador RTSP simula una secuencia de vídeo en directo mediante un archivo de vídeo almacenado que se copió en el dispositivo perimetral cuando ejecutó el [script de configuración de los recursos de Live Video Analytics](https://github.com/Azure/live-video-analytics/tree/master/edge/setup). En esta fase, ha implementado los módulos pero no hay grafos de elementos multimedia activos.

## <a name="configure-azure-iot-tools-extension-in-visual-studio-code"></a>Configuración de la extensión de Azure IoT Tools en Visual Studio Code

Inicie Visual Studio Code y siga las instrucciones que se indican a continuación para conectarse a Azure IoT Hub mediante la extensión de Azure IoT Tools.

1. Vaya a la pestaña Explorador en Visual Studio Code mediante **Ver** > **Explorer** o simplemente presione (Ctrl + Mayús + E).
1. En la pestaña Explorador, haga clic en "Azure IoT Hub" en la esquina inferior izquierda.
1. Haga clic en el icono Más opciones para ver el menú contextual y seleccione la opción "Set IoT Hub Connection String" (Establecer cadena de conexión de IoT Hub).
1. Aparecerá un cuadro de entrada y, a continuación, escriba la cadena de conexión de IoT Hub. Puede obtener la cadena de conexión de IoT Hub en ~/clouddrive/lva-sample/appsettings.json en Cloud Shell.
1. Si la conexión se realiza correctamente, se mostrará la lista de dispositivos perimetrales. Debe haber al menos un dispositivo, denominado "lva-sample-device".
1. Ahora puede administrar los dispositivos IoT Edge e interactuar con Azure IoT Hub mediante el menú contextual.
1. Para ver los módulos implementados en el dispositivo perimetral, expanda el nodo Módulos en "lva-sample-device".

    ![Nodo lva-sample-device](./media/quickstarts/lva-sample-device-node.png)

## <a name="use-direct-methods"></a>Uso de métodos directos

Puede usar el módulo para analizar secuencias de vídeo en directo mediante la invocación de métodos directos. Lea [Métodos directos de Live Video Analytics en IoT Edge](direct-methods.md) para conocer todos los métodos directos que proporciona el módulo. 

### <a name="invoke-graphtopologylist"></a>Invocación de GraphTopologyList
Aquí se enumeran todas las [topologías de grafos](media-graph-concept.md#media-graph-topologies-and-instances) del módulo.

1. Haga clic con el botón derecho en el módulo "lvaEdge" y seleccione "Invoke Module Direct Method" (Invocar método directo del módulo) en el menú contextual.
1. Verá un cuadro de edición emergente en la parte superior central de la ventana de Visual Studio Code. Escriba "GraphTopologyList" en el cuadro de edición y presione Entrar.
1. A continuación, copie y pegue la carga de JSON siguiente en el cuadro de edición y presione Entrar.

    ```
    {
        "@apiVersion" : "1.0"
    }
    ```

    En unos segundos, verá la ventana de salida en el cuadro emergente de Visual Studio Code con la siguiente respuesta

    ```
    [DirectMethod] Invoking Direct Method [GraphTopologyList] to [lva-sample-device/lvaEdge] ...
    [DirectMethod] Response from [lva-sample-device/lvaEdge]:
    {
      "status": 200,
      "payload": {
        "value": []
      }
    }
    ```
    
    Se espera la respuesta anterior ya que no se ha creado ninguna topología de grafo.
    

### <a name="invoke-graphtopologyset"></a>Invocación de GraphTopologySet

Con los mismos pasos descritos para la invocación de GraphTopologyList, puede invocar a GraphTopologySet para establecer una [topología de grafo](media-graph-concept.md#media-graph-topologies-and-instances) mediante el siguiente JSON como carga.

```
{
    "@apiVersion": "1.0",
    "name": "MotionDetection",
    "properties": {
        "description": "Analyzing live video to detect motion and emit events",
        "parameters": [
            {
                "name": "rtspUserName",
                "type": "String",
                "description": "rtsp source user name.",
                "default": "dummyUserName"
            },
            {
                "name": "rtspPassword",
                "type": "String",
                "description": "rtsp source password.",
                "default": "dummyPassword"
            },
            {
                "name": "rtspUrl",
                "type": "String",
                "description": "rtsp Url"
            }
        ],
        "sources": [
            {
                "@type": "#Microsoft.Media.MediaGraphRtspSource",
                "name": "rtspSource",
                "endpoint": {
                    "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
                    "url": "${rtspUrl}",
                    "credentials": {
                        "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
                        "username": "${rtspUserName}",
                        "password": "${rtspPassword}"
                    }
                }
            }
        ],
        "processors": [
            {
                "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
                "name": "motionDetection",
                "sensitivity": "medium",
                "inputs": [
                    {
                        "nodeName": "rtspSource"
                    }
                ]
            }
        ],
        "sinks": [
            {
                "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
                "name": "hubSink",
                "hubOutputName": "inferenceOutput",
                "inputs": [
                    {
                        "nodeName": "motionDetection"
                    }
                ]
            }
        ]
    }
}

```


La carga de JSON anterior da como resultado la creación de una topología de grafos que define tres parámetros (dos de los cuales tienen valores predeterminados). La topología tiene un nodo de origen (origen RTSP), un nodo de procesador (procesador de detección de movimiento) y un nodo receptor (receptor de IoT Hub).

En unos segundos, verá la siguiente respuesta en la ventana de salida:

```
[DirectMethod] Invoking Direct Method [GraphTopologySet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 201,
  "payload": {
    "systemData": {
      "createdAt": "2020-05-19T07:41:34.507Z",
      "lastModifiedAt": "2020-05-19T07:41:34.507Z"
    },
    "name": "MotionDetection",
    "properties": {
      "description": "Analyzing live video to detect motion and emit events",
      "parameters": [
        {
          "name": "rtspUserName",
          "type": "String",
          "description": "rtsp source user name.",
          "default": "dummyUserName"
        },
        {
          "name": "rtspPassword",
          "type": "String",
          "description": "rtsp source password.",
          "default": "dummyPassword"
        },
        {
          "name": "rtspUrl",
          "type": "String",
          "description": "rtsp Url"
        }
      ],
      "sources": [
        {
          "@type": "#Microsoft.Media.MediaGraphRtspSource",
          "name": "rtspSource",
          "transport": "Tcp",
          "endpoint": {
            "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
            "url": "${rtspUrl}",
            "credentials": {
              "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
              "username": "${rtspUserName}"
            }
          }
        }
      ],
      "processors": [
        {
          "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
          "sensitivity": "medium",
          "name": "motionDetection",
          "inputs": [
            {
              "nodeName": "rtspSource",
              "outputSelectors": []
            }
          ]
        }
      ],
      "sinks": [
        {
          "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
          "hubOutputName": "inferenceOutput",
          "name": "hubSink",
          "inputs": [
            {
              "nodeName": "motionDetection",
              "outputSelectors": []
            }
          ]
        }
      ]
    }
  }
}
```

El estado devuelto es 201, lo que indica que se ha creado una nueva topología. Pruebe lo siguiente, como se indica a continuación:

* Vuelva a invocar a GraphTopologySet y observe que el código de estado devuelto es 200. Un código de estado 200 indica que se ha actualizado correctamente una topología ya existente.
* Vuelva a invocar a GraphTopologySet, pero cambie la cadena de descripción. Observe que el código de estado de la respuesta es 200 y que la descripción se ha actualizado al nuevo valor.
* Invoque a GraphTopologyList como se describe en la sección anterior y observe que ahora puede ver la topología "MotionDetection" en la carga devuelta.

### <a name="invoke-graphtopologyget"></a>Invocación de GraphTopologyGet

Ahora, invoque a GraphTopologyGet con la carga siguiente

```

{
    "@apiVersion" : "1.0",
    "name" : "MotionDetection"
}
```

En unos segundos, verá la siguiente respuesta en la ventana de salida:

```
[DirectMethod] Invoking Direct Method [GraphTopologyGet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": {
    "systemData": {
      "createdAt": "2020-05-19T07:41:34.507Z",
      "lastModifiedAt": "2020-05-19T07:41:34.507Z"
    },
    "name": "MotionDetection",
    "properties": {
      "description": "Analyzing live video to detect motion and emit events",
      "parameters": [
        {
          "name": "rtspUserName",
          "type": "String",
          "description": "rtsp source user name.",
          "default": "dummyUserName"
        },
        {
          "name": "rtspPassword",
          "type": "String",
          "description": "rtsp source password.",
          "default": "dummyPassword"
        },
        {
          "name": "rtspUrl",
          "type": "String",
          "description": "rtsp Url"
        }
      ],
      "sources": [
        {
          "@type": "#Microsoft.Media.MediaGraphRtspSource",
          "name": "rtspSource",
          "transport": "Tcp",
          "endpoint": {
            "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
            "url": "${rtspUrl}",
            "credentials": {
              "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
              "username": "${rtspUserName}"
            }
          }
        }
      ],
      "processors": [
        {
          "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
          "sensitivity": "medium",
          "name": "motionDetection",
          "inputs": [
            {
              "nodeName": "rtspSource",
              "outputSelectors": []
            }
          ]
        }
      ],
      "sinks": [
        {
          "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
          "hubOutputName": "inferenceOutput",
          "name": "hubSink",
          "inputs": [
            {
              "nodeName": "motionDetection",
              "outputSelectors": []
            }
          ]
        }
      ]
    }
  }
}
```

Observe lo siguiente en la carga de respuesta:

* El código de estado es 200, lo que indica que se ha realizado correctamente.
* La carga útil tiene las marcas de tiempo "created" y "lastModified".

### <a name="invoke-graphinstanceset"></a>Invocación de GraphInstanceSet

A continuación, cree una instancia de grafo que haga referencia a la topología de grafo anterior. Como se explicó [aquí](media-graph-concept.md#media-graph-topologies-and-instances), las instancias de grafos le permiten analizar secuencias de vídeo en directo desde muchas cámaras con la misma topología de grafos.

Ahora, invoque el método directo GraphInstanceSet con la carga siguiente.

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1",
    "properties" : {
        "topologyName" : "MotionDetection",
        "description" : "Sample graph description",
        "parameters" : [
            { "name" : "rtspUrl", "value" : "rtsp://rtspsim:554/media/camera-300s.mkv" }
        ]
    }
}
```

Tenga en cuenta lo siguiente:

* La carga anterior especifica el nombre de la topología (MotionDetection) para la que se debe crear la instancia.
* La carga contiene el valor de parámetro para "rtspUrl", el cual no tenía un valor predeterminado en la carga de la topología de grafo.

En unos segundos, verá la siguiente respuesta en la ventana de salida:

```
[DirectMethod] Invoking Direct Method [GraphInstanceSet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 201,
  "payload": {
    "name": "Sample-Graph-1",
    "properties": {
      "created": "2020-05-19T07:44:33.868Z",
      "lastModified": "2020-05-19T07:44:33.868Z",
      "state": "Inactive",
      "description": "Sample graph description",
      "topologyName": "MotionDetection",
      "parameters": [
        {
          "name": "rtspUrl",
          "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
        }
      ]
    }
  }
}
```

Observe lo siguiente en la carga de respuesta:

* El código de estado es 201, lo que indica que se ha creado una nueva instancia.
* El estado es "Inactivo", lo que indica que la instancia de grafo se creó pero no se activó. Para más información, consulte los estados de los [grafos de elementos multimedia](media-graph-concept.md).

Pruebe lo siguiente, como se indica a continuación:

* Vuelva a invocar a GraphInstanceSet con la misma carga y observe que el código de estado devuelto es ahora 200.
* Vuelva a invocar a GraphInstanceSet, pero con una descripción diferente y observe la descripción actualizada en la carga de respuesta, la cual indica que la instancia de grafo se actualizó correctamente.
* Invoque a GraphInstanceSet pero cambie el nombre a "Sample-Graph-2" y observe la carga de respuesta. Observe que se ha creado una nueva instancia de grafo (es decir, el código de estado es 201).

### <a name="invoke-graphinstanceactivate"></a>Invocación de GraphInstanceActivate

Ahora active la instancia de grafo que inicia el flujo de vídeo en directo a través del módulo. Ahora, invoque el método directo GraphInstanceActivate con la carga siguiente.

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

En unos segundos, verá la siguiente respuesta en la ventana de salida:

```
[DirectMethod] Invoking Direct Method [GraphInstanceActivate] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

El código de estado 200 en la carga de respuesta indica que la instancia de grafo se activó correctamente.

### <a name="invoke-graphinstanceget"></a>Invocación de GraphInstanceGet

Ahora, invoque el método directo GraphInstanceGet con la carga siguiente:

```
 {
     "@apiVersion" : "1.0",
     "name" : "Sample-Graph-1"
 }
 ```

En unos segundos, verá la siguiente respuesta en la ventana de salida:

```
[DirectMethod] Invoking Direct Method [GraphInstanceGet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": {
    "name": "Sample-Graph-1",
    "properties": {
      "created": "2020-05-19T07:44:33.868Z",
      "lastModified": "2020-05-19T07:44:33.868Z",
      "state": "Active",
      "description": "graph description",
      "topologyName": "MotionDetection",
      "parameters": [
        {
          "name": "rtspUrl",
          "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
        }
      ]
    }
  }
}
```

Observe lo siguiente en la carga de respuesta:

* El código de estado es 200, lo que indica que se ha realizado correctamente.
* El estado es "Activo", lo que indica que la instancia del grafo tiene ahora ese estado.

## <a name="observe-results"></a>Observación de resultados

La instancia de grafo que hemos creado y activado anteriormente usa el nodo de procesador de detección de movimiento para detectar el movimiento en la secuencia de vídeo en directo entrante y envía eventos al nodo de receptor de IoT Hub. A continuación, estos eventos se retransmiten al centro de IoT Hub, como se puede observar ahora. Para hacerlo, siga estos pasos:

1. Abra el panel del explorador en Visual Studio Code y busque Azure IoT Hub en la esquina inferior izquierda.
2. Expanda el nodo Devices (Dispositivos).
3. Haga clic con el botón derecho en Iva-sample-device y elija la opción "Iniciar la supervisión del punto de conexión de eventos integrado".

![Iniciar supervisión de eventos de IoT Hub](./media/quickstarts/start-monitoring-iothub-events.png)

Verá los siguientes mensajes en la ventana de salida:

```
[IoTHubMonitor] [7:44:33 AM] Message received from [lva-sample-device/lvaEdge]:
{
    "body": {
    "timestamp": 143005362606360,
    "inferences": [
        {
        "type": "motion",
        "motion": {
            "box": {
            "l": 0.828452,
            "t": 0.455224,
            "w": 0.1,
            "h": 0.088889
            }
        }
        },
        {
        "type": "motion",
        "motion": {
            "box": {
            "l": 0.661088,
            "t": 0.597015,
            "w": 0.0625,
            "h": 0.051852
            }
        }
        }
    ]
    },
    "applicationProperties": {
    "topic": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/microsoft.media/mediaservices/{amsAccountName}",
    "subject": "/graphInstances/Sample-Graph-1/processors/motionDetection",
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",
    "eventTime": "2020-05-19T07:45:34.404Z",
    "dataVersion": "1.0"
    }
}
```

Observe lo siguiente en el mensaje anterior:

* El mensaje contiene una sección "body" y una sección "applicationProperties". Para comprender lo que representan estas secciones, consulte [Creación y lectura de mensajes de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).
* La sección "subject" de applicationProperties hace referencia al nodo de MediaGraph desde el que se generó el mensaje. En este caso, el mensaje se origina en el nodo del procesador de detección del movimiento.
* "eventType" en applicationProperties indica que se trata de un evento de análisis.
* "eventTime" indica la hora a la que se produjo el evento.
* "body" contiene datos sobre el evento de análisis. En este caso, el evento es un evento de inferencia y, por lo tanto, el cuerpo contiene datos de "marca de tiempo" e "inferencias".
* La sección "inferences" indica que el "tipo" es "movimiento" y contiene datos adicionales sobre el evento "movimiento".

Si permite que MediaGraph se ejecute durante algún tiempo, verá el mensaje siguiente en la ventana de salida:

```
[IoTHubMonitor] [7:47:45 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1588948185746703 1 IN IP4 172.xx.xx.xx\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets={SPS}\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-05-19T07:47:45.747Z"
  }
}
```

Observe lo siguiente en el mensaje anterior:

* La sección "subject" de applicationProperties indica que el mensaje se generó desde el nodo de origen RTSP del grafo de elementos multimedia.
* "eventType" en applicationProperties indica que se trata de un evento de diagnóstico.
* "body" contiene datos sobre el evento de diagnóstico. En este caso, el evento es MediaSessionEstablished y, por lo tanto, el cuerpo.

## <a name="invoke-additional-direct-methods-to-clean-up"></a>Invocación de métodos directos adicionales para la limpieza

Ahora, invoque métodos directos para desactivar y eliminar la instancia de grafo (en ese orden).

### <a name="invoke-graphinstancedeactivate"></a>Invocación de GraphInstanceDeactivate

Ahora, invoque el método directo GraphInstanceDeactivate con la carga siguiente.

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

En unos segundos, verá la siguiente respuesta en la ventana de salida:

```
[DirectMethod] Invoking Direct Method [GraphInstanceDeactivate] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

El código de estado 200 indica que la instancia de grafo se ha desactivado correctamente.

Pruebe lo siguiente, como se indica a continuación.

* Invoque GraphInstanceGet como se ha indicado en las secciones anteriores y observe el valor de "state".

### <a name="invoke-graphinstancedelete"></a>Invocación de GraphInstanceDelete

Ahora, invoque el método directo GraphInstanceDelete con la carga siguiente:

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

En unos segundos, verá la siguiente respuesta en la ventana de salida:

```
[DirectMethod] Invoking Direct Method [GraphInstanceDelete] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

El código de estado 200 de la respuesta indica que la instancia de grafo se eliminó correctamente.

### <a name="invoke-graphtopologydelete"></a>Invocación de GraphTopologyDelete

Invoque el método directo GraphTopologyDelete con la carga siguiente:

```
{
    "@apiVersion" : "1.0",
    "name" : "MotionDetection"
}
```

En unos segundos, verá la siguiente respuesta en la ventana de salida:

```
[DirectMethod] Invoking Direct Method [GraphTopologyDelete] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

El código de estado 200 indica que la topología de grafo se ha eliminado correctamente.

Pruebe lo siguiente, como se indica a continuación.

* Invoque GraphTopologyList y observe que no hay topologías de grafos en el módulo.
* Invoque GraphInstanceList con la misma carga que GraphTopologyList y observe que no se enumeran instancias de grafos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, elimine los recursos que ha creado en este inicio rápido.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a grabar un vídeo mediante Live Video Analytics en IoT Edge
* Más información sobre los mensajes de diagnóstico.
