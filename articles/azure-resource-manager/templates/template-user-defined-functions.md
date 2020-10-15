---
title: Funciones definidas por el usuario en plantillas
description: Se describe cómo definir y usar funciones definidas por el usuario en una plantilla de Azure Resource Manager.
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 69f4e98d389cc8dbe5cd3f4b628189676c501106
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84672942"
---
# <a name="user-defined-functions-in-azure-resource-manager-template"></a>Funciones definidas por el usuario de la plantilla de Azure Resource Manager

Dentro de la plantilla, puede crear sus propias funciones. Estas funciones están disponibles para su uso en la plantilla. Las funciones definidas por el usuario son independientes de las [funciones de plantilla estándares](template-functions.md) que están disponibles automáticamente dentro de la plantilla. Cree sus propias funciones cuando tenga expresiones complicadas que se usen repetidamente en la plantilla.

En este artículo se describe cómo agregar funciones definidas por el usuario a una plantilla de Azure Resource Manager.

## <a name="define-the-function"></a>Definición de la función

Las funciones requieren un valor de espacio de nombres para evitar conflictos de nomenclatura con las funciones de plantilla. En el ejemplo siguiente se muestra una función que devuelve un nombre único:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

## <a name="use-the-function"></a>Uso de la función

En el ejemplo siguiente se muestra una plantilla que incluye una función definida por el usuario. Esa función se utiliza para obtener un nombre único para una cuenta de almacenamiento. La plantilla tiene un parámetro denominado **storageNamePrefix** que se pasa como un parámetro a la función.

```json
{
 "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
 "contentVersion": "1.0.0.0",
 "parameters": {
   "storageNamePrefix": {
     "type": "string",
     "maxLength": 11
   }
 },
 "functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
 "resources": [
   {
     "type": "Microsoft.Storage/storageAccounts",
     "apiVersion": "2019-04-01",
     "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
     "location": "South Central US",
     "sku": {
       "name": "Standard_LRS"
     },
     "kind": "StorageV2",
     "properties": {
       "supportsHttpsTrafficOnly": true
     }
   }
 ]
}
```

## <a name="limitations"></a>Limitaciones

Al definir una función de usuario, hay algunas restricciones:

* La función no puede acceder a las variables.
* La función solo puede usar los parámetros que se definen en la función. Cuando usa la función [parameters](template-functions-deployment.md#parameters) dentro de una función definida por el usuario, los parámetros de esa función serán los que limiten sus acciones.
* La función no puede llamar a otras funciones definidas por el usuario.
* La función no puede usar la función [reference](template-functions-resource.md#reference) ni ninguna de las funciones [list](template-functions-resource.md#list).
* Los parámetros de la función no pueden tener valores predeterminados.


## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las propiedades disponibles para funciones definidas por el usuario, consulte [Información sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
* Para obtener una lista de las funciones de plantilla disponibles, consulte [Funciones de la plantilla de Azure Resource Manager](template-functions.md).
