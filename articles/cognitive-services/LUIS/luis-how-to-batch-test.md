---
title: 'Realización de una prueba por lotes: LUIS'
titleSuffix: Azure Cognitive Services
description: Use los conjuntos de pruebas por lotes de Language Understanding (LUIS) para buscar expresiones con intenciones y entidades incorrectas.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: 4d3535d419b0f99491795b3920fdd295e3128299
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654000"
---
# <a name="batch-testing-with-a-set-of-example-utterances"></a>Pruebas por lotes con un conjunto de expresiones de ejemplo

 Las pruebas por lotes son una prueba completa en el modelo entrenado actual para medir su rendimiento en LUIS. Los conjuntos de datos que se usan para realizar pruebas de lote no deben incluir expresiones de ejemplo en las intenciones ni en las grabaciones recibidas desde un punto de conexión de tiempo de ejecución de predicción.

<a name="batch-testing"></a>

## <a name="import-a-dataset-file-for-batch-testing"></a>Importar un archivo de conjunto de datos para las pruebas por lotes

1. Seleccione **Prueba** en la barra superior y, después, seleccione el **panel Pruebas por lotes**.

    ![Vínculo de Pruebas por lotes](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Seleccione **Importar conjunto de datos**. Aparece el cuadro de diálogo **Import new dataset** (Importar nuevo conjunto de datos). Seleccione **Choose File** (Elegir archivo) y ubique un archivo JSON con el [formato JSON](luis-concept-batch-test.md#batch-file-format) correcto que contiene *no más de 1000* expresiones para probar.

    Los errores de importación se notifican en una barra de notificación de color rojo en la parte superior del explorador. Cuando una importación tiene errores, no se crea ningún conjunto de datos. Para obtener más información, vea [Errores comunes](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. En el campo **Nombre del conjunto de datos**, escriba un nombre para el archivo del conjunto de datos. El archivo del conjunto de datos incluye una **matriz de expresiones** que incluye la *intención etiquetada* y las *entidades*. Revise el [archivo por lotes de ejemplo](luis-concept-batch-test.md#batch-file-format) para consultar la sintaxis.

4. Seleccione **Listo**. Se agrega el archivo del conjunto de datos.

## <a name="run-rename-export-or-delete-dataset"></a>Ejecutar, cambiar el nombre, exportar o eliminar el conjunto de datos

Para ejecutar, cambiar el nombre, exportar o eliminar el conjunto de datos, use el botón de puntos suspensivos (***...***) al final de la fila del conjunto de datos.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de la lista de pruebas por lotes con opciones](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Ejecutar una prueba por lotes en la aplicación entrenada

Para ejecutar la prueba, seleccione el nombre del conjunto de nombres y, a continuación, **Ejecutar** en la barra de herramientas contextual. Cuando se complete la prueba, esta fila muestra el resultado de la prueba del conjunto de datos.

El conjunto de datos que se puede descargar es el mismo archivo que se cargó para realizar la prueba por lotes.

|State|Significado|
|--|--|
|![Icono de círculo verde de prueba correcta](./media/luis-how-to-batch-test/batch-test-result-green.png)|Todas las expresiones son correctas.|
|![Icono de X de color rojo de prueba incorrecta](./media/luis-how-to-batch-test/batch-test-result-red.png)|Al menos una intención de expresión no coincidió con la predicción.|
|![Icono de preparado para la prueba](./media/luis-how-to-batch-test/batch-test-result-blue.png)|La prueba está lista para ejecutarse.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>

## <a name="view-batch-test-results"></a>Ver los resultados de la prueba por lotes

Para revisar los resultados de la prueba por lotes, seleccione **Ver resultados**.

<a name="filter-chart-results-by-intent-or-entity"></a>

## <a name="filter-chart-results"></a>Filtrado de los resultados del gráfico

Para filtrar el gráfico por una intención o entidad específica, seleccione la intención o entidad en el panel de filtrado del lado derecho. Los puntos de datos y su distribución se actualizan en el gráfico según su selección.

![Resultado de la prueba por lotes visualizado](./media/luis-how-to-batch-test/filter-by-entity.png)

## <a name="view-single-point-utterance-data"></a>Ver los datos de expresión de un único punto

En el gráfico, mantenga el puntero sobre un punto de datos para ver la puntuación de exactitud de la predicción. Seleccione un punto de datos para recuperar su expresión correspondiente en la lista de expresiones en la parte inferior de la página.

![Expresión seleccionada](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>

## <a name="view-section-data"></a>Ver los datos de la sección

En el gráfico de cuatro secciones, seleccione el nombre de sección, como **Falso positivo** en la parte superior derecha del gráfico. Debajo del gráfico se muestran todas las expresiones de esa sección en una lista.

![Expresiones seleccionadas por la sección](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

En la imagen anterior, la expresión `switch on` está etiquetada con la intención TurnAllOn, pero recibió la predicción de intención None. Se trata de un valor que indica que la intención TurnAllOn necesita más expresiones de ejemplo para realizar la predicción esperada.

Las dos secciones del gráfico en rojo indican las expresiones que no coincidieron con la predicción esperada. Estas indican expresiones para las que LUIS necesita más entrenamiento.

Las dos secciones del gráfico en color verde coincidieron con la predicción esperada.

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Pasos siguientes

Si las pruebas indican que la aplicación de LUIS no reconoce las entidades y las intenciones correctas, puede trabajar para mejorar el rendimiento de la aplicación de LUIS etiquetando más expresiones o agregando características.

* [Label suggested utterances with LUIS](luis-how-to-review-endpoint-utterances.md) (Etiquetado de expresiones sugeridas con LUIS)
* [Use features to improve your LUIS app's performance](luis-how-to-add-features.md) (Uso de características para mejorar el rendimiento de la aplicación de LUIS)
* [Comprenda las pruebas por lotes con este tutorial](luis-tutorial-batch-testing.md)
* [Obtenga información sobre los conceptos de las pruebas por lotes](luis-concept-batch-test.md).
