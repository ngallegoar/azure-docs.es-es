---
title: Elemento de la interfaz de usuario ServicePrincipalSelector
description: Describe el elemento de la interfaz de usuario Microsoft.Common.ServicePrincipalSelector para Azure Portal. Proporciona una lista desplegable para elegir un identificador de aplicación y un cuadro de texto para especificar una contraseña o una huella digital de certificado.
author: tfitzmac
ms.topic: conceptual
ms.date: 09/29/2020
ms.author: tomfitz
ms.openlocfilehash: 73b242754bfae53b6df5abd9c2c8dee33b973dad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91576003"
---
# <a name="microsoftcommonserviceprincipalselector-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common. ServicePrincipalSelector

Un control que permite a los usuarios seleccionar una entidad de servicio o registrar una nueva. Al seleccionar **Crear nuevo**, se recorren los pasos para registrar una nueva aplicación. Al seleccionar una aplicación existente, el control proporciona un cuadro de texto para escribir una contraseña o una huella digital de certificado.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario

La vista predeterminada viene determinada por los valores de la propiedad `defaultValue`. Si la propiedad `principalId` contiene un identificador único global (GUID) válido, el control busca el identificador de objeto de la aplicación. El valor predeterminado se aplica si el usuario no realiza una selección en la lista desplegable.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-initial.png" alt-text="Vista inicial de Microsoft.Common.ServicePrincipalSelector":::

Al seleccionar **Crear nuevo** o un identificador de aplicación existente en la lista desplegable, se muestra el **tipo de autenticación** para escribir una contraseña o huella digital del certificado en el cuadro de texto.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-selection.png" alt-text="Vista inicial de Microsoft.Common.ServicePrincipalSelector":::

## <a name="schema"></a>Schema

```json
{
  "name": "ServicePrincipal",
  "type": "Microsoft.Common.ServicePrincipalSelector",
  "label": {
    "principalId": "App Id",
    "password": "Password",
    "certificateThumbprint": "Certificate thumbprint",
    "authenticationType": "Authentication Type",
    "sectionHeader": "Service Principal"
  },
  "toolTip": {
    "principalId": "App Id",
    "password": "Password",
    "certificateThumbprint": "Certificate thumbprint",
    "authenticationType": "Authentication Type"
  },
  "defaultValue": {
    "principalId": "<default guid>",
    "name": "(New) default App Id"
  },
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideCertificate": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Observaciones

- Las propiedades necesarias son las siguientes:
  - `name`
  - `type`
  - `label`
  - `defaultValue`: especifica los valores de `principalId` y `name` predeterminados.

- Las propiedades opcionales son las siguientes:
  - `toolTip`: Adjunta una información sobre herramientas `infoBalloon` a cada etiqueta.
  - `visible`: Oculte o muestre el control.
  - `options`: Especifica si la opción de huella digital del certificado debe estar disponible.
  - `constraints`: Restricciones regex para la validación de contraseñas.

## <a name="example"></a>Ejemplo

A continuación se muestra un ejemplo del control `Microsoft.Common.ServicePrincipalSelector`. La propiedad `defaultValue` establece `principalId` en `<default guid>` como un marcador de posición para un GUID de identificador de aplicación predeterminado.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [],
    "steps": [
      {
        "name": "SPNcontrol",
        "label": "SPNcontrol",
        "elements": [
          {
            "name": "ServicePrincipal",
            "type": "Microsoft.Common.ServicePrincipalSelector",
            "label": {
              "principalId": "App Id",
              "password": "Password",
              "certificateThumbprint": "Certificate thumbprint",
              "authenticationType": "Authentication Type",
              "sectionHeader": "Service Principal"
            },
            "toolTip": {
              "principalId": "App Id",
              "password": "Password",
              "certificateThumbprint": "Certificate thumbprint",
              "authenticationType": "Authentication Type"
            },
            "defaultValue": {
              "principalId": "<default guid>",
              "name": "(New) default App Id"
            },
            "constraints": {
              "required": true,
              "regex": "^[a-zA-Z0-9]{8,}$",
              "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
            },
            "options": {
              "hideCertificate": false
            },
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "appId": "[steps('SPNcontrol').ServicePrincipal.appId]",
      "objectId": "[steps('SPNcontrol').ServicePrincipal.objectId]",
      "password": "[steps('SPNcontrol').ServicePrincipal.password]",
      "certificateThumbprint": "[steps('SPNcontrol').ServicePrincipal.certificateThumbprint]",
      "newOrExisting": "[steps('SPNcontrol').ServicePrincipal.newOrExisting]",
      "authenticationType": "[steps('SPNcontrol').ServicePrincipal.authenticationType]"
    }
  }
}
```

## <a name="example-output"></a>Salida de ejemplo

`appId` es el identificador del registro de la aplicación que ha seleccionado o creado. `objectId` es una matriz de objectId para las entidades de servicio configuradas para el registro de la aplicación seleccionada.

Cuando no se realiza ninguna selección desde la lista desplegable, el valor de la propiedad `newOrExisting` es **nuevo**:

```json
{
  "appId": {
    "value": "<default guid>"
  },
  "objectId": {
    "value": ["<default guid>"]
  },
  "password": {
    "value": "<password>"
  },
  "certificateThumbprint": {
    "value": ""
  },
  "newOrExisting": {
    "value": "new"
  },
  "authenticationType": {
    "value": "password"
  }
}
```

Cuando se selecciona **Crear nuevo** o un identificador de aplicación existente en la lista desplegable, el valor de la propiedad `newOrExisting` es **existente**:

```json
{
  "appId": {
    "value": "<guid>"
  },
  "objectId": {
    "value": ["<guid>"]
  },
  "password": {
    "value": "<password>"
  },
  "certificateThumbprint": {
    "value": ""
  },
  "newOrExisting": {
    "value": "existing"
  },
  "authenticationType": {
    "value": "password"
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
- Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
