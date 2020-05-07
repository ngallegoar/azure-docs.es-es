---
title: Implementación de varias instancias de recursos
description: Use la operación de copia y las matrices en una plantilla de Azure Resource Manager para realizar varias iteraciones al implementar recursos.
ms.topic: conceptual
ms.date: 04/29/2020
ms.openlocfilehash: d4f40b606ffd56019b44cc8b67e5629b935bf50c
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583390"
---
# <a name="resource-iteration-in-arm-templates"></a>Iteración de recursos en las plantillas de ARM

En este artículo se explica cómo se crean varias instancias de un recurso en su plantilla de Azure Resource Manager (ARM). Al agregar el elemento **copy** a la sección de recursos de la plantilla, puede establecer de forma dinámica el número de elementos de los recursos que va a implementar. Asimismo, evitará tener que repetir la sintaxis de la plantilla.

También puede usar el elemento copy con [propiedades](copy-properties.md), [variables](copy-variables.md) y [salidas](copy-outputs.md).

Si tiene que especificar si un recurso se implementa, consulte [Elemento condition](conditional-resource-deployment.md).

## <a name="syntax"></a>Sintaxis

El elemento copy tiene el siguiente formato general:

```json
"copy": {
  "name": "<name-of-loop>",
  "count": <number-of-iterations>,
  "mode": "serial" <or> "parallel",
  "batchSize": <number-to-deploy-serially>
}
```

La propiedad **name** es cualquier valor que identifique el bucle. La propiedad **count** especifica el número de iteraciones que desea realizar en el tipo de recurso.

Utilice las propiedades **mode** y **batchSize** para especificar si los recursos van a implementarse simultáneamente o por orden. Estas propiedades se describen en el artículo [En serie o en paralelo](#serial-or-parallel).

## <a name="copy-limits"></a>Límites de copia

El valor de count no puede superar 800.

El valor de count no puede ser un número negativo. Puede ser cero si implementa la plantilla con una versión reciente de la CLI de Azure, PowerShell o la API de REST. Concretamente, se debe usar:

* Azure PowerShell **2.6** o posterior
* CLI de Azure **2.0.74** o posterior
* API de REST versión **2019-05-10** o posterior
* Las [implementaciones vinculadas](linked-templates.md) deben usar la versión **10-05-2019** o posterior de la API para el tipo de recurso de implementación

Las versiones anteriores de PowerShell, la CLI y API REST no admiten un valor de count de cero.

Tenga cuidado al usar la [implementación de modo completo](deployment-modes.md) con copia. Si vuelve a implementar con el modo completo en un grupo de recursos, se eliminan todos los recursos que no se especifican en la plantilla después de resolver el bucle de copia.

## <a name="resource-iteration"></a>Iteración de recursos

En el ejemplo siguiente, se crea el número de cuentas de almacenamiento especificado en el parámetro **storageCount**.

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
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
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
    "outputs": {}
}
```

Tenga en cuenta que el nombre de cada recurso incluye la función `copyIndex()`, que devuelve la iteración actual del bucle. `copyIndex()` es de base cero. Así, en el ejemplo siguiente:

```json
"name": "[concat('storage', copyIndex())]",
```

Crea estos nombres:

* storage0
* storage1
* storage2.

Para desplazar el valor de índice, puede pasar un valor de la función copyIndex(). El número de iteraciones que se especifica en el elemento copy, pero el valor de copyIndex se desplaza el valor especificado. Así, en el ejemplo siguiente:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Crea estos nombres:

* storage1
* storage2
* storage3

La operación de copia es útil al trabajar con matrices, ya que puede iterar a través de cada elemento de la matriz. Use la función `length` en la matriz para especificar el número de iteraciones, y `copyIndex` para recuperar el índice actual de la matriz.

En el ejemplo siguiente, se crea una cuenta de almacenamiento para cada uno de los nombres proporcionados en el parámetro.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageNames": {
          "type": "array",
          "defaultValue": [
            "contoso",
            "fabrikam",
            "coho"
          ]
      }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(parameters('storageNames')[copyIndex()], uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": "[length(parameters('storageNames'))]"
      }
    }
  ],
  "outputs": {}
}
```

Si desea devolver valores de los recursos implementados, puede usar [copy en la sección de salidas](copy-outputs.md).

## <a name="serial-or-parallel"></a>En serie o en paralelo

Resource Manager crea los recursos en paralelo de forma predeterminada. No se aplica ningún límite en el número de recursos implementados en paralelo, aparte del límite total de 800 recursos de la plantilla. No se garantiza el orden en el que se crean.

Sin embargo, es posible que quiera especificar que los recursos se implementen en secuencia. Por ejemplo, al actualizar un entorno de producción, puede que quiera escalonar las actualizaciones para que solo una cantidad determinada se actualice al mismo tiempo. Para implementar en serie varias instancias de un recurso, establezca `mode` en el valor **serial** y `batchSize` en el número de instancias que se implementarán a la vez. Con mode establecido en serial, Resource Manager crea una dependencia en las instancias anteriores del bucle, por lo que no se inicia ningún lote hasta que se completa el lote anterior.

Por ejemplo, para implementar en serie dos cuentas de almacenamiento a la vez, use lo siguiente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 4,
        "mode": "serial",
        "batchSize": 2
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

La propiedad mode también acepta **parallel**, que es el valor predeterminado.

## <a name="depend-on-resources-in-a-loop"></a>Dependencia de los recursos de un bucle

Especifique que un recurso se implemente después de otro recurso mediante el elemento `dependsOn`. Para implementar un recurso que dependa de la colección de recursos de un bucle, proporcione el nombre del bucle copy en el elemento dependsOn. En el ejemplo siguiente, se muestra cómo se implementan tres cuentas de almacenamiento antes de implementar la máquina virtual. No se incluye la definición completa de la máquina virtual. Tenga en cuenta que el elemento copy tiene el nombre establecido en `storagecopy` y que el elemento dependsOn de la máquina virtual también está establecido en `storagecopy`.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2015-06-15",
      "name": "[concat('VM', uniqueString(resourceGroup().id))]",
      "dependsOn": ["storagecopy"],
      ...
    }
  ],
  "outputs": {}
}
```

## <a name="iteration-for-a-child-resource"></a>Iteración para un recurso secundario

No puede usar un bucle copy en un recurso secundario. Para crear varias instancias de un recurso que se define normalmente como anidado dentro de otro recurso, debe crear dicho recurso como uno de nivel superior. La relación con el recurso principal se define a través de las propiedades type y name.

Por ejemplo, supongamos que suele definir un conjunto de datos como un recurso secundario dentro de una factoría de datos.

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
  "resources": [
    {
      "type": "datasets",
      "name": "exampleDataSet",
      "dependsOn": [
        "exampleDataFactory"
      ],
      ...
    }
  ]
```

Para crear más de un conjunto de datos, muévalos fuera de la factoría de datos. El conjunto de datos debe estar en el mismo nivel que la factoría de datos, pero seguirá siendo un recurso secundario de la factoría de datos. La relación entre el conjunto de datos y la factoría de datos se conserva a través de los parámetros type y name. Puesto que la propiedad de type ya no se puede inferir de su posición en la plantilla, debe proporcionar el nombre completo del tipo con el formato: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Para establecer una relación de principal-secundario con una instancia de la factoría de datos, proporcione un nombre para el conjunto de datos que incluya el nombre de recurso principal. Utilice el formato: `{parent-resource-name}/{child-resource-name}`.

En el siguiente ejemplo se muestra la implementación:

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
},
{
  "type": "Microsoft.DataFactory/datafactories/datasets",
  "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
  "dependsOn": [
    "exampleDataFactory"
  ],
  "copy": {
    "name": "datasetcopy",
    "count": "3"
  },
  ...
}]
```

## <a name="example-templates"></a>Plantillas de ejemplo

En los ejemplos siguientes se muestran escenarios comunes para crear más de una instancia de un recurso o propiedad.

|Plantilla  |Descripción  |
|---------|---------|
|[Almacenamiento de copias](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Implementa más de una cuenta de almacenamiento con un número de índice en el nombre. |
|[Almacenamiento de copias en serie](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Implementa varias cuentas de almacenamiento, una tras otra. El nombre incluye el número de índice. |
|[Almacenamiento de copias con una matriz](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Implementa varias cuentas de almacenamiento. El nombre incluye el valor de una matriz. |
|[Implementación de máquinas virtuales con un número variable de discos de datos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Implementa varios discos de datos con una máquina virtual. |
|[Varias reglas de seguridad](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Implementa varias reglas de seguridad en un grupo de seguridad de red. Crea las reglas de seguridad a partir de un parámetro. Para el parámetro, consulte el [archivo de parámetros de varios grupos de seguridad de red](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Pasos siguientes

* Para seguir los pasos de un tutorial, consulte [Tutorial: Creación de varias instancias de recursos con plantillas de ARM](template-tutorial-create-multiple-instances.md).
* Para otros usos del elemento copy, consulte:
  * [Iteración de propiedades en las plantillas de ARM](copy-properties.md)
  * [Iteración de variables en las plantillas de ARM](copy-variables.md)
  * [Iteración de salida en las plantillas de ARM](copy-outputs.md)
* Para obtener información acerca del uso de copy con plantillas anidadas, consulte [Uso de copy](linked-templates.md#using-copy).
* Para obtener información sobre las secciones de una plantilla, consulte el artículo sobre cómo [crear plantillas de ARM](template-syntax.md).
* Para más información sobre cómo implementar su plantilla, consulte el artículo [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](deploy-powershell.md).

