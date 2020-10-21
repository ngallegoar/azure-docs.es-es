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
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: ec39b3692a90f22409e85b5502d3ea874e3282d6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91742067"
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

2. Etiquete el contenido con `translate="no"`. Esto solo funciona cuando el tipo de texto de entrada está establecido como HTML.

   Ejemplo:

   ```html
   <span translate="no">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div translate="no">This will not be translated.</div>
   <div>This will be translated. </div>

3. Use the [dynamic dictionary](dynamic-dictionary.md) to prescribe a specific translation.

4. Don't pass the string to the Translator for translation.

5. Custom Translator: Use a [dictionary in Custom Translator](custom-translator/what-is-dictionary.md) to prescribe the translation of a phrase with 100% probability.


## Next steps
> [!div class="nextstepaction"]
> [Use the Translate operation to translate text](reference/v3-0-translate.md)
