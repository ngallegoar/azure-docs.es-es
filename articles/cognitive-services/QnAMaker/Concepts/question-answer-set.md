---
title: 'Diseño de una base de conocimiento: QnA Maker'
description: Una base de conocimiento de QnA Maker consta de una serie de conjuntos de preguntas y respuestas, así como metadatos opcionales asociados a cada par de pregunta y respuesta.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: cb71de0f6a3e372d8c1c1fede67ba0c0354532ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76844330"
---
# <a name="question-and-answer-set-concepts"></a>Conceptos del conjunto de preguntas y respuestas

Una base de conocimiento consta de conjuntos de preguntas y respuestas.  Cada conjunto contiene una respuesta, además de toda la información asociada a dicha _respuesta_. Una respuesta se parece un poco a una fila de base de datos o una instancia de estructura de datos.

## <a name="question-and-answer-sets"></a>Conjuntos de preguntas y respuestas

Los valores de configuración **obligatorios** en un conjunto de preguntas y respuestas son:

* una **pregunta**: texto de una consulta de usuario que se usa para que el aprendizaje automático de QnA Maker lo alinee con el texto de las preguntas de usuarios formuladas de forma distinta, pero con la misma respuesta.
* una **respuesta**: respuesta del conjunto que se devuelve cuando una consulta de usuario coincide con la pregunta asociada.

Cada conjunto se representa mediante un **id.**

La configuración **opcional** de un conjunto incluye:

* **Formas alternativas de la pregunta**: esto ayuda a que QnA Maker devuelva la respuesta correcta para una variedad más amplia de estructuras de preguntas.
* **Metadatos**: los metadatos son etiquetas asociadas a un par de QnA y se representan como pares de clave-valor. Las etiquetas de metadatos se usan para filtrar los pares de QnA y limitar el conjunto sobre el que se realiza la coincidencia de la consulta.
* **Avisos multiturno**, que se usan para continuar una conversación de varios turnos.

![Bases de conocimiento de QnA Maker](../media/qnamaker-concepts-knowledgebase/knowledgebase.png)

## <a name="editorially-add-to-knowledge-base"></a>Adición en modo editorial a la base de conocimiento

Si no tiene contenido creado previamente para rellenar la base de conocimiento, puede redactar y agregar conjuntos de preguntas y respuestas en el portal de QnA Maker. Consulte [aquí](../How-To/edit-knowledge-base.md) cómo actualizar la base de conocimiento.

## <a name="editing-your-knowledge-base-locally"></a>Edición de la base de conocimiento localmente

Una vez que se crea una base de conocimiento, es recomendable realizar modificaciones en el texto de dicha base en el [portal de QnA Maker](https://qnamaker.ai), en lugar de exportar y volver a importar a través de archivos locales. Sin embargo, puede haber ocasiones en las que necesite editar una base de conocimiento localmente.

Exporte la base de conocimiento desde la página **Configuración** y luego edite dicha base con Microsoft Excel. Si decide utilizar otra aplicación para editar el archivo exportado, la aplicación puede introducir errores de sintaxis porque no es totalmente compatible con TSV. Por lo general, los archivos TSV de Microsoft Excel no introducen errores de formato.

Cuando termine de realizar las modificaciones, vuelva a importar el archivo TSV desde la página **Configuración**. Esto reemplazará completamente la base de conocimiento actual por la base de conocimiento importada.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ciclo de vida de una base de conocimiento de QnA Maker](./development-lifecycle-knowledge-base.md)