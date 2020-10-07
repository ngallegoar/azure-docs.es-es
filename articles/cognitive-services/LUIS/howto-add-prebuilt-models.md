---
title: Modelos precompilados para Language Understanding
titleSuffix: Azure Cognitive Services
description: LUIS incluye un conjunto de modelos precompilados para agregar rápidamente escenarios de usuario comunes y de conversación.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/17/2020
ms.openlocfilehash: 2080139732d71aa5df9a9d4ad13fcc4545128dff
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541924"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Adición de modelos precompilados en escenarios de uso común

LUIS incluye un conjunto de modelos precompilados para agregar rápidamente escenarios de usuario comunes y de conversación. Esta es una manera rápida y fácil de agregar capacidades a la aplicación cliente de conversación sin tener que diseñar los modelos correspondientes a esas capacidades.

## <a name="add-a-prebuilt-domain"></a>Incorporación de un dominio creado previamente

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.

1. Seleccione **Dominios creados previamente** en la barra de herramientas de la izquierda.

1. Busque el dominio que quiere agregar a la aplicación y, luego, seleccione el botón **Agregar dominio**.

    > [!div class="mx-imgBorder"]
    > ![Agregar un dominio de Calendario creado previamente](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

## <a name="add-a-prebuilt-intent"></a>Agregar una intención creada previamente

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.

1. En la página **Intents** (Intenciones), seleccione **Add prebuilt domain intent** (Agregar intención de dominio creado previamente) en la barra de herramientas sobre la lista de intenciones.

1. Seleccione una intención en el cuadro de diálogo del elemento emergente.

    > [!div class="mx-imgBorder"]
    > ![Agregar una intención creada previamente](./media/luis-prebuilt-domains/add-prebuilt-domain-intents.png)

1. Haga clic en el botón **Done** (Listo).

## <a name="add-a-prebuilt-entity"></a>Adición de una entidad precompilada
1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. Seleccione **Entidades** en el lado izquierdo.

1. En la página **Entidades**, seleccione **Agregar entidades predefinidas**.

1. En el cuadro de diálogo **Add prebuilt entities** (Agregar entidades precompiladas), seleccione la entidad precompilada.

    > [!div class="mx-imgBorder"]
    > ![Cuadro de diálogo Add prebuilt entity (Agregar entidades precompiladas)](./media/luis-prebuilt-domains/add-prebuilt-entity.png)

1. Seleccione **Listo**. Después de agregar la entidad, no es necesario entrenar la aplicación.

## <a name="add-a-prebuilt-domain-entity"></a>Incorporación de una entidad de dominio precompilada
1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. Seleccione **Entidades** en el lado izquierdo.

1. En la página **Entidades**, seleccione **Agregar entidad de dominio predefinida**.

1. En el cuadro de diálogo **Agregar modelos de dominio predefinido** seleccione la entidad de dominio precompilada.

1. Seleccione **Listo**. Después de agregar la entidad, no es necesario entrenar la aplicación.

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Publicación para visualizar el modelo precompilado desde el punto de conexión de predicción

La manera más fácil de ver el valor de un modelo precompilado es consultar desde el punto de conexión publicado.

## <a name="entities-containing-a-prebuilt-entity-token"></a>Entidades que contienen un token de entidad creada previamente

Si tiene una entidad de aprendizaje automático que necesita una característica obligatoria de una entidad precompilada, agregue una subentidad a la entidad de aprendizaje automático y, luego, agregue una característica _obligatoria_ de una entidad precompilada.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Compilación de un modelo a partir de archivo. csv con las API REST](./luis-tutorial-node-import-utterances-csv.md)
