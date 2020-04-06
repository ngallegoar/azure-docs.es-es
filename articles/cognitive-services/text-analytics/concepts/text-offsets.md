---
title: Desplazamientos de texto en Text Analytics API
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los desplazamientos provocados por codificaciones de varios idiomas y Emojis.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 03/09/2020
ms.author: aahi
ms.reviewer: jdesousa
ms.openlocfilehash: 6e404c710a244f06676edf50c3f5c95a7d681e35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218530"
---
# <a name="text-offsets-in-the-text-analytics-api-output"></a>Desplazamientos de texto en la salida de Text Analytics API

La compatibilidad con varios idiomas y con Emojis ha dado lugar a codificaciones Unicode que usan más de un [punto de código](https://wikipedia.org/wiki/Code_point) para representar un solo carácter mostrado, llamado grafema. Por ejemplo, los Emojis como 🌷 y 👍 pueden utilizar varios caracteres para crear la forma con caracteres adicionales para los atributos visuales, como el tono de máscara. Del mismo modo, la palabra Hindi `अनुच्छेद` está codificada como cinco letras y tres marcas de combinación.

Debido a las distintas longitudes de posibles codificaciones de varios idiomas y Emojis, Text Analytics API puede devolver desplazamientos en la respuesta.

## <a name="offsets-in-the-api-response"></a>Desplazamientos en la respuesta de API. 

Siempre que se devuelvan desplazamientos en la respuesta de API, como [Reconocimiento de entidades con nombre](../how-tos/text-analytics-how-to-entity-linking.md) o [Análisis de sentimiento](../how-tos/text-analytics-how-to-sentiment-analysis.md), recuerde lo siguiente:

* Los elementos de la respuesta pueden ser específicos del punto de conexión al que se llamó. 
* Las cargas de HTTP POST/GET están codificadas en [UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp), que puede ser o no la codificación de caracteres predeterminada del compilador del lado cliente o sistema operativo.
* Los desplazamientos hacen referencia a recuentos de grafemas basados en el estándar [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0), no recuentos de caracteres.

## <a name="extracting-substrings-from-text-with-offsets"></a>Extracción de subcadenas de texto con desplazamientos

Los desplazamientos pueden causar problemas al usar métodos substring basados en caracteres, por ejemplo el método [substring()](https://docs.microsoft.com/dotnet/api/system.string.substring?view=netframework-4.8) de .NET. Un problema es que un desplazamiento puede hacer que un método substring finalice en mitad de una codificación de grafemas de carácter múltiple en lugar de al final.

En .NET, tenga en cuenta el uso de la clase [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8), que le permite trabajar con una cadena como una serie de elementos textuales, en lugar de objetos de caracteres individuales. También puede buscar bibliotecas del separador de grafemas en su entorno de software preferido. 

Text Analytics API devuelve estos elementos textuales también, por motivos de comodidad.

## <a name="see-also"></a>Consulte también

* [Información general de Text Analytics](../overview.md)
* [Análisis de opiniones](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Reconocimiento de entidades](../how-tos/text-analytics-how-to-entity-linking.md)
* [Detección de idioma](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Reconocimiento de idioma](../how-tos/text-analytics-how-to-language-detection.md)
