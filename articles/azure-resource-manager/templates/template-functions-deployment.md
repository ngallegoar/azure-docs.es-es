---
title: 'Funciones de plantilla: implementación'
description: Describe las funciones para usar en una plantilla de Azure Resource Manager para recuperar información de implementación.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 86a1d3d7e05fedacd7a3c044ecab241ca9d059c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156334"
---
# <a name="deployment-functions-for-arm-templates"></a>Funciones de implementación para plantillas de ARM 

Resource Manager proporciona las funciones siguientes para obtener los valores relacionados con la implementación actual de la plantilla de Azure Resource Manager:

* [deployment](#deployment)
* [environment](#environment)
* [parameters](#parameters)
* [variables](#variables)

Para obtener valores de recursos, grupos de recursos o suscripciones, consulte [Funciones de recursos](template-functions-resource.md).

## <a name="deployment"></a>implementación

`deployment()`

Devuelve información sobre la operación de implementación actual.

### <a name="return-value"></a>Valor devuelto

Esta función devuelve el objeto pasado durante la implementación. Las propiedades del objeto devuelto varían en función de si el objeto de implementación se ha pasado como un vínculo o como un objeto en línea. Cuando se pasa el objeto de implementación en línea, como cuando se usa el parámetro **-TemplateFile** en Azure PowerShell para orientarlo a un archivo local, el objeto devuelto tiene el formato siguiente:

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Cuando el objeto se pasa como un vínculo, como cuando se usa el parámetro **-TemplateUri** para orientarlo a un objeto remoto, se devuelve el objeto en el formato siguiente: 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Al [implementar en una suscripción a Azure](deploy-to-subscription.md), en lugar de un grupo de recursos, el objeto devuelto incluye una propiedad `location`. La propiedad de ubicación se incluye al implementar una plantilla local o externa.

### <a name="remarks"></a>Observaciones

Puede usar deployment() para establecer un vínculo con otra plantilla basada en el identificador URI de la plantilla primaria.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

Si vuelve a implementar una plantilla desde el historial de implementación en el portal, la plantilla se implementará como un archivo local. La propiedad `templateLink` no se devuelve en la función de la implementación. Si la plantilla se basa en `templateLink` para construir un vínculo con otra plantilla, no use el portal para volver a implementarla. En su lugar, use los comandos que utilizó originalmente para implementar la plantilla.

### <a name="example"></a>Ejemplo

La [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deployment.json) siguiente devuelve el objeto de implementación:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

El ejemplo anterior devuelve el objeto siguiente:

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

Para una plantilla de nivel de suscripción que usa la función de implementación, consulte [subscription deployment function](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deploymentsubscription.json) (función de implementación de la suscripción). Se implementa con los comandos `az deployment create` o `New-AzDeployment`.

## <a name="environment"></a>Environment

`environment()`

Devuelve información sobre el entorno de Azure que se usa para la implementación.

### <a name="return-value"></a>Valor devuelto

Esta función devuelve las propiedades del entorno de Azure actual. En el ejemplo siguiente se muestran las propiedades de Azure global. Las nubes soberanas pueden devolver propiedades ligeramente distintas.

```json
{
  "name": "",
  "gallery": "",
  "graph": "",
  "portal": "",
  "graphAudience": "",
  "activeDirectoryDataLake": "",
  "batch": "",
  "media": "",
  "sqlManagement": "",
  "vmImageAliasDoc": "",
  "resourceManager": "",
  "authentication": {
    "loginEndpoint": "",
    "audiences": [
      "",
      ""
    ],
    "tenant": "",
    "identityProvider": ""
  },
  "suffixes": {
    "acrLoginServer": "",
    "azureDatalakeAnalyticsCatalogAndJob": "",
    "azureDatalakeStoreFileSystem": "",
    "azureFrontDoorEndpointSuffix": "",
    "keyvaultDns": "",
    "sqlServerHostname": "",
    "storage": ""
  }
}
```

### <a name="example"></a>Ejemplo

La plantilla de ejemplo siguiente devuelve el objeto de entorno.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "environmentOutput": {
            "value": "[environment()]",
            "type" : "object"
        }
    }
}
```

En el ejemplo anterior se devuelve el objeto siguiente cuando se implementa en Azure global:

```json
{
  "name": "AzureCloud",
  "gallery": "https://gallery.azure.com/",
  "graph": "https://graph.windows.net/",
  "portal": "https://portal.azure.com",
  "graphAudience": "https://graph.windows.net/",
  "activeDirectoryDataLake": "https://datalake.azure.net/",
  "batch": "https://batch.core.windows.net/",
  "media": "https://rest.media.azure.net",
  "sqlManagement": "https://management.core.windows.net:8443/",
  "vmImageAliasDoc": "https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json",
  "resourceManager": "https://management.azure.com/",
  "authentication": {
    "loginEndpoint": "https://login.windows.net/",
    "audiences": [
      "https://management.core.windows.net/",
      "https://management.azure.com/"
    ],
    "tenant": "common",
    "identityProvider": "AAD"
  },
  "suffixes": {
    "acrLoginServer": ".azurecr.io",
    "azureDatalakeAnalyticsCatalogAndJob": "azuredatalakeanalytics.net",
    "azureDatalakeStoreFileSystem": "azuredatalakestore.net",
    "azureFrontDoorEndpointSuffix": "azurefd.net",
    "keyvaultDns": ".vault.azure.net",
    "sqlServerHostname": ".database.windows.net",
    "storage": "core.windows.net"
  }
}
```

## <a name="parameters"></a>parámetros

`parameters(parameterName)`

Devuelve un valor de parámetro. El nombre del parámetro especificado debe definirse en la sección de parámetros de la plantilla.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| parameterName |Sí |string |El nombre del parámetro que se va a devolver. |

### <a name="return-value"></a>Valor devuelto

Valor del parámetro especificado.

### <a name="remarks"></a>Observaciones

Por lo general, se usan parámetros para establecer los valores de recurso. En el ejemplo siguiente se establece el nombre del sitio web en el valor del parámetro pasado durante la implementación.

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a>Ejemplo

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/parameters.json) siguiente se muestra un uso simplificado de la función de los parámetros.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| stringOutput | String | opción 1 |
| intOutput | Int | 1 |
| objectOutput | Object | {"one": "a", "two": "b"} |
| arrayOutput | Array | [1, 2, 3] |
| crossOutput | String | opción 1 |

Para más información sobre el uso de los parámetros, consulte [Parámetros en plantillas de Azure Resource Manager](template-parameters.md).

## <a name="variables"></a>variables

`variables(variableName)`

Devuelve el valor de variable. El nombre de la variable especificada debe definirse en la sección de variables de la plantilla.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| variableName |Sí |String |El nombre de la variable que se va a devolver. |

### <a name="return-value"></a>Valor devuelto

Valor de la variable especificada.

### <a name="remarks"></a>Observaciones

Por lo general, para simplificar la plantilla se usan variables para crear valores complejos de una sola vez. En el ejemplo siguiente se crea un nombre único para una cuenta de almacenamiento.

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a>Ejemplo

La [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/variables.json) siguiente devuelve distintos valores de variable.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
          }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| exampleOutput1 | String | myVariable |
| exampleOutput2 | Array | [1, 2, 3, 4] |
| exampleOutput3 | String | myVariable |
| exampleOutput4 |  Object | {"property1": "value1", "property2": "value2"} |

Para más información sobre el uso de las variables, consulte [Variables en plantillas de Azure Resource Manager](template-variables.md).

## <a name="next-steps"></a>Pasos siguientes
* Para obtener una descripción de las secciones de una plantilla de Azure Resource Manager, vea [Creación de plantillas de Azure Resource Manager](template-syntax.md).
* Para combinar varias plantillas, vea [Uso de plantillas vinculadas con Azure Resource Manager](linked-templates.md).
* Para iterar una cantidad de veces específica al crear un tipo de recurso, vea [Creación de varias instancias de recursos en el Administrador de recursos de Azure](copy-resources.md).
* Para saber cómo implementar la plantilla que creó, consulte [Implementación de una aplicación con una plantilla de Azure Resource Manager](deploy-powershell.md).

