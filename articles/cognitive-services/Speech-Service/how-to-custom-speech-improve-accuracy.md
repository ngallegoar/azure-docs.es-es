---
title: 'Mejora de un modelo de Habla personalizada: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Determinados tipos de transcripciones etiquetadas por humanos y el texto relacionado pueden mejorar la precisión del reconocimiento de un modelo de conversión de voz a texto en función del escenario en que se habla.
services: cognitive-services
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: v-demjoh
ms.openlocfilehash: bb904482f3cb5900b724803816269f1b10ab3720
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83727868"
---
# <a name="improve-custom-speech-accuracy"></a>Mejora de la precisión de Habla personalizada

En este artículo, aprenderá a mejorar la calidad de un modelo personalizado mediante la incorporación del audio, las transcripciones etiquetadas por humanos y el texto relacionado.

## <a name="accuracy-in-different-scenarios"></a>Precisión en diferentes escenarios

Los escenarios de reconocimiento de voz varían en función de la calidad y del idioma (vocabulario y estilo del habla) del audio. En la siguiente tabla se examinan cuatro escenarios habituales:

| Escenario | Calidad de audio | Vocabulario | Estilo del habla |
|----------|---------------|------------|----------------|
| Centro de llamadas | Baja, 8 kHz, podrían ser dos seres humanos en un canal de audio, se podría comprimir. | Limitado, único para dominio y productos. | Conversacional, estructura flexible. |
| Asistente por voz (como Cortana o una ventana de autoservicio) | Alta, 16 kHz. | Entidad intensa (títulos de canciones, productos y ubicaciones). | Palabras y frases claramente indicadas. |
| Dictado (mensaje instantáneo, notas, búsqueda) | Alta, 16 kHz. | Varía | Toma de notas. |
| Subtítulos de vídeos | Variada, incluido el uso variado del micrófono, con música. | Variado (reuniones, voz recitada, letras de canciones). | Lectura, preparada o estructura flexible. |

Los distintos escenarios generan resultados de distinta calidad. En la tabla siguiente se examina cómo se evalúa el contenido de estos cuatro escenarios en [Word Error Rate (WER)](how-to-custom-speech-evaluate-data.md). En la tabla se muestran los tipos de error más comunes en cada escenario.

| Escenario | Calidad del reconocimiento de voz | Errores de inserción | Errores de eliminación | Errores de sustitución |
|----------|----------------------------|------------------|-----------------|---------------------|
| Centro de llamadas | Medio (WER inferior al 30 %) | Pocos, excepto cuando otras personas hablan en segundo plano. | Pueden ser muchos. En los centros de llamadas puede haber mucho ruido y el hecho de que haya muchas personas hablando a la vez puede confundir al modelo. | Mediana. Los nombres de los productos y personas pueden provocar estos errores. |
| Asistente por voz | Alta (el WER puede ser inferior al 10 %) | Bajo | Bajo | Medio, debido títulos de canciones, nombres de producto o ubicaciones. |
| Dictado | Alta (el WER puede ser inferior al 10 %) | Bajo | Bajo | Alto |
| Subtítulos de vídeos | Depende del tipo de vídeo (el WER puede ser inferior al 50 %) | Bajo | Puede ser un número elevado debido a la música, a ruidos y a la calidad del micrófono. | La jerga puede generar estos errores. |

La determinación de los componentes del WER (número de errores de inserción, eliminación y sustitución) ayuda a determinar qué tipo de datos se van a agregar para mejorar el modelo. Use el [portal de Habla personalizada](https://speech.microsoft.com/customspeech) para ver la calidad de un modelo de referencia. El portal notifica las tasas de error de inserción, sustitución y eliminación que se combinan en la tasa de calidad del WER.

## <a name="improve-model-recognition"></a>Mejora del reconocimiento de modelos

Para reducir los errores de reconocimiento, agregue datos de entrenamiento en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech). 

Agregue materiales de origen periódicamente para realizar un mantenimiento del modelo personalizado. El modelo personalizado necesita entrenamiento adicional para mantenerse al tanto de los cambios que se producen en sus entidades. Por ejemplo, es posible que necesite actualizar los nombres de producto, los nombres de canciones o las nuevas ubicaciones del servicio.

En las secciones siguientes se describe de qué forma puede reducir el número de errores cada uno de los tipo de datos de entrenamiento adicionales.

### <a name="add-related-text-sentences"></a>Adición de oraciones de texto relacionadas

Las oraciones de texto relacionadas adicionales pueden reducir principalmente los errores de sustitución debidos al reconocimiento erróneo de palabras comunes y palabras específicas del dominio ya que las muestra en contexto. Las palabras específicas del dominio pueden ser palabras poco frecuentes o compuestas, pero su pronunciación debe ser fácil de reconocer.

> [!NOTE]
> Evite las oraciones de texto relacionadas que tengan ruido, como caracteres o palabras irreconocibles.

### <a name="add-audio-with-human-labeled-transcripts"></a>Adición de audio con transcripciones etiquetadas por humanos

El audio con transcripciones etiquetadas por humanos ofrece las máximas mejoras en cuando a la precisión si el audio procede del caso de uso de destino. Los ejemplos deben cubrir todo el ámbito de la voz. Por ejemplo, el centro de llamadas de una tienda recibirá sobre todo llamadas relacionadas con trajes de baño o gafas de sol en los meses de verano. Asegúrese de que el ejemplo incluya todo el ámbito de voz que desea detectar.

Tenga en cuenta estos detalles:

* Habla personalizada solo puede capturar el contexto de las palabras para reducir los errores de sustitución, no los de inserción ni los de eliminación.
* Evite ejemplos que incluyan errores de transcripción, pero incluya audio con distintas calidades.
* Evite oraciones no relacionadas con el dominio del problema. Las oraciones no relacionadas pueden dañar el modelo.
* Cuando la calidad de las transcripciones varía, puede duplicar las frases cuya calidad sea excepcionalmente buena (como las transcripciones excelentes que incluyan frases clave) para aumentar su ponderación.

### <a name="add-new-words-with-pronunciation"></a>Adición de nuevas palabras con su pronunciación

Es posible que las palabras compuestas o cuyo grado de especialización sea elevado tengan pronunciaciones únicas. Estas palabras se pueden reconocer si cada una de ellas se puede desglosar en palabras más pequeñas para pronunciarla. Por ejemplo, para reconocer **Xbox**, pronúnciela como **X box**. Este enfoque no aumentará la precisión general, pero puede aumentar el reconocimiento de estas palabras clave.

> [!NOTE]
> Actualmente, esta técnica solo está disponible para algunos idiomas. Para más información, vea la personalización de la pronunciación en [la tabla de conversión de voz a texto](language-support.md).

## <a name="sources-by-scenario"></a>Orígenes por escenario

En la tabla siguiente se muestran escenarios de reconocimiento de voz y se enumeran los materiales de origen que se deben tener en cuenta en las tres categorías de contenido de entrenamiento mencionadas anteriormente.

| Escenario | Oraciones de texto relacionadas | Transcripciones de audio con etiqueta humana | Nuevas palabras con su pronunciación |
|----------|------------------------|------------------------------|------------------------------|
| Centro de llamadas             | Documentos de marketing, sitios web, reseñas de productos relacionados con la actividad del centro de llamadas. | Llamadas al centro de llamadas transcritas por humanos. | Términos que tienen pronunciación ambigua (vea el ejemplo anterior de Xbox). |
| Asistente por voz         | Enumerar oraciones que usan todas las combinaciones de comandos y entidades. | Grabar voces que dicen comandos a un dispositivo y transcribirlas en texto. | Nombres (películas, canciones, productos) que tienen pronunciaciones únicas. |
| Dictado               | Entrada escrita, como mensajes instantáneos o correos electrónicos. | Similar a la anterior. | Similar a la anterior. |
| Subtítulos de vídeos | Guiones de programas de TV, películas, contenido de marketing y resúmenes en vídeo. | Transcripciones exactas de vídeos. | Similar a la anterior. |

## <a name="next-steps"></a>Pasos siguientes

- [Entrenamiento del modelo](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Recursos adicionales

- [Preparación y prueba de los datos](how-to-custom-speech-test-data.md)
- [Inspección de los datos](how-to-custom-speech-inspect-data.md)