---
title: 'Edición de una base de conocimiento: QnA Maker'
description: QnA Maker le permite administrar el contenido de la base de conocimiento, proporcionando una experiencia de edición sencilla.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 3cf5dcedd4d2cfa82fda002b71abca1e5bea4b18
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756727"
---
# <a name="edit-qna-pairs-in-your-knowledge-base"></a>Edición de pares de QnA en la base de conocimiento

QnA Maker le permite administrar el contenido de la base de conocimiento, proporcionando una experiencia de edición sencilla.

Los pares de QnA se agregan desde un origen de datos, como un archivo o una dirección URL, o lo hacen como un origen editorial. Un origen editorial indica que el par de QnA se agregó manualmente en el portal de QnA. Todos los pares de QnA están disponibles para su edición.

## <a name="add-an-editorial-qna-pair"></a>Adición de un par de QnA editorial
1. Inicie sesión en el [portal de QnA](https://www.qnamaker.ai/) y, luego, seleccione la base de conocimiento en la que quiere agregar el par de QnA.
1. En la página **EDIT** (EDITAR) de la base de conocimiento, seleccione **Add QnA pair** (Agregar par de QnA) para agregar un nuevo par de QnA.

1. En la fila del nuevo par de QnA, agregue los campos **Question** (Pregunta) y **Answer** (Respuesta) obligatorios. Todos los demás campos son opcionales. Todos los campos se pueden cambiar en cualquier momento.

1. Opcionalmente, agregue **frases alternativas**. Las frases alternativas son cualquier forma de la pregunta que sea bastante diferente de la pregunta original, pero deben proporcionar la misma respuesta.

    Cuando se publica la base de conocimiento y se habilita el [aprendizaje activo](use-active-learning.md), QnA Maker recopila opciones de frases alternativas para que las acepte. Estas opciones se seleccionan para aumentar la precisión de la predicción.

1. Opcionalmente, agregue **metadatos**. Para ver los metadatos, seleccione **View options** (Ver opciones) en el menú contextual. Los metadatos proporcionan filtros para las respuestas que facilita la aplicación cliente, como un bot de chat.

1. También tiene la opción de agregar **avisos de seguimiento**. Los avisos de seguimiento proporcionan rutas de conversación adicionales a las que presenta la aplicación cliente al usuario.

1. Seleccione **Save and train** (Guardar y entrenar) para ver las predicciones que incluye el nuevo par de QnA.

## <a name="edit-a-qna-pair"></a>Edición de un par de QnA

Se puede editar cualquier campo de cualquier par de QnA, sin importar el origen de datos original. Puede que algunos campos no estén visibles debido a la configuración actual de **View Options** (Ver opciones), que se encuentra en la barra de herramientas de contexto.

## <a name="delete-a-qna-pair"></a>Eliminación de un par de QnA

Para eliminar un QnA, haga clic en el icono **eliminar** en el extremo derecho de la fila QnA. Esta es una operación permanente. No se puede deshacer. Considere la posibilidad de exportar la base de conocimiento desde la página **Publish** (Publicar) antes de eliminar los conjuntos.

![Eliminación de un par de QnA](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="find-the-qna-pair-id"></a>Búsqueda del identificador del par de QnA

Si necesita encontrar el identificador del par de QnA, puede buscarlo en dos lugares:

* Mantenga el puntero sobre el icono de eliminación en la fila del par de QnA que le interese. El texto de navegación incluye el identificador del par de QnA.
* Exporte la base de conocimiento. Cada par de QnA de knowledge base incluye el identificador del par de QnA.

## <a name="add-alternate-questions"></a>Agregar preguntas alternativas

Agregue preguntas alternativas a un par de QnA existente para mejorar la probabilidad de encontrar una coincidencia para una consulta de usuario.

![Agregar preguntas alternativas](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="linking-qna-pairs"></a>Vinculación de pares de QnA

La vinculación de pares de QnA se proporciona con [avisos de seguimiento](multiturn-conversation.md). Esta es una conexión lógica entre los pares de QnA, administrada en el nivel de la base de conocimiento. Puede editar los avisos de seguimiento en el portal de QnA Maker.

No se pueden vincular pares de QnA en los metadatos de la respuesta.

## <a name="add-metadata"></a>Agregar metadatos

Para agregar conjuntos de metadatos, seleccione primero **View options** (Ver opciones) y, luego, elija **Show metadata** (Mostrar metadatos). Como resultado, se muestra la columna de metadatos. A continuación, seleccione el signo **+** para agregar un conjunto de metadatos. Este conjunto consta de una clave y un valor.

## <a name="save-changes-to-the-qna-pairs"></a>Guardar los cambios en los pares de QnA

Seleccione periódicamente **Save and train** (Guardar y entrenar) después de realizar modificaciones para evitar perder los cambios.

![Agregar metadatos](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Colaborar en una base de conocimiento](./collaborate-knowledge-base.md)

* [Administración de recursos de Azure que se usan en QnA Maker](set-up-qnamaker-service-azure.md)