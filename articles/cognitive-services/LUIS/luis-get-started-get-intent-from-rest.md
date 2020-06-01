---
title: 'Inicio rápido: Obtención de intención con las API REST (LUIS)'
description: En este inicio rápido de API REST, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional.
ms.topic: quickstart
ms.date: 05/18/2020
zone_pivot_groups: programming-languages-set-one
ms.openlocfilehash: 90d98e56e53e28991fb5aada9eab5a7e9c2e69c3
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654264"
---
# <a name="quickstart-get-intent-with-rest-apis"></a>Inicio rápido: Obtención de intención con las API REST

En este inicio rápido, usará una aplicación de LUIS para determinar la intención de un usuario a partir de texto conversacional. Envíe la intención del usuario como texto al punto de conexión de predicción HTTP de la aplicación de Pizza. En el punto de conexión, LUIS aplica el modelo de la aplicación de Pizza para analizar el texto en lenguaje natural y lo que significa, y así determinar la intención general y extraer los datos que son pertinentes para el dominio del sujeto de la aplicación.

En esta guía de inicio rápido se usa la API de REST del punto de conexión. Para más información, consulte la [documentación de la API del punto de conexión](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78).

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
