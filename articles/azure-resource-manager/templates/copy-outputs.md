---
title: Definición de varias instancias de un valor de salida
description: Utilice la operación de copia en una plantilla de Azure Resource Manager para realizar varias iteraciones al devolver un valor desde una implementación.
ms.topic: conceptual
ms.date: 04/17/2020
ms.openlocfilehash: 0315af2f083285c4704b08fec608341b6f0b2231
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617830"
---
# <a name="output-iteration-in-arm-templates"></a>Iteración de salida en las plantillas de ARM

En este artículo se muestra cómo crear más de un valor para una salida en la plantilla de Azure Resource Manager (ARM). Al agregar el elemento **copy** a la sección de salidas de la plantilla, puede devolver de forma dinámica varios elementos durante la implementación.

También puede utilizar el elemento copy con [recursos](copy-resources.md), [propiedades de un recurso](copy-properties.md) y [variables](copy-variables.md).

## <a name="outputs-iteration"></a>Iteración de salidas

El elemento copy tiene el siguiente formato general:

```json
"copy": {
  "count": <number-of-iterations>,
  "input": <values-for-the-output>
}
```

La propiedad **count** especifica el número de iteraciones que desea para el valor de salida.

La propiedad **input** especifica las propiedades que desea repetir. Tiene que crear una matriz de elementos construida a partir del valor de la propiedad **input**. Puede tratarse de una propiedad única (como una cadena) o de un objeto con varias propiedades.

En el ejemplo siguiente se crea un número variable de cuentas de almacenamiento y se devuelve un punto de conexión para cada cuenta de almacenamiento:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageEndpoints": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
            }
        }
    }
}
```

La plantilla anterior devuelve una matriz con los siguientes valores:

```json
[
    "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
    "https://1storagecfrbqnnmpeudi.blob.core.windows.net/"
]
```

En el ejemplo siguiente se devuelven tres propiedades de las nuevas cuentas de almacenamiento.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageInfo": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": {
                    "id": "[reference(concat(copyIndex(), variables('baseName')), '2019-04-01', 'Full').resourceId]",
                    "blobEndpoint": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]",
                    "status": "[reference(concat(copyIndex(), variables('baseName'))).statusOfPrimary]"
                }
            }
        }
    }
}
```

El ejemplo anterior devuelve una matriz con los valores siguientes:

```json
[
    {
        "id": "Microsoft.Storage/storageAccounts/0storagecfrbqnnmpeudi",
        "blobEndpoint": "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
        "status": "available"
    },
    {
        "id": "Microsoft.Storage/storageAccounts/1storagecfrbqnnmpeudi",
        "blobEndpoint": "https://1storagecfrbqnnmpeudi.blob.core.windows.net/",
        "status": "available"
    }
]
```

## <a name="next-steps"></a>Pasos siguientes

* Para seguir los pasos de un tutorial, consulte [Tutorial: Creación de varias instancias de recursos con plantillas de ARM](template-tutorial-create-multiple-instances.md).
* Para otros usos del elemento copy, consulte:
  * [Iteración de recursos en las plantillas de ARM](copy-resources.md)
  * [Iteración de propiedades en las plantillas de ARM](copy-properties.md)
  * [Iteración de variables en las plantillas de ARM](copy-variables.md)
* Para obtener información sobre las secciones de una plantilla, consulte el artículo sobre cómo [crear plantillas de ARM](template-syntax.md).
* Para obtener información sobre cómo implementar su plantilla, consulte el artículo sobre cómo [implementar una aplicación con la plantilla de ARM](deploy-powershell.md).

