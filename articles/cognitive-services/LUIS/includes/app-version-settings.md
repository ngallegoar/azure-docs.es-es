---
title: archivo de inclusión
ms.topic: include
ms.custom: include file
ms.date: 5/17/2020
ms.openlocfilehash: 79a45825be66b44d869ef741917436de87a1f5d0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653212"
---
Obtenga información sobre los [conceptos](../luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) de la normalización y cómo usar las API de [versión](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) y versión para actualizar esta configuración, o cómo usar la sección **Administrar** del portal de LUIS, página **Configuración**.


|Configuración de UI|Configuración de la API|Information|
|--|--|--|
|Usar aprendizaje no determinista|`UseAllTrainingData`|El entrenamiento usa un pequeño porcentaje de muestreo negativo. Si quiere utilizar todos los datos en lugar del pequeño muestreo negativo, se debe establecer en `true`. |
|Normalizar signos diacríticos|`NormalizeDiacritics`|Al normalizar los signos diacríticos se reemplazan los caracteres con signos diacríticos en las expresiones por caracteres normales. Esta opción solo está disponible en [idiomas](../luis-reference-application-settings.md#diacritics-normalization) que admitan marcas diacríticas.|
|Normalizar la puntuación|`NormalizePunctuation`|La normalización de la puntuación significa que antes de que los modelos se entrenen y antes de que las consultas de punto de conexión se predigan, se quitará la puntuación de las expresiones.|
|Normalizar los formularios Word|`NormalizeWordForm`|Omita los formularios Word más allá de la raíz.|
