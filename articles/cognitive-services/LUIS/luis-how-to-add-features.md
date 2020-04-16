---
title: 'Descriptores: LUIS'
titleSuffix: Azure Cognitive Services
description: Uso de Language Understanding (LUIS) para agregar características de aplicación que pueden mejorar la detección o predicción de intenciones y entidades de categorías y patrones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: diberry
ms.openlocfilehash: 7560fdcbfc77ea2655e8af641794478ead4c11c7
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631458"
---
# <a name="use-descriptors-to-boost-signal-of-word-list"></a>Uso de descriptores para aumentar la señal de la lista de palabras

Puede agregar características a la aplicación de LUIS para mejorar su precisión. Las características ayudan a LUIS al proporcionar sugerencias sobre ciertas palabras y frases que forman parte de un vocabulario de dominio de aplicación.

Un [descriptor](luis-concept-feature.md) (lista de frases) incluye un grupo de valores (palabras o frases) que pertenecen a la misma clase y que se deben tratar de forma similar (por ejemplo, nombres de ciudades o productos). Lo que LUIS aprende sobre una de ellas se aplica automáticamente al resto. Esta lista no es lo mismo que una [entidad de listas](reference-entity-list.md) (coincidencias de texto exactas) de palabras coincidentes.

Un descriptor se agrega al vocabulario del dominio de aplicación como una segunda señal para LUIS sobre esas palabras.

Revise los [conceptos de características](luis-concept-feature.md) para comprender cuándo y por qué usar un descriptor.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-descriptor"></a>Adición de descriptor

1. Abra la aplicación con un clic en su nombre en la página **Mis aplicaciones**, haga clic en **Compilar** y luego en **Descriptores** en el panel izquierdo de la aplicación.

1. En la página **Descriptores**, haga clic en **+Add Descriptor** (+ Agregar descriptor).

1. En el cuadro de diálogo para **crear nuevo descriptor de lista de frases**, escriba un nombre como `Cities` para el descriptor. En el cuadro **Valor**, escriba los valores de los descriptores, como `Seattle`. Puede escribir un valor a la vez o un conjunto de valores separados por comas y luego presione **ENTRAR**.

    > [!div class="mx-imgBorder"]
    > ![Adición de ciudades de descriptor](./media/luis-add-features/add-phrase-list-cities.png)

    Una vez que haya especificado suficientes valores para LUIS, aparecerán sugerencias. Puede **agregar todos** los valores propuestos o seleccionar términos individuales.

1. Mantenga activado **Estos valores son intercambiables** si los valores agregados de la lista de frases son alternativas que se pueden usar indistintamente.

1. La lista de frases se puede aplicar a toda la aplicación con la configuración **Global**, o bien a un modelo específico (intención o entidad). Si crea la lista de frases, como un _descriptor_ de una intención o entidad, la alternancia se establece en no global. En este caso, el significado de la alternancia es que el descriptor es una característica solo de ese modelo, por lo que _no es global_  para la aplicación.

1. Seleccione **Listo**. El nuevo descriptor se agrega a la página de **descriptores**.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Puede eliminar o desactivar un suscriptor desde la barra de herramientas contextual de la página de **descriptores**.

## <a name="next-steps"></a>Pasos siguientes

Después de agregar, editar, eliminar o desactivar un descriptor, vuelva a [entrenar y probar la aplicación](luis-interactive-test.md) para ver si mejora el rendimiento.
