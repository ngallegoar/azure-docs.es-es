---
title: 'Edición de una base de conocimiento: QnA Maker'
description: QnA Maker le permite administrar el contenido de la base de conocimiento, proporcionando una experiencia de edición sencilla.
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 223ad3a607adc20bbe608598da9742d56788b2c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131650"
---
# <a name="edit-qna-sets-in-your-knowledge-base"></a>Edición de conjuntos de QnA en la base de conocimiento

QnA Maker le permite administrar el contenido de la base de conocimiento, proporcionando una experiencia de edición sencilla.

Los conjuntos de QnA se agregan desde un origen de datos, como un archivo o una dirección URL, o lo hacen como un origen editorial. Un origen editorial indica que el conjunto de QnA se agregó manualmente en el portal de QnA. Todos los conjuntos de QnA están disponibles para su edición.

## <a name="add-an-editorial-qna-set"></a>Adición de un conjunto de QnA editorial
1. Inicie sesión en el [portal de QnA](https://www.qnamaker.ai/) y, luego, seleccione la base de conocimiento en la que quiere agregar el conjunto de QnA.
1. En la página **EDIT** (EDITAR) de la base de conocimiento, seleccione **Add QnA set** (Agregar conjunto de QnA) para agregar un nuevo conjunto de QnA.

1. En la fila del nuevo conjunto de QnA, agregue los campos **Question** (Pregunta) y **Answer** (Respuesta) obligatorios. Todos los demás campos son opcionales. Todos los campos se pueden cambiar en cualquier momento.

1. Opcionalmente, agregue **frases alternativas**. Las frases alternativas son cualquier forma de la pregunta que sea bastante diferente de la pregunta original, pero deben proporcionar la misma respuesta.

    Cuando se publica la base de conocimiento y se activa el aprendizaje activo, QnA Maker recopila opciones de frases alternativas para que las acepte. Estas opciones se seleccionan para aumentar la precisión de la predicción.

1. Opcionalmente, agregue **metadatos**. Para ver los metadatos, seleccione **View options** (Ver opciones) en el menú contextual. Los metadatos proporcionan filtros para las respuestas que facilita la aplicación cliente, como un bot de chat.

1. También tiene la opción de agregar **avisos de seguimiento**. Los avisos de seguimiento proporcionan rutas de conversación adicionales a las que presenta la aplicación cliente al usuario.

1. Seleccione **Save and train** (Guardar y entrenar) para ver las predicciones que incluye el nuevo conjunto de QnA.

## <a name="edit-a-qna-set"></a>Edición de un conjunto de QnA

Se puede editar cualquier campo de cualquier conjunto de QnA, sin importar el origen de datos original. Puede que algunos campos no estén visibles debido a la configuración actual de **View Options** (Ver opciones), que se encuentra en la barra de herramientas de contexto.

## <a name="delete-a-qna-set"></a>Eliminación de un conjunto de QnA

Para eliminar un QnA, haga clic en el icono **eliminar** en el extremo derecho de la fila QnA. Esta es una operación permanente. No se puede deshacer. Considere la posibilidad de exportar la base de conocimiento desde la página **Publish** (Publicar) antes de eliminar los conjuntos.

![Eliminación de un conjunto de QnA](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="find-the-qna-set-id"></a>Búsqueda del identificador del conjunto de QnA

Si necesita encontrar el identificador del conjunto de QnA, puede buscarlo en dos lugares:

* Mantenga el puntero sobre el icono de eliminación en la fila del conjunto de QnA que le interese. El texto de navegación incluye el identificador del conjunto de QnA.
* Exporte la base de conocimiento. Cada conjunto de QnA de la base de conocimiento incluye el identificador del conjunto de QnA.

## <a name="add-alternate-questions"></a>Agregar preguntas alternativas

Agregue preguntas alternativas a un conjunto de QnA existente para mejorar la probabilidad de encontrar una coincidencia con una consulta de usuario.

![Agregar preguntas alternativas](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="linking-qna-sets"></a>Vinculación de conjuntos de QnA

La vinculación de conjuntos de QnA se proporciona con [avisos de seguimiento](multiturn-conversation.md). Esta es una conexión lógica entre los conjuntos de QnA, administrada en el nivel de la base de conocimiento. Puede editar los avisos de seguimiento en el portal de QnA Maker.

No se pueden vincular conjuntos de QnA en los metadatos de la respuesta.

## <a name="add-metadata"></a>Agregar metadatos

Para agregar conjuntos de metadatos, seleccione primero **View options** (Ver opciones) y, luego, elija **Show metadata** (Mostrar metadatos). Como resultado, se muestra la columna de metadatos. A continuación, seleccione el signo **+** para agregar un conjunto de metadatos. Este conjunto consta de una clave y un valor.

## <a name="save-changes-to-the-qna-sets"></a>Guardar los cambios en los conjuntos de QnA

Seleccione periódicamente **Save and train** (Guardar y entrenar) después de realizar modificaciones para evitar perder los cambios.

![Agregar metadatos](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Colaborar en una base de conocimiento](./collaborate-knowledge-base.md)

* [Administración de recursos de Azure que se usan en QnA Maker](set-up-qnamaker-service-azure.md)