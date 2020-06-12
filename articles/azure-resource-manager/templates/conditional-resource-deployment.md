---
title: Implementación condicional con plantillas
description: En este artículo se describe cómo realizar la implementación condicional de un recurso en una plantilla de Azure Resource Manager.
ms.topic: conceptual
ms.date: 06/01/2020
ms.openlocfilehash: effa7fe6ee1393e44a124bc087609da5d4898210
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84259327"
---
# <a name="conditional-deployment-in-arm-templates"></a>Implementación condicional en las plantillas de ARM

A veces, tiene que implementar un recurso en una plantilla de Azure Resource Manager (ARM). Use el elemento `condition` para especificar si se implementó el recurso. El valor de este elemento se resuelve como true o false. Cuando el valor es true, el recurso se crea. Cuando el valor es false, el recurso no se crea. El valor solo se puede aplicar a todo el recurso.

> [!NOTE]
> La implementación condicional no se aplica en cascada a los [recursos secundarios](child-resource-name-type.md). Si desea implementar condicionalmente un recurso y sus recursos secundarios, debe aplicar la misma condición a cada tipo de recurso.

## <a name="new-or-existing-resource"></a>Recurso nuevo o existente

Puede usar la implementación condicional para crear un recurso nuevo o usar uno existente. En el ejemplo siguiente se muestra cómo usar una condición para implementar una cuenta de almacenamiento nueva o usar una existente.

```json
{
  "condition": "[equals(parameters('newOrExisting'),'new')]",
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "2017-06-01",
  "name": "[variables('storageAccountName')]",
  "location": "[parameters('location')]",
  "sku": {
    "name": "[variables('storageAccountType')]"
  },
  "kind": "Storage",
  "properties": {}
}
```

Cuando el parámetro **newOrExisting** está establecido en **new**, la condición se evalúa como true. Se implementa la cuenta de almacenamiento. Sin embargo, cuando **newOrExisting** está establecido **existing**, la condición se evalúa como false y no se implementa la cuenta de almacenamiento.

Para una plantilla de ejemplo completo que usa el elemento `condition`, consulte [VM with a new or existing Virtual Network, Storage, and Public IP](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) (Máquina virtual con una red virtual nueva o existente, almacenamiento y dirección IP pública).

## <a name="allow-condition"></a>Permitir una condición

Puede pasar un valor de parámetro que indica si se permite una condición. En el ejemplo siguiente se implementa un servidor SQL Server y, opcionalmente, se permiten las direcciones IP de Azure.

```json
{
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2015-05-01-preview",
  "name": "[parameters('serverName')]",
  "location": "[parameters('location')]",
  "properties": {
    "administratorLogin": "[parameters('administratorLogin')]",
    "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
    "version": "12.0"
  },
  "resources": [
    {
      "condition": "[parameters('allowAzureIPs')]",
      "type": "firewallRules",
      "apiVersion": "2015-05-01-preview",
      "name": "AllowAllWindowsAzureIps",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Sql/servers/', parameters('serverName'))]"
      ],
      "properties": {
        "endIpAddress": "0.0.0.0",
        "startIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Para ver la plantilla completa, consulte el artículo sobre el [servidor lógico de Azure SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server).

## <a name="runtime-functions"></a>Funciones en tiempo de ejecución

Si usa una función [reference](template-functions-resource.md#reference) o [list](template-functions-resource.md#list) con un recurso que se implementa de forma condicional, se puede evaluar la función incluso si el recurso no está implementado. Se genera un error si la función hace referencia a un recurso que no existe.

Use la función [if](template-functions-logical.md#if) para asegurarse de que la función se evalúa solo para las condiciones en las que se implementa el recurso. Consulte la función [if](template-functions-logical.md#if) para una plantilla de ejemplo que use if y reference con un recurso implementado de forma condicional.

Puede establecer un [recurso como dependiente](define-resource-dependency.md) en un recurso condicional exactamente como lo haría con cualquier otro recurso. Cuando un recurso condicional no está implementado, Azure Resource Manager lo quita automáticamente de las dependencias necesarias.

## <a name="complete-mode"></a>Modo completo

Si implementa una plantilla con [modo completo](deployment-modes.md) y no se implementa un recurso porque la condición se evalúa como false, el resultado depende de la versión de API REST que use para implementar la plantilla. Si usa una versión anterior a 2019-05-10, el recurso **no se elimina**. Con 2019-05-10 o posterior, el recurso **se elimina**. Las versiones más recientes de Azure PowerShell y la CLI de Azure eliminan el recurso cuando la condición es falsa.

## <a name="next-steps"></a>Pasos siguientes

* Para recomendaciones sobre la creación de platillas, consulte [Procedimientos recomendados de plantillas de ARM](template-best-practices.md).
* Para crear varias instancias de un recurso, consulte [Iteración de recursos en plantillas de ARM](copy-resources.md).
