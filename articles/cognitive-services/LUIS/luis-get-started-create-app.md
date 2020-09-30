---
title: 'Inicio rápido: Creación de una aplicación (LUIS)'
description: En este inicio rápido se indica cómo crear una aplicación de LUIS que utiliza el dominio pregenerado `HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/05/2020
ms.openlocfilehash: 28bf79b61c0278a3f45820a23cd2c69f0b609700
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91316499"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Inicio rápido: Uso de automatización del hogar compilada previamente

En esta guía de inicio rápido, creará una aplicación de LUIS que utiliza el dominio pregenerado`HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

[!INCLUDE [Select authoring resource](./includes/select-authoring-resource.md)]

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
Las aplicaciones se pueden crear y administrar en **My Apps** (Mis aplicaciones).

1. En la lista Mis aplicaciones, seleccione **+ New app for conversation** (+ Nueva aplicación para conversación) y, a continuación, en la lista de opciones, vuelva a seleccionar **+ New app for conversation** (+ Nueva aplicación para conversación).

1. En el cuadro de diálogo asigne el nombre `Home Automation` a la aplicación.
1. Seleccione la referencia cultural **English** (Inglés).
1. Escriba una descripción opcional.
1. No seleccione un recurso de predicción si aún no ha creado el recurso. Para usar el punto de conexión de predicción de la aplicación (almacenamiento provisional o producción), debe asignar un recurso de predicción.
1. Seleccione **Listo**.

    Se crea la aplicación en LUIS.

    ![En el cuadro de diálogo, asigne a la aplicación l nombre "Home Automation".](./media/create-new-app-details.png)

    >[!NOTE]
    >La referencia cultural no se puede cambiar una vez creada la aplicación.

## <a name="add-prebuilt-domain"></a>Incorporación de un dominio creado previamente

1. En el panel de navegación izquierdo, seleccione **Dominios creados previamente**.
1. Busque **HomeAutomation**.
1. Seleccione **Add domain** (Agregar dominio) en la tarjeta HomeAutomation.

    > [!div class="mx-imgBorder"]
    > ![Seleccione "Dominios creados previamente" y, luego, busque "HomeAutomation". Seleccione "Agregar dominio" en la tarjeta HomeAutomation.](media/luis-quickstart-new-app/home-automation.png)

    Cuando el dominio se agrega correctamente, el cuadro de dominio pregenerado muestra un botón **Remove domain** (Quitar dominio).

## <a name="intents-and-entities"></a>Intenciones y entidades

1. Seleccione **Intents** (Intenciones) para revisar las intenciones del dominio HomeAutomation. Las intenciones del dominio precompilado tienen expresiones de ejemplo.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la lista de intenciones de HomeAutomation](media/luis-quickstart-new-app/home-automation-intents.png "Captura de pantalla de la lista de intenciones de HomeAutomation")

    > [!NOTE]
    > **None** (Ninguna) es una intención que proporcionan todas las aplicaciones de LUIS. Se usa para controlar las expresiones que no corresponden a la funcionalidad que proporciona la aplicación.

1. Seleccione la intención **HomeAutomation.TurnOff**. La intención contiene una lista de expresiones de ejemplo que están etiquetadas con entidades.

    > [!div class="mx-imgBorder"]
    > [![Captura de pantalla de la intención HomeAutomation.TurnOff](media/luis-quickstart-new-app/home-automation-turnoff.png "Captura de pantalla de la intención HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="test-your-app"></a>Prueba de la aplicación
Una vez que haya entrenado a su aplicación, puede probarla.

1. Select **Test** (Probar) en el panel de navegación superior derecho.

1. Escriba una expresión de prueba, como `Turn off the lights` en el panel de prueba interactivo y presione Entrar.

    ```
    Turn off the lights
    ```

    En este ejemplo, `Turn off the lights` está identificada correctamente como la intención con puntuación más alta de **HomeAutomation.TurnOff**.

    ![Captura de pantalla del panel de prueba con expresión resaltada](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

1. Seleccione **Inspeccionar** para ver más información sobre la predicción.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del panel de prueba con información de inspección](media/luis-quickstart-new-app/test.png)

1. Cierre el panel de prueba.

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión

[!INCLUDE [LUIS How to Publish steps](./includes/howto-publish.md)]

<a name="query-the-v2-api-prediction-endpoint"></a>

## <a name="query-the-v3-api-prediction-endpoint"></a>Consulta del punto de conexión de predicción de la API V3

[!INCLUDE [LUIS How to get endpoint first step](./includes/v3-prediction-endpoint.md)]

2. En la barra de direcciones del explorador, asegúrese de que las siguientes barras de nombre y valor se encuentran en la dirección URL de la cadena de consulta. Si no están en la cadena de consulta, agréguelas:

    |Par nombre-valor|
    |--|
    |`verbose=true`|
    |`show-all-intents=true`|

3. En la barra de direcciones del explorador, vaya al final de la dirección URL y escriba `turn off the living room light` como el valor de _consulta_ y, luego, presione Entrar.

    ```json
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOff",
            "intents": {
                "HomeAutomation.TurnOff": {
                    "score": 0.969448864
                },
                "HomeAutomation.QueryState": {
                    "score": 0.0122336326
                },
                "HomeAutomation.TurnUp": {
                    "score": 0.006547436
                },
                "HomeAutomation.TurnDown": {
                    "score": 0.0050634006
                },
                "HomeAutomation.SetDevice": {
                    "score": 0.004951761
                },
                "HomeAutomation.TurnOn": {
                    "score": 0.00312553928
                },
                "None": {
                    "score": 0.000552945654
                }
            },
            "entities": {
                "HomeAutomation.Location": [
                    "living room"
                ],
                "HomeAutomation.DeviceName": [
                    [
                        "living room light"
                    ]
                ],
                "HomeAutomation.DeviceType": [
                    [
                        "light"
                    ]
                ],
                "$instance": {
                    "HomeAutomation.Location": [
                        {
                            "type": "HomeAutomation.Location",
                            "text": "living room",
                            "startIndex": 13,
                            "length": 11,
                            "score": 0.902181149,
                            "modelTypeId": 1,
                            "modelType": "Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "HomeAutomation.DeviceName": [
                        {
                            "type": "HomeAutomation.DeviceName",
                            "text": "living room light",
                            "startIndex": 13,
                            "length": 17,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "HomeAutomation.DeviceType": [
                        {
                            "type": "HomeAutomation.DeviceType",
                            "text": "light",
                            "startIndex": 25,
                            "length": 5,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
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

    Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).


## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Puede llamar al punto de conexión desde el código:

> [!div class="nextstepaction"]
> [Llamada a un punto de conexión de LUIS mediante código](luis-get-started-cs-get-intent.md)
