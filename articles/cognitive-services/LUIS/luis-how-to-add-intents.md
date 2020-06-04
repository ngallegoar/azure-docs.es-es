---
title: 'Adición de intenciones: LUIS'
titleSuffix: Azure Cognitive Services
description: Agregue intenciones a la aplicación de LUIS para identificar los grupos de preguntas o comandos que tienen las mismas intenciones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/17/2020
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: fe1c157d6847366a59739cd5128987127d01da94
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344431"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Adición de intenciones para determinar la intención de las expresiones del usuario

Agregue [intenciones](luis-concept-intent.md) a la aplicación de LUIS para identificar los grupos de preguntas o comandos que tienen la misma intención.

En el portal del LUIS, las intenciones se administran desde la barra de navegación superior de la sección **Build** (Compilar), desde el panel izquierdo **Intents** (Intenciones).

## <a name="add-an-intent-to-your-app"></a>Incorporación de una intención a la aplicación

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. En la página **Intents** (Intenciones), seleccione **+ Create** (+ Crear).
1. En el cuadro de diálogo **Create new intent** (Crear intención), escriba el nombre de la intención `ModifyOrder` y seleccione **Done** (Listo).

    > [!div class="mx-imgBorder"]
    > ![Adición de intención](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    La intención necesita [expresiones de ejemplo](luis-concept-utterance.md) para predecir expresiones en el punto de conexión publicado de la predicción.

## <a name="add-an-example-utterance"></a>Incorporación de una expresión de ejemplo

Las expresiones de ejemplo son ejemplos de texto de preguntas de los usuarios o de comandos. Para enseñar a Language Understanding (LUIS) cuándo predecir esta intención, debe agregar expresiones de ejemplo a una intención. LUIS necesita entre 15 y 30 expresiones de ejemplo para empezar a entender la intención. No agregue expresiones de ejemplo de forma masiva. Cada expresión debe elegirse con cuidado por lo que la diferencia de los ejemplos que ya están en la intención.

1. En la página de detalles de las intenciones, escriba una expresión pertinente que espere de los usuarios, como `Deliver a large cheese pizza` en el cuadro de texto situado debajo del nombre de la intención y, después, presione Entrar.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la página de detalles de las intenciones, con la expresión resaltada](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png)

    LUIS pasa todas las expresiones a minúsculas y agrega espacios alrededor de los [tokens](luis-language-support.md#tokenization) como guiones.

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>Errores de predicción de intenciones

Se determina un error de predicción de la intención cuando no se predice la expresión con la aplicación entrenada para ella.

1. Para encontrar errores de predicción de expresiones y corregirlos, use las opciones Incorrecto y Poco claro de **Filtro**.

    > [!div class="mx-imgBorder"]
    > ![Para encontrar errores de predicción de expresiones y corregirlos, use la opción de filtro.](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

1. Para mostrar el valor de puntuación en la página de detalles de la intención, seleccione **Show details intent scores** (Mostrar puntuaciones detalladas de las intenciones) en el menú de opciones **Ver**.

    Cuando se aplican los filtros y la vista, y hay expresiones de ejemplo con errores, la lista de expresiones de ejemplo muestra las expresiones y los problemas.

En cada fila se muestra la puntuación de predicción del entrenamiento actual de la expresión de ejemplo y la puntuación del rival más cercano, que es la diferencia entre estas dos puntuaciones.

### <a name="fixing-intents"></a>Corrección de intenciones

Para saber cómo solucionar errores de predicción de intenciones, use el [panel de resumen](luis-how-to-use-dashboard.md). El panel de resumen proporciona un análisis del último entrenamiento de la versión activa y ofrece las principales sugerencias para corregir el modelo.

## <a name="using-the-contextual-toolbar"></a>Uso de la barra de herramientas contextual

La barra de herramientas de contexto ofrece otras acciones:

* Edición o eliminación de la expresión de ejemplo
* Reasignación de la expresión de ejemplo a otra intención
* Filtros y vistas: solo muestran expresiones que contienen entidades filtradas o detalles opcionales de vistas
* Búsqueda a través de expresiones de ejemplo

## <a name="train-your-app-after-changing-model-with-intents"></a>Entrenar la aplicación después de cambiar el modelo con intenciones

Después de agregar, editar o quitar intenciones, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. No entrene después de realizar cada cambio. Entrene después de un grupo de cambios.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la incorporación de [expresiones de ejemplo](luis-how-to-add-example-utterances.md) con entidades.
