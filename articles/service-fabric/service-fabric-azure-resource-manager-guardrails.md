---
title: Límites de protecciones en la implementación de Azure Resource Manager de Service Fabric
description: En este artículo se proporciona información general sobre los errores comunes que se producen al implementar un clúster de Service Fabric mediante Azure Resource Manager y cómo evitarlos.
services: service-fabric
documentationcenter: .net
author: peterpogorski
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: pepogors
ms.openlocfilehash: a61b0cf30ca46eb77837eb09d6a9a0b6f30e89a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368584"
---
# <a name="service-fabric-guardrails"></a>Protecciones de Service Fabric 
Al implementar un clúster de Service Fabric, se colocan protecciones, lo que producirá un error en una implementación de Azure Resource Manager en el caso de que haya una configuración de clúster no válida. En las secciones siguientes se proporciona información general sobre los problemas comunes de configuración de clústeres y los pasos necesarios para mitigarlos. 

## <a name="durability-mismatch"></a>Error de coincidencia de durabilidad
### <a name="overview"></a>Información general
El valor de durabilidad de un tipo de nodo de Service Fabric se define en dos secciones diferentes de una plantilla de Azure Resource Manager: la sección de la extensión del conjunto de escalado de máquinas virtuales del recurso del conjunto de escalado de máquinas virtuales y la sección del tipo de nodo del recurso de clúster de Service Fabric. El valor de durabilidad de estas secciones debe coincidir; de lo contrario, se producirá un error en la implementación del recurso.

En la siguiente sección se incluye un ejemplo de un error de coincidencia de durabilidad entre el valor de durabilidad de la extensión del conjunto de escalado de máquinas virtuales y el valor de durabilidad del tipo de nodo de Service Fabric:  

**Valor de durabilidad del conjunto de escalado de máquinas virtuales**
```json 
{
  "extensions": [
    {
      "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
      "properties": {
        "type": "ServiceFabricNode",
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
          "durabilityLevel": "Silver",
        }
      }
    }
  ]
}
```

**Valor de durabilidad del tipo de nodo de Service Fabric** 
```json
{
  "nodeTypes": [
    {
      "name": "[variables('vmNodeType0Name')]",
      "durabilityLevel": "Bronze",
      "isPrimary": true,
      "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    }
  ]
}
```

### <a name="error-messages"></a>Mensajes de error
* El valor de durabilidad del conjunto de escalado de máquinas virtuales no coincide con el nivel de durabilidad del tipo de nodo actual de Service Fabric
* La durabilidad del conjunto de escalado de máquinas virtuales no coincide con el nivel de durabilidad del tipo de nodo de Service Fabric de destino
* La durabilidad del conjunto de escalado de máquinas virtuales coincide con el nivel de durabilidad de Service Fabric actual o el nivel de durabilidad del tipo de nodo de Service Fabric de destino 

### <a name="mitigation"></a>Mitigación
Para solucionar un error de coincidencia de durabilidad, que se indica mediante cualquiera de los mensajes de error anteriores:
1. Actualice el nivel de durabilidad en la extensión del conjunto de escalado de máquinas virtuales o en la sección del tipo de nodo de Service Fabric de la plantilla de Azure Resource Manager para asegurarse de que los valores coinciden.
2. Vuelva a implementar la plantilla de Azure Resource Manager con los valores actualizados.


## <a name="seed-node-deletion"></a>Eliminación de nodo de inicialización 
### <a name="overview"></a>Información general
Un clúster de Service Fabric tiene una propiedad de [nivel de confiabilidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) que se usa para determinar el número de réplicas de los servicios del sistema que se ejecutan en el tipo de nodo principal del clúster. El número de réplicas necesarias determinará el número mínimo de nodos que se deben mantener en el tipo de nodo principal del clúster. Si el número de nodos en el tipo de nodo principal es inferior al mínimo necesario para el nivel de confiabilidad, el clúster se volverá inestable.  

### <a name="error-messages"></a>Mensajes de error 
Se ha detectado una operación de eliminación del nodo de inicialización y se rechazará. 
* Esta operación daría lugar a que solo {0} nodos de inicialización potenciales permanecieran en el clúster, mientras que se necesitan {1} como mínimo.
* Quitar {0} nodos de inicialización de {1} daría lugar a que el clúster se apagara debido a la pérdida del cuórum de nodos de inicialización. El número máximo de nodos de inicialización que se pueden quitar a la vez es de {2}.
 
### <a name="mitigation"></a>Mitigación 
Asegúrese de que el tipo de nodo principal tenga suficientes máquinas virtuales para la confiabilidad especificada en el clúster. No podrá quitar una máquina virtual si esto hace que el conjunto de escalado de máquinas virtuales quede por debajo del número mínimo de nodos para el nivel de confiabilidad establecido.
* Si el nivel de confiabilidad se especifica correctamente, asegúrese de tener nodos suficientes en el tipo de nodo principal según sea necesario para el nivel de confiabilidad. 
* Si el nivel de confiabilidad es incorrecto, inicie un cambio en el recurso de Service Fabric para reducir primero el nivel de confiabilidad antes de iniciar las operaciones con el conjunto de escalado de máquinas virtuales, y espere a que se complete.
* Si el nivel de confiabilidad es bronce, siga estos [pasos](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#manually-remove-vms-from-a-node-typevirtual-machine-scale-set) para reducir verticalmente el clúster correctamente.

## <a name="next-steps"></a>Pasos siguientes
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster independiente con Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Crear un clúster Linux](service-fabric-cluster-creation-via-portal.md)
* Solución de problemas de Service Fabric: [Guías de solución de problemas](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)
