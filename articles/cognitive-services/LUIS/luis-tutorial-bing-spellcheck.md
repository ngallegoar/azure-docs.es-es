---
title: 'Corrección de palabras mal escritas: LUIS'
titleSuffix: Azure Cognitive Services
description: Corrija palabras incorrectas en expresiones mediante la adición de Bing Spell Check API V7 a las consultas de punto de conexión de LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: a1e43cfc55611c432652055db2ac8411d835608b
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396828"
---
# <a name="correct-misspelled-words-with-bing-spell-check"></a>Corregir palabras incorrectas con Bing Spell Check

Se puede integrar la aplicación de LUIS con [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) para corregir las palabras incorrectas en las expresiones antes de que LUIS prediga la puntuación y las entidades de la expresión. 

[!INCLUDE [Not supported in V3 API prediction endpoint](./includes/v2-support-only.md)]


## <a name="create-first-key-for-bing-spell-check-v7"></a>Crear la primera clave para Bing Spell Check V7

La [primera clave de Bing Spell Check API v7](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) es gratuita. 

![Creación de una clave gratuita](./media/luis-tutorial-bing-spellcheck/free-key.png)

<a name="create-subscription-key"></a>

## <a name="create-endpoint-key"></a>Creación de punto de conexión
Si la clave gratuita expiró, cree una clave de punto de conexión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 

2. Haga clic en **Crear un recurso** en la esquina superior izquierda.

3. En el cuadro de búsqueda, escriba `Bing Spell Check API V7`.

    ![Búsqueda de Bing Spell Check API V7](./media/luis-tutorial-bing-spellcheck/portal-search.png)

4. Seleccione el servicio. 

5. Aparece un panel de información a la derecha con información, incluido el Aviso legal. Haga clic en **Crear** para iniciar el proceso de creación de la suscripción. 

6. En el panel siguiente, escriba la configuración del servicio. Espere a que finalice el proceso de creación del servicio.

    ![Introducción de la configuración del servicio](./media/luis-tutorial-bing-spellcheck/subscription-settings.png)

7. Seleccione **Todos los recursos** bajo el título **Favoritos** en el panel de navegación de la izquierda.

8. Seleccione el servicio nuevo. Su tipo es **Cognitive Services** y la ubicación es **global**. 

9. En el panel principal, haga clic en **Keys** (Claves) para ver las claves nuevas.

    ![Obtención de las claves](./media/luis-tutorial-bing-spellcheck/grab-keys.png)

10. Copie la primera clave. Solo necesita una de las dos claves. 

<!--
## Using the key in LUIS test panel
There are two places in LUIS to use the key. The first is in the [test panel](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). The key isn't saved into LUIS but instead is a session variable. You need to set the key every time you want the test panel to apply the Bing Spell Check API v7 service to the utterance. See [instructions](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel) in the test panel for setting the key.
-->
## <a name="adding-the-key-to-the-endpoint-url"></a>Agregar la clave a la dirección URL del punto de conexión
La consulta de punto de conexión necesita la clave que se pasa en los parámetros de cadena de consulta para cada consulta a la que quiera aplicar la corrección ortográfica. Es posible que tenga un bot de chat que llama a LUIS o puede llamar directamente a la API de punto de conexión de LUIS. Con independencia de cómo se llame al punto de conexión, cada llamada debe incluir la información necesaria para que las correcciones ortográficas funcionen correctamente.

La dirección URL del punto de conexión tiene varios valores que se deben pasar correctamente. La clave de Bing Spell Check API v7 es simplemente uno de ellos. Debe establecer el parámetro **spellCheck** en true y el valor de **bing-spell-check-subscription-key** en el valor de la clave:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appID}?subscription-key={luisKey}&spellCheck=true&bing-spell-check-subscription-key={bingKey}&verbose=true&timezoneOffset=0&q={utterance}`

## <a name="send-misspelled-utterance-to-luis"></a>Enviar expresiones incorrectas a LUIS
1. En un explorador web, copie la cadena anterior y reemplace `region`, `appId`, `luisKey` y `bingKey` con sus propios valores. Asegúrese de usar la región del punto de conexión, si es diferente de la [región](luis-reference-regions.md) de publicación.

2. Agregue una expresión incorrecta como "How far is the mountainn?". En inglés, `mountain`, con una `n`, es la ortografía correcta. 

3. Presione Entrar para enviar la consulta a LUIS.

4. LUIS responde con un resultado JSON para `How far is the mountain?`. Si Bing Spell Check API v7 detecta un error ortográfico, el campo `query` de la respuesta JSON de la aplicación de LUIS contiene la consulta original y el campo `alteredQuery` contiene la consulta corregida enviada a LUIS.

```json
{
  "query": "How far is the mountainn?",
  "alteredQuery": "How far is the mountain?",
  "topScoringIntent": {
    "intent": "Concierge",
    "score": 0.183866
  },
  "entities": []
}
```

## <a name="ignore-spelling-mistakes"></a>Omitir los errores ortográficos

Si no desea usar el servicio Bing Spell Check API v. 7, debe agregar la ortografía correcta e incorrecta. 

Hay dos soluciones:

* Etiquete las expresiones de ejemplo que tengan todas las diferentes ortografías para que LUIS pueda aprender la correcta, así como los errores tipográficos. Esta opción requiere más esfuerzo de etiquetado que el uso de un corrector ortográfico.
* Cree una lista de frases con todas las variaciones de la palabra. Con esta solución, no es necesario etiquetar las variaciones de palabras en las expresiones de ejemplo. 

## <a name="publishing-page"></a>Página de publicación
La página de [publicación](luis-how-to-publish-app.md) contiene una casilla **Enable Bing spell checker** (Habilitar el corrector ortográfico de Bing). Se trata de una comodidad para crear la clave y comprender cómo cambia la dirección URL del punto de conexión. Tendrá que seguir usando los parámetros de punto de conexión correctos para que se corrija la ortografía de cada expresión. 

> [!div class="nextstepaction"]
> [Más información sobre las expresiones de ejemplo](luis-how-to-add-example-utterances.md)
