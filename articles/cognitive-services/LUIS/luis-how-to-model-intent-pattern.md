---
title: 'Precisión de la adición de patrones: LUIS'
titleSuffix: Azure Cognitive Services
description: Agregue plantillas de patrón para mejorar la precisión de las predicciones en aplicaciones de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: diberry
ms.openlocfilehash: 21afb12bf2464218119ebf52ebd980745e3d731d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76311723"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Incorporación de patrones para mejorar la precisión de las predicciones
Después de que una aplicación de LUIS reciba expresiones de punto de conexión, use un [patrón](luis-concept-patterns.md) para mejorar la precisión de las predicciones para las expresiones que revelan un patrón en el orden y la elección de las palabras. Los patrones utilizan [sintaxis](luis-concept-patterns.md#pattern-syntax) específica para indicar la ubicación de: [entidades](luis-concept-entity-types.md), [roles](luis-concept-roles.md) de entidad y texto opcional.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

> [!CAUTION]
> Los patrones solo incluyen elementos primarios de la entidad de aprendizaje automático, no los subcomponentes.

## <a name="adding-example-utterances-as-pattern"></a>Agregar expresiones de ejemplo como patrón

Si quiere agregar un patrón para una entidad, la forma _más sencilla_ es crear el patrón a partir de la página de detalles de la intención. Esto garantiza que la sintaxis coincidirá con la expresión de ejemplo.

1. En el [portal de LUIS en versión preliminar](https://preview.luis.ai), seleccione la aplicación en la página **Mis aplicaciones**.
1. En la página de lista **Intenciones**, seleccione el nombre de intención de la expresión de ejemplo a partir de la que quiere crear una expresión de plantilla.
1. En la página Detalles de intención, seleccione la fila para la expresión de ejemplo que quiera usar como expresión de plantilla y, a continuación, seleccione **+ Agregar como patrón** en la barra de herramientas de contexto.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la selección de la expresión de ejemplo como patrón de plantilla en la página de detalles de intención.](./media/luis-how-to-model-intent-pattern/add-example-utterances-as-pattern-template-utterance-from-intent-detail-page.png)

1. En el cuadro emergente, seleccione **Listo** en la página **Confirmar patrones**. No es necesario definir subcomponentes, restricciones o descriptores de las entidades. Solo necesita mostrar la entidad de aprendizaje automático.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de confirmación de la expresión de ejemplo como patrón de plantilla en la página de detalles de intención.](./media/luis-how-to-model-intent-pattern/confirm-patterns-from-example-utterance-intent-detail-page.png)

1. Si necesita editar la plantilla; por ejemplo, para seleccionar texto como opcional, con los corchetes `[]` (cuadrados), debe hacer esta edición desde la página **Patrones**.

1. En la barra de navegación, seleccione **Entrenar** para entrenar la aplicación con el nuevo patrón.

## <a name="add-template-utterance-using-correct-syntax"></a>Agregar una expresión de plantilla con la sintaxis correcta

1. Para abrir la aplicación, seleccione su nombre en la página **My Apps** (Mis aplicaciones) y, después, haga clic en **Patterns** (Patrones) en el panel de la izquierda, en **Improve app performance** (Mejorar el rendimiento de la aplicación).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la lista de patrones](./media/luis-how-to-model-intent-pattern/patterns-1.png)

1. Seleccione la intención correcta para el patrón.

1. En el cuadro de texto de la plantilla, escriba la expresión de plantilla y presione Entrar. Cuando quiera escribir el nombre de la entidad, debe usar la sintaxis de entidad de patrón correcta. Comience la sintaxis de la entidad con `{`. Se mostrará la lista de entidades. Seleccione la entidad correcta.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la entidad del patrón](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Si la entidad incluye un [rol](luis-concept-roles.md), indíquelo con un signo de dos puntos, `:`, después del nombre de la entidad, como en `{Location:Origin}`. La lista de roles para las entidades se muestra en una lista. Seleccione el rol y, después, presione Entrar.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la entidad con el rol](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Después de seleccionar la entidad correcta, termine de escribir el patrón y, luego, presione Entrar. Cuando haya terminado de escribir los patrones, [entrene](luis-how-to-train.md) la aplicación.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del patrón introducido con ambos tipos de entidades](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Entrenar la aplicación después de cambiar el modelo con patrones
Después de agregar, editar o quitar un patrón, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión.

<a name="search-patterns"></a>
<a name="edit-a-pattern"></a>
<a name="reassign-individual-pattern-to-different-intent"></a>
<a name="reassign-several-patterns-to-different-intent"></a>
<a name="delete-a-single-pattern"></a>
<a name="delete-several-patterns"></a>
<a name="filter-pattern-list-by-entity"></a>
<a name="filter-pattern-list-by-intent"></a>
<a name="remove-entity-or-intent-filter"></a>
<a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>

## <a name="use-contextual-toolbar"></a>Uso de la barra de herramientas contextual

La barra de herramientas contextual encima de la lista de patrones permite:

* Buscar patrones
* Editar un patrón
* Reasignar un patrón individual a otra intención
* Reasignar varios patrones a otra intención
* Eliminar un solo patrón
* Eliminar varios patrones
* Filtrar la lista de patrones por entidad
* Filtrar la lista de patrones por intención
* Quitar el filtro de intención o entidad
* Adición de un patrón a partir de una de expresión existente en la página de la intención o entidad

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear un patrón](luis-tutorial-pattern.md) con pattern.any y roles con un tutorial.
* Obtenga información sobre cómo [entrenar](luis-how-to-train.md) la aplicación.
