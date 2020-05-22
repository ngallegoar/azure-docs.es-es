---
title: archivo de inclusión
ms.topic: include
ms.custom: include file
ms.date: 5/04/2020
ms.openlocfilehash: 819dfa3127eb91a2f08687a76ea0586439b650a4
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83591000"
---
|Nivel|Configuración de UI|Configuración de la API|Information|
|--|--|--|--|
|Aplicación|Hacer que los puntos de conexión sean públicos|`Public`|Cualquier persona puede tener acceso a la aplicación pública si tiene una clave de predicción y conoce el id. de la aplicación. |
|Versión|Usar aprendizaje no determinista|`UseAllTrainingData`|El entrenamiento usa un pequeño porcentaje de muestreo negativo. Si quiere utilizar todos los datos en lugar del pequeño muestreo negativo, se debe establecer en `true`. |
|Versión|Normalizar signos diacríticos|`NormalizeDiacritics`|Al normalizar los signos diacríticos se reemplazan los caracteres con signos diacríticos en las expresiones por caracteres normales.|
|Versión|Normalizar la puntuación|`NormalizePunctuation`|La normalización de la puntuación significa que antes de que los modelos se entrenen y antes de que las consultas de punto de conexión se predigan, se quitará la puntuación de las expresiones.|
|Versión|Normalizar los formularios Word|`NormalizeWordForm`|Omita los formularios Word más allá de la raíz.|

Obtenga información sobre los [conceptos](../luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) de la normalización y cómo usar las API de [aplicación](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58aeface39e2bb03dcd5909e) y [versión](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) para actualizar esta configuración o cómo usar la sección **Administrar** del portal de LUIS, página **Configuración de la aplicación**.