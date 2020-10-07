---
ms.openlocfilehash: c87b9dc22ecd937abb27417aeddc1e30c0d724e7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "85114573"
---

## <a name="using-this-example-knowledge-base"></a>Uso de esta base de conocimiento de ejemplo

La base de conocimiento de este inicio rápido comienza con 2 pares conversacionales de QnA cuyo objetivo de simplificar el ejemplo y tener identificadores muy predecibles para usar en el método de actualización, de forma que se asocien los avisos de seguimiento con las preguntas a nuevos pares. Esta actividad se planificó e implementó en un orden específico para este inicio rápido.

Si planea desarrollar la base de conocimiento a lo largo del tiempo con avisos de seguimiento que dependen de los pares de QnA existentes, puede elegir:
* En el caso de bases de conocimiento más grandes, administrar la base de conocimiento en un editor de texto o en una herramienta TSV que admita automatización y, luego, reemplazar por completo la base de conocimiento inmediatamente con una actualización.
* En el caso de bases de conocimiento más pequeñas, administrar los avisos de seguimiento por completo en el portal de QnA Maker.

Detalles sobre los pares de QnA usados en este inicio rápido:
* Tipos de pares de QnA: hay 2 tipos de pares de QnA en esta base de conocimiento, después de la actualización: chitchat e información específica del dominio. Esto es habitual si la base de conocimiento está asociada a una aplicación de conversación, como un bot de chat.
* Aunque las respuestas de la base de conocimiento se pueden filtrar por metadatos o mediante el uso de mensajes de seguimiento, en este inicio rápido no se muestra eso. Busque esos ejemplos de generateAnswer independientes del lenguaje [aquí](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md).
* Aunque el texto de respuesta tiene el formato Markdown y puede contener una [gran variedad](../reference-markdown-format.md) de contenido de Markdown, como imágenes (imágenes disponibles públicamente basadas en Internet), vínculos (a direcciones URL disponibles públicamente) y viñetas, este inicio rápido no usa esa variedad.
