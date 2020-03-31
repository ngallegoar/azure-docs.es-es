---
title: Recuentos de caracteres de Translator Text API
titleSuffix: Azure Cognitive Services
description: En este artículo se explica cómo la Translator Text API de Azure Cognitive Services cuenta los caracteres para que pueda entenderse cómo ingiere el contenido.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cb70b8624ac0d909511032622948f14f8764b153
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73888155"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Cómo hace Translator Text API para contar caracteres

Translator Text API cuenta cada punto de código Unicode de texto de entrada como un carácter. Cada traducción de un texto a un idioma se considera una traducción independiente, incluso si la solicitud se hizo en una única llamada API de traducción a varios idiomas. La longitud de la respuesta no importa.

Lo que importa es:

* El texto que se pasa a Translator Text API en el cuerpo de la solicitud
   * `Text` al usar los métodos Translate, Transliterate y Dictionary Lookup
   * `Text` y `Translation` al usar el método Dictionary Examples
* Todo el marcado: HTML, etiquetas XML, etc., en el campo de texto del cuerpo de la solicitud. La notación JSON que se usa para compilar la solicitud (por ejemplo "Text:") no se cuenta.
* Una letra individual
* Signos de puntuación
* Un espacio, tabulación, marcado y cualquier tipo de carácter de espacio en blanco
* Cada punto de código que se define en Unicode
* Una traducción repetida, incluso si ha traducido previamente el mismo texto

Para los scripts basados en ideogramas, como el chino y kanji japonés, Translator Text API aún contará el número de puntos de código Unicode, un carácter por ideograma. Excepción: Los suplentes Unicode cuentan como dos caracteres.

El número de solicitudes, palabras, frases o bytes es irrelevante en el recuento de caracteres.

Las llamadas a los métodos Detect y BreakSentence no se cuentan en el consumo de caracteres. Sin embargo, se espera que las llamadas a los métodos Detect y BreakSentence tengan una proporción razonable frente al uso de otras funciones que se cuentan. Si el número de llamadas Detect o BreakSentence que hace supera en 100 veces el número de otros métodos contados, Microsoft se reserva el derecho a restringir el uso de los métodos Detect y BreakSentence.


Para obtener más información sobre los recuentos de caracteres, consulte las [preguntas más frecuentes sobre Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
