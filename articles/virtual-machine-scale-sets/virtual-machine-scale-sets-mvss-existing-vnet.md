---
title: Referencia a una red virtual existente en una plantilla de conjunto de escalado de Azure
description: Obtenga información sobre cómo agregar una red virtual a una plantilla de un conjunto de escalado de máquinas virtuales de Microsoft Azure existente
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: networking
ms.date: 04/26/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: fab6e6742fa43e1e38ee661b67896ae4aa11b3ed
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83124829"
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
