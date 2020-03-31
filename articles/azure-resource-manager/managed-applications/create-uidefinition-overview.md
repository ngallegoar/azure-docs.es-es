---
title: Archivo CreateUiDefinition.jso para el panel del portal
description: Describe cómo crear definiciones de interfaz de usuario para Azure Portal. Se usa al definir Azure Managed Applications.
author: tfitzmac
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: tomfitz
ms.openlocfilehash: 2956c76f5bec353639b39228b982db21b6932deb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294893"
---
# <a name="createuidefinitionjson-for-azure-managed-applications-create-experience"></a>CreateUiDefinition.json para la experiencia de creación de aplicaciones administradas de Azure

Este documento presenta los conceptos básicos del archivo **createUiDefinition.json**, que Azure Portal utiliza para definir la interfaz de usuario al crear una aplicación administrada.

La plantilla es la siguiente:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Azure.CreateUIDef",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { },
      "resourceTypes": [ ]
   }
}
```

CreateUiDefinition siempre contiene tres propiedades: 

* handler
* version
* parámetros

Handler siempre debería ser `Microsoft.Azure.CreateUIDef`, y la última versión admitida es `0.1.2-preview`.

El esquema de la propiedad parameters depende de la combinación de los valores de handler y version especificados. Para las aplicaciones administradas, las propiedades admitidas son `basics`, `steps` y `outputs`. Las propiedades basics y steps contienen los [elementos](create-uidefinition-elements.md), como cuadros de texto y listas desplegables, que se mostrarán en Azure Portal. La propiedad outputs se utiliza para asignar los valores de salida de los elementos especificados a los parámetros de la plantilla de implementación de Azure Resource Manager.

Se recomienda incluir `$schema`, aunque es opcional. Si se especifica, el valor de `version` debe coincidir con la versión en el identificador URI de `$schema`.

Puede usar un editor de JSON para crear el valor createUiDefinition y luego probarlo en el [espacio aislado createUiDefinition](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) para obtener una vista previa. Para obtener más información sobre el espacio aislado, consulte [Probar la interfaz del portal de Azure Managed Applications](test-createuidefinition.md).

## <a name="basics"></a>Aspectos básicos

El paso Aspectos básicos es el primer paso que se genera cuando Azure Portal analiza el archivo. Además de mostrar los elementos especificados en `basics`, el portal inserta elementos para que los usuarios elijan la suscripción, el grupo de recursos y la ubicación de la implementación. Cuando es posible, los elementos que consultan los parámetros de toda la implementación, como el nombre de un clúster o las credenciales del administrador, deben ir en este paso.

## <a name="steps"></a>Pasos

La propiedad steps puede contener cero o más pasos adicionales que se mostrarán después de basics, cada uno de los cuales contiene uno o más elementos. Considere la posibilidad de agregar pasos por rol o nivel de aplicación que se está implementando. Por ejemplo, agregue un paso para las entradas de los nodos principales y un paso para los nodos de trabajo de un clúster.

## <a name="outputs"></a>Salidas

Azure Portal usa la propiedad `outputs` para asignar elementos de `basics` y `steps` a los parámetros de la plantilla de implementación de Azure Resource Manager. Las claves de este diccionario son los nombres de los parámetros de plantilla, y los valores son propiedades de los objetos de salida de los elementos a los que se hace referencia.

Para definir el nombre del recurso de la aplicación administrada, debe incluir un valor llamado `applicationResourceName` en la propiedad outputs. Si no establece este valor, la aplicación asigna un GUID al nombre. Puede incluir un cuadro de texto en la interfaz de usuario que requiera un nombre de usuario.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="resource-types"></a>Tipos de recursos

Para limitar las ubicaciones disponibles a solo las que admiten los tipos de recursos que se van a implementar, proporcione una matriz de los tipos de recursos. Si se proporciona más de un tipo de recurso, solo se devuelven las ubicaciones que admiten todos los tipos de recursos. Esta propiedad es opcional.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Azure.CreateUIDef",
    "version": "0.1.2-preview",
    "parameters": {
      "resourceTypes": ["Microsoft.Compute/disks"],
      "basics": [
        ...
```  

## <a name="functions"></a>Functions

CreateUiDefinition proporciona [funciones](create-uidefinition-functions.md) para trabajar con entradas y salidas de los elementos, y características como los condicionales. La sintaxis y funcionalidad de estas funciones son similares a las de las funciones de las plantillas de Azure Resource Manager.

## <a name="next-steps"></a>Pasos siguientes

El propio archivo createUiDefinition.json tiene un esquema simple. Su profundidad real procede de todos los elementos y funciones que admite. Dichos elementos se describen con más detalle en:

- [Elementos](create-uidefinition-elements.md)
- [Funciones](create-uidefinition-functions.md)

Un esquema JSON actual para createUiDefinition está disponible aquí: `https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json`.

Para un archivo de interfaz de usuario de ejemplo, vea [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/Managed%20Application%20Sample%20Packages/201-managed-app-using-existing-vnet/createUiDefinition.json).
