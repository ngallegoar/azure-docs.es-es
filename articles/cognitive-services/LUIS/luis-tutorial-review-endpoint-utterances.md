---
title: 'Tutorial: Revisión de expresiones de punto de conexión: LUIS'
description: En este tutorial, va a mejorar las predicciones de las aplicaciones mediante la comprobación o corrección de las expresiones recibidas mediante el punto de conexión HTTPS de LUIS de las que LUIS no está seguro. Algunas expresiones puede que se comprueben para la intención y otras puede que necesiten comprobarse para la entidad.
services: cognitive-services
ms.topic: tutorial
ms.date: 04/01/2020
ms.openlocfilehash: 307c18d3326cb1a64b884463a571985a015834ed
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548722"
---
# <a name="tutorial-fix-unsure-predictions-by-reviewing-endpoint-utterances"></a>Tutorial: Corrección de predicciones poco seguras mediante la revisión de las expresiones del punto de conexión
En este tutorial, va a mejorar las predicciones de las aplicaciones mediante la comprobación o corrección de las expresiones recibidas mediante el punto de conexión HTTPS de LUIS de las que LUIS no está seguro. Debe revisar las expresiones de punto de conexión como una parte convencional del mantenimiento programado de LUIS.

Este proceso de revisión permite que LUIS aprenda el dominio de aplicación. LUIS selecciona las expresiones que aparecen en la lista de revisión. Esta lista:

* Es específica de la aplicación.
* Está pensada para mejorar la precisión de la predicción de la aplicación.
* Debe revisarse periódicamente.

Al revisar las expresiones de punto de conexión, debe comprobar o corregir la intención pronosticada de la expresión.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Revisar las expresiones de punto de conexión
> * Entrenamiento y publicación de la aplicación
> * Consulta del punto de conexión de la aplicación para ver la respuesta JSON de LUIS

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importar la aplicación de ejemplo

Siga estos pasos para importar una aplicación.

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-sentiment-HumanResources.json?raw=true).

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Entrenamiento de la aplicación para aplicar los cambios de la entidad a la aplicación

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Publicación de la aplicación para tener acceso a ella desde el punto de conexión HTTP

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="add-utterances-at-the-endpoint"></a>Adición de expresiones en el punto de conexión

En esta aplicación, tiene intenciones y entidades, pero no tiene ningún uso de punto de conexión. Este uso del punto de conexión es necesario para mejorar la aplicación con la revisión de expresiones del punto de conexión.

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Vaya al final de la dirección URL de la barra de direcciones y reemplace _YOUR_QUERY_HERE_ por las expresiones de la siguiente tabla. Con cada una, envíe la expresión y obtenga el resultado. Después, reemplace la expresión al final por la próxima expresión.

    |Expresión del punto de conexión|Intención alineada|
    |--|--|
    |`I'm looking for a job with Natural Language Processing`|`GetJobInformation`|
    |`I want to cancel on March 3`|`Utilities.Cancel`|
    |`When were HRF-123456 and hrf-234567 published in the last year?`|`FindForm`|
    |`shift 123-45-6789 from Z-1242 to T-54672`|`MoveEmployee`|
    |`Please relocation jill-jones@mycompany.com from x-2345 to g-23456`|`MoveEmployee`|
    |`Here is my c.v. for the programmer job`|`ApplyForJob`|
    |`This is the lead welder paperwork.`|`ApplyForJob`|
    |`does form hrf-123456 cover the new dental benefits and medical plan`|`FindForm`|
    |`Jill Jones work with the media team on the public portal was amazing`|`EmployeeFeedback`|

    Existe un único grupo de expresiones para revisar, independientemente de la versión que esté editando activamente o de la versión de la aplicación que se haya publicado en el punto de conexión.

## <a name="review-endpoint-utterances"></a>Revisar las expresiones de punto de conexión

Revise las expresiones del punto de conexión en busca de una intención alineada correctamente. Aunque hay un único grupo de expresiones para revisar en todas las versiones, el proceso de alinear correctamente la intención agrega la expresión de ejemplo solo al _modelo activo_ actual.

1. En la sección **Build** (Compilar) del portal, seleccione **Review endpoint utterances** (Revisar expresiones del punto de conexión) en el panel de navegación izquierdo. La lista se filtra para la intención **ApplyForJob**.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del botón Review endpoint utterances (Revisar expresiones del punto de conexión) en el panel de navegación izquierdo](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png)

    Esta expresión, `I'm looking for a job with Natural Language Processing`, no se encuentra en la intención correcta.

1.  Para alinear esta expresión, en la fila de la expresión, seleccione la **intención alineada** correcta de `GetJobInformation`. Agregue la expresión cambiada a la aplicación; para ello, seleccione la marca de verificación.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del botón Review endpoint utterances (Revisar expresiones del punto de conexión) en el panel de navegación izquierdo](./media/luis-tutorial-review-endpoint-utterances/select-correct-aligned-intent-for-endpoint-utterance.png)

    Revise el resto de expresiones de esta intención, corrigiendo la intención alineada según sea necesario. Use la tabla de expresiones inicial de este tutorial para ver la intención alineada.

    La lista **Review endpoint utterances** (Revisar expresiones del punto de conexión) ya no debe tener las expresiones corregidas. Si aparecen más expresiones, continúe trabajando en la lista y corrija las intenciones alineadas hasta que la lista esté vacía.

    Cualquier corrección del etiquetado de la entidad se realiza después de que la intención se alinee, desde la página de detalles de la intención.

1. Vuelva a entrenar y publicar la aplicación.

## <a name="get-intent-prediction-from-endpoint"></a>Obtención de la predicción de la intención desde un punto de conexión

Para comprobar que las expresiones de ejemplo correctamente alineadas han mejorado la predicción de la aplicación, pruebe una expresión cercana a la expresión corregida.

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Vaya al final de la dirección URL de la barra de direcciones y reemplace _YOUR_QUERY_HERE_ por: `Are there any natural language processing jobs in my department right now?`.

   ```json
    {
        "query": "Are there any natural language processing jobs in my department right now?",
        "prediction": {
            "topIntent": "GetJobInformation",
            "intents": {
                "GetJobInformation": {
                    "score": 0.903607249
                },
                "EmployeeFeedback": {
                    "score": 0.0312187821
                },
                "ApplyForJob": {
                    "score": 0.0230276529
                },
                "MoveEmployee": {
                    "score": 0.008322801
                },
                "Utilities.Stop": {
                    "score": 0.004480808
                },
                "FindForm": {
                    "score": 0.00425248267
                },
                "Utilities.StartOver": {
                    "score": 0.004224336
                },
                "Utilities.Help": {
                    "score": 0.00373591436
                },
                "None": {
                    "score": 0.0034621188
                },
                "Utilities.Cancel": {
                    "score": 0.00230977475
                },
                "Utilities.Confirm": {
                    "score": 0.00112078607
                }
            },
            "entities": {
                "keyPhrase": [
                    "natural language processing jobs",
                    "department"
                ],
                "datetimeV2": [
                    {
                        "type": "datetime",
                        "values": [
                            {
                                "timex": "PRESENT_REF",
                                "resolution": [
                                    {
                                        "value": "2019-12-05 23:23:53"
                                    }
                                ]
                            }
                        ]
                    }
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "natural language processing jobs",
                            "startIndex": 14,
                            "length": 32,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "builtin.keyPhrase",
                            "text": "department",
                            "startIndex": 53,
                            "length": 10,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "datetimeV2": [
                        {
                            "type": "builtin.datetimeV2.datetime",
                            "text": "right now",
                            "startIndex": 64,
                            "length": 9,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
   ```

   Ahora que las expresiones no seguras están correctamente alineadas, la intención correcta se predecía con una **puntuación alta**.

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>¿Se puede reemplazar la revisión al agregar más expresiones?
Tal vez se pregunte por qué no agregar más expresiones de ejemplo. ¿Cuál es el propósito de la revisión de las expresiones de punto de conexión? En una aplicación LUIS del mundo real, las expresiones de punto de conexión proceden de usuarios con una elección y disposición de palabras que todavía no han utilizado. Si hubiera usado la misma elección y disposición de palabras, la predicción original tendría un porcentaje mayor.

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>¿Por qué está la intención principal en la lista de expresiones?
Algunas de las expresiones de punto de conexión tendrán una puntuación de predicción alta en la lista de revisión. Aun así deberá revisar y comprobar dichas expresiones. Están en la lista porque la siguiente intención más elevada tenía una puntuación demasiado cercana a la puntuación máxima de la intención. Desea una diferencia de aproximadamente el 15 % entre dos de las intenciones con más puntuación.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial revisó las expresiones enviadas en el punto de conexión, de las que LUIS no estaba seguro. Una vez que comprobadas estas expresiones y trasladadas a las intenciones correctas como expresiones de ejemplo, LUIS mejorará la precisión de la predicción.

> [!div class="nextstepaction"]
> [Más información sobre cómo usar los patrones](luis-tutorial-pattern.md)
