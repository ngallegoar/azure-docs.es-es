---
title: Prueba de la aplicación en el portal de LUIS
description: Use Language Understanding (LUIS) para trabajar continuamente en la aplicación para refinarla y mejorar la comprensión del lenguaje.
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 574bacdb5e1f167c9c9174d4a119552391059004
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/10/2020
ms.locfileid: "84677750"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>Prueba de la aplicación de LUIS en el portal de LUIS

La [prueba](luis-concept-test.md) de una aplicación es un proceso iterativo. Tras entrenar la aplicación de LUIS, pruébela con expresiones de ejemplo para ver si las entidades e intenciones se reconocen correctamente. En caso contrario, realice actualizaciones en la aplicación de LUIS, entrénela y pruébela de nuevo.

<!-- anchors for H2 name changes -->
<a name="train-your-app"></a>
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="train-before-testing"></a>Entrenamiento antes de las pruebas

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. Para probar con la versión más reciente de la aplicación activa, seleccione **Entrenar** en el menú superior, antes de realizar las pruebas.

## <a name="test-an-utterance"></a>Entrenar una expresión

La expresión de prueba no debe ser exactamente igual que cualquier expresión de ejemplo de la aplicación. La expresión de prueba debe incluir la elección de la palabra, la longitud de la frase y el uso de la entidad que se espera para un usuario.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.

1. Para acceder al panel deslizante **Prueba**, seleccione **Prueba** en el panel superior de la aplicación.

    > [!div class="mx-imgBorder"]
    > ![Página de entrenamiento y prueba de la aplicación](./media/luis-how-to-interactive-test/test.png)

1. Escriba una expresión en el cuadro de texto y presione Entrar. Puede escribir tantas expresiones de prueba como quiera en **Prueba**, pero solo una expresión cada vez.

1. La expresión, su intención superior y la puntuación se agregan a la lista de expresiones bajo el cuadro de texto.

    > [!div class="mx-imgBorder"]
    > ![Las pruebas interactivas identifican la intención incorrecta](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-the-prediction"></a>Inspección de la predicción

Puede inspeccionar los detalles del resultado de las pruebas en el panel **Inspeccionar**.

1. Con el panel deslizante **Prueba** abierto, haga clic en **Inspeccionar** para una expresión que quiera comparar.

    > [!div class="mx-imgBorder"]
    > ![Seleccione el botón Inspeccionar para ver más detalles acerca de los resultados de la prueba](./media/luis-how-to-interactive-test/inspect.png).

1. Aparece el panel **Inspección**. El panel incluye la intención de mayor puntuación, así como cualquier entidad identificada. El panel muestra la predicción de la expresión seleccionada.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla parcial del panel Inspeccionar prueba](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="add-to-example-utterances"></a>Incorporación a expresiones de ejemplo

En el panel de inspección, puede agregar la expresión de prueba a una intención; para ello, seleccione **Incorporación a expresiones de ejemplo**.

## <a name="disable-required-features"></a>Deshabilitar las características necesarias

Esta opción le ayuda a determinar si la aplicación entrenada está prediciendo correctamente las entidades en función de las características necesarias. La configuración predeterminada es aplicar la característica según sea necesario durante la predicción. Seleccione este botón de alternancia para ver cuál sería la predicción si la característica de la subentidad no fuera necesaria.

### <a name="when-to-disable-required-features"></a>Cuándo deshabilitar las características necesarias

La aplicación entrenada puede predecir de forma inadecuada una entidad de aprendizaje automático debido a alguno de los siguientes aspectos:
* El etiquetado de las expresiones de ejemplo es incorrecto.
* La característica necesaria no coincide con el texto.

Un ejemplo es una entidad de aprendizaje automático con una subentidad del nombre de una persona.

:::image type="content" source="media/luis-how-to-interactive-test/disable-required-feature.png" alt-text="Captura de pantalla del esquema de entidades de aprendizaje automático del portal de LUIS con la característica necesaria":::

Una expresión de ejemplo para esta entidad de aprendizaje automático es `Assign Bob Jones to work on the new security feature`.

La extracción debe ser `security feature` como descripción del vale y `Bob Jones` como ingeniero (dos subentidades de la entidad `Assign ticket`).

Para que la subentidad pueda realizar una predicción correcta, agregue la entidad predefinida [PersonName](luis-reference-prebuilt-person.md) a la subentidad `engineer` como característica. Si hace que la característica sea necesaria, la subentidad solo se extraerá si se predice la entidad predefinida PersonName para el texto. Esto significa que no se devolverá como subentidad etiquetada (`engineer`) ningún nombre del texto que no realice predicciones con la subentidad PersonName.

Cuando use el panel de prueba interactivo y vea que una subentidad, con una característica necesaria, no realiza predicciones, active o desactive esta opción para ver si la subentidad se predice sin que se requiera la característica. Es posible que la subentidad pueda predecirse correctamente sin que se requiera la característica gracias al etiquetado correcto de las expresiones de ejemplo.

## <a name="view-sentiment-results"></a>Ver los resultados de sentimiento

Si el **análisis de sentimiento** está configurado en la página **[Publicar](luis-how-to-publish-app.md#enable-sentiment-analysis)** , los resultados de la prueba incluyen el sentimiento que se encontró en la expresión.

## <a name="correct-matched-patterns-intent"></a>Corregir la intención del patrón coincidente

Si usa [Patrones](luis-concept-patterns.md) y la expresión coincide con un patrón, pero se predijo una intención incorrecta, seleccione el vínculo **Editar** junto al patrón y haga clic en la intención correcta.

## <a name="compare-with-published-version"></a>Comparar con la versión publicada

Puede probar la versión activa de la aplicación con la versión del [punto de conexión](luis-glossary.md#endpoint) publicado. En el panel **Inspeccionar**, seleccione **Comparar con el publicado**. Todas las pruebas en el modelo publicado se deducirán del saldo de la cuota de suscripción de Azure.

> [!div class="mx-imgBorder"]
> ![Comparar con el publicado](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Ver el punto de conexión de JSON en el panel Prueba
Puede ver el punto de conexión de JSON devuelto para la comparación si selecciona la **vista Mostrar JSON**.

> [!div class="mx-imgBorder"]
> ![Respuesta JSON publicada](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

## <a name="additional-settings-in-test-panel"></a>Configuración adicional en el panel de pruebas

### <a name="luis-endpoint"></a>Punto de conexión de LUIS

Si tiene varios puntos de conexión de LUIS, use el vínculo **Configuración adicional** del panel Publicado de Prueba para cambiar el punto de conexión usado para realizar pruebas. Si no está seguro de qué punto de conexión usar, seleccione el valor predeterminado **Starter_Key**.

> [!div class="mx-imgBorder"]
> ![Panel de prueba con el vínculo Configuración adicional resaltado](media/luis-how-to-interactive-test/additional-settings-v3-settings.png)


## <a name="batch-testing"></a>Pruebas por lotes
Vea los [conceptos](luis-concept-batch-test.md) de las pruebas por lotes y obtenga información sobre [cómo](luis-how-to-batch-test.md) probar un lote de expresiones.

## <a name="next-steps"></a>Pasos siguientes

Si las pruebas indican que la aplicación de LUIS no reconoce las entidades y las intenciones correctas, puede trabajar para mejorar la precisión de la aplicación de LUIS etiquetando más expresiones o agregando características.

* [Label suggested utterances with LUIS](luis-how-to-review-endpoint-utterances.md) (Etiquetado de expresiones sugeridas con LUIS)
* [Use features to improve your LUIS app's performance](luis-how-to-add-features.md) (Uso de características para mejorar el rendimiento de la aplicación de LUIS)
