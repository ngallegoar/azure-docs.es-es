---
title: Obtención de una intención mediante API REST
description: En este artículo, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, devx-track-js, devx-track-csharp
zone_pivot_groups: programming-languages-set-one
ms.openlocfilehash: b49d90ffe2a262895727fdd4ef5f8a7950104102
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96437021"
---
# <a name="how-to-get-an-intent-using-the-rest-apis"></a>Obtención de una intención mediante las API REST

En este artículo, usará una aplicación de LUIS para determinar la intención de un usuario a partir de texto conversacional. Envíe la intención del usuario como texto al punto de conexión de predicción HTTP de la aplicación de Pizza. En el punto de conexión, LUIS aplica el modelo de la aplicación de Pizza para analizar el texto en lenguaje natural y lo que significa, y así determinar la intención general y extraer los datos que son pertinentes para el dominio del sujeto de la aplicación.

Para este artículo, necesita una cuenta gratuita de [LUIS](https://www.luis.ai).

<a name="create-luis-subscription-key"></a>

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# and REST](./includes/get-started-get-intent-rest-csharp.md)]
::: zone-end

::: zone pivot="programming-language-go"
[!INCLUDE [Get intent with Go and REST](./includes/get-started-get-intent-rest-go.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Get intent with Java and REST](./includes/get-started-get-intent-rest-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js and REST](./includes/get-started-get-intent-rest-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python and REST](./includes/get-started-get-intent-rest-python.md)]
::: zone-end
