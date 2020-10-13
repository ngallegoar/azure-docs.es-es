---
title: Etiquetado de una entidad en una expresión de ejemplo
description: Aprenda a etiquetar una entidad de aprendizaje automático con subentidades en una expresión de ejemplo en una página de detalles de la intención del portal de LUIS.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/20/2020
ms.openlocfilehash: ffbaa2e40d5924ba61e548398e63295cf7dba2b0
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91303733"
---
# <a name="label-machine-learning-entity-in-an-example-utterance"></a>Etiquetado de una entidad de aprendizaje automático en una expresión de ejemplo

Etiquetar una entidad en una expresión de ejemplo da a LUIS un ejemplo de qué es la entidad y dónde puede aparecer la entidad en la expresión.

Puede etiquetar entidades y subentidades de aprendizaje automático.

Dado que no se pueden etiquetar expresiones regulares, listas o entidades generadas previamente, cree una entidad o subentidad y, a continuación, agregue estas entidades como características, si procede, a la entidad o subentidad.

## <a name="label-example-utterances-from-the-intent-detail-page"></a>Etiquetado de expresiones de ejemplo desde la página de detalles de la intención

Para etiquetar ejemplos de entidades dentro de la expresión, seleccione la intención de esta.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. Seleccione la intención que contiene las expresiones de ejemplo que desea etiquetar para la extracción con una entidad.
1. Seleccione el texto que desea etiquetar y, a continuación, seleccione la entidad.

## <a name="two-techniques-to-label-entities"></a>Dos técnicas para etiquetar entidades

En la página de detalles de la intención se admiten dos técnicas de etiquetado.
* Seleccione entidad o subentidad en [Paleta de entidades](#label-with-the-entity-palette-visible) y, después, en el ejemplo seleccione el texto de la expresión. Esta es la técnica recomendada porque puede comprobar visualmente que está trabajando con la entidad o subentidad correcta, según el esquema.
* Seleccione en primer lugar el texto de la expresión de ejemplo. Al hacerlo, se muestra un menú emergente de [opciones de etiquetado](#how-to-label-entity-from-in-place-menu).

## <a name="label-with-the-entity-palette-visible"></a>Etiquetado con la paleta de entidades visible

Una vez [planeado el esquema con entidades](luis-how-plan-your-app.md), mantenga la **paleta de entidades** visible durante el etiquetado. La **paleta de entidades** es un recordatorio de las entidades que planea extraer.

Para acceder a la **paleta de entidades**, seleccione el símbolo **@** en la barra de herramientas contextual que se encuentra encima de la lista de expresiones de ejemplo.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de la paleta de entidades en la página de detalles de la intención.](media/label-utterances/entity-palette-from-tool-bar.png)

## <a name="how-to-label-entity-from-entity-palette"></a>Etiquetado de una entidad desde la paleta de entidades

La paleta de entidades ofrece una alternativa a la experiencia de etiquetado anterior. Permite cambiar el texto para etiquetarlo al instante con una entidad.

1. Para abrir la paleta de entidades, seleccione el símbolo **@** situado en la parte superior derecha de la tabla de expresiones.

2. Seleccione la entidad de la paleta que desea etiquetar. Esta acción se indica visualmente con un nuevo cursor. El cursor sigue al mouse mientras se mueve por el portal de LUIS.

3. En la expresión de ejemplo, _pinte_ la entidad con el cursor.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla que muestra la entidad pintada con el cursor.](media/label-utterances/example-1-label-machine-learned-entity-palette-label-action.png)

## <a name="adding-entity-as-a-feature-from-the-entity-palette"></a>Incorporación de entidades como una característica de la paleta de entidades

La sección inferior de la paleta de entidades permite agregar características a la entidad seleccionada actualmente. Puede seleccionar entre todas las entidades y las listas de frases existentes o crear una nueva lista de frases.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de la paleta de entidades con la entidad como una característica](media/label-utterances/entity-palette-entity-as-a-feature.png)

## <a name="labeling-entity-roles"></a>Etiquetado de roles de entidad

Los roles de entidad se etiquetan mediante la **paleta de entidades**.

1. En la página Intent detail (Detalle de intención), seleccione la **paleta de entidades** de la barra de herramientas contextual.
1. Una vez que se abra la paleta de entidades, seleccione la entidad en la lista de entidades.
1. Debajo de la lista de entidades, seleccione un rol existente.
1. En el texto de la expresión de ejemplo, etiquete el texto con el rol de entidad.

## <a name="how-to-label-entity-from-in-place-menu"></a>Etiquetado de una entidad desde un menú contextual

El etiquetado contextual permite seleccionar rápidamente el texto de la expresión y etiquetarlo. También puede crear una entidad de aprendizaje automático o de lista a partir del texto etiquetado.

Observe la expresión de ejemplo, `hi, please I want a cheese pizza in 20 minutes`.

Seleccione el texto situado más a la izquierda y, a continuación, seleccione el texto situado más a la derecha de la entidad y, después, en el menú contextual, elija la entidad con la que desea etiquetar.

> [!div class="mx-imgBorder"]
> ![Etiqueta completa de la entidad de aprendizaje automático](media/label-utterances/label-steps-in-place-menu.png)

## <a name="review-labeled-text"></a>Revisión del texto etiquetado

Después del etiquetado, revise la expresión de ejemplo y asegúrese de que el intervalo de texto seleccionado se ha subrayado con la entidad elegida. La línea sólida indica que el texto se ha etiquetado.

> [!div class="mx-imgBorder"]
> ![Entidad de aprendizaje automático etiquetada por completo](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="confirm-predicted-entity"></a>Confirmación de entidad predicha

Si hay un cuadro con líneas de puntos alrededor del intervalo de texto, indica que el texto está predicho y, sin embargo, _todavía no se ha etiquetado_. Para convertir la predicción en una etiqueta, seleccione la fila de la expresión y, a continuación, seleccione **Confirm entities** (Confirmar entidades) en la barra de herramientas contextual.

## <a name="relabeling-over-existing-entities"></a>Volver a etiquetar las entidades existentes

Si vuelve a etiquetar un texto que ya está etiquetado, LUIS puede dividir o combinar las etiquetas existentes.

## <a name="labeling-for-punctuation"></a>Etiquetado para la puntuación

No es necesario etiquetar para la puntuación. Utilice la [configuración de la aplicación](luis-reference-application-settings.md) para controlar cómo afecta la puntuación a las predicciones de expresiones.

## <a name="unlabel-entities"></a>Anulación de etiquetas de entidades

> [!NOTE]
> Solo se pueden quitar las etiquetas de las entidades con aprendizaje automático. No se puede etiquetar ni quitar las etiquetas de entidades de expresiones regulares, entidades de lista o entidades generadas previamente.

Para quitar la etiqueta de una entidad, seleccione la entidad y, después, seleccione **Eliminar la etiqueta** del menú contextual.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla que muestra la eliminación de la etiqueta de una entidad](media/label-utterances/unlabel-entity-using-in-place-menu.png)

## <a name="automatic-labeling-for-parent-and-child-entities"></a>Etiquetado automático para entidades primarias y secundarias

Si va a etiquetar una entidad primaria, se etiquetará cualquier subentidad que se pueda predecir en función de la versión entrenada actualmente.

Si va a etiquetar una subentidad, la entidad primaria se etiquetará automáticamente.

## <a name="automatic-labeling-for-non-machine-learned-entities"></a>Etiquetado automático de entidades sin aprendizaje automático

Entre las entidades sin aprendizaje automático se incluyen las entidades predefinidas, las entidades de expresiones regulares, las entidades de lista y las entidades pattern.any. Estas se etiquetan automáticamente por LUIS, por lo que no es necesario que los usuarios las etiqueten manualmente.

## <a name="intent-prediction-errors"></a>Errores de predicción de intenciones

Un error de predicción de la intención indica que la expresión de ejemplo, dada la aplicación entrenada actual, no sería predecible para la intención.

Aprenda a [ver estos errores](luis-how-to-add-intents.md#intent-prediction-errors) en la página de detalles de la intención.

## <a name="entity-prediction-errors"></a>Errores de predicción de entidades

Los errores de predicción de entidades indican que la entidad predicha no coincide con la entidad etiquetada. Esto se visualiza con un indicador de precaución junto a la expresión.

> [!div class="mx-imgBorder"]
> ![Paleta de entidades para la entidad de aprendizaje automático](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Pasos siguientes

Use el [panel](luis-how-to-use-dashboard.md) y [revise las expresiones de punto de conexión](luis-how-to-review-endpoint-utterances.md) para mejorar la calidad de la predicción de la aplicación.
