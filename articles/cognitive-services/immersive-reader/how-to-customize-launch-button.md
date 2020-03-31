---
title: Personalización del botón del Lector inmersivo
titleSuffix: Azure Cognitive Services
description: En este artículo se muestra cómo personalizar el botón que inicia el Lector inmersivo.
services: cognitive-services
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: 6461cc60a237cf834226b07db5e1ea6fbbea7a92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946067"
---
# <a name="how-to-customize-the-immersive-reader-button"></a>Personalización del botón del Lector inmersivo

En este artículo se muestra cómo personalizar el botón que inicia el Lector inmersivo para ajustarse a las necesidades de la aplicación.

## <a name="add-the-immersive-reader-button"></a>Agregar el botón del Lector inmersivo

El SDK del Lector inmersivo proporciona el estilo predeterminado para el botón para iniciar el Lector inmersivo. Use el atributo de clase `immersive-reader-button` para habilitar este estilo.

```html
<div class='immersive-reader-button'></div>
```

## <a name="customize-the-button-style"></a>Personalizar el estilo del botón

Utilice el atributo `data-button-style` para establecer el estilo del botón. Los valores permitidos son: `icon`, `text` y `iconAndText`. El valor predeterminado es `icon`.

### <a name="icon-button"></a>Botón de icono

```html
<div class='immersive-reader-button' data-button-style='icon'></div>
```

Esto representa lo siguiente:

![Botón de icono](./media/button-icon.png)

### <a name="text-button"></a>Botón de texto

```html
<div class='immersive-reader-button' data-button-style='text'></div>
```

Esto representa lo siguiente:

![Botón de icono](./media/button-text.png)

### <a name="icon-and-text-button"></a>Botón de icono y texto

```html
<div class='immersive-reader-button' data-button-style='iconAndText'></div>
```

Esto representa lo siguiente:

![Botón de icono](./media/button-icon-and-text.png)

## <a name="customize-the-button-text"></a>Personalizar el texto del botón

Configure el idioma y el texto alternativo para el botón mediante el atributo `data-locale`. El idioma predeterminado es el inglés.

```html
<div class='immersive-reader-button' data-locale='fr-FR'></div>
```

## <a name="customize-the-size-of-the-icon"></a>Personalizar el tamaño del icono

El tamaño del icono del Lector inmersivo se puede configurar mediante el atributo `data-icon-px-size`. Esto establece el tamaño del icono en píxeles. El tamaño predeterminado es de 20px.

```html
<div class='immersive-reader-button' data-icon-px-size='50'></div>
```

## <a name="next-steps"></a>Pasos siguientes

* Lea la [Referencia del SDK del Lector inmersivo](./reference.md)