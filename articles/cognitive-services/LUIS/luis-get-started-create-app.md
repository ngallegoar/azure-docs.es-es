---
title: 'Inicio rápido: Creación de una aplicación (LUIS)'
description: En este inicio rápido se indica cómo crear una aplicación de LUIS que utiliza el dominio pregenerado `HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 10/13/2020
ms.openlocfilehash: 2048079a9b1eb5234d03c6fe57073228bd892d53
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95019167"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Inicio rápido: Uso de automatización del hogar compilada previamente

En esta guía de inicio rápido, creará una aplicación de LUIS que utiliza el dominio pregenerado`HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

[!INCLUDE [Select authoring resource](./includes/select-authoring-resource.md)]

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
Las aplicaciones se pueden crear y administrar en **My Apps** (Mis aplicaciones).

### <a name="create-an-application"></a>Crear una aplicación

Para crear una aplicación, haga clic en  **+ Nueva aplicación**. 

En la ventana que aparece, escriba la siguiente información:

|Nombre  |Descripción  |
|---------|---------|
|Nombre     | El nombre de la aplicación. Por ejemplo, "home automation".        |
|Cultura     | El idioma que la aplicación entiende y habla.   |
|Descripción | La descripción de la aplicación.
|Recurso de predicción | El recurso de predicción que recibirá las consultas. |

Seleccione **Listo**.

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

1. Seleccione **Intenciones** en el menú de navegación izquierdo para ver las intenciones del dominio HomeAutomation. Tiene expresiones de ejemplo, como `HomeAutomation.QueryState` y `HomeAutomation.SetDevice`.

    > [!NOTE]
    > **None** (Ninguna) es una intención que proporcionan todas las aplicaciones de LUIS. Se usa para controlar las expresiones que no corresponden a la funcionalidad que proporciona la aplicación.

1. Seleccione la intención **HomeAutomation.TurnOff**. La intención contiene una lista de expresiones de ejemplo que están etiquetadas con entidades.

    > [!div class="mx-imgBorder"]
    > [![Captura de pantalla de la intención HomeAutomation.TurnOff](media/luis-quickstart-new-app/home-automation-turnoff.png "Captura de pantalla de la intención HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnoff.png)

1. Si desea ver las entidades de la aplicación, seleccione **Entidades**. Si hace clic en una de las entidades, como **HomeAutomation.DeviceName** verá una lista de valores que tiene asociados. 
 
    :::image type="content" source="media/luis-quickstart-new-app/entities-page.png" alt-text="Texto alternativo de la imagen" lightbox="media/luis-quickstart-new-app/entities-page.png":::

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="test-your-app"></a>Prueba de la aplicación
Una vez que haya entrenado a su aplicación, puede probarla.

1. Select **Test** (Probar) en el panel de navegación superior derecho.

1. Escriba una expresión de prueba, como `Turn off the lights` en el panel de prueba interactivo y presione Entrar. Por ejemplo, *Turn off the lights*.

    En este ejemplo, `Turn off the lights` está identificada correctamente como la intención con puntuación más alta de **HomeAutomation.TurnOff**.

    ![Captura de pantalla del panel de prueba con expresión resaltada](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

1. Seleccione **Inspeccionar** para ver más información sobre la predicción.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del panel de prueba con información de inspección](media/luis-quickstart-new-app/test.png)

1. Cierre el panel de prueba.

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión

Para recibir una predicción de LUIS en un bot de chat u otra aplicación cliente, tiene que publicar la aplicación en el punto de conexión de predicción.

1. Seleccione **Publish** (Publicar) en la esquina derecha de la ventana.

1. Seleccione el espacio de **producción** y, después, **Done** (Listo).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la publicación de LUIS en el punto de conexión](media/howto-publish/publish-app-popup.png)

1. Seleccione el vínculo **Access your endpoint URLs** (Acceder a las direcciones URL del punto de conexión) de la notificación para ir a la página **Azure Resources** (Recursos de Azure). Las direcciones URL del punto de conexión se muestran como **Example Query** (Ejemplo de consulta).

<!-- [!INCLUDE [LUIS How to Publish steps](./includes/howto-publish.md)] -->

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
> [Llamada a un punto de conexión de LUIS mediante código](./luis-get-started-get-intent-from-rest.md)