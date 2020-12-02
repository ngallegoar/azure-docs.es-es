---
title: Variables en plantillas
description: Se describe cómo definir variables en una plantilla de Azure Resource Manager.
ms.topic: conceptual
ms.date: 11/24/2020
ms.openlocfilehash: 7758451747503cb8c42566dc081a91f84bafa375
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95916010"
---
# <a name="variables-in-azure-resource-manager-template"></a>Variables en una plantilla de Azure Resource Manager

En este artículo se describe cómo definir y usar variables en una plantilla de Azure Resource Manager. Las variables se utilizan para simplificar la plantilla. En lugar de repetir expresiones complicadas en toda la plantilla, defina una variable que contenga la expresión complicada. A continuación, haga referencia a esa variable en toda la plantilla tanto como sea necesario.

Resource Manager resuelve las variables antes de iniciar las operaciones de implementación. Siempre que la variable se use en la plantilla, Resource Manager la reemplaza por el valor resuelto.

El formato de cada variable debe coincidir con uno de los [tipos de datos](template-syntax.md#data-types).

## <a name="define-variable"></a>Definición de la variable

El ejemplo siguiente muestra una variable de definición. Crea un valor de cadena para un nombre de cuenta de almacenamiento. Usa varias funciones de plantilla para obtener un valor de parámetro y lo concatena a una cadena única.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

La función no puede usar la función [reference](template-functions-resource.md#reference) ni ninguna de las funciones [list](template-functions-resource.md#list) de la sección de variables. Estas funciones obtienen el estado de tiempo de ejecución de un recurso y no se pueden ejecutar antes de la implementación cuando se resuelven variables.

## <a name="use-variable"></a>Uso de la variable

En la plantilla, debe hacer referencia al valor del parámetro con la función [variables](template-functions-deployment.md#variables). En el ejemplo siguiente se muestra cómo usar la variable para una propiedad de recurso.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

## <a name="configuration-variables"></a>Variables de configuración

Puede definir variables que contengan valores relacionados para configurar un entorno. La variable se define como un objeto con los valores. En el ejemplo siguiente se muestra un objeto que contiene valores para dos entornos: **test** y **prod**.

```json
"variables": {
  "environmentSettings": {
    "test": {
      "instanceSize": "Small",
      "instanceCount": 1
    },
    "prod": {
      "instanceSize": "Large",
      "instanceCount": 4
    }
  }
},
```

En parámetros, se crea un valor que indica que valores de configuración usar.

```json
"parameters": {
  "environmentName": {
    "type": "string",
    "allowedValues": [
      "test",
      "prod"
    ]
  }
},
```

Para recuperar la configuración del entorno especificado, use la variable y el parámetro conjuntamente.

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="example-templates"></a>Plantillas de ejemplo

En los siguientes ejemplos se muestran escenarios para usar variables.

|Plantilla  |Descripción  |
|---------|---------|
| [definiciones de variable](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Muestra los diferentes tipos de variables. La plantilla no implementa ningún recurso. Genera valores de variable y devuelve esos valores. |
| [variable de configuración](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Muestra el uso de una variable que define los valores de configuración. La plantilla no implementa ningún recurso. Genera valores de variable y devuelve esos valores. |
| [reglas de seguridad de red](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) y [archivo de parámetro](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Genera una matriz en el formato correcto para asignar las reglas de seguridad a un grupo de seguridad de red. |

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las propiedades disponibles para variables, consulte [Información sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
* Para obtener recomendaciones sobre cómo crear variables, consulte [Procedimientos recomendados: variables](template-best-practices.md#variables).
