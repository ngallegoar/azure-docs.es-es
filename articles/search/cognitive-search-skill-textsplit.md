---
title: Aptitud cognitiva División de texto
titleSuffix: Azure Cognitive Search
description: Divida texto en fragmentos o páginas de texto en función de la longitud de una canalización de enriquecimiento con IA de Azure Cognitive Search.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: 52aaeb01fef551eee350c6db662c2690ef7b3e78
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84981955"
---
# <a name="text-split-cognitive-skill"></a>Aptitud cognitiva División de texto

La aptitud **División de texto** divide el texto en fragmentos de texto. Puede especificar si desea dividir el texto en oraciones o en páginas de una longitud determinada. Esta aptitud es especialmente útil si hay requisitos de longitud de texto máxima en otras aptitudes de bajada. 

> [!NOTE]
> Esta aptitud no está enlazada a una API de Cognitive Services y no se le cobrará por usarla. Sin embargo, debe [adjuntar un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md) para invalidar la opción del recurso **Gratis**, que tiene un límite de unos pocos enriquecimientos al día.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Parámetros de la aptitud

Los parámetros distinguen mayúsculas de minúsculas.

| Nombre de parámetro     | Descripción |
|--------------------|-------------|
| `textSplitMode`    | "pages" o "sentences" | 
| `maximumPageLength` | Si textSplitMode se establece en "pages", se refiere a la longitud de página máxima medida por `String.Length`. El valor mínimo es 300.  Si textSplitMode se establece en "pages", el algoritmo intentará dividir el texto en fragmentos con un tamaño máximo definido en "maximumPageLenth". En este caso, el algoritmo hará todo lo posible para dividir el texto con un límite de oraciones con lo que el tamaño del fragmento puede ser ligeramente menor que "maximumPageLength". | 
| `defaultLanguageCode` | (Opcional) Uno de los siguientes códigos de idioma: `da, de, en, es, fi, fr, it, ko, pt`. El valor predeterminado es inglés (en). Aspectos que se deben tener en cuenta:<ul><li>Si se pasa un formato “código de idioma-código de país”, solo se usa la parte “código de idioma” del formato.</li><li>Si el idioma no está en la lista anterior, la aptitud de división divide el texto en límites de carácter.</li><li>Proporcionar un código de idioma es útil para evitar cortar una palabra por la mitad para idiomas sin espacios como el chino, japonés y coreano.</li><li>Si no conoce el idioma (es decir, debe dividir el texto de la entrada en [LanguageDetectionSkill](cognitive-search-skill-language-detection.md)), el valor predeterminado de inglés (en) debe ser suficiente. </li></ul>  |


## <a name="skill-inputs"></a>Entradas de la aptitud

| Nombre de parámetro       | Descripción      |
|----------------------|------------------|
| `text`    | Texto que se dividirá en subcadenas. |
| `languageCode`    | (Opcional) Código de idioma para el documento. Si no conoce el idioma (es decir, debe dividir el texto de la entrada en [LanguageDetectionSkill](cognitive-search-skill-language-detection.md)), es seguro quitar esta entrada.  |

## <a name="skill-outputs"></a>Salidas de la aptitud 

| Nombre de parámetro     | Descripción |
|--------------------|-------------|
| `textItems`   | Una matriz de subcadenas que se han extraído. |


##  <a name="sample-definition"></a>Definición de ejemplo

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
        }
    ]
}
```

##  <a name="sample-input"></a>Entrada de ejemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "This is a the loan application for Joe Romero, a Microsoft employee who was born in Chile and who then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
                "languageCode": "en"
            }
        }
    ]
}
```

##  <a name="sample-output"></a>Salida de ejemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Casos de error
Si no se admite un idioma, se genera una advertencia y el texto se divide en límites de carácter.

## <a name="see-also"></a>Consulte también

+ [Aptitudes integradas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
