---
title: ¿Qué es Lector inmersivo?
titleSuffix: Azure Cognitive Services
description: Lector inmersivo es una herramienta diseñada para ayudar a personas con dificultades de aprendizaje o para ayudar a los nuevos lectores y los estudiantes de idiomas con la comprensión lectora.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 01/4/2020
ms.author: metan
ms.custom: cog-serv-seo-aug-2020
keywords: readers, language learners, display pictures, improve reading, read content, translate
ms.openlocfilehash: a537845c0256316f9c47317a8805fa989a624f60
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "90985737"
---
# <a name="what-is-immersive-reader"></a>¿Qué es Lector inmersivo?

[Lector inmersivo](https://www.onenote.com/learningtools) es una herramienta diseñada de manera inclusiva que implementa técnicas demostradas para mejorar la comprensión lectora de nuevos lectores, estudiantes de idiomas y personas con dificultades de aprendizaje, como la dislexia. Con la biblioteca cliente de Lector inmersivo, puede aprovechar la misma tecnología que se usa en Microsoft Word y Microsoft OneNote para mejorar sus aplicaciones web. 

## <a name="use-immersive-reader-to-improve-reading-accessibility"></a>Uso de Lector inmersivo para mejorar la accesibilidad a la lectura 

Lector inmersivo está diseñado para hacer que la lectura sea más fácil y accesible para todos. Echemos un vistazo a algunas de las características principales de Lector inmersivo.

### <a name="isolate-content-for-improved-readability"></a>Aislar el contenido para una legibilidad mejorada

El lector inmersivo aísla contenido para mejorar la legibilidad. 

  ![Aislar el contenido para una legibilidad mejorada con Lector inmersivo](./media/immersive-reader.png)

### <a name="display-pictures-for-common-words"></a>Mostrar imágenes para palabras frecuentes

Para los términos de uso frecuente, Lector inmersivo mostrará una imagen.

  ![Diccionario de imágenes con Lector inmersivo](./media/picture-dictionary.png)

### <a name="highlight-parts-of-speech"></a>Resaltar elementos de la oración

Lector inmersivo puede usarse para ayudar a los lectores a comprender elementos de la oración y la gramática resaltando verbos, sustantivos, pronombres, etc.

  ![Mostrar partes de la oración con Lector inmersivo](./media/parts-of-speech.png)

### <a name="read-content-aloud"></a>Lectura de contenido en voz alta

La síntesis de voz (o texto a voz) está integrada en el servicio Lector inmersivo para permitir a los lectores seleccionar texto para que se lea en voz alta. 

  ![Lectura en voz alta de texto con Lector inmersivo](./media/read-aloud.png)

### <a name="translate-content-in-real-time"></a>Traducción de contenido en tiempo real

Lector inmersivo puede traducir texto en muchos idiomas en tiempo real. Esto resulta útil para mejorar la comprensión de los lectores a la hora de aprender un nuevo idioma.

  ![Traducción de texto con Lector inmersivo](./media/translation.png)

### <a name="split-words-into-syllables"></a>División de palabras en sílabas

Con Lector inmersivo, puede dividir las palabras en sílabas para mejorar la legibilidad o para pronunciar sílaba a sílaba palabras nuevas.

  ![Dividir palabras en sílabas con Lector inmersivo](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>¿Cómo funciona el Lector inmersivo?

Lector inmersivo es una aplicación web independiente. Cuando se invoca mediante la biblioteca cliente de Lector inmersivo, aparece en la parte superior de la aplicación web existente en un `iframe`. Cuando la aplicación web llama al servicio Lector inmersivo, puede especificar el contenido para mostrar en el lector. La biblioteca cliente de Lector inmersivo controla la creación y el estilo del `iframe` y la comunicación con el servicio back-end de Lector inmersivo. El servicio Lector inmersivo procesa contenido de elementos de la oración, realiza la conversión de texto a voz, ofrece traducción, etc.

## <a name="get-started-with-immersive-reader"></a>Introducción a Lector inmersivo

La biblioteca cliente de Lector inmersivo está disponible en C#, JavaScript, Java (Android), Kotlin (Android) y Swift (iOS). Empiece por:

* [Inicio rápido: Uso de la biblioteca cliente del Lector inmersivo](quickstarts/client-libraries.md)

## <a name="next-steps"></a>Pasos siguientes

Introducción a Lector inmersivo:

* Lea la [referencia sobre la biblioteca cliente de Lector inmersivo](./reference.md).
* Explore la [biblioteca cliente del Lector inmersivo en GitHub](https://github.com/microsoft/immersive-reader-sdk).
