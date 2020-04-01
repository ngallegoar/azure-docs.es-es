---
title: 'Alineación de palabras: Translator Text API'
titleSuffix: Azure Cognitive Services
description: Para recibir información de alineación, utilice el método Traducir e incluya el parámetro includeAlignment opcional.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: dd4ff1e39c062910f4627973c801dc3c51f345e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837223"
---
# <a name="how-to-receive-word-alignment-information"></a>Cómo recibir información de alineación de palabras

## <a name="receiving-word-alignment-information"></a>Recibir información de alineación de palabras
Para recibir información de alineación, utilice el método Traducir e incluya el parámetro includeAlignment opcional.

## <a name="alignment-information-format"></a>Formato de información de alineación
La alineación se devuelve como un valor de cadena del siguiente formato para cada palabra del origen. La información de cada palabra se divide por un espacio, incluso en el caso de los idiomas no separados por espacios (scripts), como el chino:

[[SourceTextStartIndex]\:[SourceTextEndIndex]–[TgtTextStartIndex]\:[TgtTextEndIndex]] *

Cadena de alineación de ejemplo: "0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21".

En otras palabras, los dos puntos separan el índice inicial y final, el guión separa los idiomas y el espacio separa las palabras. Una palabra se puede alinear con ninguna, una o varias palabras de otro idioma y las palabras alineadas pueden ser no contiguas. Si no existe información de alineación disponible, el elemento alignment estará vacío. El método no devuelve ningún error en ese caso.

## <a name="restrictions"></a>Restricciones
La alineación solo se devuelve para un subconjunto de pares de idiomas en este momento:
* de inglés a cualquier otro idioma;
* de cualquier otro idioma a inglés, excepto de chino simplificado, chino tradicional y letón a inglés;
* de japonés a coreano o de coreano a japonés. No recibirá información de alineación si la frase es una traducción preestablecida. Un ejemplo de traducción preestablecida es "Esto es una prueba", "Te quiero" y otras frases que se usan con mucha frecuencia.

## <a name="example"></a>Ejemplo

Ejemplo de JSON

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
