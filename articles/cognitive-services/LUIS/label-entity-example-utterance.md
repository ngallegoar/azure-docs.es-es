---
title: Etiquetado de una entidad en una expresión de ejemplo
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo etiquetar una entidad de aprendizaje automático con subcomponentes en una expresión de ejemplo en una página de detalles de la intención del portal de LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: diberry
ms.openlocfilehash: 5499e8362e1107a18b997a6a485e11343c328d0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898373"
---
# <a name="label-machine-learned-entity-in-an-example-utterance"></a>Etiquetado de una entidad de aprendizaje automático en una expresión de ejemplo

Etiquetar una entidad en una expresión de ejemplo da a LUIS un ejemplo de qué es la entidad y dónde puede aparecer la entidad en la expresión.

## <a name="labeling-machine-learned-entity"></a>Etiquetado de una entidad de aprendizaje automático

Analice en la frase `hi, please I want a cheese pizza in 20 minutes`.

1. Seleccione el texto situado más a la izquierda y, a continuación, seleccione el texto situado más a la derecha de la entidad y, después, elija la entidad con la que desea etiquetar, en este caso, Orden completo. El _pedido completo_ aparece etiquetado en la siguiente imagen.

    > [!div class="mx-imgBorder"]
    > ![Etiqueta completa de la entidad de aprendizaje automático](media/label-utterances/example-1-label-machine-learned-entity-complete-order.png)

1. Seleccione la entidad en la ventana emergente. La entidad completa etiquetada de pedido de pizza incluye todas las palabras (de izquierda a derecha en inglés) que están etiquetadas.

## <a name="review-labeled-text"></a>Revisión del texto etiquetado

Después del etiquetado, revise la expresión de ejemplo y asegúrese de que el intervalo de texto seleccionado se ha subrayado con la entidad elegida. La línea sólida indica que el texto se ha etiquetado.

> [!div class="mx-imgBorder"]
> ![Entidad de aprendizaje automático etiquetada por completo](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="confirm-predicted-entity"></a>Confirmación de entidad predicha

Si hay un cuadro con líneas de puntos alrededor del intervalo de texto y el nombre de la entidad está por encima de la expresión, indica que el texto está predicho y, sin embargo, _todavía no se ha etiquetado_. Para convertir la predicción en una etiqueta, seleccione la fila de la expresión y, a continuación, seleccione **Confirm entity predictions** (Confirmar predicciones de entidad).

> [!div class="mx-imgBorder"]
> ![Predicción completa de la entidad de aprendizaje automático](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted.png)

Como alternativa, puede seleccionar el nombre de la entidad que se encuentra sobre el texto y seleccionar **Confirm Prediction** (Confirmar predicción) en el menú que aparece.

> [!div class="mx-imgBorder"]
> ![Predicción completa de la entidad de aprendizaje automático con menú](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted-menu.png)

## <a name="label-entity-by-painting-with-entity-palette-cursor"></a>Entidad de etiqueta pintando con el cursor de la paleta de entidades

La paleta de entidades ofrece una alternativa a la experiencia de etiquetado anterior. Permite cambiar el texto para etiquetarlo al instante con una entidad.

1. Abra la paleta de entidades seleccionando el icono de marcador de resaltado situado en la parte superior derecha de la tabla de expresiones.

    > [!div class="mx-imgBorder"]
    > ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/example-1-label-machine-learned-entity-palette.png)

2. Seleccione el componente de la entidad. Esta acción se indica visualmente con un nuevo cursor. El cursor sigue al mouse mientras se mueve por el portal.

    > [!div class="mx-imgBorder"]
    > ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/example-1-label-machine-learned-entity-palette-menu.png)

3. En la expresión de ejemplo, _pinte_ la entidad con el cursor.

    > [!div class="mx-imgBorder"]
    > ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/example-1-label-machine-learned-entity-palette-label-action.png)

## <a name="labeling-subcomponents-of-a-machine-learned-entity"></a>Etiquetado de subcomponentes de una entidad de aprendizaje automático

Los subcomponentes de las entidades se etiquetan exactamente de la misma manera que las entidades de nivel superior. Al seleccionar texto, las entidades disponibles en la ventana emergente son relativas al contexto en el que aparece el texto. Por ejemplo, si tiene una entidad de aprendizaje automático de cinco niveles y selecciona texto que se ha etiquetado con el primero y segundo nivel (indicado por un nombre de entidad etiquetada en la expresión de ejemplo), las entidades disponibles en la ventana emergente se limitan al contexto de los subcomponentes del tercer nivel. Para etiquetar el texto con otras entidades, seleccione la opción **Label as another entity** (Etiquetar como otra entidad).

> [!div class="mx-imgBorder"]
> ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/example-1-label-machine-learned-entity-subcomponent.png)

Los subcomponentes solo se pueden etiquetar si el elemento primario también está etiquetado.

## <a name="labeling-entity-roles"></a>Etiquetado de roles de entidad

Los roles de entidad se etiquetan mediante la paleta de entidades.

1. En la página Intent detail (Detalle de intención), seleccione la **paleta de entidades** de la barra de herramientas contextual.
1. Una vez que se abra la paleta de entidades, seleccione la entidad en la lista de entidades.
1. Vaya al **inspector de entidades** y seleccione un rol existente o cree uno nuevo.
1. En el texto de la expresión de ejemplo, etiquete el texto con el rol de entidad.

## <a name="unlabel-entities"></a>Anulación de etiquetas de entidades

Para anular la etiqueta de una entidad, seleccione el nombre de la entidad debajo del texto y seleccione **Anular etiqueta**. Si la entidad a la que está intentando anular la etiqueta tiene subcomponentes etiquetados, los subcomponentes no deben etiquetarse en primer lugar.

## <a name="editing-labels-using-the-entity-palette"></a>Edición de las etiquetas mediante la paleta de entidades

Si comete un error al etiquetar, la paleta de entidades es una herramienta sencilla que permite modificaciones rápidas. Por ejemplo, si una etiqueta de entidad abarca una palabra adicional por error y ya tiene subcomponentes etiquetados, puede usar la paleta de entidades para cambiar el intervalo de palabras más corto necesario.

Por ejemplo:

1. El subcomponente de tipo de pizza abarca "pizza de queso con", que incluye la palabra incorrecta adicional "con"

    > [!div class="mx-imgBorder"]
    > ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/edit-label-with-palette-1.png)

2. Use la paleta de entidades para elegir el tipo de pizza y cambie a "pizza de queso". El resultado es que solo la pizza de queso tiene ahora la etiqueta Tipo de pizza.

    > [!div class="mx-imgBorder"]
    > ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/edit-label-with-palette-2.png)

## <a name="labels-for-matching-text-entities"></a>Etiquetas para entidades de texto coincidentes

Entre las entidades con texto coincidente se incluyen las entidades predefinidas, las entidades de expresiones regulares, las entidades de lista y las entidades pattern.any. Estas se etiquetan automáticamente por LUIS, por lo que no es necesario que los usuarios las etiqueten manualmente.

## <a name="entity-prediction-errors"></a>Errores de predicción de entidades

Los errores de predicción de entidades indican que la entidad predicha no coincide con la entidad etiquetada. Esto se visualiza con un indicador de precaución junto a la expresión.

> [!div class="mx-imgBorder"]
> ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Pasos siguientes

Use el [panel](luis-how-to-use-dashboard.md) y [revise las expresiones de punto de conexión](luis-how-to-review-endpoint-utterances.md) para mejorar la calidad de la predicción de la aplicación.