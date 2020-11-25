---
title: Elemento de la interfaz de usuario ServicePrincipalSelector
description: Describe el elemento de la interfaz de usuario Microsoft.Common.ServicePrincipalSelector para Azure Portal. Proporciona un control para elegir una aplicación y un cuadro de texto para especificar una contraseña o una huella digital de certificado.
author: tfitzmac
ms.topic: conceptual
ms.date: 11/17/2020
ms.author: tomfitz
ms.openlocfilehash: 9d41e41f110e927f436b38d6291719c138defa53
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94745821"
---
# <a name="microsoftcommonserviceprincipalselector-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common. ServicePrincipalSelector

Control que permite a los usuarios seleccionar una [entidad de servicio](/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) o registrar una aplicación nueva. Al seleccionar **Crear nuevo**, se recorren los pasos para registrar una nueva aplicación. Al seleccionar una aplicación existente, el control proporciona un cuadro de texto para escribir una contraseña o una huella digital de certificado.

## <a name="ui-samples"></a>Ejemplos de interfaz de usuario

Puede usar una aplicación predeterminada, crear una nueva aplicación o utilizar una aplicación existente.

### <a name="use-default-application-or-create-new"></a>Uso de una aplicación predeterminada o creación de una nueva

La vista predeterminada la definen los valores de la propiedad `defaultValue` y el **tipo de entidad de servicio** se establece en **Crear nueva**. Si la propiedad `principalId` contiene un identificador único global (GUID) válido, el control busca el elemento `objectId` de la aplicación. El valor predeterminado se aplica si el usuario no realiza una selección en el control.

Si quiere registrar una nueva aplicación, seleccione **Cambiar selección** y se mostrará el cuadro de diálogo **Registrar una aplicación**. Escriba el **nombre**, el **tipo de cuenta admitida** y pulse el botón **Registrar**.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-default.png" alt-text="Vista inicial de Microsoft.Common.ServicePrincipalSelector":::.

Después de registrar una nueva aplicación, use el **Tipo de autenticación** para escribir una contraseña o una huella digital de certificado.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-authenticate.png" alt-text="Autenticación de Microsoft.Common.ServicePrincipalSelector.":::

### <a name="use-existing-application"></a>Uso de la aplicación existente

Para usar una aplicación existente, pulse **Seleccionar existentes** y, a continuación, **Hacer la selección**. Use el cuadro de diálogo **Seleccionar una aplicación** para buscar el nombre de la aplicación. En los resultados, seleccione la aplicación y, a continuación, el botón **Seleccionar**. Después de seleccionar una aplicación, el control muestra el **tipo de autenticación** para escribir una contraseña o una huella digital de certificado.

:::image type="content" source="./media/managed-application-elements/microsoft-common-serviceprincipal-existing.png" alt-text="Microsoft. Common. ServicePrincipalSelector para seleccionar una aplicación existente.":::

## <a name="schema"></a>Schema

```json
{
  "name": "ServicePrincipal",
  "type": "Microsoft.Common.ServicePrincipalSelector",
  "label": {
    "password": "Password",
    "certificateThumbprint": "Certificate thumbprint",
    "authenticationType": "Authentication Type",
    "sectionHeader": "Service Principal"
  },
  "toolTip": {
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
              "password": "Password",
              "certificateThumbprint": "Certificate thumbprint",
              "authenticationType": "Authentication Type",
              "sectionHeader": "Service Principal"
            },
            "toolTip": {
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

`appId` es el identificador del registro de la aplicación que ha seleccionado o creado. `objectId` es una matriz de los id. de objeto correspondientes a las entidades de servicio configuradas para el registro de la aplicación seleccionada.

Cuando no se realiza ninguna selección desde el control, el valor de la propiedad `newOrExisting` es **nuevo**:

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

Cuando se selecciona **Crear nueva** o una aplicación existente en el control, el valor de la propiedad `newOrExisting` es **existente**:

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
