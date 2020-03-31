---
title: Libros de Azure Monitor y plantillas de Azure Resource Manager
description: Simplifique los informes complejos con libros de Azure Monitor parametrizados predefinidos y personalizados implementados a partir de plantillas de Azure Resource Manager
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 2c2d70d1c945e700a3fa42609f8aa0e1607ba77c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658411"
---
# <a name="programmatically-manage-workbooks"></a>Administración de libros mediante programación

Los propietarios de recursos tienen la opción de crear y administrar sus libros mediante programación a través de plantillas de Resource Manager. 

Esto puede ser útil en escenarios como:
* Implementación de informes de análisis específicos de la organización o del dominio junto con las implementaciones de recursos. Por ejemplo, puede implementar libros de error y rendimiento específicos de la organización para las nuevas aplicaciones o máquinas virtuales.
* Implementación de informes estándar o paneles con los libros para los recursos existentes.

El libro se creará en el grupo o subgrupo de recursos deseado y con el contenido especificado en las plantillas de Resource Manager.

## <a name="azure-resource-manager-template-for-deploying-workbooks"></a>Plantilla de Azure Resource Manager para implementar libros
1. Abra un libro que desee implementar mediante programación.
2. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas _Editar_.
3. Abra el _Editor avanzado_ con el botón _</>_ de la barra de herramientas.
4. En el editor, cambie _Tipo de plantilla_ a _Plantilla de Resource Manager_.
5. La plantilla de Resource Manager para la creación se muestra en el editor. Copie el contenido y úselo tal cual o mézclelo con una plantilla más grande que también implemente el recurso de destino.

    ![Imagen en la que se muestra cómo obtener la plantilla de Resource Manager desde la interfaz de usuario del libro](./media/workbooks-automate/programmatic-template.png)

## <a name="sample-azure-resource-manager-template"></a>Plantilla de Azure Resource Manager de ejemplo
Esta plantilla muestra cómo implementar un libro sencillo que muestra un '¡Hola mundo!'
```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workbookDisplayName":  {             
            "type":"string",
            "defaultValue": "My Workbook",
            "metadata": {
                "description": "The friendly name for the workbook that is used in the Gallery or Saved List. Needs to be unique in the scope of the resource group and source" 
            }
        },
        "workbookType":  {             
            "type":"string",
            "defaultValue": "tsg",
            "metadata": {
                "description": "The gallery that the workbook will been shown under. Supported values include workbook, `tsg`, Azure Monitor, etc." 
            }
        },
        "workbookSourceId":  {             
            "type":"string",
            "defaultValue": "<insert-your-resource-id-here>",
            "metadata": {
                "description": "The id of resource instance to which the workbook will be associated" 
            }
        },
        "workbookId": {
            "type":"string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "The unique guid for this workbook instance" 
            }
        }
    },    
    "resources": [
        {
            "name": "[parameters('workbookId')]",
            "type": "Microsoft.Insights/workbooks",
            "location": "[resourceGroup().location]",
            "kind": "shared",
            "apiVersion": "2018-06-17-preview",
            "dependsOn": [],
            "properties": {
                "displayName": "[parameters('workbookDisplayName')]",
                "serializedData": "{\"version\":\"Notebook/1.0\",\"items\":[{\"type\":1,\"content\":\"{\\\"json\\\":\\\"Hello World!\\\"}\",\"conditionalVisibility\":null}],\"isLocked\":false}",
                "version": "1.0",
                "sourceId": "[parameters('workbookSourceId')]",
                "category": "[parameters('workbookType')]"
            }
        }
    ],
    "outputs": {
        "workbookId": {
            "type": "string",
            "value": "[resourceId( 'Microsoft.Insights/workbooks', parameters('workbookId'))]"
        }
    }
}
```

### <a name="template-parameters"></a>Parámetros de plantilla

| Parámetro | Explicación |
| :------------- |:-------------|
| `workbookDisplayName` | Nombre descriptivo para el libro que se usa en la galería o en la lista guardada. Debe ser único en el grupo de recursos y origen |
| `workbookType` | La galería en la que se mostrará el libro. Los valores admitidos son libro, `tsg`, Azure Monitor, etc. |
| `workbookSourceId` | Identificador de la instancia de recurso a la que se asociará el libro. El nuevo libro se mostrará relacionado con esta instancia de recurso, por ejemplo, en la tabla de contenido del recurso en _Libro_. Si desea que el libro aparezca en la galería de libros en Azure Monitor, use la cadena _Azure Monitor_ en lugar de un identificador de recurso. |
| `workbookId` | El GUID único para esta instancia de libro. Use _[newGuid()]_ para crear automáticamente un nuevo GUID. |
| `kind` | Se utiliza para especificar si el libro creado es compartido o privado. Use el valor _compartido_ para los libros compartidos y _usuario_ para los usuarios privados. |
| `location` | Ubicación de Azure donde se creará el libro. Use _[resourceGroup().location]_ para crearlo en la misma ubicación que el grupo de recursos |
| `serializedData` | Contiene el contenido o la carga que se va a usar en el libro. Use la plantilla de Resource Manager de la interfaz de usuario de los libros para obtener el valor |

### <a name="workbook-types"></a>Tipos de libros
Tipos de libros especifica el tipo de galería de libros en el que se mostrará la nueva instancia de libro. Las opciones incluyen:

| Tipo | Ubicación de la galería |
| :------------- |:-------------|
| `workbook` | El valor predeterminado que se usa en la mayoría de los informes, incluida la galería de libros de Application Insights, Azure Monitor, etc.  |
| `tsg` | La galería de guías de solución de problemas en Application Insights |
| `usage` | La galería _Más_ en _Uso_ en Application Insights |

### <a name="limitations"></a>Limitaciones
Por motivos técnicos, este mecanismo no se puede usar para crear instancias de libro en la galería de _Libros_ de Application Insights. Estamos trabajando actualmente para poder resolver este incidente. Mientras tanto, se recomienda usar la galería de la guía de solución de problemas (workbookType: `tsg`) para implementar libros relacionados de Application Insights.

## <a name="next-steps"></a>Pasos siguientes

Explore cómo se usan los libros para potenciar la nueva [experiencia de Azure Monitor para Storage](../insights/storage-insights-overview.md).

