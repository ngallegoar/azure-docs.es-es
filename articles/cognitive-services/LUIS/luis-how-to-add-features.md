---
title: 'Características: LUIS'
titleSuffix: Azure Cognitive Services
description: Uso de Language Understanding (LUIS) para agregar características de aplicación que pueden mejorar la detección o predicción de intenciones y entidades de categorías y patrones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: diberry
ms.openlocfilehash: 12445ec5b14f4c274e471bf1b061a3b221664d20
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592311"
---
# <a name="use-features-to-boost-signal-of-word-list"></a>Uso de características para aumentar la señal de la lista de palabras

Puede agregar características a la aplicación de LUIS para mejorar su precisión. Las características ayudan a LUIS al proporcionar sugerencias sobre ciertas palabras y frases que forman parte de un vocabulario de dominio de aplicación.

Revise los [conceptos](luis-concept-feature.md) para comprender cuándo y por qué usar una característica.

## <a name="add-phrase-list-as-a-feature"></a>Adición de una lista de frases como característica

1. Inicie sesión en el [portal de LUIS](https://www.luis.ai), seleccione su **Suscripción** y **Recurso de creación** para ver las aplicaciones asignadas a ese recurso de creación.
1. Abra la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**.
1. Seleccione **Compilación** y, a continuación, seleccione **Características** en el panel izquierdo de la aplicación.

1. En la página **Características**, seleccione **+ Crear**.

1. En el cuadro de diálogo **Create new phrase list feature** (Crear característica de lista de frases), escriba un nombre, como `Cities`. En el cuadro **Valor**, escriba ejemplos de ciudades, como `Seattle`. Puede escribir un valor a la vez o un conjunto de valores separados por comas y luego presione **ENTRAR**.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la adición de la característica (lista de frases) Cities](./media/luis-add-features/add-phrase-list-cities.png)

    Una vez que haya especificado suficientes valores para LUIS, aparecerán sugerencias. Puede **agregar todos** los valores propuestos o seleccionar términos individuales.

1. Mantenga activado **Estos valores son intercambiables** si las frases se pueden usar indistintamente.

1. La lista de frases se puede aplicar a toda la aplicación con la configuración **Global**, o bien a un modelo específico (intención o entidad). Si crea la lista de frases, como una _característica_ de una intención o entidad, la alternancia no se establece en global. En este caso, el significado de la alternancia es que la característica es solo local para ese modelo, por lo que _no es global_  para la aplicación.

1. Seleccione **Listo**. La nueva característica se agrega a la página **ML Features** (Características de ML).

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Puede eliminar o desactivar una lista de frases desde la barra de herramientas contextual de la página **ML Features**.

## <a name="next-steps"></a>Pasos siguientes

Después de agregar, editar, eliminar o desactivar una característica, vuelva a [entrenar y probar la aplicación](luis-interactive-test.md) para ver si mejora el rendimiento.
