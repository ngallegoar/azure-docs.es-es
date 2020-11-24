---
title: 'Diseño de una base de conocimiento: QnA Maker'
description: Una base de conocimiento de QnA Maker consta de un conjunto de pares de preguntas y respuestas, así como metadatos opcionales asociados a cada par de pregunta y respuesta.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 09/01/2020
ms.openlocfilehash: ef5476ade205109f5dfede1b3bb2c3a4ae2e81a7
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506012"
---
# <a name="question-and-answer-pair"></a>Par de pregunta y respuesta

Una base de conocimiento consta de pares de pregunta y respuesta (PyR).  Cada par contiene una respuesta, además de toda la información asociada a dicha _respuesta_. Una respuesta se parece un poco a una fila de base de datos o una instancia de estructura de datos.

## <a name="question-and-answer-pairs"></a>Pares de preguntas y respuestas

La configuración **obligatoria** en un par de pregunta y respuesta (PyR) son:

* una **pregunta**: texto de una consulta de usuario que se usa para que el aprendizaje automático de QnA Maker lo alinee con el texto de las preguntas de usuarios formuladas de forma distinta, pero con la misma respuesta.
* la **respuesta**: respuesta del par que se devuelve cuando una consulta de usuario coincide con la pregunta asociada.

Cada par se representa mediante un **id.**

La configuración **opcional** de un par incluye:

* **Formas alternativas de la pregunta**: esto ayuda a que QnA Maker devuelva la respuesta correcta para una variedad más amplia de estructuras de preguntas.
* **Metadatos**: los metadatos son etiquetas asociadas a un par de QnA y se representan como pares de clave-valor. Las etiquetas de metadatos se usan para filtrar los pares de QnA y limitar el conjunto sobre el que se realiza la coincidencia de la consulta.
* **Avisos multiturno**, que se usan para continuar una conversación de varios turnos.

![Bases de conocimiento de QnA Maker](media/qnamaker-concepts-knowledgebase/knowledgebase.png)

## <a name="editorially-add-to-knowledge-base"></a>Adición en modo editorial a la base de conocimiento

Si no tiene contenido creado previamente para rellenar la base de conocimiento, puede redactar y agregar pares de QnA en el portal de QnA Maker. Consulte [aquí](How-To/edit-knowledge-base.md) cómo actualizar la base de conocimiento.

## <a name="editing-your-knowledge-base-locally"></a>Edición de la base de conocimiento localmente

Una vez que se crea una base de conocimiento, es recomendable realizar modificaciones en el texto de dicha base en el [portal de QnA Maker](https://qnamaker.ai), en lugar de exportar y volver a importar a través de archivos locales. Sin embargo, puede haber ocasiones en las que necesite editar una base de conocimiento localmente.

Exporte la base de conocimiento desde la página **Configuración** y luego edite dicha base con Microsoft Excel. Si decide utilizar otra aplicación para editar el archivo exportado, la aplicación puede introducir errores de sintaxis porque no es totalmente compatible con TSV. Por lo general, los archivos TSV de Microsoft Excel no introducen errores de formato.

Cuando termine de realizar las modificaciones, vuelva a importar el archivo TSV desde la página **Configuración**. Esto reemplazará completamente la base de conocimiento actual por la base de conocimiento importada.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ciclo de vida de una base de conocimiento de QnA Maker](Concepts/development-lifecycle-knowledge-base.md)
