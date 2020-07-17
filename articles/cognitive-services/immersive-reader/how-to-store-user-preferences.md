---
title: Almacenamiento de las preferencias del usuario
titleSuffix: Azure Cognitive Services
description: En este artículo se muestra cómo almacenar las preferencias del usuario.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: metan
ms.openlocfilehash: beb053551dc1fa28672c488b31dfb29ca3b53651
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85486882"
---
# <a name="how-to-store-user-preferences"></a>Procedimiento: Almacenamiento de las preferencias del usuario

En este artículo se muestra cómo almacenar las preferencias del usuario. Esta funcionalidad está pensada como medio alternativo para almacenar las preferencias del usuario en el caso donde el uso de cookies no sea deseable o factible.

## <a name="how-to-enable-storing-user-preferences"></a>Cómo habilitar el almacenamiento de preferencias del usuario

El parámetro `options` contiene la devolución de llamada `onPreferencesChanged`. Se llamará a esta función siempre que el usuario cambie sus preferencias (por ejemplo, cambie el tamaño del texto, el color del tema, la fuente, etc.). El parámetro `value` contiene una cadena, que representa las preferencias actuales del usuario. Esta cadena puede almacenarse con cualquier mecanismo que usted prefiera.

```typescript
const options = {
    onPreferencesChanged: (value: string) => {
        // Store user preferences here
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="how-to-load-user-preferences-into-the-immersive-reader"></a>Cómo cargar las preferencias del usuario en el Lector inmersivo

Pase las preferencias del usuario al Lector inmersivo mediante el parámetro `preferences`. A continuación encontrará un ejemplo trivial para almacenar y cargar las preferencias del usuario:

```typescript
let userPreferences = null;
const options = {
    preferences: userPreferences,
    onPreferencesChanged: (value: string) => {
        userPreferences = value;
    }
};
```

## <a name="next-steps"></a>Pasos siguientes

* Lea la [Referencia del SDK del Lector inmersivo](./reference.md)