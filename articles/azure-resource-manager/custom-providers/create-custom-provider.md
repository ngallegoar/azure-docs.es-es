---
title: Creación de un proveedor de recursos
description: Se describe cómo crear un proveedor de recursos e implementar sus tipos de recursos personalizados.
author: MSEvanhi
ms.topic: tutorial
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: 393993a44c860525b9bd9a540ed7afff78e5b93c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75648512"
---
# <a name="quickstart-create-custom-provider-and-deploy-custom-resources"></a>Inicio rápido: Creación de un proveedor de recursos e implementación de recursos personalizados

En este inicio rápido, creará su propio proveedor de recursos e implementará distintos tipos de recursos personalizados para dicho proveedor de recursos. Para más información sobre los proveedores personalizados, vea la [introducción a los proveedores personalizados de Azure en versión preliminar](overview.md).

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos de este inicio rápido es preciso llamar a operaciones de REST. Hay [distintas formas de enviar solicitudes REST](/rest/api/azure/). Si no dispone de una herramienta para las operaciones de REST, instale [ARMClient](https://github.com/projectkudu/ARMClient). Se trata de una herramienta de línea de comandos de código abierto que simplifica la invocación de la API de Azure Resource Manager.

## <a name="deploy-custom-provider"></a>Implementación de un proveedor personalizado

Para configurar el proveedor personalizado, implemente una [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/custom-providers/customprovider.json) en su suscripción de Azure.

Después de implementar la plantilla, la suscripción tiene los siguientes recursos:

* Aplicación de función con las operaciones para los recursos y las acciones.
* Cuenta de almacenamiento para almacenar los usuarios que se crean mediante el proveedor personalizado.
* Proveedor personalizado que define las acciones y los tipos de recursos personalizados. Usa el punto de conexión de la aplicación de función para enviar las solicitudes.
* Recurso personalizado del proveedor personalizado.

Para implementar el proveedor personalizado con PowerShell, use:

```azurepowershell-interactive
$rgName = "<resource-group-name>"
$funcName = "<function-app-name>"

New-AzResourceGroup -Name $rgName -Location eastus
New-AzResourceGroupDeployment -ResourceGroupName $rgName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/custom-providers/customprovider.json `
  -funcname $funcName
```

También puede implementar la solución con el siguiente botón:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-docs-json-samples%2Fmaster%2Fcustom-providers%2Fcustomprovider.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="view-custom-provider-and-resource"></a>Ver recurso y proveedor personalizado

En el portal, el proveedor personalizado es un tipo de recurso oculto. Para confirmar que se ha implementado el proveedor de recursos, navegue hasta el grupo de recursos. Seleccione la opción **Mostrar tipos ocultos**.

![Mostrar tipos de recursos ocultos](./media/create-custom-provider/show-hidden.png)

Para ver el tipo de recurso personalizado que ha implementado, use la operación GET en el tipo de recurso.

```
GET https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users?api-version=2018-09-01-preview
```

Con ARMClient, use:

```powershell
$subID = (Get-AzContext).Subscription.Id
$requestURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users?api-version=2018-09-01-preview"

armclient GET $requestURI
```

Recibirá la respuesta:

```json
{
  "value": [
    {
      "properties": {
        "provisioningState": "Succeeded",
        "FullName": "Santa Claus",
        "Location": "NorthPole"
      },
      "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/santa",
      "name": "santa",
      "type": "Microsoft.CustomProviders/resourceProviders/users"
    }
  ]
}
```

## <a name="call-action"></a>Acción llamar

El proveedor personalizado también tiene una acción denominada **ping**. El código que procesa la solicitud se implementa en la aplicación de función. La acción ping responde con un saludo.

Para enviar una solicitud ping, utilice la operación POST en su proveedor personalizado.

```
POST https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/ping?api-version=2018-09-01-preview
```

Con ARMClient, use:

```powershell
$pingURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/ping?api-version=2018-09-01-preview"

armclient POST $pingURI
```

Recibirá la respuesta:

```json
{
  "pingcontent": {
    "source": "<function-name>.azurewebsites.net"
  },
  "message": "hello <function-name>.azurewebsites.net"
}
```

## <a name="create-resource-type"></a>Creación de un tipo de recurso

Para crear el tipo de recurso personalizado, puede implementar el recurso en una plantilla. Este método se muestra en la plantilla que se ha implementado en este inicio rápido. También puede enviar una solicitud PUT para el tipo de recurso.

```
PUT https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/<resource-name>?api-version=2018-09-01-preview

{"properties":{"FullName": "Test User", "Location": "Earth"}}
```

Con ARMClient, use:

```powershell
$addURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users/testuser?api-version=2018-09-01-preview"
$requestBody = "{'properties':{'FullName': 'Test User', 'Location': 'Earth'}}"

armclient PUT $addURI $requestBody
```

Recibirá la respuesta:

```json
{
  "properties": {
    "provisioningState": "Succeeded",
    "FullName": "Test User",
    "Location": "Earth"
  },
  "id": "/subscriptions/<sub-ID>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/testuser",
  "name": "testuser",
  "type": "Microsoft.CustomProviders/resourceProviders/users"
}
```

## <a name="next-steps"></a>Pasos siguientes

Para consultar una introducción a los proveedores de recursos, vea la [introducción a los proveedores personalizados de Azure en versión preliminar](overview.md).
