---
title: Tipos de nodo y conjuntos de escalado de máquinas virtuales
description: Obtenga información sobre cómo los tipos de nodos de Azure Service Fabric se relacionan con los conjuntos de escalado de máquinas virtuales y cómo conectarse de forma remota a una instancia de conjunto de escalado o un nodo de clúster.
ms.topic: conceptual
ms.date: 03/23/2018
ms.author: pepogors
ms.custom: sfrev
ms.openlocfilehash: 03076a711041812f7587aa1c388b0889b49725d2
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82787140"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Tipos de nodos y conjuntos de escalado de máquinas virtuales de Azure Service Fabric

Los [conjuntos de escalado de máquinas virtuales](/azure/virtual-machine-scale-sets) son un recurso de Azure Compute. Puede usarlos para implementar y administrar una colección de máquinas virtuales como conjunto. Cada tipo de nodo que se define en un clúster de Azure Service Fabric configura exactamente un conjunto de escalado: varios tipos de nodo no pueden estar respaldados por el mismo conjunto de escalado y un tipo de nodo no debe estar respaldado (en la mayoría de los casos) por varios conjuntos de escalado. Una excepción es una situación poco frecuente del [escalado vertical](service-fabric-best-practices-capacity-scaling.md#vertical-scaling-considerations) de un tipo de nodo, cuando tiene temporalmente dos conjuntos de escalado con el mismo valor `nodeTypeRef`, mientras que las réplicas se migran desde el original al conjunto de escalado actualizado.

La extensión de máquina virtual *Microsoft.Azure.ServiceFabric* instala el runtime de Service Fabric en cada máquina virtual del conjunto de escalado. Cada tipo de nodo se puede escalar o reducir verticalmente de forma independiente; puede cambiar la SKU del sistema operativo que se ejecuta en cada nodo de clúster, tener diferentes conjuntos de puertos abiertos y usar distintas métricas de capacidad.

En la ilustración siguiente se muestra un clúster que tiene dos tipos de nodos llamados *FrontEnd* y *BackEnd*. A su vez, cada tipo de nodo tiene cinco nodos.

![Un clúster que tiene dos tipos de nodos][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Asignación de instancias de conjuntos de escalado de máquinas virtuales a los nodos

Como se muestra en la ilustración anterior, las instancias del conjunto de escalado empiezan por la instancia 0 y, a continuación, se van incrementando de uno en uno. La numeración se refleja en los nombres de los nodos. Por ejemplo, el nodo BackEnd_0 es la instancia 0 del conjunto de escalado de BackEnd. En concreto, este conjunto de escalado tiene cinco instancias, denominadas BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 y BackEnd_4.

Al escalar horizontalmente un conjunto de escalado, se crea una nueva instancia. El nuevo nombre de la instancia de conjunto de escalado será normalmente el nombre del conjunto de escalado seguido del número de instancia. En nuestro ejemplo, es BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Asignación de los equilibradores de carga del conjunto de escalado a los tipos de nodos y conjuntos de escalado

Si ha implementado el clúster en Azure Portal o ha usado la plantilla de Azure Resource Manager de ejemplo, obtendrá una lista de todos los recursos en un grupo de recursos. Puede ver los equilibradores de carga de cada conjunto de escalado o tipo de nodo. El nombre del equilibrador de carga usa el formato siguiente: **LB-&lt;nombre del tipo de nodo&gt;** . Por ejemplo: LB-sfcluster4doc-0, tal y como se muestra en la ilustración siguiente:

![Recursos][Resources]

## <a name="service-fabric-virtual-machine-extension"></a>Extensión de máquina virtual de Service Fabric

La extensión de máquina virtual de Service Fabric se utiliza para arrancar Service Fabric en Azure Virtual Machines y configurar la seguridad de nodo.

A continuación se incluye un fragmento de código de la extensión de máquina virtual de Service Fabric:

```json
"extensions": [
  {
    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
    "properties": {
      "type": "ServiceFabricLinuxNode",
      "autoUpgradeMinorVersion": true,
      "protectedSettings": {
        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
       },
       "publisher": "Microsoft.Azure.ServiceFabric",
       "settings": {
         "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
         "nodeTypeRef": "[variables('vmNodeType0Name')]",
         "durabilityLevel": "Silver",
         "enableParallelJobs": true,
         "nicPrefixOverride": "[variables('subnet0Prefix')]",
         "dataPath": "D:\\\\SvcFab",
         "certificate": {
           "commonNames": [
             "[parameters('certificateCommonName')]"
           ],
           "x509StoreName": "[parameters('certificateStoreValue')]"
         }
       },
       "typeHandlerVersion": "1.1"
     }
   },
```

Estas son las descripciones de la propiedad:

| **Nombre** | **Valores permitidos** | **Orientación o breve descripción** |
| --- | --- | --- | --- |
| name | string | Nombre único para la extensión |
| type | "ServiceFabricLinuxNode" o "ServiceFabricWindowsNode" | Identifica el sistema operativo donde arranca Service Fabric |
| autoUpgradeMinorVersion | true o false | Habilita la actualización automática de las versiones secundarias del tiempo de ejecución de SF |
| publisher | Microsoft.Azure.ServiceFabric | Nombre del editor de la extensión de Service Fabric |
| clusterEndpont | string | URI:PUERTO al punto de conexión de administración |
| nodeTypeRef | string | Nombre de nodeType |
| durabilityLevel | bronze, silver, gold, platinum | Tiempo permitido para pausar la infraestructura inmutable de Azure |
| enableParallelJobs | true o false | Habilita los trabajos paralelos del proceso, como quitar la VM y reiniciarla en el mismo conjunto de escalado en paralelo |
| nicPrefixOverride | string | Prefijo de la subred, como "10.0.0.0/24" |
| commonNames | string[] | Nombres comunes de los certificados de clúster instalados |
| x509StoreName | string | Nombre del almacén donde se encuentra el certificado de clúster instalado |
| typeHandlerVersion | 1.1 | Versión de la extensión. Se recomienda actualizar la versión clásica 1.0 de la extensión a 1.1. |
| dataPath | string | Ruta de acceso a la unidad que se usa para guardar el estado de datos de aplicaciones y servicios del sistema de Service Fabric.

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Descripción general de la característica "Deploy anywhere" y comparación con los clústeres administrados de Azure](service-fabric-deploy-anywhere.md).
* Obtenga más información sobre la [seguridad del clúster](service-fabric-cluster-security.md).
* [Conexión remota](service-fabric-cluster-remote-connect-to-azure-cluster-node.md) a la instancia específica del conjunto de escalado
* [Actualización de los valores del intervalo de puertos RDP](./scripts/service-fabric-powershell-change-rdp-port-range.md) en máquinas virtuales del clúster después de la implementación
* [Cambio de la contraseña y el nombre de usuario administrador](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) para máquinas virtuales del clúster

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
