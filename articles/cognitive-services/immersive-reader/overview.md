---
title: ¿Qué es la API de Lector inmersivo?
titleSuffix: Azure Cognitive Services
description: La API de Lector inmersivo es una herramienta que se puede usar para dar cabida a personas con diferencias de aprendizaje o para ayudar a los nuevos lectores y estudiantes de idiomas.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 01/4/2020
ms.author: metan
ms.openlocfilehash: 2eab7ab62f6a7a105dab33aa889af9f2be3a3fa3
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80052385"
---
# <a name="what-is-immersive-reader"></a>¿Qué es Lector inmersivo?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

El [Lector inmersivo](https://www.onenote.com/learningtools) es una herramienta diseñada de manera inclusiva que implementa técnicas demostradas para mejorar la comprensión lectora de nuevos lectores, estudiantes de idiomas y personas con diferencias de aprendizaje, como la dislexia.

Puede usar Lector inmersivo en la aplicación web mediante el SDK correspondiente.

## <a name="what-does-immersive-reader-do"></a>¿Qué hace el Lector inmersivo?

El Lector inmersivo está diseñado para hacer que la lectura sea más accesible para todos.

* Muestra el contenido en una vista de lectura mínima.

  ![Lector inmersivo](./media/immersive-reader.png)

* Muestra imágenes de las palabras más usadas.

  ![Diccionario de imágenes](./media/picture-dictionary.png)

* Resalta los sustantivos, verbos, adjetivos y adverbios.

  ![Elementos de la oración](./media/parts-of-speech.png)

* Lee el contenido en voz alta.

  ![Lectura en voz alta](./media/read-aloud.png)

* Traduce el contenido a otro idioma.

  ![Traducción](./media/translation.png)

* Divide las palabras en sílabas.

  ![Separación de sílabas](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>¿Cómo funciona el Lector inmersivo?

El Lector inmersivo es una aplicación web independiente que, cuando se invoca mediante el SDK de JavaScript para Lector inmersivo, aparece en la parte superior de la aplicación web existente en una`iframe`. Cuando se llama a la API para iniciar el Lector inmersivo, puede especificar el contenido que quiere mostrar en el Lector inmersivo. El SDK controla la creación y el estilo de `iframe` y la comunicación con el servicio back-end del Lector inmersivo, que procesa el contenido de los elementos de la oración, el texto a voz, la traducción, etc.

## <a name="next-steps"></a>Pasos siguientes

Introducción a Lector inmersivo:

* Vaya al [inicio rápido](./quickstart.md).
* Explorar el [SDK del Lector inmersivo en GitHub](https://github.com/microsoft/immersive-reader-sdk)
* Lea la [referencia del SDK del Lector inmersivo](./reference.md).
