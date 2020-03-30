---
title: Establecimiento de la directiva de cookies del Lector inmersivo
titleSuffix: Azure Cognitive Services
description: En este artículo se explica cómo establecer la directiva de cookies del Lector inmersivo.
services: cognitive-services
author: pasta
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: pasta
ms.openlocfilehash: 6de651f1eb51ea1bc941c2af675c8a8d5b0f9cd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946031"
---
# <a name="how-to-set-the-cookie-policy-for-the-immersive-reader"></a>Cómo establecer la directiva de cookies del Lector inmersivo

El Lector inmersivo deshabilitará el uso de cookies de forma predeterminada. Si habilita el uso de cookies, el Lector inmersivo puede usar las cookies para mantener las preferencias del usuario y realizar un seguimiento del uso de características. Si habilita el uso de cookies en el Lector inmersivo, tenga en cuenta los requisitos de la directiva de cumplimiento de cookies de la UE. Es responsabilidad de la aplicación host obtener cualquier consentimiento del usuario necesario según la directiva de cumplimiento de cookies de la UE.

La directiva de cookies se puede establecer a través de las opciones de [Lector inmersivo](../reference.md#options). Consulte la [enumeración CookiePolicy](../reference.md#cookiepolicy-enum) para obtener más información.

## <a name="enable-cookie-usage"></a>Habilitar uso de cookies

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Enable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="disable-cookie-usage"></a>Deshabilitar uso de cookies

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Disable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="next-steps"></a>Pasos siguientes

* Consulte el [inicio rápido de Node.js](../quickstart-nodejs.md) para ver qué más puede hacer con el SDK del Lector inmersivo con Node.js
* Vea el [tutorial para Python](../tutorial-python.md) para consultar qué más puede hacer con el SDK del Lector inmersivo con Python.
* Vea el [tutorial para Swift](../tutorial-ios-picture-immersive-reader.md) para consultar qué más puede hacer con el SDK del Lector inmersivo con Swift.
* Explorar el [SDK del Lector inmersivo](https://github.com/microsoft/immersive-reader-sdk) y agregar la [Referencia del SDK del Lector inmersivo](../reference.md)