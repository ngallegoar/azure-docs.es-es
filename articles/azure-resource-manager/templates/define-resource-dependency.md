---
title: Establecimiento del orden de implementación para recursos
description: Describe cómo establecer un recurso como dependiente de otro recurso durante la implementación para garantizar el orden de implementación correcto de los recursos.
ms.topic: conceptual
ms.date: 12/03/2019
ms.openlocfilehash: f11f79df875492a568a76f494dfffb4a163f64cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153291"
---
# <a name="define-the-order-for-deploying-resources-in-arm-templates"></a>Definición del orden de implementación de recursos en las plantillas de ARM

Al implementar un recurso, es posible que tenga que asegurarse de que existen otros antes de implementarlo. Por ejemplo, necesitará un servidor SQL Server antes de implementar una base de datos SQL. Esta relación se define al marcar un recurso como dependiente del otro. Una dependencia se define con el elemento **dependsOn** o mediante la función **reference**.

Administrador de recursos evalúa las dependencias entre recursos y los implementa en su orden dependiente. Cuando no hay recursos dependientes entre sí, Resource Manager los implementa en paralelo. Solo tiene que definir las dependencias de recursos que se implementan en la misma plantilla.

## <a name="dependson"></a>dependsOn

Dentro de la plantilla, el elemento dependsOn permite definir un recurso como dependiente de uno o varios recursos. Su valor es una lista de nombres de recursos separados por coma. La lista puede incluir recursos [implementados condicionalmente](conditional-resource-deployment.md). Cuando un recurso condicional no está implementado, Azure Resource Manager lo quita automáticamente de las dependencias necesarias.

En el ejemplo siguiente se muestra un conjunto de escalado de máquinas virtuales que depende de un equilibrador de carga, una red virtual y un bucle que crea varias cuentas de almacenamiento. Esos otros recursos no se muestran en el ejemplo siguiente, pero tendrían que existir en alguna otra parte de la plantilla.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "apiVersion": "2016-03-30",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

En el ejemplo anterior, se incluye una dependencia de los recursos que se crean a través de un bucle de copia denominado "**storageLoop**". Para ver un ejemplo, consulte [Creación de varias instancias de recursos en el Administrador de recursos de Azure](copy-resources.md)

Al definir las dependencias, puede incluir el espacio de nombres de proveedor de recursos y el tipo de recurso para evitar la ambigüedad. Por ejemplo, para aclarar un equilibrador de carga y la red virtual que puede tener los mismos nombres que otros recursos, utilice el formato siguiente:

```json
"dependsOn": [
  "[resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName'))]",
  "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworkName'))]"
]
```

Si bien puede que se sienta tentado a usar dependsOn para asignar relaciones entre los recursos, es importante comprender por qué lo hace. Por ejemplo, para documentar cómo están interconectados los recursos, dependsOn no es el enfoque correcto. No se pueden consultar los recursos que se definieron en el elemento dependsOn después de realizar la implementación. El uso de dependsOn podría llegar a repercutir en el tiempo de implementación, ya que Resource Manager no implementa dos recursos en paralelo que tengan una dependencia.

## <a name="child-resources"></a>Recursos secundarios

La propiedad resources permite especificar los recursos secundarios que están relacionados con el recurso que se está definiendo. Los recursos secundarios solo se pueden definir en cinco niveles de profundidad. Es importante tener en cuenta que no se crea una dependencia de implementación implícita entre un recurso secundario y el recurso primario. Si necesita que el recurso secundario se implemente después del recurso primario, debe declarar explícitamente esa dependencia con la propiedad dependsOn.

Cada recurso primario solo acepta determinados tipos de recursos como recursos secundarios. Los tipos de recursos que se aceptan se especifican en el [esquema de plantilla](https://github.com/Azure/azure-resource-manager-schemas) del recurso principal. El nombre del tipo de recurso secundario incluye el nombre del tipo de recurso primario, por ejemplo, **Microsoft.Web/sites/config** y **Microsoft.Web/sites/extensions** son ambos recursos secundarios de **Microsoft.Web/Sites**.

En el ejemplo siguiente se muestran un servidor SQL y una base de datos SQL. Observe que se ha definido una dependencia explícita entre la base de datos SQL y el servidor SQL, a pesar de que la base de datos es un elemento secundario del servidor.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "apiVersion": "2014-04-01-preview",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "apiVersion": "2014-04-01-preview",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "tags": {
          "displayName": "Database"
        },
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-and-list-functions"></a>Funciones de referencia y lista

La [función reference](template-functions-resource.md#reference) permite que una expresión derive su valor de otros pares de valor y nombre JSON o de recursos en tiempo de ejecución. Las [funciones list*](template-functions-resource.md#list) devuelven valores para un recurso de una operación de lista.  Las expresiones de referencia y lista declaran implícitamente que un recurso depende de otro, cuando el recurso al que se hace referencia está implementado en la misma plantilla y se hace referencia a él por su nombre (no por el identificador de recurso). Si se pasa el identificador de recurso a las funciones de referencia o lista, no se crea una referencia implícita.

El formato general de la función de referencia es:

```json
reference('resourceName').propertyPath
```

El formato general de la función listKeys es:

```json
listKeys('resourceName', 'yyyy-mm-dd')
```

En el ejemplo siguiente, un punto de conexión de CDN depende explícitamente del perfil de CDN e implícitamente de una aplicación web.

```json
{
    "name": "[variables('endpointName')]",
    "apiVersion": "2016-04-02",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "dependsOn": [
      "[variables('profileName')]"
    ],
    "properties": {
      "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
      ...
    }
```

Puede usar este elemento o el elemento dependsOn para especificar las dependencias, pero no es necesario usar ambos para el mismo recurso dependiente. Siempre que sea posible, use una referencia implícita para evitar agregar una dependencia innecesaria.

Para más información, consulte [función reference](template-functions-resource.md#reference).

## <a name="circular-dependencies"></a>Dependencias circulares

Resource Manager identifica dependencias circulares durante la validación de plantillas. Si recibe un error que indica que existe una dependencia circular, evalúe la plantilla para ver si algunas dependencias no son necesarias y se pueden quitar. Si no es suficiente con quitar dependencias, puede evitar dependencias circulares moviendo algunas operaciones de implementación a recursos secundarios que se implementan después de los recursos que tienen la dependencia circular. Por ejemplo, suponga que va a implementar dos máquinas virtuales pero debe establecer propiedades en cada una que hagan referencia a la otra. Puede implementarlas en el orden siguiente:

1. vm1
2. vm2
3. La extensión en vm1 depende de vm1 y vm2. La extensión establece valores en vm1 que obtiene de vm2.
4. La extensión en vm2 depende de vm1 y vm2. La extensión establece valores en vm2 que obtiene de vm1.

Para información sobre cómo evaluar el orden de implementación y resolver errores de dependencia, consulte [Solución de errores comunes de implementación de Azure con Azure Resource Manager](common-deployment-errors.md).

## <a name="next-steps"></a>Pasos siguientes

* Para seguir los pasos de un tutorial, consulte [Tutorial: Creación de plantillas de Azure Resource Manager con recursos dependientes](template-tutorial-create-templates-with-dependent-resources.md).
* Para obtener recomendaciones al establecer las dependencias, consulte [Procedimientos recomendados de plantilla de Azure Resource Manager](template-best-practices.md).
* Para aprender a solucionar los problemas de dependencias durante la implementación, consulte [Solución de errores comunes de implementación de Azure con Azure Resource Manager](common-deployment-errors.md).
* Para más información sobre la creación de plantillas del Administrador de recursos de Azure, consulte [Creación de plantillas](template-syntax.md).
* Para obtener una lista de las funciones disponibles en una plantilla, consulte [Funciones de plantilla](template-functions.md).

