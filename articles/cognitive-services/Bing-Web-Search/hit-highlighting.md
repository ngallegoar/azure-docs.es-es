---
title: Uso de marcadores de decoración para resaltar texto en Bing Web Search API
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar los marcadores de decoración para resaltar texto y referencias en los resultados de búsqueda con Bing Web Search API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 5365B568-EA55-4D97-8FBE-0AF60158D4D5
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: scottwhi
ms.openlocfilehash: a6d394fec6e7cf0a230f61ad05c236a1f84dad9d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "68854006"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Uso de marcadores de decoración para resaltar texto

Bing admite el resaltado de aciertos, que marca los términos de la consulta (u otros términos que Bing considere pertinentes) en las cadenas de presentación de algunas respuestas. Por ejemplo, los campos `name`, `displayUrl` y `snippet` del resultado de una página web pueden contener términos de consulta marcados. 

De forma predeterminada, Bing no incluye marcadores de resaltado en las cadenas de presentación. Para habilitar los marcadores, incluya el parámetro de consulta `textDecorations` en la solicitud y establézcalo en `true`.

## <a name="hit-highlighting-example"></a>Ejemplo de resaltado de aciertos

En el ejemplo siguiente se muestra un resultado de web para `Sailing Dinghy`. Bing ha marcado el principio y el final del término de consulta con los caracteres Unicode E000 y E001.
  
![Resaltado de aciertos](./media/cognitive-services-bing-web-api/bing-hit-highlighting.png) 

Antes de mostrar la cadena en la interfaz de usuario, reemplace los caracteres Unicode por aquellos adecuados para el formato de presentación.

## <a name="marker-formatting"></a>Formato del marcador

Bing ofrece la opción de usar caracteres Unicode o etiquetas HTML como marcadores. Para especificar los marcadores que se van a usar, incluya el parámetro de consulta [textFormat](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#textformat): 

| Value             | Marcador                       |
|-------------------|------------------------------|
| `textFormat=Raw`  | Caracteres Unicode (valor predeterminado) |
| `textFormat=HTML` | Caracteres HTML              |

## <a name="additional-text-decorations"></a>Decoraciones de texto adicionales

Bing puede devolver varias decoraciones de texto diferentes. Por ejemplo, una respuesta de `Computation` puede contener marcadores de subíndice para el término de consulta `log(2)` en el campo `expression`.

![marcadores de cálculo](./media/cognitive-services-bing-web-api/bing-markers-computation.png) 

Si la solicitud no hubiese especificado decoraciones, el campo `expression` contendría `log10(2)`. 

Si `textDecorations` es `true`, Bing puede incluir los siguientes marcadores en las cadenas de presentación de las respuestas. Si no hay ninguna etiqueta HTML equivalente, la celda correspondiente de la tabla aparece vacía.

|Unicode|HTML|Descripción
|-|-|-
|U+E000|\<b>|Marca el principio del término de la consulta (resaltado de aciertos)
|U+E001|\</b>|Marca el final del término de la consulta
|U+E002|\<i>|Marca el principio del contenido en cursiva 
|U+E003|\</i>|Marca el final del contenido en cursiva
|U+E004|\<br/>|Marca un salto de línea
|U+E005||Marca el principio de un número de teléfono
|U+E006||Marca el final de un número de teléfono
|U+E007||Marca el principio de una dirección
|U+E008||Marca el final de una dirección
|U+E009|\&nbsp;|Marca un espacio de no separación
|U+E00C|\<strong>|Marca el principio del contenido en negrita
|U+E00D|\</strong>|Marca el final del contenido en negrita
|U+E00E||Marca el principio del contenido cuyo fondo debe ser más claro que el fondo circundante
|U+E00F||Marca el final del contenido cuyo fondo debe ser más claro que el fondo circundante
|U+E010||Marca el principio del contenido cuyo fondo debe ser más oscuro que el fondo circundante
|U+E011||Marca el final del contenido cuyo fondo debe ser más oscuro que el fondo circundante
|U+E012|\<del>|Marca el principio del contenido que debe tacharse
|U+E013|\</del>|Marca el final del contenido que debe tacharse
|U+E016|\<sub>|Marca el principio del contenido de subíndice
|U+E017|\</sub>|Marca el final del contenido de subíndice
|U+E018|\<sup>|Marca el principio del contenido de superíndice
|U+E019|\</sup>|Marca el final del contenido de superíndice

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es Bing Web Search API?](overview.md) 
* [Cambio de tamaño y recorte de miniaturas](resize-and-crop-thumbnails.md)