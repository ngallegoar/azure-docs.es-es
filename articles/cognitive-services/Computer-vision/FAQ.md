---
title: 'Preguntas más frecuentes: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Obtenga respuestas a las preguntas más frecuentes sobre Computer Vision API en Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: e8ab205a43e5cb1e8e2b96dbd9600e1fceb29403
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "68564602"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Preguntas frecuentes sobre Computer Vision API

> [!TIP]
> Si no puede encontrar respuestas a sus preguntas en estas P+F, puede plantearlas en la comunidad de Computer Vision API en [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) o ponerse en contacto con [Ayuda y soporte técnico en UserVoice](https://cognitive.uservoice.com/).

---

**Pregunta**: *¿Puedo entrenar a Computer Vision API para usar etiquetas personalizadas?  Por ejemplo, me gustaría introducir imágenes de razas de gatos para "entrenar" a AI y luego recibir el valor de raza en una solicitud de AI.*

**Respuesta**: Esta función no está disponible actualmente. Sin embargo, nuestros ingenieros están trabajando para incorporar esta funcionalidad a Computer Vision.

---

**Pregunta**: *¿Se puede usar Computer Vision localmente sin una conexión a Internet?*

**Respuesta**: Actualmente no ofrecemos una solución local.

---

**Pregunta**: *¿Se puede usar Computer Vision para leer matrículas de entidad de almacén?*

**Respuesta**: Vision API ofrece una buena detección de texto con OCR, pero en este momento no está actualizada para matrículas de entidad de almacén. En nuestro esfuerzo constante por mejorar nuestros servicios, hemos agregado OCR para el reconocimiento automático de matrículas a nuestra lista de solicitudes de características.

---

**Pregunta**: *¿Qué tipos de superficies se admiten para el reconocimiento de escritura a mano?*

**Respuesta**: La tecnología funciona con distintas clases de superficies, como pizarras, notas del producto y notas rápidas.

---

**Pregunta**: *¿Cuánto tarda la operación de reconocimiento de escritura a mano?*

**Respuesta**: Depende de la longitud del texto. Con textos más largos, puede tardar hasta varios segundos. Por lo tanto, una vez finalizada la operación de reconocimiento de texto escrito a mano, puede que deba esperar antes de poder recuperar los resultados mediante la operación de obtención de resultados de la operación de texto escrito a mano.

---

**Pregunta**: *¿Cómo trata la tecnología de reconocimiento de escritura a mano el texto que se ha insertado mediante un signo de intercalación en medio de una línea?*

**Respuesta**: La operación de reconocimiento de escritura a mano devuelve dicho texto como una línea independiente.

---

**Pregunta**: *¿Cómo trata la tecnología de reconocimiento de escritura a mano las palabras o líneas tachadas?*

**Respuesta**: Si las palabras están tachadas con varias líneas para presentarlas como irreconocibles, la operación de reconocimiento de escritura a mano no las selecciona. Sin embargo, si las palabras están tachadas con una sola línea, ese tachado se trata como ruido y la operación de reconocimiento de escritura a mano sigue seleccionando las palabras.

---

**Pregunta**: *¿Qué orientación de texto se admite en la tecnología de reconocimiento de escritura a mano?*

**Respuesta**: La operación de reconocimiento de escritura a mano puede seleccionar texto orientado en ángulos de entre 30 y 40 grados.

---
