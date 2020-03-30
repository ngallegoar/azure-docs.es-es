---
title: Configuración de una puerta de enlace para enrutar solicitudes
description: Obtenga información sobre cómo configurar la puerta de enlace que controla el tráfico entrante de las aplicaciones que se ejecutan en Service Fabric Mesh.
author: dkkapur
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: ec408403d4baa0f211c6bfe867a15c96513693cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75461956"
---
# <a name="configure-a-gateway-resource-to-route-requests"></a>Configuración de un recurso de puerta de enlace para enrutar solicitudes

Se usa un recurso de puerta de enlace para enrutar el tráfico entrante a la red que aloja la aplicación. Realice la configuración para especificar reglas a través de las cuales se dirigen las solicitudes a servicios o puntos de conexión específicos según la estructura de la solicitud. Vea [Introducción a las redes en las aplicaciones de Service Fabric Mesh](service-fabric-mesh-networks-and-gateways.md) para obtener más información sobre las redes y las puertas de enlace en Mesh. 

Los recursos de puerta de enlace deben declararse como parte de la plantilla de implementación (JSON o yaml) y dependen de un recurso de red. En este documento se describen las distintas propiedades que se pueden configurar en la puerta de enlace y se trata la configuración de ejemplo de una puerta de enlace.

## <a name="options-for-configuring-your-gateway-resource"></a>Opciones para configurar el recurso de puerta de enlace

Como el recurso de puerta de enlace actúa como un puente entre la red de la aplicación y la re de la infraestructura subyacente (la red `open`), puede que solo tenga que configurar una (en la versión preliminar de Mesh, hay un límite de una puerta de enlace por aplicación). La declaración del recurso se compone de dos partes principales: metadatos del recurso y las propiedades. 

### <a name="gateway-resource-metadata"></a>Metadatos del recurso de puerta de enlace

Una puerta de enlace se declara con los metadatos siguientes:
* `apiVersion`: debe establecerse en "2018-09-01-preview" (o posterior, en el futuro).
* `name`: nombre de cadena de esta puerta de enlace.
* `type`: "Microsoft.ServiceFabricMesh/gateways".
* `location`: debe establecerse en la ubicación de la aplicación o la red; normalmente, será una referencia al parámetro de ubicación de la implementación.
* `dependsOn`: la red para la que esta puerta de enlace actuará como un punto de entrada.

Este es su aspecto en una plantilla de implementación de Azure Resource Manager (JSON): 

```json
{
  "apiVersion": "2018-09-01-preview",
  "name": "myGateway",
  "type": "Microsoft.ServiceFabricMesh/gateways",
  "location": "[parameters('location')]",
  "dependsOn": [
    "Microsoft.ServiceFabricMesh/networks/myNetwork"
  ],
  "properties": {
    [...]
  }
}
```

### <a name="gateway-properties"></a>Propiedades de la puerta de enlace

La sección de propiedades se utiliza para definir las redes entre las que se encuentra la puerta de enlace y las reglas para enrutar las solicitudes. 

#### <a name="source-and-destination-network"></a>Red de origen y destino 

Cada puerta de enlace requiere `sourceNetwork` y `destinationNetwork`. La red de origen se define como la red desde la que la aplicación recibirá las solicitudes entrantes. La propiedad de nombre siempre debe establecerse en "Abierto". La red de destino es la red a la que se dirigen las solicitudes. El valor del nombre en este caso debe configurarse como el nombre del recurso de la red local de la aplicación (debe incluir una referencia completa al recurso). Consulte la información siguiente para ver un ejemplo de configuración del aspecto que esto tendría para una implementar en una red denominada "myNetwork".

```json 
"properties": {
  "description": "Service Fabric Mesh Gateway",
  "sourceNetwork": {
    "name": "Open"
  },
  "destinationNetwork": {
    "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'myNetwork')]"
  },
  [...]
}
```

#### <a name="rules"></a>Reglas 

Una puerta de enlace puede tener varias reglas de enrutamiento que determinan cómo se controlará el tráfico entrante. Una regla de enrutamiento define la relación entre el puerto de escucha y el punto de conexión de destino de una aplicación determinada. En el caso de las reglas de enrutamiento TCP, se realiza una asignación 1:1 entre el puerto y el punto de conexión. En el caso de las reglas de enrutamiento HTTP, puede definir reglas de enrutamiento más complejas que examinen la ruta de acceso a la solicitud y, de forma opcional, a los encabezados, para decidir cómo se enrutará la solicitud. 

Las reglas de enrutamiento se especifican por cada puerto. Cada puerto de entrada tiene su propia matriz de reglas en la sección de propiedades de la configuración de la puerta de enlace. 

#### <a name="tcp-routing-rules"></a>Reglas de enrutamiento TCP 

Una regla de enrutamiento TCP consta de las siguientes propiedades: 
* `name`: referencia a la regla que puede ser cualquier cadena de su elección. 
* `port`: puerto para escuchar las solicitudes entrantes. 
* `destination`: especificación del punto de conexión que incluye los valores `applicationName`, `serviceName` y `endpointName`, que es donde deben enrutarse las solicitudes.

Este es un ejemplo de regla de enrutamiento TCP:

```json
"properties": {
  [...]
  "tcp": [
    {
      "name": "web",
      "port": 80,
      "destination": {
        "applicationName": "myApp",
        "serviceName": "myService",
        "endpointName": "myListener"
      }
    }
  ]
}
```


#### <a name="http-routing-rules"></a>Reglas de enrutamiento HTTP 

Una regla de enrutamiento HTTP consta de las siguientes propiedades: 
* `name`: referencia a la regla que puede ser cualquier cadena de su elección. 
* `port`: puerto para escuchar las solicitudes entrantes. 
* `hosts`: matriz de directivas aplicables a las solicitudes que llegan a los distintos "hosts" en el puerto especificado anteriormente. Los hosts son el conjunto de aplicaciones y servicios que pueden estar ejecutándose en la red y pueden atender las solicitudes entrantes, como una aplicación web. Las directivas de host se interpretan en orden, para que pueda crear lo siguiente en niveles de especificidad descendentes.
    * `name`: el nombre DNS del host para el que se especifican las siguientes reglas de enrutamiento. Al usar "*" aquí se crearían reglas de enrutamiento para todos los hosts.
    * `routes`: una matriz de directivas para este host específico.
        * `match`: especificación de la estructura de la solicitud entrante para que esta regla se aplique, en función de un parámetro `path`.
            * `path`: contiene un parámetro `value` (URI de entrada), `rewrite` (cómo desea que se reenvíe la solicitud), y un parámetro `type` (actualmente solo puede ser "Prefijo").
            * `header`: es una matriz opcional de valores de encabezados que deben coincidir con el encabezado de la solicitud, para que la solicitud coincida con la especificación de la ruta de acceso (véase el parámetro anterior).
              * Cada entrada contiene `name` (nombre de cadena del encabezado con el que debe coincidir), `value` (valor de cadena del encabezado de la solicitud) y `type` (actualmente solo puede ser "Exacto").
        * `destination`: si la solicitud coincide, se enrutará a este destino, que se especifica con el uso de los parámetros `applicationName`, `serviceName` y `endpointName`.

Este es un ejemplo de una regla de enrutamiento HTTP que se aplicaría a las solicitudes que llegan al puerto 80, a todos los hosts atendidos por las aplicaciones de esta red. Si la dirección URL de la solicitud tiene una estructura que coincide con la especificación de la ruta de acceso, es decir, `<IPAddress>:80/pickme/<requestContent>`, entonces se dirigirá al punto de conexión `myListener`.  

```json
"properties": {
  [...]
  "http": [
    {
      "name": "web",
      "port": 80,
      "hosts": [
        {
          "name": "*",
          "routes": [
            {
              "match": {
                "path": {
                  "value": "/pickme",
                  "rewrite": "/",
                  "type": "Prefix"
                }
              },
              "destination": {
                "applicationName": "meshApp",
                "serviceName": "myService",
                "endpointName": "myListener"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="sample-config-for-a-gateway-resource"></a>Configuración de ejemplo de un recurso de puerta de enlace 

Este es un ejemplo del aspecto de la configuración de un recurso de puerta de enlace completo, adaptado a partir del ejemplo de entrada disponible en el [repositorio de ejemplos de Mesh](https://github.com/Azure-Samples/service-fabric-mesh/blob/2018-09-01-preview/templates/ingress/meshingress.linux.json)):

```json
{
  "apiVersion": "2018-09-01-preview",
  "name": "ingressGatewayLinux",
  "type": "Microsoft.ServiceFabricMesh/gateways",
  "location": "[parameters('location')]",
  "dependsOn": [
    "Microsoft.ServiceFabricMesh/networks/meshNetworkLinux"
  ],
  "properties": {
    "description": "Service Fabric Mesh Gateway for Linux mesh samples.",
    "sourceNetwork": {
      "name": "Open"
    },
    "destinationNetwork": {
      "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'meshNetworkLinux')]"
    },
    "http": [
      {
        "name": "web",
        "port": 80,
        "hosts": [
          {
            "name": "*",
            "routes": [
              {
                "match": {
                  "path": {
                    "value": "/hello",
                    "rewrite": "/",
                    "type": "Prefix"
                  }
                },
                "destination": {
                  "applicationName": "meshAppLinux",
                  "serviceName": "helloWorldService",
                  "endpointName": "helloWorldListener"
                }
              },
              {
                "match": {
                  "path": {
                    "value": "/counter",
                    "rewrite": "/",
                    "type": "Prefix"
                  }
                },
                "destination": {
                  "applicationName": "meshAppLinux",
                  "serviceName": "counterService",
                  "endpointName": "counterServiceListener"
                }
              }
            ]
          }
        ]
      }
    ]
  }
}
```

Esta puerta de enlace está configurada para una aplicación Linux, "meshAppLinux", que consta de al menos dos servicios: "helloWorldService" y "counterService", y que escucha en el puerto 80. En función de la estructura de la dirección URL de la solicitud de entrada, enrutará la solicitud a uno de estos servicios. 
* "\<DirecciónIP>:80/helloWorld/\<solicitud\>" dirigiría una solicitud a "helloWorldListener" en helloWorldService. 
* "\<DirecciónIP>:80/counter/\<solicitud\>" dirigiría la solicitud a "counterListener" en counterService. 

## <a name="next-steps"></a>Pasos siguientes
* Implemente el [ejemplo de entrada](https://github.com/Azure-Samples/service-fabric-mesh/tree/2018-09-01-preview/templates/ingress) para ver la puerta de enlace en acción.
