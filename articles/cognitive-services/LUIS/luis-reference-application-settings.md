---
title: 'Configuración de la aplicación: LUIS'
description: La configuración de las aplicaciones de Language Understanding de Azure Cognitive Services se almacena en la aplicación y en el portal.
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 9e17736cd6ff5074a6eab76a6cf5bdb8acedc185
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382204"
---
# <a name="application-settings"></a>Configuración de la aplicación

Esta configuración de la aplicación se almacena en la aplicación [exportada](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) y [actualizada](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) con las API REST. Al cambiar la configuración de la versión de la aplicación, se restablece el estado de entrenamiento de la aplicación a no entrenado.

Obtenga información sobre los [conceptos](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) de diacríticos y puntuación.

|Configuración|Valor predeterminado|Notas|
|--|--|--|
|NormalizePunctuation|True|Quita los signos de puntuación.|
|NormalizeDiacritics|True|Quita las marcas diacríticas.|

## <a name="diacritics-normalization"></a>Normalización de las marcas diacríticas

Active la normalización de expresiones para las marcas diacríticas en el archivo de aplicación JSON de LUIS en el parámetro `settings`.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
]
```

Las declaraciones siguientes muestran cómo afecta la normalización de las marcas diacríticas a las grabaciones de voz:

|Con las marcas diacríticas establecidas en false|Con las marcas diacríticas establecidas en true|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Idiomas admitidos para marcas diacríticas

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Marcas diacríticas `pt-br` de portugués (brasileño)

|Marcas diacríticas establecidas en false|Marcas diacríticas establecidas en true|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`|
|||

#### <a name="dutch-nl-nl-diacritics"></a>Marcas diacríticas `nl-nl` del neerlandés

|Marcas diacríticas establecidas en false|Marcas diacríticas establecidas en true|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Marcas diacríticas `fr-` del francés

Esto incluye las referencias culturales secundarias del francés y canadiense.

|Marcas diacríticas establecidas en false|Marcas diacríticas establecidas en true|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`|
|`ê`|`e`|
|`î`|`i`|
|`ô`|`o`|
|`û`|`u`|
|`ç`|`c`|
|`ë`|`e`|
|`ï`|`i`|
|`ü`|`u`|
|`ÿ`|`y`|

#### <a name="german-de-de-diacritics"></a>Marcas diacríticas `de-de` del alemán

|Marcas diacríticas establecidas en false|Marcas diacríticas establecidas en true|
|--|--|
|`ä`|`a`|
|`ö`|`o`|
|`ü`|`u`|

#### <a name="italian-it-it-diacritics"></a>Marcas diacríticas `it-it` del italiano

|Marcas diacríticas establecidas en false|Marcas diacríticas establecidas en true|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`|
|`í`|`i`|
|`î`|`i`|
|`ò`|`o`|
|`ó`|`o`|
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>Marcas diacríticas `es-` del español

Esto incluye tanto al mexicano y español.

|Marcas diacríticas establecidas en false|Marcas diacríticas establecidas en true|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Normalización de la puntuación

Active la normalización de expresiones para la puntuación en el archivo de aplicación JSON de LUIS en el parámetro `settings`.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
]
```

Las expresiones siguientes muestran cómo la puntuación influye en las expresiones:

|Con la puntuación establecida en false|Con la puntuación establecida en true|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Puntuación suprimida

Se quita la siguiente puntuación con `NormalizePunctuation` establecido en true.

|Signos de puntuación|
|--|
|`-`|
|`.`|
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre los [conceptos](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) de diacríticos y puntuación.
