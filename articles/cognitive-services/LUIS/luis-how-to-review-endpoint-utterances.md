---
title: 'Revisión de expresiones del usuario: LUIS'
titleSuffix: Azure Cognitive Services
description: Revise expresiones capturadas por el aprendizaje activo para seleccionar la intención y marcar las entidades para las expresiones del mundo real; aceptar cambios, entrenar y publicar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: diberry
ms.openlocfilehash: 95b7c7446a47fafd26d00b0da4d880786340fcd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218754"
---
# <a name="how-to-improve-the-luis-app-by-reviewing-endpoint-utterances"></a>Cómo mejorar la aplicación LUIS revisando las expresiones de punto de conexión

El proceso de revisión de las expresiones de punto de conexión para obtener predicciones correctas se denomina [Aprendizaje activo](luis-concept-review-endpoint-utterances.md). El aprendizaje activo captura las consultas de punto de conexión y selecciona las expresiones de punto de conexión del usuario de las que no está seguro. Revise estas expresiones para seleccionar la intención y marque las entidades para estas expresiones de lectura. Acepte estos cambios en sus expresiones de ejemplo, entrene y publique. LUIS identificará después las expresiones con mayor precisión.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="enable-active-learning"></a>Habilitación del aprendizaje activo

Para habilitar el aprendizaje activo, debe registrar las consultas de usuario. Esto se consigue llamando a la [consulta de punto de conexión](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint) con el parámetro y el valor de la cadena de consulta `log=true`.

Use el portal de LUIS para construir la consulta de punto de conexión correcta.

1. En el [portal de LUIS en versión preliminar](https://preview.luis.ai/), seleccione la aplicación en la lista de aplicaciones.
1. Vaya a la sección **Administrar** y seleccione **Recursos de Azure**.
1. En el recurso de predicción asignado, seleccione la opción de **cambiar parámetros de consulta**.

    > [!div class="mx-imgBorder"]
    > ![Use el portal de LUIS para guardar los registros, que es necesario para el aprendizaje activo.](./media/luis-tutorial-review-endpoint-utterances/azure-portal-change-query-url-settings.png)

1. Active la opción **Guardar registros** y, a continuación, guárdelos con **Listo**.

    > [!div class="mx-imgBorder"]
    > ![Use el portal de LUIS para guardar los registros, que es necesario para el aprendizaje activo.](./media/luis-tutorial-review-endpoint-utterances/luis-portal-manage-azure-resource-save-logs.png)

     Esta acción cambia la dirección URL, ya que agrega el parámetro QueryString `log=true`. Copie y utilice la dirección URL de consulta de ejemplo modificada al realizar consultas de predicción en el punto de conexión del entorno de ejecución.

## <a name="correct-intent-predictions-to-align-utterances"></a>Corregir las predicciones de intención para alinear expresiones

Cada expresión tiene una intención sugerida que se muestra en la columna **Aligned intent** (Intención alineada).

> [!div class="mx-imgBorder"]
> [![Revise las expresiones de punto de conexión de las que LUIS no está seguro](./media/label-suggested-utterances/review-endpoint-utterances.png)](./media/label-suggested-utterances/review-endpoint-utterances.png#lightbox)

Si está de acuerdo con esa intención, seleccione la marca de verificación. Si no está de acuerdo con la sugerencia, seleccione la intención correcta en la lista desplegable de intenciones alineadas y después haga clic en la marca de verificación situada la derecha de la intención alineada. Después de seleccionar la marca de verificación, la expresión se mueve a la intención y se quita de la lista **Revisar expresiones de extremo**.

> [!TIP]
> Es importante ir a la página de detalles de intención para revisar y corregir las predicciones de entidad de todas las expresiones de ejemplo de la lista **Revisar expresiones de extremo**.

## <a name="delete-utterance"></a>Eliminar la expresión

Cada expresión se puede eliminar de la lista de revisión. Una vez eliminada, no volverá a aparecer en la lista. Esto sucede incluso si el usuario escribe la misma expresión desde el punto de conexión.

Si no está seguro de si la expresión se debe eliminar, muévala a la intención None, o bien cree una intención como `miscellaneous` y mueva la expresión a esa intención.

## <a name="disable-active-learning"></a>Deshabilitación del aprendizaje activo

Para deshabilitar el aprendizaje activo, no registre las consultas de usuario. Esto se logra mediante el establecimiento de la [consulta de punto de conexión](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint) con el parámetro y el valor de la cadena de consulta `log=false`, o la no utilización del valor de la cadena de consulta porque el valor predeterminado es false.

## <a name="next-steps"></a>Pasos siguientes

Para probar cómo mejora el rendimiento después de etiquetar las expresiones sugeridas, puede acceder a la consola de prueba si hace clic en **Test** (Prueba) en el panel superior. Para obtener instrucciones sobre cómo probar la aplicación mediante la consola de prueba, vea [Train and test your app](luis-interactive-test.md) (Entrenar y probar la aplicación).
