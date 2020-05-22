---
title: 'Evitación de la traducción de contenido: Translator'
titleSuffix: Azure Cognitive Services
description: Impida que se traduzca contenido con Translator. Translator le permite etiquetar contenido para que no se traduzca.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: swmachan
ms.openlocfilehash: 33939976a0824ce8afeb2e6f6fb19e7033098683
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592702"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator"></a>Evitación de la traducción de contenido con Translator

Translator le permite etiquetar contenido para que no se traduzca. Por ejemplo, es posible que desee etiquetar código, un nombre de marca, una palabra o una frase que no tengan sentido si se traducen.

## <a name="methods-for-preventing-translation"></a>Métodos para impedir la traducción

1. Etiquete el contenido con `notranslate`. Esto solo funciona cuando el elemento textType de entrada se establece como HTML.

   Ejemplo:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

2. Utilice el [diccionario dinámico](dynamic-dictionary.md) para prescribir una traducción específica.

3. No pase la cadena a Translator para que se traduzca.

4. Traductor personalizado: utilice un [diccionario de Traductor personalizado](custom-translator/what-is-dictionary.md) para prescribir la traducción de una frase cuya probabilidad es del 100 %.


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Impedir la traducción en la llamada de Translator](reference/v3-0-translate.md)
