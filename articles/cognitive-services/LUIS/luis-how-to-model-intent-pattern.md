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
ms.topic: how-to
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: 4af8f90abf2a5581ec18ea550cbc5f1a5df50a4e
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344907"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Incorporación de patrones para mejorar la precisión de las predicciones
Después de que una aplicación de LUIS reciba expresiones de punto de conexión, use un [patrón](luis-concept-patterns.md) para mejorar la precisión de las predicciones para las expresiones que revelan un patrón en el orden y la elección de las palabras. Los patrones utilizan [sintaxis](luis-concept-patterns.md#pattern-syntax) específica para indicar la ubicación de: [entidades](luis-concept-entity-types.md), [roles](luis-concept-roles.md) de entidad y texto opcional.

> [!CAUTION]
> Los patrones solo incluyen elementos primarios de entidad de aprendizaje automático, no subentidades.

## <a name="add-template-utterance-using-correct-syntax"></a>Agregar una expresión de plantilla con la sintaxis correcta

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. Seleccione **Patrones** en el panel izquierdo, en **Mejorar el rendimiento de la aplicación**.

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

## <a name="create-a-patternany-entity"></a>Creación de una entidad pattern.any

Las entidades [pattern.any](luis-concept-entity-types.md) solo son válidas en los [patrones](luis-how-to-model-intent-pattern.md), no en las expresiones de ejemplo de las intenciones. Este tipo de entidad ayuda a LUIS a buscar el final de las entidades de longitud variable y la elección de palabras. Dado que esta entidad se usa en un patrón, LUIS sabe dónde está el final de la entidad en la plantilla de expresión.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. En la sección **Compilar**, seleccione **Entidades** en el panel de la izquierda y seleccione **+ Crear**.

1. En el cuadro de diálogo **Choose an entity type** (Elegir un tipo de entidad), escriba el nombre de la entidad en el cuadro **Nombre** y seleccione **Pattern.Any** como **Tipo** y luego seleccione **Crear**.

    Después de [crear una expresión de patrón](luis-how-to-model-intent-pattern.md) con esta entidad, la entidad se extrae con un algoritmo combinado de coincidencia de texto y aprendizaje automático.

## <a name="adding-example-utterances-as-pattern"></a>Agregar expresiones de ejemplo como patrón

Si quiere agregar un patrón para una entidad, la forma _más sencilla_ es crear el patrón a partir de la página de detalles de la intención. Esto garantiza que la sintaxis coincidirá con la expresión de ejemplo.

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. En la página de lista **Intenciones**, seleccione el nombre de intención de la expresión de ejemplo a partir de la que quiere crear una expresión de plantilla.
1. En la página Detalles de intención, seleccione la fila para la expresión de ejemplo que quiera usar como expresión de plantilla y, a continuación, seleccione **+ Agregar como patrón** en la barra de herramientas de contexto.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la selección de la expresión de ejemplo como patrón de plantilla en la página de detalles de intención.](./media/luis-how-to-model-intent-pattern/add-example-utterances-as-pattern-template-utterance-from-intent-detail-page.png)

    Para poder crear un patrón a partir de la expresión, esta debe incluir una entidad.

1. En el cuadro emergente, seleccione **Listo** en la página **Confirmar patrones**. No es necesario definir las subentidades ni características de las entidades. Solo necesita mostrar la entidad de aprendizaje automático.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de confirmación de la expresión de ejemplo como patrón de plantilla en la página de detalles de intención.](./media/luis-how-to-model-intent-pattern/confirm-patterns-from-example-utterance-intent-detail-page.png)

1. Si necesita editar la plantilla; por ejemplo, para seleccionar texto como opcional, con los corchetes `[]` (cuadrados), debe hacer esta edición desde la página **Patrones**.

1. En la barra de navegación, seleccione **Entrenar** para entrenar la aplicación con el nuevo patrón.

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
