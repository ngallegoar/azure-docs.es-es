---
title: Modelos precompilados para Language Understanding
titleSuffix: Azure Cognitive Services
description: LUIS incluye un conjunto de modelos precompilados para agregar rápidamente escenarios de usuario comunes y de conversación.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: diberry
ms.openlocfilehash: 0a733aff14b71c6378971c0b72a9ca1f3cd4cfc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74013556"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Adición de modelos precompilados en escenarios de uso común 

LUIS incluye un conjunto de modelos precompilados para agregar rápidamente escenarios de usuario comunes y de conversación. Esta es una manera rápida y fácil de agregar capacidades a la aplicación cliente de conversación sin tener que diseñar los modelos correspondientes a esas capacidades. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-a-prebuilt-domain"></a>Incorporación de un dominio creado previamente

1. En la página **My Apps** (Mis aplicaciones), seleccione su aplicación. Esta opción abre su aplicación a la sección **Build** (Compilar) de la aplicación. 

1. Seleccione **Dominios creados previamente** en la barra de herramientas de la izquierda. 

1. Busque el dominio que quiere agregar a la aplicación y, luego, seleccione el botón **Agregar dominio**.

    > [!div class="mx-imgBorder"]
    > ![Agregar un dominio de Calendario creado previamente](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

## <a name="add-a-prebuilt-intent"></a>Agregar una intención creada previamente

1. En la página **My Apps** (Mis aplicaciones), seleccione su aplicación. Esta opción abre su aplicación a la sección **Build** (Compilar) de la aplicación. 

1. En la página **Intents** (Intenciones), seleccione **Add prebuilt domain intent** (Agregar intención de dominio creado previamente) en la barra de herramientas sobre la lista de intenciones. 

1. Seleccione la intención **Utilities.Cancel** en el cuadro de diálogo emergente. 

    > [!div class="mx-imgBorder"]
    > ![Agregar una intención creada previamente](./media/luis-prebuilt-domains/add-prebuilt-domain-intents.png)

1. Haga clic en el botón **Done** (Listo).

## <a name="add-a-prebuilt-entity"></a>Adición de una entidad precompilada

1. Haga clic en la aplicación para abrirla desde la página **Mis aplicaciones** y, después, haga clic en **Entidades** a la izquierda. 

1. En la página **Entities** (Entidades), haga clic en **Add prebuilt entities** (Agregar entidad precompilada).

1. En el cuadro de diálogo **Add prebuilt entities** (Agregar entidades precompiladas), seleccione la entidad precompilada. 

    > [!div class="mx-imgBorder"]
    > ![Cuadro de diálogo Add prebuilt entity (Agregar entidades precompiladas)](./media/luis-prebuilt-domains/add-prebuilt-entity.png)

1. Seleccione **Listo**. Después de agregar la entidad, no es necesario entrenar la aplicación. 

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Publicación para visualizar el modelo precompilado desde el punto de conexión de predicción

La manera más fácil de ver el valor de un modelo precompilado es consultar desde el punto de conexión publicado. 

## <a name="entities-containing-a-prebuilt-entity-token"></a>Entidades que contienen un token de entidad creada previamente
 
Si tiene una entidad de aprendizaje automático restringida por una entidad precompilada, agregue un subcomponente a la entidad de aprendizaje automático y, luego, agregue una restricción de una entidad precompilada.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Compilación de un modelo a partir de archivo. csv con las API REST](./luis-tutorial-node-import-utterances-csv.md)
