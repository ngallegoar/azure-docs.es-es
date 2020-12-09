---
title: Parámetros en plantillas
description: Se describe cómo definir parámetros en una plantilla de Azure Resource Manager (plantilla de ARM).
ms.topic: conceptual
ms.date: 11/24/2020
ms.openlocfilehash: 4ac1aeb579040b35b2a9b4cb90fb5687f91ebd3b
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96353517"
---
# <a name="parameters-in-arm-templates"></a>Parámetros en plantillas de ARM

En este artículo se describe cómo definir y usar parámetros en una plantilla de Azure Resource Manager (plantilla de ARM). Si proporciona valores diferentes para los parámetros, podrá volver a usar una plantilla para distintos entornos.

Resource Manager resuelve los valores de parámetro antes de iniciar las operaciones de implementación. Siempre que el parámetro se use en la plantilla, Resource Manager lo reemplaza por el valor resuelto.

Cada parámetro debe establecerse en uno de los [tipos de datos](template-syntax.md#data-types).

## <a name="define-parameter"></a>Definición del parámetro

En el ejemplo siguiente se muestra una definición simple de parámetro. Define un parámetro denominado **storageSKU**. El parámetro es un valor de cadena y solo acepta valores que son válidos para su uso previsto. Cuando no se proporciona ningún valor durante la implementación, el parámetro utiliza un valor predeterminado.

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }
}
```

## <a name="use-parameter"></a>Uso del parámetro

En la plantilla, debe hacer referencia al valor del parámetro con la función [parameters](template-functions-deployment.md#parameters). En el ejemplo siguiente, el valor del parámetro se usa para establecer la SKU para la cuenta de almacenamiento.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="template-functions"></a>Funciones de plantillas

Cuando se especifica el valor predeterminado de un parámetro, puede usar la mayoría de las funciones de plantilla. Puede usar otro valor de parámetro para compilar un valor predeterminado. La plantilla siguiente muestra el uso de funciones en el valor predeterminado. Cuando no se proporciona ningún nombre para el sitio, crea un valor de cadena único y lo anexa al **sitio**. Cuando no se proporciona ningún nombre para el plan de host, toma el valor del sitio y anexa **-plan**.

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

La función no puede usar la función [reference](template-functions-resource.md#reference) ni ninguna de las funciones [list](template-functions-resource.md#list) de la sección de parámetros. Estas funciones obtienen el estado de tiempo de ejecución de un recurso y no se pueden ejecutar antes de la implementación cuando se resuelven parámetros.

## <a name="objects-as-parameters"></a>Objetos como parámetros

Puede ser más fácil organizar los valores relacionados pasándolos como objetos. Con este enfoque también se reduce el número de parámetros de la plantilla.

En el ejemplo siguiente se muestra un parámetro que es un objeto. El valor predeterminado muestra las propiedades esperadas para el objeto.

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Haga referencia a las propiedades del objeto con el operador punto.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2015-06-15",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>Plantillas de ejemplo

En los siguientes ejemplos se muestran escenarios para usar parámetros.

|Plantilla  |Descripción  |
|---------|---------|
|[parámetros con funciones para los valores predeterminados](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Muestra cómo utilizar las funciones de plantilla al definir valores predeterminados para parámetros. La plantilla no implementa ningún recurso. Genera valores de parámetro y devuelve dichos valores. |
|[objeto de parámetro](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Muestra cómo utilizar un objeto para un parámetro. La plantilla no implementa ningún recurso. Genera valores de parámetro y devuelve dichos valores. |

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las propiedades disponibles para parámetros, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
* Para más información sobre cómo pasar valores de parámetro como un archivo, consulte [Creación de un archivo de parámetros de Resource Manager](parameter-files.md).
* Para obtener recomendaciones sobre cómo crear parámetros, consulte [Procedimientos recomendados: parámetros](template-best-practices.md#parameters).
