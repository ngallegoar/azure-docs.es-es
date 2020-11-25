---
title: Actualizaciones del esquema del 1 de junio de 2016
description: Versión actualizada del esquema 2016-06-01 para definiciones de aplicación lógica en Azure Logic Apps
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 07/25/2016
ms.openlocfilehash: ccc7df5bfac327fabf05f210764dbe10658b5015
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96000324"
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Actualizaciones de esquema para Azure Logic Apps, 1 de junio de 2016

El [esquema actualizado](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json) y la versión de API de Azure Logic Apps incluye importantes mejoras que aportan una mayor confiabilidad a las aplicaciones lógicas y facilitan su uso:

* [Ámbitos](#scopes): le permiten agrupar o anidar acciones como una colección de acciones.
* [Condiciones y bucles](#conditions-loops): ahora son acciones de primera clase.
* Ordenación más precisa para ejecutar acciones con la propiedad `runAfter`, sustituyendo `dependsOn`.

Para actualizar las aplicaciones lógicas del esquema de versión preliminar del 1 de agosto de 2015 al esquema del 1 de junio de 2016, [consulte la sección de actualización](#upgrade-your-schema).

<a name="scopes"></a>

## <a name="scopes"></a>Ámbitos

Este esquema incluye ámbitos, que le permiten agrupar acciones o anidar acciones unas dentro de otras. Por ejemplo, una condición puede contener otra condición. Aprenda más sobre la [sintaxis de los ámbitos](./logic-apps-control-flow-loops.md), o revise este ejemplo básico de ámbitos:

```json
{
   "actions": {
      "Scope": {
         "type": "Scope",
         "actions": {                
            "Http": {
               "inputs": {
                   "method": "GET",
                   "uri": "https://www.bing.com"
               },
               "runAfter": {},
               "type": "Http"
            }
         }
      }
   }
}
```

<a name="conditions-loops"></a>

## <a name="conditions-and-loops-changes"></a>Cambios de condiciones y bucles

En las versiones anteriores del esquema, las condiciones y los bucles eran parámetros asociados a una sola acción. Este esquema elimina esta limitación, por lo que las condiciones y los bucles están ahora disponibles como tipos de acción. Aprenda más sobre [bucles y ámbitos](./logic-apps-control-flow-loops.md), [condiciones](../logic-apps/logic-apps-control-flow-conditional-statement.md) o revise este ejemplo básico de una acción de condición:

```json
{
   "Condition - If trigger is some trigger": {
      "type": "If",
      "expression": "@equals(triggerBody(), '<trigger-name>')",
      "runAfter": {},
      "actions": {
         "Http_2": {
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com"
            },
            "runAfter": {},
            "type": "Http"
         }
      },
      "else": 
      {
         "Condition - If trigger is another trigger": {}
      }  
   }
}
```

<a name="run-after"></a>

## <a name="runafter-property"></a>Propiedad "runAfter"

La propiedad `runAfter` reemplaza a `dependsOn`, por lo que se proporciona una mayor precisión al especificar el orden de ejecución de las acciones según el estado de acciones anteriores. La propiedad `dependsOn` indica si "la acción se ejecutó correctamente", en función de si la acción anterior se realizó correctamente, no se pudo realizar o se omitió, y no del número de veces que quiso ejecutar la acción. La propiedad `runAfter` proporciona flexibilidad como un objeto que especifica todos los nombres de acción después de los cuales se ejecuta el objeto. Esta propiedad también define una matriz de estados que son aceptables como desencadenadores. Por ejemplo, si quiere que una acción se ejecute después de la acción A se realiza correctamente y también después de que la acción B se realiza correctamente o no, configure esta propiedad `runAfter`:

```json
{
   // Other parts in action definition
   "runAfter": {
      "A": ["Succeeded"],
      "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>Actualización del esquema

Para actualizar el [esquema más reciente](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json), solo debe realizar algunos pasos. El proceso de actualización incluye ejecutar el script de actualización, guardarlo como una nueva aplicación lógica y, posiblemente, sobrescribir la antigua aplicación lógica, si lo desea.

1. Abra la aplicación lógica en Azure Portal.

2. Vaya a **Overview** (Información general). En la barra de herramientas de aplicaciones lógicas, elija **Actualizar esquema**.
   
   ![Selección del esquema de actualización][1]
   
   Se devuelve la definición actualizada, que puede copiar y pegar en una definición de recurso si es necesario. 

   > [!IMPORTANT]
   > *Asegúrese* de elegir **Guardar como** para que todas las referencias de conexión sigan siendo válidas en la aplicación lógica actualizada.

3. En la barra de herramientas de la hoja de actualización, elija **Guardar como**.

4. Escriba el nombre y el estado de la aplicación lógica. Para implementar la aplicación lógica actualizada, elija **Crear**.

5. Confirme que la aplicación lógica actualizada funciona según lo previsto.
   
   > [!NOTE]
   > Si usa un desencadenador manual o de solicitud, la dirección URL de devolución de llamada cambia en la nueva aplicación lógica. Pruebe la nueva dirección URL para asegurarse de que funciona completamente. Para conservar las direcciones URL anteriores, puede clonarlas a través de la aplicación lógica existente.

6. *Opcional*: para sobrescribir la aplicación lógica anterior con la nueva versión de esquema, en la barra de herramientas, elija **Clonar**, junto a **Actualizar esquema**. Este paso solo es necesario si quiere mantener el mismo identificador de recurso o dirección URL del desencadenador de solicitud de la aplicación lógica.

## <a name="upgrade-tool-notes"></a>Notas de la herramienta de actualización

### <a name="mapping-conditions"></a>Condiciones de asignación

En la definición actualizada, la herramienta hace todo lo posible por agrupar las acciones de bifurcación verdaderas y falsas como un ámbito. En concreto, el patrón de diseñador de `@equals(actions('a').status, 'Skipped')` aparece como una acción `else`. Sin embargo, si la herramienta detecta patrones irreconocibles, podría crear condiciones distintas para la bifurcación true y false. Si es necesario, puede volver a asignar acciones después de la actualización.

#### <a name="foreach-loop-with-condition"></a>Bucle "foreach" con condición

En el nuevo esquema, puede usar la acción de filtro para replicar el patrón que usa un bucle **For each** con una condición por elemento. Sin embargo, el cambio sucede automáticamente cuando actualiza. La condición se convierte en una acción de filtro que aparece antes del bucle **For each**, que devuelve solo una matriz de elementos que cumplen la condición y pasa esa matriz a la acción **For each**. Para ver un ejemplo, consulte [Bucles y ámbitos](./logic-apps-control-flow-loops.md).

### <a name="resource-tags"></a>Etiquetas del recurso

Después de actualizar, se quitan las etiquetas del recurso, por lo que debe restablecerlas para el flujo de trabajo actualizado.

## <a name="other-changes"></a>Otros cambios

### <a name="renamed-manual-trigger-to-request-trigger"></a>Cambio de nombre de desencadenador "manual" a desencadenador "request"

El tipo de desencadenador `manual` está en desuso y ahora se llama `request` con el tipo `http`. Este cambio crea una mayor coherencia para la clase de patrón que el desencadenador suele compilar.

### <a name="new-filter-action"></a>Nueva acción 'filtro'

Para filtrar una matriz grande hasta un conjunto de elementos más pequeño, el nuevo tipo `filter` acepta una matriz y una condición, evalúa la condición de cada elemento y devuelve una matriz con los elementos que cumplen la condición.

### <a name="restrictions-for-foreach-and-until-actions"></a>Restricciones de las acciones "foreach" y "until"

Los bucles `foreach` y `until` están limitados a una sola acción.

### <a name="new-trackedproperties-for-actions"></a>Nueva propiedad "trackedProperties" para las acciones

Las acciones pueden tener ahora una propiedad adicional llamada `trackedProperties`, que está relacionada con las propiedades `runAfter` y `type`. Este objeto especifica determinadas entradas o salidas de acciones que desea incluir en la telemetría de Azure Diagnostic, que se emiten como parte de un flujo de trabajo. Por ejemplo:

``` json
{
   "Http": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com"
      },
      "runAfter": {},
      "type": "Http",
      "trackedProperties": {
         "responseCode": "@action().outputs.statusCode",
         "uri": "@action().inputs.uri"
      }
   }
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Creación de definiciones de flujo de trabajo para aplicaciones lógicas](../logic-apps/logic-apps-author-definitions.md)
* [Automatización de la implementación de aplicaciones lógicas](logic-apps-azure-resource-manager-templates-overview.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
