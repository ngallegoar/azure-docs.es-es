---
title: Comprobación de la instancia del contenedor de detección de idioma
titleSuffix: Azure Cognitive Services
description: Aprenda a comprobar la instancia del contenedor de detección de idioma.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: dc52586550f89ddae147d79458584331ed984eea
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876468"
---
### <a name="verify-the-language-detection-container-instance"></a>Comprobación de la instancia del contenedor de detección de idioma

1. Seleccione la pestaña **Información general** y copie la dirección IP.
1. Abra una nueva pestaña del explorador y escriba la dirección IP. Por ejemplo, escriba `http://<IP-address>:5000 (http://55.55.55.55:5000`). Se mostrará la página principal del contenedor, que le permitirá saber que el contenedor se está ejecutando.

    ![Visualización de la página principal del contenedor para comprobar que se está ejecutando](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Seleccione el vínculo de **Descripción de API de servicio** para ir a la página Swagger del contenedor.

1. Elija cualquiera de las API **POST** y seleccione **Probar**. Se muestran los parámetros, que incluyen la entrada de este ejemplo:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. Establezca **showStats** en `true`.

1. Seleccione **Execute** (Ejecutar) para determinar la opinión del texto.

    El modelo empaquetado en el contenedor genera una puntuación comprendida entre 0 y 1, donde 0 es una opinión negativa y 1, positiva.

    La respuesta JSON devuelta incluye las opiniones de la entrada de texto actualizado:

    ```json
    {
      "documents": [
        {
          "id": "1",
          "detectedLanguages": [
            {
              "name": "English",
              "iso6391Name": "en",
              "score": 1
            }
          ],
          "statistics": {
            "charactersCount": 11,
            "transactionsCount": 1
          }
        },
        {
          "id": "2",
          "detectedLanguages": [
            {
              "name": "French",
              "iso6391Name": "fr",
              "score": 1
            }
          ],
          "statistics": {
            "charactersCount": 21,
            "transactionsCount": 1
          }
        },
        {
          "id": "3",
          "detectedLanguages": [
            {
              "name": "Spanish",
              "iso6391Name": "es",
              "score": 1
            }
          ],
          "statistics": {
            "charactersCount": 65,
            "transactionsCount": 1
          }
        },
        {
          "id": "4",
          "detectedLanguages": [
            {
              "name": "French",
              "iso6391Name": "fr",
              "score": 1
            }
          ],
          "statistics": {
            "charactersCount": 8,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 4,
        "validDocumentsCount": 4,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 4
      }
    }
    ```

Ahora podemos correlacionar los documentos de los datos JSON de la carga de respuesta con los documentos de la carga de solicitud originales por su `id` correspondiente. Cada documento se trata de forma independiente con varias estadísticas, como `characterCount` y `transactionCount`. Además, cada documento resultante tiene la matriz `detectedLanguages` con los valores de `name`, `iso6391Name` y `score` de cada idioma detectado. Cuando se detectan varios idiomas, `score` se usa para determinar el más probable.