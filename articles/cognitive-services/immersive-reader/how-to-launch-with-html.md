---
title: Iniciar el Lector inmersivo con contenido HTML
titleSuffix: Azure Cognitive Services
description: En este artículo se explica cómo iniciar el Lector inmersivo con contenido HTML.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metang
ms.custom: devx-track-js
ms.openlocfilehash: d028e9ef2087b866b3c168483d55556e38f9a7cd
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92636568"
---
# <a name="how-to-launch-the-immersive-reader-with-html-content"></a>Cómo iniciar el Lector inmersivo con contenido HTML

En este artículo se demuestra cómo iniciar el Lector inmersivo con contenido HTML.

## <a name="prepare-the-html-content"></a>Preparación del contenido HTML

Coloque el contenido que desea representar en el Lector inmersivo dentro de un elemento contenedor. Asegúrese de que el elemento contenedor tenga un `id`único. El Lector inmersivo brinda compatibilidad con los elementos HTML básicos, consulte la [referencia](./reference.md#html-support) para obtener más información.

```html
<div id='immersive-reader-content'>
    <b>Bold</b>
    <i>Italic</i>
    <u>Underline</u>
    <strike>Strikethrough</strike>
    <code>Code</code>
    <sup>Superscript</sup>
    <sub>Subscript</sub>
    <ul><li>Unordered lists</li></ul>
    <ol><li>Ordered lists</li></ol>
</div>
```

## <a name="get-the-html-content-in-javascript"></a>Obtención del contenido HTML en JavaScript

Use el `id` del elemento contenedor para obtener el contenido HTML en el código de JavaScript.

```javascript
const htmlContent = document.getElementById('immersive-reader-content').innerHTML;
```

## <a name="launch-the-immersive-reader-with-your-html-content"></a>Inicio del Lector inmersivo con contenido HTML

Al llamar a `ImmersiveReader.launchAsync`, establezca la propiedad `mimeType` del fragmento en `text/html` para habilitar la representación HTML.

```javascript
const data = {
    chunks: [{
        content: htmlContent,
        mimeType: 'text/html'
    }]
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, data, YOUR_OPTIONS);
```

## <a name="next-steps"></a>Pasos siguientes

* Lea la [Referencia del SDK del Lector inmersivo](./reference.md)