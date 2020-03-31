---
title: Elemento de la interfaz de usuario PasswordBox
description: Describe el elemento de la interfaz de usuario Microsoft.Common.PasswordBox para Azure Portal. Permite a los usuarios proporcionar un valor secreto al implementar aplicaciones administradas.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 5f7d3a47482ccec9778b102d3509b5f8ef343185
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75649768"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Elemento de interfaz de usuario Microsoft.Common.PasswordBox

Control que puede usarse para proporcionar una contraseña y confirmarla.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario

![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>Salida de ejemplo

```json
"p4ssw0rd"
```

## <a name="remarks"></a>Observaciones

- Este elemento no admite la propiedad `defaultValue`.
- Para más detalles sobre la implementación de `constraints`, consulte [Microsoft.Common.TextBox](microsoft-common-textbox.md).
- Si `options.hideConfirmation` está establecido en **true**, se oculta el segundo cuadro de texto para confirmar la contraseña del usuario. El valor predeterminado es **false**.

## <a name="next-steps"></a>Pasos siguientes

* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
