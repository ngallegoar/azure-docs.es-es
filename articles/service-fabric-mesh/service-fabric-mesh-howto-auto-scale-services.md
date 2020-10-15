---
title: Escalado automático de una aplicación que se ejecuta en Azure Service Fabric Mesh
description: Obtenga información sobre cómo configurar directivas de escalabilidad automática para los servicios de una aplicación de Service Fabric Mesh.
author: georgewallace
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: f65fcfa76069a3de37fd3a76e38e38fba40e04ac
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843063"
---
# <a name="create-autoscale-policies-for-a-service-fabric-mesh-application"></a>Creación de directivas de escalabilidad automática de una aplicación de Service Fabric Mesh
Una de las principales ventajas de implementar aplicaciones en Service Fabric Mesh es la capacidad de escalar vertical u horizontalmente los servicios de un modo sencillo. Esto se debe usar para controlar diferentes cantidades de carga en los servicios o bien para mejorar la disponibilidad. Puede escalar o reducir los servicios horizontalmente de forma manual o configurar directivas de escalado automático.

El [escalado automático](service-fabric-mesh-scalability.md#autoscaling-service-instances) le permite escalar de manera dinámica el número de instancias de servicio (escalado horizontal). El escalado automático proporciona una gran elasticidad y permite el aprovisionamiento o la eliminación de instancias de servicio en función del uso de CPU o memoria.

## <a name="options-for-creating-an-auto-scaling-policy-trigger-and-mechanism"></a>Opciones para crear una directiva de escalado automático, un desencadenador y un mecanismo
Se define una directiva de escalado automático para cada servicio que quiere escalar. La directiva se define ya sea en el archivo de recursos del servicio YAML o en la plantilla de implementación JSON. Cada directiva de escalado consta de dos partes: un desencadenador y un mecanismo de escalado.

El desencadenador define cuándo se invoca una directiva de escalado automático.  Especifique el tipo de desencadenador (carga promedio) y la métrica de supervisión (CPU o memoria).  El umbral de carga superior y el inferior se especifican como porcentaje. El intervalo de escala define con qué frecuencia comprobar (en segundos) el uso especificado (como la carga de CPU promedio) en todas las instancias de servicio actualmente implementadas.  El mecanismo se desencadena cuando la métrica supervisada cae por debajo del umbral inferior o aumenta por encima del umbral superior.  

El mecanismo de escalado define cómo realizar la operación de escalado cuando se desencadena la directiva.  Especifique el tipo de mecanismo (agregar o quitar réplica) y los recuentos mínimo y máximo de las réplicas (como enteros).  La cantidad de réplicas de servicio nunca se escalará por debajo del recuento mínimo o por encima del recuento máximo.  También puede especificar el incremento de escala como un entero, que es el número de réplicas que se agregará o quitarán de una operación de escalado.  

## <a name="define-an-auto-scaling-policy-in-a-json-template"></a>Definición de una directiva de escalado automático en una plantilla JSON

En el ejemplo siguiente se muestra una directiva de escalado automático en una plantilla de implementación JSON.  La directiva de escalado automático se declara en una propiedad del servicio que se va a escalar.  En este ejemplo, se define un desencadenador de carga promedio de CPU.  El mecanismo se desencadenará si la carga de CPU promedio de todas las instancias implementadas cae por debajo del 0,2 (20 %) o sube por encima del 0,8 (80 %).  La carga de CPU se comprueba cada 60 segundos.  El mecanismo de escalado se define para agregar o quitar instancias si se desencadenada la directiva.  Las instancias de servicio se agregarán o quitarán en incrementos de uno.  También se definen un recuento mínimo de 1 instancia y un recuento máximo de 40 instancias.

```json
{
"apiVersion": "2018-09-01-preview",
"name": "WorkerApp",
"type": "Microsoft.ServiceFabricMesh/applications",
"location": "[parameters('location')]",
"dependsOn": [
"Microsoft.ServiceFabricMesh/networks/worker-app-network"
],
"properties": {
"services": [   
    { ... },       
    {
    "name": "WorkerService",
    "properties": {
        "description": "Worker Service",
        "osType": "linux",
        "codePackages": [
        {  ...              }
        ],
        "replicaCount": 1,
        "autoScalingPolicies": [
        {
            "name": "AutoScaleWorkerRule",
            "trigger": {
                "kind": "AverageLoad",
                "metric": {
                    "kind": "Resource",
                    "name": "cpu"
                },
                "lowerLoadThreshold": "0.2",
                "upperLoadThreshold": "0.8",
                "scaleIntervalInSeconds": "60"
            },
            "mechanism": {
                "kind": "AddRemoveReplica",
                "minCount": "1",
                "maxCount": "40",
                "scaleIncrement": "1"
            }
        }
        ],        
        ...
    }
    }
]
}
}
```

## <a name="define-an-autoscale-policy-in-a-serviceyaml-resource-file"></a>Definición de una directiva de escalabilidad automática en un archivo de recursos service.yaml
En el ejemplo siguiente se muestra una directiva de escalado automático en un archivo de recursos de servicio (YAML).  La directiva de escalado automático se declara como una propiedad del servicio que se va a escalar.  En este ejemplo, se define un desencadenador de carga promedio de CPU.  El mecanismo se desencadenará si la carga de CPU promedio de todas las instancias implementadas cae por debajo del 0,2 (20 %) o sube por encima del 0,8 (80 %).  La carga de CPU se comprueba cada 60 segundos.  El mecanismo de escalado se define para agregar o quitar instancias si se desencadenada la directiva.  Las instancias de servicio se agregarán o quitarán en incrementos de uno.  También se definen un recuento mínimo de 1 instancia y un recuento máximo de 40 instancias.

```yaml
## Service definition ##
application:
  schemaVersion: 1.0.0-preview2
  name: WorkerApp
  properties:
    services:
      - name: WorkerService
        properties:
          description: WorkerService description.
          osType: Linux
          codePackages:
            ...
          replicaCount: 1
          autoScalingPolicies:
            - name: AutoScaleWorkerRule
              trigger:
                kind: AverageLoad
                metric:
                  kind: Resource
                  name: cpu
                lowerLoadThreshold: 0.2
                upperLoadThreshold: 0.8
                scaleIntervalInSeconds: 60
              mechanism:
                kind: AddRemoveReplica
                minCount: 1
                maxCount: 40
                scaleIncrement: 1
          ...
```

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo [escalar un servicio de manera manual](service-fabric-mesh-tutorial-template-scale-services.md)
