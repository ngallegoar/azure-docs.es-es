---
title: 'Inicio rápido: Creación de una aplicación (LUIS)'
description: En este inicio rápido se indica cómo crear una aplicación de LUIS que utiliza el dominio pregenerado `HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.
ms.topic: quickstart
ms.date: 03/24/2020
ms.openlocfilehash: de6cf5e95ee63fc9500cf1b5edab78597bdb18af
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80287805"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Inicio rápido: Uso de automatización del hogar compilada previamente

En esta guía de inicio rápido, creará una aplicación de LUIS que utiliza el dominio pregenerado`HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

[!INCLUDE [Select authoring resource](./includes/select-authoring-resource.md)]

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
Las aplicaciones se pueden crear y administrar en **My Apps** (Mis aplicaciones).

1. En la lista My app (Mis aplicaciones), seleccione **+ New app for conversation** (Nueva aplicación para conversación).

1. En el cuadro de diálogo asigne el nombre `Home Automation` a la aplicación. Seleccione la referencia cultural **English** (Inglés). La descripción es opcional y no se usa para la creación ni la predicción. El recurso de predicción también es opcional cuando se crea una aplicación en LUIS. Seleccione **Listo**.

    Se crea la aplicación en LUIS. Al publicar la aplicación en producción, debe asignar un recurso de predicción para que la aplicación pueda administrar muchas solicitudes.

    ![En el cuadro de diálogo, asigne a la aplicación l nombre "Home Automation".](./media/create-new-app-details.png)

    >[!NOTE]
    >La referencia cultural no se puede cambiar una vez creada la aplicación.

## <a name="add-prebuilt-domain"></a>Incorporación de un dominio creado previamente

Seleccione **Prebuilt domains** (Dominios creados previamente) y busque **HomeAutomation**. Seleccione **Add domain** (Agregar dominio) en la tarjeta HomeAutomation.

![Seleccione Prebuilt domains (Dominios creados previamente) y busque "HomeAutomation". Seleccione "Add domain" (Agregar dominio) en la tarjeta HomeAutomation.](media/luis-quickstart-new-app/home-automation.png)

Cuando el dominio se agrega correctamente, el cuadro de dominio pregenerado muestra un botón **Remove domain** (Quitar dominio).

## <a name="intents-and-entities"></a>Intenciones y entidades

Seleccione **Intents** (Intenciones) para revisar las intenciones del dominio HomeAutomation. Las intenciones del dominio creado previamente tienen expresiones de ejemplo.

![Captura de pantalla de la lista de intenciones de HomeAutomation](media/luis-quickstart-new-app/home-automation-intents.png "Captura de pantalla de la lista de intenciones de HomeAutomation")

> [!NOTE]
> **None** (Ninguna) es una intención que proporcionan todas las aplicaciones de LUIS. Se usa para controlar las expresiones que no corresponden a la funcionalidad que proporciona la aplicación.

Seleccione la intención **HomeAutomation.TurnOff**. Puede ver que la intención contiene una lista de expresiones que están etiquetados con las entidades.

[![Captura de pantalla de la intención HomeAutomation.TurnOff](media/luis-quickstart-new-app/home-automation-turnoff.png "Captura de pantalla de la intención HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="test-your-app"></a>Prueba de la aplicación
Una vez que haya entrenado a su aplicación, puede probarla.

1. Select **Test** (Probar) en el panel de navegación superior derecho. 1. Escriba una expresión de prueba, como `Turn off the lights` en el panel de prueba interactivo y presione Entrar.

    ```
    Turn off the lights
    ```

    Compruebe que la intención con puntuación más alta se corresponde con la intención que espera para cada expresión de prueba.

    En este ejemplo, `Turn off the lights` está identificada correctamente como la intención con puntuación más alta de **HomeAutomation.TurnOff**.

    ![Captura de pantalla del panel de prueba con expresión resaltada](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

1. Seleccione **Inspeccionar** para revisar más información sobre la predicción.

    ![Captura de pantalla del panel de prueba con información de inspección](media/luis-quickstart-new-app/test.png)

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
                    "score": 0.984315455
                },
                "HomeAutomation.QueryState": {
                    "score": 0.009912962
                },
                "HomeAutomation.TurnDown": {
                    "score": 0.00626645749
                },
                "HomeAutomation.TurnUp": {
                    "score": 0.00572059769
                },
                "HomeAutomation.SetDevice": {
                    "score": 0.00379381469
                },
                "HomeAutomation.TurnOn": {
                    "score": 0.00366983772
                },
                "None": {
                    "score": 0.000623856
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
                            "score": 0.907323956,
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
