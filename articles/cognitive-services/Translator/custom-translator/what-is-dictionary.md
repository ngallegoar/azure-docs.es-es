---
title: ¿Qué es un diccionario? - Custom Translator
titleSuffix: Azure Cognitive Services
description: Un diccionario es un documento alineado que especifica una lista de frases u oraciones (con sus respectivas traducciones) que Microsoft Translator siempre debe traducir de la misma manera. A veces, los diccionarios también se denominan glosarios o bases terminológicas.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 826da5c3754ad03ac1fb62288f0b03ee2353d1f3
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85962269"
---
# <a name="what-is-a-dictionary"></a>¿Qué es un diccionario?

Un diccionario es un par alineado de documentos que especifica una lista de frases u oraciones y sus traducciones correspondientes. Use un diccionario en el entrenamiento cuando quiera que la aplicación Traductor siempre traduzca todas las instancias de la frase u oración de origen de la forma que especifique el diccionario. A veces, los diccionarios se denominan glosarios o bases terminológicas. Puede pensar en el diccionario como un "copiar y remplazar" a la fuerza para todos los términos incluidos. Además, la característica Traductor personalizado crea y usa sus propios diccionarios de uso general para mejorar la calidad de su traducción. Sin embargo, los diccionarios proporcionados por el cliente tienen prioridad y serán los primeros en usarse al buscar palabras o frases.

Los diccionarios solo funcionan en los proyectos en los pares de idiomas que tengan detrás un modelo de red neuronal general de Microsoft totalmente compatible. [Consulte la lista completa de idiomas](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization).

## <a name="phrase-dictionary"></a>Diccionario de frases
Al incluir un diccionario de frases durante el entrenamiento de un modelo, todas las palabras y frases incluidas se traducen de la forma especificada. El resto de la oración se traduce como de costumbre. Puede usar un diccionario de frases para especificar las frases que no se deben traducir si proporciona la misma frase sin traducir en los archivos de origen y de destino del diccionario.

## <a name="sentence-dictionary"></a>Diccionario de oraciones
El diccionario de oraciones le permite especificar una traducción exacta de destino para una oración de origen. Para que exista una coincidencia con el diccionario de oraciones, la oración completa que se envíe debe coincidir con la entrada de origen del diccionario.  Si solo coincide una parte de la oración, la entrada será una coincidencia.  Cuando se detecta una coincidencia, se devuelve la entrada de destino del diccionario de oraciones.

## <a name="dictionary-only-trainings"></a>Entrenamientos de solo diccionario
Puede entrenar un modelo usando solo los datos de un diccionario. Para ello, seleccione solo el documento de diccionario (o los documentos de diccionario) que quiere incluir y pulse en Crear modelo. Ya que se trata de un entrenamiento de solo diccionario, no hay un número mínimo de oraciones de aprendizaje necesarias. Normalmente, el modelo completará el entrenamiento mucho más rápido que un entrenamiento estándar.  Los modelos resultantes utilizarán los modelos base de Microsoft para la traducción, con la adición de los diccionarios que ha agregado.  No obtendrá un informe de pruebas.

>[!Note]
>Custom Translator no alinea las oraciones en los archivos de diccionario, por lo que es importante que los documentos de diccionario incluyan el mismo número de frases u oraciones tanto de origen como de destino, y que estén perfectamente alineados.

## <a name="recommendations"></a>Recomendaciones

- Los diccionarios no reemplazan al entrenamiento de modelos mediante datos de aprendizaje. Se recomienda evitarlos y dejar que el sistema aprenda de los datos de aprendizaje. Sin embargo, cuando las oraciones o los nombres compuestos deban representarse tal cual, se debe usar un diccionario.
- El diccionario de frases debe usarse con moderación. Por tanto, debe saber que cuando se reemplaza una frase dentro de una oración, el contexto de esa oración se pierde o se ve limitado para continuar con la traducción del resto de la oración. Como resultado, aunque la frase o palabra en la oración se traducirá según el diccionario proporcionado, la calidad general de la traducción de la oración a menudo se verá afectada.
- El diccionario de frases funciona bien para los nombres compuestos, como nombres de producto ("Microsoft SQL Server"), nombres propios ("Ciudad de Hamburgo") o las características del producto ("tabla dinámica"). No funciona igual de bien para los verbos o adjetivos, ya que normalmente se declinan en el idioma de origen o en el de destino. El procedimiento recomendado es agregar al diccionario de frases solamente nombres compuestos.
- Cuando se usa un diccionario de frases, las mayúsculas y los signos de puntuación son importantes. Las entradas del diccionario solo coincidirán con las palabras y frases de la oración de entrada que utilicen exactamente las mismas mayúsculas y minúsculas que se especifican en el archivo de diccionario de origen. Además, las traducciones reflejarán tanto las mayúsculas y minúsculas como los signos de puntuación proporcionados en el archivo de diccionario de destino. Por ejemplo, si ha entrenado un sistema de inglés a español que usa un diccionario de frases que especifica "US" en el archivo de código fuente y "EE. UU." en el archivo de destino. Cuando solicite la traducción de una frase que incluya la palabra "us" (no en mayúsculas), NO coincidirá con el diccionario. Sin embargo, si solicita la traducción de una frase que contiene la palabra "US" (en mayúsculas), coincidiría con el diccionario y la traducción contendría "EE. UU." Tenga en cuenta que es posible que las mayúsculas y minúsculas, y la puntuación de la traducción sean diferentes tanto de las especificadas en el archivo de destino del diccionario como de las del origen. Sigue las reglas del idioma de destino.
- Si una palabra aparece varias veces en un archivo de diccionario, el sistema usará siempre la última entrada proporcionada. Por consiguiente, el diccionario no debe contener varias traducciones de la misma palabra.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información en relación a las [directrices sobre los formatos de documento](document-formats-naming-convention.md).
