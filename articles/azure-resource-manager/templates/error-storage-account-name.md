---
title: Errores de nombre de cuenta de almacenamiento
description: Describe los errores que pueden producirse al especificas un nombre de cuenta de almacenamiento.
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.openlocfilehash: 5b2706d8540ea38ef08bf7ca0f804e6811a93085
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153979"
---
# <a name="resolve-errors-for-storage-account-names"></a>Resolución de errores de nombres de cuenta de almacenamiento

En este artículo se describen errores de nombre que pueden producirse al implementar una cuenta de almacenamiento.

## <a name="symptom"></a>Síntoma

Si el nombre de cuenta de almacenamiento incluye caracteres prohibidos, recibirá un error como:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

Para las cuentas de almacenamiento, debe proporcionar un nombre al recurso que sea único en Azure. Si no lo hace, recibirá un error como el siguiente:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Si implementa una cuenta de almacenamiento con el mismo nombre que una que ya hay en la suscripción, pero proporciona una ubicación diferente, recibirá un error que indica que la cuenta de almacenamiento ya se encuentra en otra ubicación. Elimine la cuenta de almacenamiento que ya existe o proporcione la misma ubicación que la de la cuenta de almacenamiento.

## <a name="cause"></a>Causa

Los nombres de cuentas de almacenamiento deben tener entre 3 y 24 caracteres, y usar solo números y letras minúsculas. El nombre debe ser único.

## <a name="solution"></a>Solución

Asegúrese de que el nombre de la cuenta de almacenamiento es único. Puede crear un nombre único concatenando la convención de nomenclatura con el resultado de la función [uniqueString](template-functions-string.md#uniquestring) .

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Asegúrese de que el nombre de cuenta de almacenamiento no supere los 24 caracteres. La función [uniqueString](template-functions-string.md#uniquestring) devuelve 13 caracteres. Si concatena un prefijo o un sufijo con el resultado **uniqueString**, proporcione un valor que tenga 11 caracteres o menos.

```json
"parameters": {
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
    "description": "The value to use for starting the storage account name."
    }
  }
}
```

Asegúrese de que el nombre de cuenta de almacenamiento no incluye caracteres especiales ni letras mayúsculas.