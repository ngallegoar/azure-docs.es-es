---
title: Referencia a una red virtual existente en una plantilla de conjunto de escalado de Azure
description: Obtenga información sobre cómo agregar una red virtual a una plantilla de un conjunto de escalado de máquinas virtuales de Microsoft Azure existente
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: e725e75b8b19fd8b3295226639b5e5aeb3736e34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76275547"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Incorporación de una referencia a una red virtual existente en una plantilla de conjunto de escalado de Azure

En este artículo se muestra cómo modificar la [plantilla de conjunto de escalado básico](virtual-machine-scale-sets-mvss-start.md) para realizar la implementación en una red virtual existente en lugar de crear una nueva.

## <a name="change-the-template-definition"></a>Cambio de la definición de la plantilla

En un [artículo anterior](virtual-machine-scale-sets-mvss-start.md) creamos una plantilla de conjunto de escalado básico. Ahora usaremos esa plantilla anterior y la modificaremos para crear una plantilla que implementa un conjunto de escalado en una red virtual existente. 

Primero, agregue un parámetro `subnetId`. Esta cadena se pasa a la configuración del conjunto de escalado, lo que permite que el conjunto de escalado identifique la subred creada anteriormente en la que implementar las máquinas virtuales. Esta cadena debe tener el formato: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.

Por ejemplo, para implementar el conjunto de escalado en una red virtual existente con el nombre `myvnet`, la subred `mysubnet`, el grupo de recursos `myrg` y la suscripción `00000000-0000-0000-0000-000000000000`, el valor de subnetId sería: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

A continuación, elimine el recurso de red virtual de la matriz `resources`, puesto que usa una red virtual existente y no necesita implementar una nueva.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2018-11-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

La red virtual ya existe antes de que se implemente la plantilla, así que no es necesario especificar una cláusula dependsOn entre el conjunto de escalado y la red virtual. Elimine las líneas siguientes:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Por último, pase el parámetro `subnetId` definido por el usuario (en lugar de usar `resourceId` para obtener el identificador de una red virtual en la misma implementación, que es lo que hace la plantilla de conjunto de escalado viable básico).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
