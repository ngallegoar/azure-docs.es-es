---
title: 'Análisis de vídeo en directo con la inteligencia artificial preferida: Azure'
description: En este artículo se obtiene información para compilar un módulo de IoT Edge que se puede integrar con Live Video Analytics en IoT Edge a fin de analizar vídeo en directo con un modelo de Computer Vision de su elección.
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: 0ac2af280eefd5ce293a8be422551d5ee6f6d3f3
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84260648"
---
# <a name="analyze-live-video-with-ai-of-your-choice"></a>Análisis de vídeo en directo con la inteligencia artificial preferida

En este artículo se obtiene información para compilar un módulo de IoT Edge que se puede integrar con Live Video Analytics en IoT Edge a fin de analizar vídeo en directo con un modelo de Computer Vision de su elección. 

## <a name="pre-reading"></a>Lectura previa

[Concepto de grafo multimedia](media-graph-concept.md)

## <a name="media-graph-extension"></a>Extensión de grafos multimedia

Live Video Analytics en IoT Edge define un modelo de extensibilidad que permite extender las capacidades de procesamiento de grafos multimedia a los componentes de análisis multimedia propios por medio de una extensión de grafos. El componente de análisis puede usar técnicas tradicionales de procesamiento de imágenes o modelos de inteligencia artificial de Computer Vision. Las extensiones de grafos se habilitan al incluir el nodo del [procesador de extensión HTTP](media-graph-concept.md#http-extension-processor) en un grafo multimedia. El procesador de extensión HTTP puede retransmitir fotogramas de vídeo a un punto de conexión HTTP especificado por el usuario y actúa como la interfaz del componente a través de él. La conexión se puede realizar con un punto de conexión local o remoto y se puede proteger mediante autenticación y cifrado TLS, si fuera necesario. Además, el procesador permite el escalado y la codificación opcionales de los fotogramas de vídeo antes de retransmitirlos.

Puede ejecutar el modelo de inferencia que prefiera en cualquier runtime de inferencia disponible, como ONNX, TensorFlow o PyTorch, u otros en su propio contenedor de Docker. También puede usar el lenguaje de programación y las bibliotecas que prefiera para exponer las capacidades de inferencia de imágenes a través de un servidor HTTP en su propio contenedor. El contenedor de inferencia debe implementarse junto con el módulo perimetral Live Video Analytics para obtener el mejor rendimiento y se invoca a través del procesador de extensión HTTP incluido en la topología de grafos.
Además, la frecuencia de las llamadas al módulo personalizado se puede limitar si se agregan un [procesador de detección de movimiento](media-graph-concept.md#motion-detection-processor) y un [procesador de filtrado de velocidad de fotogramas](media-graph-concept.md#frame-rate-filter-processor) ascendentes al procesador de extensión HTTP.

En el diagrama siguiente se muestra el flujo de datos general:

![Dispositivo perimetral](./media/analyze-live-video-with-ai-your-choice-how-to/edge-device.png)

Esto le permite analizar vídeo con los modelos de inteligencia artificial que prefiera para satisfacer sus necesidades empresariales exclusivas. Los modelos de inteligencia artificial pueden proceder de la comunidad de código abierto, de sus propios científicos de datos o de un proveedor de inteligencia artificial especializado.

## <a name="media-graph-http-extension-contract-definitions"></a>Definiciones del contrato de extensión HTTP de grafos multimedia

En esta sección se define el contrato HTTP que define el flujo de datos.

### <a name="http-extensibility-protocol"></a>Protocolo de extensibilidad HTTP  

El contrato HTTP se define de la siguiente manera:

* El módulo actúa como servidor HTTP.
* El módulo Live Video Analytics en IoT Edge actúa como cliente HTTP.

### <a name="request"></a>Solicitud

Las solicitudes del módulo Live Video Analytics al módulo serían así:

|||
|---|---|
|POST| `https://hostname/optional-path?optional-query`|
|Accept|application/json, */*|
|Authorization| Básica, Implícita, Portador (por medio de compatibilidad con encabezado personalizado)|
|Content-Type|image/jpeg<br/>image/png<br/>image/bmp<br/>image/x-raw|
|Content-Length|Longitud del cuerpo, en bytes|
|User-Agent|Azure Media Services|
|Body|Bytes de imagen, codificados en binario en uno de los tipos de contenido admitidos.|

#### <a name="example"></a>Ejemplo

```
POST http://localhost:8080/inference HTTP/1.1
Host: localhost:8080
x-ms-client-request-id: d6050cd4-c9f2-42d3-9adc-53ba7e440f17
Content-Type: image/bmp
Content-Length: 519222

(Image Binary Content)
```

### <a name="response"></a>Response

Las respuestas del módulo al módulo Live Video Analytics deben ser de esta manera

|||
|---|---|
|Códigos de estado|200 Correcto: Resultados de inferencia encontrados<br/>204 Sin contenido: La IA no ha encontrado ningún contenido<br/>400 Solicitud incorrecta: Inesperado<br/>500 Error interno del servidor: Inesperado<br/>503 Servidor ocupado: AMS se interrumpe en función del encabezado "Retry-after" o de una cantidad de tiempo predeterminada en caso de que el encabezado no esté preestablecido|
|Content-Type|application/json|
|Content-Length|    Longitud del cuerpo, en bytes|
|Body|Objeto JSON con una propiedad "inferences" única|

#### <a name="example"></a>Ejemplo

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 468
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 17 Apr 2020 04:44:01 GMT

{
  "inferences": [
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.9048132 },
        "box": { "l": 0.42681578, "t": 0.47660735, "w": 0.019501392, "h": 0.020954132 }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.8953932 },
        "box": { "l": 0.55083525, "t": 0.4843858, "w": 0.046550274, "h": 0.046502113 }
      }
    }    
  ]
}
```

Se recomienda encarecidamente que las respuestas se devuelvan mediante documentos JSON válidos según el esquema preestablecido definido a continuación. Esto garantiza mejor la interoperabilidad con otros componentes y posibles capacidades futuras agregadas al módulo Live Video Analytics.

Si el módulo devuelve una respuesta en la que el tipo de contenido no es "application/json", Live Video Analytics codifica el mensaje como contenido Base 64 y lo serializa como una carga JSON opaca.

Si el módulo devuelve una respuesta cuyo tipo de contenido es "application/json" pero el esquema JSON no sigue el [esquema de metadatos de inferencia descrito a continuación](#inference-metadata-schema), la carga del mensaje se reenvía a través de la canalización, pero se reduce la interoperabilidad.

> [!NOTE]
> Si el módulo no genera ningún resultado, debe devolver el código de estado HTTP 204 (Sin contenido) con un cuerpo de respuesta vacío. Live Video Analytics lo entiende como un resultado vacío y no reenvía el evento a través de la canalización.

### <a name="inference-metadata-schema"></a>Esquema de metadatos de inferencia

Cada objeto de inferencia sigue este esquema de superconjunto:

```
{
  "type": "[classification|motion|entity|text|other]",
  "subtype": "",              // Free form subtype id

  // Object has *at most one of* the following
  "classification":
  {
    "tag": 
    {
      "value": "",            // Tag value
      "confidence": 0.0       // Tag confidence
    },
    "attributes":[            // Optional attributes
      {
        "name": "",           
        "value": "",          
        "confidence": 0.0     
      }
    ]
  },

  "motion":
  {
    "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
    // "regionId": ""
  },

  "entity":
  {
    "tag": 
    {
      "value": "",            // Tag value
      "confidence": 0.0       // Tag confidence
    },
    "attributes":[            // Optional attributes
      {
        "name": "",           
        "value": "",          
        "confidence": 0.0     
      }
    ],
    "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
  },

  "text":
  {
    "value": "",              // Text value
    "language": "",           // Optional BCP47 Language Code (https://tools.ietf.org/html/bcp47)
    "startTimestamp": 0,      // Optional start timestamp
    "endTimestamp": 0         // Optional end timestamp
  },

  // Plus every object can have zero or more extensions
  "extensions":
  {
    "name1": "value1",
    "name2": "value2"
    // ...
  }
}
```
 
### <a name="data-contracts---class-hierarchy"></a>Contratos de datos: jerarquía de clases

![Contratos de datos: jerarquía de clases](./media/analyze-live-video-with-ai-your-choice-how-to/data-contracts.png)

### <a name="examples"></a>Ejemplos  

El ejemplo siguiente contiene un solo evento con todos los tipos de inferencia admitidos:

```
{
  "inferences": [
    // Light detection
    {
      "type": "classification",
      "subtype": "lightDetection",
      "classification": {
        "tag": { "value": "daylight", "confidence": 0.86 },
        "attributes": [
            { "name": "isBlackAndWhite", "value": "false", "confidence": 0.71 }
        ]
      }
    },

    // Motion detection
    {
      "type": "motion",
      "subtype": "motionDetection",
      "motion":
      {
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // Object detection
    {
      "type": "entity",
      "subtype": "objectDetection",    
      "entity":
      {
        "tag": { "value": "dog", "confidence": 0.97 },
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // Vehicle identification
    {
      "type": "entity",
      "subtype": "vehicleIdentification",    
      "entity":
      {
        "tag": { "value": "007-SPY", "confidence": 0.82 },
        "attributes":[  
          { "name": "color", "value": "black", "confidence": 0.90 },
          { "name": "body", "value": "coupe", "confidence": 0.87 },
          { "name": "make", "value": "Aston Martin", "confidence": 0.35 },
          { "name": "model", "value": "DBS V12", "confidence": 0.33 }
        ],
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // People identification
    {
      "type": "entity",
      "subtype": "peopleIdentification",    
      "entity":
      {
        "tag": { "value":"Erwin Schrödinger", "confidence": 0.50 },
        "attributes":[  
          { "name": "age", "value": "73", "confidence": 0.87 },
          { "name": "glasses", "value": "yes", "confidence": 0.94 }
        ],
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      },
    }
  ]
}
```
 
## <a name="sample-http-extension-modules"></a>Módulos de extensión HTTP de ejemplo

Hay algunos módulos de extensión HTTP de ejemplo en el [repositorio de GitHub de Live Video Analytics](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis). Uno de estos [ejemplos de análisis de vídeo](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) muestra cómo usar el modelo [Yolov3](https://pjreddie.com/darknet/yolo/) [ONNX](http://onnx.ai/) para compilar un módulo de IoT Edge para la detección de objetos. Puede usar el mismo enfoque para compilar un módulo propio con el modelo de inteligencia artificial que prefiera.

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas](troubleshoot-how-to.md)
