---
title: Cambio del modelo de LUIS mediante API REST
titleSuffix: Azure Cognitive Services
description: En este artículo, agregará expresiones de ejemplo para cambiar un modelo y entrenar la aplicación.
services: cognitive-services
manager: nitinme
ms.custom: seodec18, devx-track-python, devx-track-js, devx-track-csharp
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/30/2020
zone_pivot_groups: programming-languages-set-one
ms.openlocfilehash: 7346037ad804da4f68a2bc6512494c461d7fc5bd
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96436188"
---
# <a name="how-to-change-the-luis-model-with-rest-apis"></a>Cambio del modelo de LUIS mediante API REST

En este artículo, agregará expresiones de ejemplo a una aplicación de pizza y entrenará la aplicación. Las expresiones de ejemplo son texto de usuario conversacional que se asigna a una intención. Al proporcionar expresiones de ejemplo para las intenciones, enseñará a LUIS qué clases de texto suministrado por el usuario pertenecen a qué intención.

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# and REST](./includes/get-started-get-model-rest-csharp.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Get intent with Java and REST](./includes/get-started-get-model-rest-java.md)]
::: zone-end

::: zone pivot="programming-language-go"
[!INCLUDE [Get intent with Go and REST](./includes/get-started-get-model-rest-go.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js and REST](./includes/get-started-get-model-rest-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python and REST](./includes/get-started-get-model-rest-python.md)]
::: zone-end
