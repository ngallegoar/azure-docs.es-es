---
title: 'Tutorial: Pruebas en lote para encontrar problemas: LUIS'
description: En este tutorial se muestra cómo usar las pruebas en lote para asegurarse de la calidad de la aplicación Language Understanding (LUIS).
ms.topic: tutorial
ms.date: 05/07/2020
ms.openlocfilehash: c823cbd854c28ab356dd9968e6f8f1a12b6421be
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592600"
---
# <a name="tutorial-batch-test-data-sets"></a>Tutorial: Pruebas en lote de conjuntos de datos

En este tutorial se muestra cómo usar las pruebas en lote para asegurar la calidad de la aplicación Language Understanding (LUIS).

Las pruebas por lotes permiten validar el estado del modelo entrenado activo con un conjunto conocido de entidades y expresiones etiquetadas. En el archivo por lotes con formato JSON, agregue las expresiones y establezca las etiquetas de entidad que necesita predecir dentro de la expresión.

Requisitos de las pruebas por lotes:

* Máximo de 1000 expresiones por prueba.
* Sin duplicados.
* Tipos de entidad permitidos: solo entidades de aprendizaje automático.

Cuando use una aplicación que no sea este tutorial, asegúrese de *no* usar las expresiones de ejemplo que ya están agregadas a su aplicación.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Crear un archivo de prueba por lotes
> * Ejecutar una prueba por lotes
> * Revisar los resultados de la prueba

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importar la aplicación de ejemplo

Importe una aplicación que tome un pedido de pizza como `1 pepperoni pizza on thin crust`.

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json?raw=true).

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Importe el archivo JSON en una aplicación nueva, denomine la aplicación como `Pizza app`.


1. Seleccione **Entrenar** en la esquina superior derecha de la navegación para entrenar la aplicación.

## <a name="what-should-the-batch-file-utterances-include"></a>Qué deben incluir las expresiones del archivo por lotes

El archivo por lotes debe incluir expresiones con entidades de aprendizaje automático de nivel superior etiquetadas incluyendo las posiciones de inicio y fin. Las expresiones no deben ser parte de los ejemplos que ya hay en la aplicación. Deben ser expresiones para las que desee realizar una predicción positiva de la intención y las entidades.

Puede separar las pruebas por intención o entidad o tener todas las pruebas (hasta 1000 expresiones) en el mismo archivo.

## <a name="batch-file"></a>Filtro por lotes

El ejemplo JSON incluye una expresión con una entidad etiquetada para ilustrar el aspecto de un archivo de prueba. En sus propias pruebas, debe tener muchas expresiones con una intención correcta y una entidad de aprendizaje automático etiquetada.

1. Cree `pizza-with-machine-learned-entity-test.json` en un editor de texto o [descárguelo](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/batch-tests/pizza-with-machine-learned-entity-test.json?raw=true).

2. En el archivo por lotes con formato JSON, agregue una expresión con la **intención** que quiere predecir en la prueba.

   [!code-json[Add the intents to the batch test file](~/samples-cognitive-services-data-files/luis/batch-tests/pizza-with-machine-learned-entity-test.json "Add the intent to the batch test file")]

## <a name="run-the-batch"></a>Ejecute el lote

1. Haga clic en **Probar** en la barra de navegación superior.

2. Seleccione el **panel Prueba por lotes** en el panel de la derecha.

3. Seleccione **Importar conjunto de datos**.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de una aplicación de LUIS con Importar conjunto de datos resaltado](./media/luis-tutorial-batch-testing/import-dataset-button.png)

4. Elija la ubicación de archivo del archivo `pizza-with-machine-learned-entity-test.json`.

5. Asigne un nombre al conjunto de datos `pizza test` y seleccione **Done** (Listo).

    > [!div class="mx-imgBorder"]
    > ![Selección del archivo](./media/luis-tutorial-batch-testing/import-dataset-modal.png)

6. Haga clic en el botón **Ejecutar**.

7. Seleccione **Ver resultados**.

8. Revise los resultados en el gráfico y la leyenda.

## <a name="review-batch-results-for-intents"></a>Revisión de los resultados de lote para determinar intenciones

Los resultado de la prueba muestran gráficamente cómo se han predicho las expresiones de prueba con respecto a la versión activa.

El gráfico por lotes muestra cuatro cuadrantes de resultados. Hay un filtro a la derecha del gráfico. El filtro contiene intenciones y entidades. Cuando se selecciona una [sección del gráfico](luis-concept-batch-test.md#batch-test-results) o un punto dentro del gráfico, las expresiones asociadas se muestra debajo del gráfico.

Si mantiene el mouse sobre el gráfico y gira la rueda del mouse, puede agrandar o disminuir la visualización del gráfico. Esto resulta útil cuando hay muchos puntos en el gráfico estrechamente agrupados.

El gráfico está en cuatro cuadrantes, donde dos de las secciones aparecen en rojo.

1. Seleccione la intención **ModifyOrder** en la lista de filtros.

    > [!div class="mx-imgBorder"]
    > ![Selección de la intención ModifyOrder en la lista de filtros](./media/luis-tutorial-batch-testing/select-intent-from-filter-list.png)

    La expresión se predice como un **Verdadero positivo**, lo que significa que la expresión ha coincidido correctamente con su predicción positiva indicada en el archivo por lotes.

    > [!div class="mx-imgBorder"]
    > ![La expresión ha coincidido correctamente con su predicción positiva](./media/luis-tutorial-batch-testing/intent-predicted-true-positive.png)

    Las marcas de verificación verdes de la lista de filtros también indican el éxito de la prueba para cada intención. Todas las demás intenciones se muestran con una puntuación positiva 1/1 porque la expresión se ha probado para cada intención, como una prueba negativa para cualquier intención no incluida en la prueba por lotes.

1. Seleccione la intención **Confirmation**. Esta intención no aparece en la prueba por lotes, por lo que se trata de una prueba negativa de la expresión que aparece en la prueba por lotes.

    > [!div class="mx-imgBorder"]
    > ![Predicción negativa correcta de la expresión para una intención no indicada en el archivo por lotes](./media/luis-tutorial-batch-testing/true-negative-intent.png)

    La prueba negativa se realizó correctamente, como se indica por el texto verde del filtro y la cuadrícula.

## <a name="review-batch-test-results-for-entities"></a>Revisión de los resultados de la prueba por lotes para determinar entidades

La entidad ModifyOrder, como entidad de máquina con subentidades, se muestra si la entidad de nivel superior coincide y muestra cómo se predicen las subentidades.

1. Seleccione la entidad **ModifyOrder** en la lista de filtros y, a continuación, seleccione el círculo de la cuadrícula.

1. La predicción de entidad se muestra debajo del gráfico. Se muestran líneas continuas para las predicciones que coinciden con la expectativa y líneas de puntos para las predicciones que no coinciden con la expectativa.

    > [!div class="mx-imgBorder"]
    > ![Entidad principal predicha correctamente en el archivo por lotes](./media/luis-tutorial-batch-testing/labeled-entity-prediction.png)

## <a name="finding-errors-with-a-batch-test"></a>Búsqueda de errores con una prueba por lotes

En este tutorial se ha mostrado cómo ejecutar una prueba e interpretar los resultados. No se ha abordado la filosofía de las pruebas o cómo responder a las pruebas con errores.

* Asegúrese de incluir las expresiones positivas y negativas en su prueba, incluidas aquellas que pueden predecirse para una intención distinta pero relacionada.
* En el caso de expresiones con errores, realice las siguientes tareas y vuelva a ejecutar las pruebas:
    * Revise los ejemplos actuales de intenciones y entidades, y asegúrese de que las expresiones de ejemplo de la versión activa sean correctas para la intención y el etiquetado de la entidad.
    * Agregue características que ayuden a la aplicación a predecir las intenciones y las entidades.
    * Agregue más expresiones positivas de ejemplo.
    * Revise el saldo de expresiones de ejemplo entre las intenciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](./includes/cleanup-resources-preview-portal.md)]

## <a name="next-step"></a>Paso siguiente

En el tutorial, se ha usado una prueba por lotes para asegurarse del modelo actual.

> [!div class="nextstepaction"]
> [Información sobre los patrones](luis-tutorial-pattern.md)

