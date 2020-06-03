---
title: 'Diccionario dinámico: Translator'
titleSuffix: Azure Cognitive Services
description: En este artículo se explica cómo usar la característica de diccionario dinámico de Translator de Azure Cognitive Services.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: de45867e717f001ab54e16c4b21f04494affd326
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996988"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Cómo usar un diccionario dinámico

Si ya conoce la traducción que quiere aplicar a una palabra o frase, puede proporcionarla como marcado dentro de la solicitud. El diccionario dinámico solo es seguro para los nombres compuestos, como nombres propios y nombres de productos.

**Sintaxis:**

<mstrans:dictionary translation="traducción de la frase">phrase</mstrans:dictionary>

**Requisitos:**

* Los idiomas `From` y `To` deben incluir inglés y otro idioma compatible. 
* Debe incluir el parámetro `From` en la solicitud de traducción de API en lugar de usar la característica de detección automática. 

**Ejemplo: en-de:**

Entrada de origen: `The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.`

Salida de destino: `Das Wort "wordomatic" ist ein Wörterbucheintrag.`

Esta característica funciona del mismo modo con y sin el modo HTML.

Úsela con moderación. Una manera mejor de personalizar la traducción es usar el Traductor personalizado. Custom Translator hace un uso completo del contexto y las probabilidades estadísticas. Si tiene o puede crear datos de aprendizaje que muestren el trabajo o la frase en contexto, obtendrá resultados mucho mejores. Para más información sobre el traductor personalizado, consulte [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).