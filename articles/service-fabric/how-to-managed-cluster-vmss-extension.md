---
title: Incorporación de una extensión de conjunto de escalado de máquinas virtuales a un tipo de nodo de clúster administrado de Service Fabric (versión preliminar)
description: Aquí le mostramos cómo agregar una extensión de conjunto de escalado de máquinas virtuales a un tipo de nodo de clúster administrado de Service Fabric
ms.topic: article
ms.date: 09/28/2020
ms.openlocfilehash: 10487bad4fce41c68b4e2cb90c311b986d709eee
ms.sourcegitcommit: b48e8a62a63a6ea99812e0a2279b83102e082b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2020
ms.locfileid: "91410279"
---
# <a name="add-a-virtual-machine-scale-set-extension-to-a-service-fabric-managed-cluster-node-type-preview"></a>Incorporación de una extensión de conjunto de escalado de máquinas virtuales a un tipo de nodo de clúster administrado de Service Fabric (versión preliminar)

Cada tipo de nodo de un clúster administrado de Service Fabric se encuentra respaldado por un conjunto de escalado de máquinas virtuales. De este modo, puede agregar [extensiones de conjunto de escalado de máquinas virtuales](../virtual-machines/extensions/overview.md) a los tipos de nodo de clúster administrado de Service Fabric.

Puede agregar una extensión de conjunto de escalado de máquinas virtuales a un tipo de nodo mediante el comando de PowerShell [Add-AzServiceFabricManagedNodeTypeVMExtension](https://docs.microsoft.com/powershell/module/az.servicefabric/add-azservicefabricmanagednodetypevmextension?view=azps-4.7.0&preserve-view=true).

También puede agregar una extensión del conjunto de escalado de máquinas virtuales en un tipo de nodo de clúster administrado por Service Fabric en la plantilla de Azure Resource Manager, por ejemplo:

```json
{
    "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
    "apiVersion": "[variables('sfApiVersion')]",
    "name": "[concat(parameters('clusterName'), '/', parameters('nodeTypeName'))]",
    "dependsOn": [
        "[concat('Microsoft.ServiceFabric/managedclusters/', parameters('clusterName'))]"
    ],
    "location": "[resourceGroup().location]",
    "properties": {
        "isPrimary": true,
        "vmInstanceCount": 3,
        "dataDiskSizeGB": 100,
        "vmSize": "Standard_D2",
        "vmImagePublisher": "MicrosoftWindowsServer",
        "vmImageOffer": "WindowsServer",
        "vmImageSku": "2019-Datacenter",
        "vmImageVersion": "latest",
        "vmExtensions": [{
            "name": "ExtensionA",
            "properties": {
                "publisher": "ExtensionA.Publisher",
                "type": "KeyVaultForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                }
            }
        }]
    }
}
```

Para obtener más información sobre la configuración de los tipos de nodo de clúster administrados por Service Fabric, consulte [Tipo de nodo de clúster administrado](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2020-01-01-preview/managedclusters/nodetypes).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de los clústeres administrados de Service Fabric, consulte:

> [!div class="nextstepaction"]
> [Preguntas más frecuentes sobre los clústeres administrados de Service Fabric](./faq-managed-cluster.md)
