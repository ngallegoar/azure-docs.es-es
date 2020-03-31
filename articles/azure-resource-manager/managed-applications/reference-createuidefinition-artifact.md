---
title: Artefacto createUiDefinition
description: Muestra cómo crear el artefacto createUiDefinition para una aplicación administrada de Azure. El archivo se llama createUiDefinition.json.
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 07/11/2019
ms.openlocfilehash: 9be1cb02c419314a33b9487bf183ef3659b2b549
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75649516"
---
# <a name="reference-user-interface-elements-artifact"></a>Referencia: Artefacto de elementos de la interfaz de usuario

Este artículo es una referencia para un artefacto *createUiDefinition.json* en Azure Managed Applications. Para más información sobre la creación de elementos de interfaz de usuario, consulte los [elementos para crear una interfaz de usuario](create-uidefinition-elements.md).

## <a name="user-interface-elements"></a>Elementos de la interfaz de usuario

El JSON siguiente muestra un ejemplo de un archivo *createUiDefinition.json* para Azure Managed Applications:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "applicationSettings",
        "label": "Application Settings",
        "subLabel": {
          "preValidation": "Configure your application settings",
          "postValidation": "Done"
        },
        "bladeTitle": "Application Settings",
        "elements": [
          {
            "name": "funcname",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the function to be created",
            "toolTip": "Name of the function to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "storagename",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the storage to be created",
            "toolTip": "Name of the storage to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "zipFileBlobUri",
            "type": "Microsoft.Common.TextBox",
            "defaultValue": "https://github.com/Azure/azure-quickstart-templates/tree/master/101-custom-rp-with-function/artifacts/functionzip/functionpackage.zip",
            "label": "The Uri to the uploaded function zip file",
            "toolTip": "The Uri to the uploaded function zip file",
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "funcname": "[steps('applicationSettings').funcname]",
      "storageName": "[steps('applicationSettings').storagename]",
      "zipFileBlobUri": "[steps('applicationSettings').zipFileBlobUri]"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Creación de una aplicación administrada con acciones y recursos personalizados](tutorial-create-managed-app-with-custom-provider.md).
- [Referencia: Artefacto de plantilla de implementación](reference-main-template-artifact.md).
- [Referencia: Artefacto de definición de vista](reference-view-definition-artifact.md).